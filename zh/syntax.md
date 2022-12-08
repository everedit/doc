# 语法着色
## 概述
EverEdit有着优异的语法着色引擎，可以高亮现存的绝大多数的编程语言。在EverEdit的语法着色中有Region和Item两个概念，Region表示着不同的区块。而Item则代表着这些区块中不同的部分。一般情况下，Region表示跨行的文本匹配，而Item表示一行内的文本匹配。另外，Region也支持空匹配，也就是说您可以指定某个特定的位置用来表达Region的起始或者结束，比如行头和行尾甚至于某个特定的列，从而可以完美的高亮markdown等语言的文件。同时Item还支持捕获分组，可以极大地提高定义着色文件的准确性。在安装目录的`syntax`文件夹中已包含很多语法着色文件，您可以参照它们写出更加优美正确的着色效果！

## 代码片段
不同的Region可以绑定不同的代码片段，比如html中的css,js区域等。比如php.mac中的：

```
rPhp.AddSnippet "php.snippet"
```
这样就把snippet加入到了php区域中了。这些snippet仅在该区域起作用，用户输入完整的提示字符，然后按下tab键就可以快速展开。一个区域可以使用AddSnippet函数添加多个snippet。同样，一个文件也可能会有多个不同的着色区域，通过如此搭配，可以实现严格的区分效果。

## 类和函数
着色文件是可编程的，每个着色文件都是一个按照指定语法书写的脚本文件。因EverEdit开发的历史原因，目前着色文件的语法仅支持vbscript。

## 颜色定义
在定义语法文件的时候，一定会想到要把不同的部分显示为不同的颜色。EverEdit内置多种颜色，赶快让您的文件多彩起来吧！下面是syntax目录中const.mac描述的颜色值。注意：颜色的具体设定是在主题里面指定的，这些只是颜色的定义，不是具体的颜色表达。

```
Const COLOR_DEFAULT       =0
Const COLOR_COMMENT1      =1
Const COLOR_COMMENT2      =2
Const COLOR_STRING1       =3
Const COLOR_STRING2       =4
Const COLOR_TAG           =5
Const COLOR_MACRO         =6
Const COLOR_URL           =7
Const COLOR_EMAIL         =8
Const COLOR_NUMBER        =9
Const COLOR_FOUND         =10
Const COLOR_PAIR          =11
Const COLOR_FUNCTION      =12
Const COLOR_VAR           =13
Const COLOR_SUBLAN        =14
Const COLOR_OPERATOR      =15
Const COLOR_WORD1         =16
Const COLOR_WORD2         =17
Const COLOR_WORD3         =18
Const COLOR_WORD4         =19
Const COLOR_HIGHLIGHT1    =20
Const COLOR_HIGHLIGHT2    =21
Const COLOR_HIGHLIGHT3    =22
Const COLOR_HIGHLIGHT4    =23
Const COLOR_HIGHLIGHT5    =24
Const COLOR_HIGHLIGHT6    =25
Const COLOR_HIGHLIGHT7    =26
Const COLOR_HIGHLIGHT8    =27
Const COLOR_IGNORE        =29
Const COLOR_CONCEAL       =30
```

**注意:**凡是被指定为COLOR_IGNORE模式的字符串，它的前景色将和主界面的背景色一致，看起来好像不存在一样。只有被选择的时候，才会看到！而COLOR_CONCEAL则是一个比较有意思的模式，被指定为COLOR_CONCEAL的字符仅当处于选区或者当前行的时候，才是可见的！借助这个特性，可以实现出很多非常有用的功能！

## SyntaxItem
```
void Capture(int group, int state);
string Name;//get,set
```

## WordItem
```
bool AutoCase;//get,set
string Name;//get,set
```

