## 基本概念
在Emacs中有mod这个概念，同样在textmate中也有bundle，这些东西都对某种特定的文件类型定义了特定的操作和表现形式，EverEdit也借鉴了这个概念，称之为模式(mode)。文本编辑器是一个比较单纯的工具，而我们在日常工作中需要用到各种各样的文件，比如html,css,javascript,c++,java等等。很多的时候，我们希望有一套与这些文件类型绑定的快捷键、工具条、菜单等。比如我们在切换到html类型的文件的时候，工具条和菜单都会显示为html的，其中有加粗，预览等操作；在切换到java的时候，我们又期望显示为java独有的工具条和菜单，比如编译，javadoc的操作等等。EverEdit充分考虑这些需求，提供了最简单、直接而且异常强大的支持。

## 构成元素
模式是由一系列的元素构成的，这些独特的元素可以绑定到某个特定的文件类型中，常见的元素有：

* 快捷键
* 工具条
* 菜单

## 新建一个模式
模式文件保存于安装目录的mode文件夹，所有以esm结尾的文件都是模式文件，其中描述了模式的构成。下面我们新建一个简单的文件，叫做MyMode.esm，内容如下：

```
[Menu]
Title=MyMode
[Menu0]
Key=C+O
Command0=0,Insert Some Text,left text$0right text
```

然后刷新所有的模式，在`主菜单→扩展→模式`中，就可以看到它了。点击它的话，当前文件就会切换到MyMode，同时主菜单会出现一个MyMode的菜单。点一下看看效果吧！

## 命令详解
下面我们来详细了解MyMode.esm中各个字段所代表的意思。Menu.Title就不用说了，表示当前模式文件的标题；Menu0.Key表示当前操作的快捷键；Menu0.Command0表示要执行的具体命令，它后面的字符串是以逗号分割的文本，有着特殊的意义，构成如下：

**操作类型,标题,执行命令**

操作类型有如下几种：
0：以snippet的形式插入文本
1：调用DLL里面的函数
2：执行EXE或者打开URL
3：执行一个脚本文件

标题的部分可以随便写，命令部分需要按照类型指定的格式书写。当类型是0的时候，可以使用snippet中使用的常见技巧，同时也可以使用${SELECTED}表示选择的文本，遇到${SELECTED}的时候，EverEdit会用普通选区的文本进行替换；当类型是1的时候，之后的字符串为**dll路径,调用的导出函数**（注意：这部分视DLL的需求可能不一样）;当类型是2的时候，表示打开一个EXE或者网址；当类型是3的时候，后面的则是一个需要EverEdit调用的脚本的路径。其中我们可以使用${AppPath}，它表示安装路径，从而可以组合出各种路径。

**添加更多的菜单项**：上面的实例中使用了Menu0表示一个菜单项，因此如果想添加更多的菜单项的话，可以按照顺序逐个使用Menu1,Menu2…

**添加分隔条**：有时候为了美观的需要，我们期望添加一个分隔条，添加一个特殊的Menu,如下

```
[Menu3]
SEP=Y
```

**添加子菜单**：如果期望为某个菜单项添加子菜单的话，需要制定这个Menu为Popup样式，以HTML.esm为例

```
[Menu3]
Popup=Y
Title=Emmet
[Menu3\0]
Key=C+E
Command0=3,Abbreviation,${AppPath}\mode\html\abbrev.ejs
[Menu3\1]
Key=C+0
Command0=3,Match Pair Inward,${AppPath}\mode\html\match_pair_inward.ejs
```

子菜单的项目为Menu3+斜线+序号,是不是很简单？**注意**：在菜单中，所有的命令必须为Command0。

## 工具条
EverEdit不仅支持自定义菜单，也支持对模式自定义工具条，描述信息同样存储于esm文件中。工具条支持16X16或者32X32的图标列表，文件可以是bmp或者png。定义图标文件：

```
[ToolBar]
Image16=common.bmp
```

如需定义32X32的，则加入Image32=xxx.bmp即可。当用户在工具条上选择大图标、小图标时，EverEdit将会自动的使用用户定义的大、小图片切换显示。其中32大小的图片不定义的话，将会始终使用16大小的图片，EverEdit完美支持高分屏，即使之定义16x16的图片，EverEdit也会经过优化的算法保证在高分屏幕下完美显示。示例：

```
;定义第8个按钮
[Button8]
;使用第28个Icon
Icon=28
;定义第一个命令。
Command0=0,Block Quote,<blockquote>${SELECTED}</blockquote>
```

当有多个命令定义到一个按钮中时，按钮会自动显示下拉箭头，供用户进行选择。如下，定义多个操作：
```
[Button22]
Icon=14
command0=0,Text Box,<input type="text" name="$0">
command1=0,List Box,<select name="">\n\t<option value="$0" selected>\n\t<option value="">\n</select>
command2=0,Radio Button,<input type="radio" name="$0">
command3=0,Check Box,<input type="checkbox" name="$0">
```

如需在按钮之间插入分割条的话，使用如下代码：
```
[Button3]
SEP=TRUE
```

## 快捷键
模式文件中可以使用任意快捷键，包括多级快捷键。模式文件的快捷键将会覆盖全局快捷键，并且仅在该模式中起作用。快捷键同时可以用于模式的工具条按钮和菜单。示例：

```
;定义Ctrl+Shift+B到某一个操作
Key=CS+B
Command0=...
```

## DLL原型说明
EverEdit的模式文件中同样支持对于dll中函数的调用，从而完成非常复杂的操作。Dll被调用的函数需要遵循以下的函数原型：

```
DWORD YourDllName(EE_Context* pContext, LPRECT lpButton, LPCTSTR lpText);
EE_Context：请参考EverEdit的插件SDK的资料。
lpButton：被点击的矩形大小，通常用于菜单弹出的位置判断
lpText：从定义文件传来的参数文本
```

示例：
```
 #include <windows.h>
 #include "eecore.h"
 #include "eesdk.h"

extern "C" {
    _declspec(dllexport) DWORD MyToolBarCallBack(EE_Context* pContext, LPRECT lpRect, LPCTSTR lpText);
};

DWORD MyToolBarCallBack(EE_Context* pContext, LPRECT lpRect, LPCTSTR lpText)
{
    ::MessageBox(pContext->hMain, _T("Hello world!"), _T("My DLL"), MB_OK );
    return 0;
}

BOOL APIENTRY DllMain( HMODULE hModule,
                       DWORD  ul_reason_for_call,
                       LPVOID lpReserved
                )
{
    switch (ul_reason_for_call)
    {
        case DLL_PROCESS_ATTACH:
        case DLL_THREAD_ATTACH:
        case DLL_THREAD_DETACH:
        case DLL_PROCESS_DETACH:
        break;
    }
    return TRUE;
}
```

模式文件中调用Dll函数：`Command0=1,Hover text,MyDll.dll,MyDllFunctionName,lpText`，**注意**：lpText处的文本将会作为参数第三个传给MyDllFunctionName

## 绑定模式
在您定义完毕一个模式文件之后，EverEdit并不知道该模式将会被用在哪个语法文件中。所以需要绑定一下。主菜单→工具→设置→语法着色，选定您的语法文件，然后点击高级，绑定即可！