## SyntaxRegion
### 函数
```
bool AddSnippet(string strName, bool bCase=true)
添加snippet到该Region。strName为snippet的文件名称，因所有的snippet文件必须放到snippet文件夹，所以只需要提供snippet的文件名称即可。bCase:snippet在匹配的时候是否区分大小写
void AddItem(SyntaxItem item );
void AddWord(WordItem item );
void AddRegion(SyntaxRegion child );
void FoldText(string strFold, bool bFCase, string strUnFold, bool bUFCase);
指定语法文件的折叠方法。strFold和strUnFold指示了折叠和反折叠的正则表达式。bFCase/bUFCase:是否匹配大小写
void CommentBlock(string strOn, string strOff);
块注释的文本。主要用于Ctrl+Shift+/的调用。比如c++中，可以这么设置 cpp.CommentBlock "/*", “*/"
void CommentLine(string strText);
行注释的文本。用户Ctrl+/的调用。比如c++中，可以这么设置 cpp.CommentLine "//"
void SetPairs(string strText);
指示该Region可用的配对字符。strText:包含所有的配对字符。配对字符必须连续的放在一块定义，很显然strText的长度应为偶数。
void IndentText(string strIndnet, bool c1, string strUnIndnet, bool c2);
定义用于缩进和反缩进的正则表达式。
void AddCallTip( string strPathName, bool bCase, string strWord="", string strBegin="(", string strSep=",", string strEnd=")", bool bRemoveSpace=false );
定义函数提示，具体参考函数提示部分。

void AddSnippet( string title, string trigger, string text, bool script=false);
```

### 属性
```
void TransRegion;//set
当该区域的文本结束时，该Region默认情况下将会自动跳转到父Region；如果定义了TransRegion那么将会跳转到Region。
string Name;//get;set
该Region的名称。设置该名称之后，在脚本中可以获取光标位置的词法状态。
```

## Parser
Parser默认情况下包含了一个DefaultRegion。部分针对Default Region函数的使用可以参考SyntaxRegion的说明。

### 函数
```
bool AddSnippet(string strPathName, bool case_sensitve=true );
void AddItem(SyntaxItem item );
void AddWord(WordItem item );
void AddRegion(SyntaxRegion child);
SyntaxRegion CopyRegion(SyntaxRegion pCopy);
复制一个Region。用于某两个不相关的Region可能包含类似的代码。该函数较耗费资源。
SyntaxItem CreateItem(int state, string strMatch, bool bCase, bool bToRight=false);
创建正则表达式描述的匹配规则。
state:颜色值。
strMatch:正则描述的匹配文本
bCase:是否区分大小写
bToRight:如果该匹配正好在行末，那么背景色是否延伸到窗口右侧。
WordItem CreateWord(int state, string strMatch, bool bCase, string strDelimiters="");
创建关键字匹配。理论上CreateItem可以代替CreateWord，但CreateWord的效率更高且可以自动完成和自动纠正大小写。
strMatch:以空格分割的关键字字符串。
bCase:是否区分大小写
strDelimiters:默认的情况下strMatch所匹配的文本只包括字母数字和下划线，
strDelimiters则表示哪些特殊字符可以被当作一个词，比如中划线-等。

void SetParseMax(int value);
设置单行着色的最大字符上限。默认的情况下，EverEdit只着色每一行的前1000个字符。注意：过大的设置，可能导致程序运行速度过于缓慢！

void AddSnippet( string title, string trigger, string text, bool script=false);
直接在语法文件中，声明一个代码片段

WordItem CreateWordFromFile(int color, string strFileName, bool bCase)
从文件中创建一个关键字列表并指定颜色和大小写，该文件必须放置于syntax目录
AddWordFromFile(string strFileName, string strDelimiters)
从文件中添加指定规则的关键字，它不返回创建的对象，直接按照规则把文件中所有的关键字加入到指定的region。文件的每一行是一个关键字，头尾不可有空格。；开头的是注释，#开头的是新的关键字描述,后面更有3个数字，比如#16,1,0。第一个数字表示词法状态，第二个表示是否区分大小写，第三个表示是否自动更正大小写。
典型的keyword文件如下：
#16,1,0
long
...
#17,1,0
string
...

SyntaxRegion CreateRegion(int state, string strBegin, string strEnd, bool bCase, bool bToRight=false );
创建一个正则表达式描述的Region。
strBegin/strEnd:描述该区域开始或者结束的正则表达式
bCase:strBegin/strEnd描述的正则是否区分大小写
bToRight:如果该Region在行末,那么扩展背景色到右侧窗口
注意:被+两个加号+包围起来的文本表示这不是一个正则表达式，就是普通的文字匹配(效率更高)
SyntaxRegion CreateStringRegion(int state, string strChar, string strEscape, bool mline, string strContinueChar );
创建字符串匹配。
strChar:字符串指示字符，通常为"或者'。
strEscape:转义字符，通常为\
mline:字符串是否跨行。
strContinueChar:字符串续行符，比如C++中的\
void FoldText(string strFold, bool bFCase, string strUnFold, bool bUFCase);
void IndentText(string strIndnet, bool c1, string strUnIndnet, bool c2);
void FoldAnyText(int nLevel, string strText);
正则表达式描述的较为宽松的折叠。
nLevel:折叠的层级
strText:正则表达式
void SetPairs(string strText);
void CommentBlock(string strOn, string strOff);
void CommentLine(string strText);
void AddCallTip( string strPathName, bool bCase, string strWord="", string strBegin="(", string strSep=",", string strEnd=")", bool bRemoveSpace=false );

void SetFont( string font_name, int font_size, int base_line, bool bold);
void SetCJKFont( string font_name, int font_size, int base_line, bool bold);
为该语法文件独立绑定一个字体，当打开用该语法文件渲染的文本时，将会忽略用户的设置，自动使用语法着色文件指定的字体。
```

### 属性
```
string Name;//get,set
默认区域的命名
string FoldingMethod;//get,set
该语法文件的折叠方法。取值为syntax/indent/anytext
string WordChars;//get,set
该语法文件可以被认为是单词的特殊字符。当鼠标双击或者其它操作选择词汇时将会使用该设置。
SyntaxRegion DefaultRegion;//get
string OnOutLine;//set
事件处理脚本，当显示该文件类型的大纲视图时，会自动的调用这个脚本，该脚本应该按照指定格式输出大纲视图识别的格式，该格式后续会介绍。
string OnNewLine;
事件处理脚本，当按下回车键时，会自动调用该脚本，该脚本可以执行常见的脚本函数。
```

## 实例1:创建C++着色器
以C语言为例，描述一下如何去定义一个新的语法描述文件！那么，首先让我们看一下C语言中常见的元素: 

* 单行注释
* 多行注释
* 字符串
* 关键字 

绝大多数的着色文件均是由上面几个非常简单的元素构成。好了，开始我们的自定义吧！建立一个新的文件mycpp.mac并把它放到syntax目录下。为了使用颜色值，我们可以把已经定义好的cosnt.mac包含进来，当然您也可以直接把该文件的内容粘贴到这个文件当中。

```
Include( ".\const.mac" )
```

创建一个自定义的Parser，命名为cpp。

```
Set cpp=Parser.CreateParser()
```

在创建完自定义Parser之后,我们就要开始为之添加各种各样的元素了！
**创建单行注释匹配**：单行注释很显然是一个Region，而不是一个Item，因为它有开始和结束的标志！那就是以//开始，以行尾结束！
```
Set regionLine=cpp.CreateRegion( COLOR_COMMENT1, "+//+", "$", True )
```

**注意**：这儿我们使用了++, 其中被+包围了, 表示这儿为了提高效率不使用正则表达式进行匹配! 结束标志我们把它定义成$, 明确表示匹配到行尾, 不然的话只会匹配//了!
创建多行注释匹配：
```
Set regionBlock=cpp.CreateRegion( COLOR_COMMENT1, "+/*+", "+*/+", True )
```

创建字符串匹配：
```
Set regionString=cpp.CreateStringRegion( COLOR_STRING1, """", "\", False )
```

创建关键字匹配：
```
Set itemWord=cpp.CreateItem(COLOR_WORD1, "\b(int|float|double|char|void|
for|while|if|else|return|break|continue)\b", True)
```

**注意**：为了更好的创建关键字匹配, 可以使用CreateWord函数, 比如上文中的关键字匹配, 也可以这么写，效率更高：
```
Set itemWord2=cpp.CreateWord(COLOR_WORD1, "int float double char void for while if else return break continue", True)
```

把所有创建的匹配加入到主区域中：
```
cpp.AddRegion( regionLine )
cpp.AddRegion( regionBlock )
cpp.AddRegion( regionString )
cpp.AddItem( itemWord )
```

到这儿为止我们就自定义了一个简单的C语言着色引擎，看看成品吧！
```
Include( ".\const.mac" )
Set cpp=Parser.CreateParser()
Set regionLine=cpp.CreateRegion( COLOR_COMMENT1, "+//+", "", True )
Set regionBlock=cpp.CreateRegion( COLOR_COMMENT1, "+/*+", "+*/+", True )
Set regionString=cpp.CreateStringRegion( COLOR_STRING1, """", "\", False )
Set itemWord=cpp.CreateItem(COLOR_WORD1, "\b(int|float|double|char|void|for|while|if|else|return|break|continue)\b", True)
cpp.AddRegion( regionLine )
cpp.AddRegion( regionBlock )
cpp.AddRegion( regionString )
cpp.AddItem( itemWord )
```

别着急，还剩下最后一步。那就是让EverEdit可以识别它！打开语法着色对话框，新建一文件类型为MyCpp，并选择语法文件为刚刚定义的mycpp.mac！

## 实例2：细化C++着色器
在实例1中我们学习了如何创造一个自定义的着色引擎, 那么让我们来进一步丰富这个引擎吧!首先, 我想让注释中所有大写的TODO都显示为一种特殊的颜色! 注意仅仅是注释中, 其它的部分不变!

### 定义TODO匹配
```
Set itemTodo=cpp.CreateItem(COLOR_HIGHLIGHT1, "\bTODO\b", True)
```

把TODO匹配加入到单行和多行注释中：
```
regionLine.AddItem( itemTodo )
regionBlock.AddItem( itemTodo )
```

这样我们就可以仅仅在注释中显示高亮为COLOR_HIGHLIGHT1颜色的TODO文字了！EverEdit同样为代码折叠提供了简单方便的调用接口。比如当遇到{时折叠一次，遇到}时反折叠一次，那么可以简单的像下面书写：

```
cpp.FoldText "\{", False, "\}", False
```

### 自动缩进
假设在mycpp中有这样一种缩进：当您在行尾是EEA的时候，输入回车时，自动缩进一次；输入的行满足含有EverEdit时，自动反缩进一次。

```
cpp.IndentText "EEA$ ", False, "EVEREDIT", False
```

### 括号匹配
我们定义[],{},(),"",''是匹配的字符。

```
cpp.SetPairs "[]{}()""""''"
```

可以被快捷键调用的快速注释：

```
cpp.CommentLine "//"
cpp.CommentBlock "/*", "*/"
```
到这儿为止，我们为mycpp添加了TODO高亮;添加了缩进和反缩进，添加了括号匹配和代码折叠，同时还添加了可以被快捷键或者菜单命令调用的注释和反注释的匹配！EverEdit以其强大的功能，为您提供了大量的自定义特性！赶快动手试一下吧！

## 和着色文件绑定的操作
着色文件不仅描述了如何以分色的方式显示一串文字的不同部分，同时还有一些特定的操作是和着色文件绑定在一起的，比如缩进规则、折叠规则、大纲显示的规则、自定义工具、菜单、工具条等等，这些在EverEdit中均可实现。

## 缩进规则
在EverEdit中，按下回车的时候，是缩进的时机，这个时候应该根据上下行的规则进行细节判断是缩进还是不缩进。那么反缩进呢？反缩进没有更好的时机，在EverEdit中当用户输入的时候，如果光标行所在的文本满足特定的正则表达式的话，进行一次反缩进。这么做不是100%完美的，多少会有些瑕疵。

## 折叠规则
首先我们需要知道的是，完美的代码折叠如果不做语义分析的话，是做不到的，往往眼睛看到的、直观的、应该折叠的地方，却无法找到一个有效的模式去描述。EverEdit的折叠规则非常简单，如果该行文本满足折叠表达式，则折叠一次；满足反折叠表达式，则反折叠一次；同时满足，则折叠归零。这么做非常高效，可以解决绝大数的常见的折叠，同时EverEdit还可以自动的对处于同一个Region的代码进行折叠，不同的Region还可以绑定不同的折叠规则，就像html中的js区域和css区域一样。

那么，有一些语言是比较特殊的，比如ini，它应该折叠折叠每一个section，这些section无法用折叠表达式来描述，所以EverEdit又提供了另一种折叠描述，那就是anytext。anytext是指定义一个规则和指定的level，遇到该规则，则自动的使用指定的level而不是EverEdit自动判断的level。看看ini中的代码折叠。

```
ini.FoldingMethod="anytext"
ini.FoldAnyText 0, "^\s*\[.*?\]$"
```

另外，EverEdit还提供了依据缩进进行折叠，很多缩进规则比较好的语言，用缩进更美观。比如python中的折叠。

```
python.FoldingMethod="indent"
```

## 主题
EverEdit的主题非常的强大，但是它有一个缺点，那就是所有的语法文件都会应用到同一个主题。有时候我们期望某个独特类型的文件能使用自己的主题，可不可以呢？当然没问题！只要打开对应的语法着色文件，然后加一行代码就可以了！

```
parser.SetTheme(“xxxx.ini”)
```

**为什么要独立设定主题？**
比如markdown里面有加粗、倾斜等字体样式，这些样式有时候我们只希望在markdown中应用，这个时候独立的主题就很有用了。

## 模式
模式文件的绑定需要通过界面进行操作。主菜单→工具→设置→语法着色，选定您的语法文件，然后点击高级，绑定即可！

## 帮助文件
每一种文件类型可以绑定后缀为chm或者hlp的帮助文件，当打开对应的帮助文件的时候，EverEdit会自动根据选区的内容，定位到适合的章节。比如我们可以把php的帮助文档，绑定到php中，这样当不知道某个函数的用法的时候，选定这个函数，`帮助→关联帮助`就可以迅速定位了。除此之外，在模式中也可以调用脚本完成，实现更多的帮助文件的绑定。

## 函数提示
EverEdit支持简单的基于字符匹配的函数提示,当输入函数的某个开始字符时,会在上方弹出一个文本框,显示该函数的原型和释义，方便用户确认函数的参数和用法，在输入函数参数的分割符时，EverEdit会自动地以下划虚线的方式强调表示。函数提示是以文本文件的形式存储在calltip文件夹中的，如果需要在calltip中显示中日韩字符，要确保该文件是以UTF8不带BOM的形式进行保存的。

下面来看看该文件的存储格式，以c.ecp为例进行说明。
```
abs(int x):int\nReturns the absolute value of x
```

其中有\n字符，表示这是一个换行符，后面的部分表示函数的释义，当然这部分是可以省略的。前面的部分表示函数的原型。abs一定要写在最开始，EverEdit会从最开始进行匹配，所以我们把函数的返回类型写在了最后。在定义完一个calltip之后，我们需要明确的告知EverEdit：该calltip要被那个着色文件的哪个region加载，使用下面的函数：

```
void AddCallTip( string strFileName, bool bCase, string strWord="", string strBegin="(", string strSep=",", string strEnd=")", bool bRemoveSpace=false );

strFileName：文件名称，不需要带路径。比如c.ecp。
bCase：前面匹配的部分是否要区分大小写
strWord:该参数是指在匹配的时候，哪些字符可以被认为是单词。以c.ecp的abs函数为例，因为该文件定义的函数都是以字母开始的，所以不需要特殊的参数。如果某个函数是诸如abs-def(xx)这样的话，我们可以把-加入到这个参数中，那就是"-"。
strBegin：函数开始字符，一般是左括号(
strSep：参数分割字符，一般是逗号，
strEnd：函数结束字符，一般是右括号)
bRemoveSpace:在匹配的时候是否要删除空格。比如abs    (…)，设置该参数为true的话，则忽略abs到(之前的空格。
```