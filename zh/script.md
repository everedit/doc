## 概述
EverEdit内置JScript/VBS作为基础的脚本解释引擎（理论上支持以ActiveScript形式运行的ruby和python）。在EverEdit内部，JScript/VBS不仅可以用来进行常见的文本操作，也可以定义着色文件。JScript/VBS易学易懂，和Windows关系紧密。借助于脚本， EverEdit可以完成很多匪夷所思的功能，JScript和JavaScript非常的像，语法基本一致，JavaScript使用起来毫无难度！ 本文并不着重于介绍VBS/JScript的基础语法和用法， 如您需要学习VBS/JScript的基础部分， 请参考相关文档。

为了简化和方便管理，被调用的脚本，要统一地放置到安装目录下的`macro`目录中。您可以在`macro`目录中，使用子目录作为分类，脚本的后缀必须为mac，ejs，evb，erb，epy（暂时不识别其它的后缀）。所有被放置到`macro`目录中的文件都会显示在`扩展`菜单中，如未显示，您可以点击`刷新脚本菜单`试试看。

## 类和函数
EverEdit以面向对象的方式封装了一些常用的类和函数，在脚本中处于最顶层的对象是`App`，其它的对象均由此创建而来。

## 全局函数和变量
* `SCRIPT_PATH`: 获取当前脚本的全路径
* `SCRIPT_NAME`: 获取当前脚本的文件名称
* `Include()`: 包含某个文件。注意：EverEdit会直接读取被包含的文件，然后在当前上下文中进行执行。

## Application
### 函数
```c
// 创建菜单。该菜单的弹出将会跟随鼠标位置。
Menu CreateMenu();

bool BindShortcut(string strCommand, string strKey, bool bSaveNow);

string FindShortcut(string strCommand);

void AddTemplate(string strTitle, string strType, string strFile, bool bRunAsSnippet);

Document GetDoc(int index);

void Sleep(int dwMillisec);

// 发送整数形式的命令到主窗口，nCmd是整数值，对应着菜单中的某个菜单项的ID。
void SendCommand(int nCmd);

// 发送字符串形式的命令到主窗口，具体命令可参考快捷键中显示的文本。
void SendCommandEx(string strText);

// web预览指定路径的文件，预览的web文件将会链接当前活动的文档窗口。可以使用Ctrl+B在链接的文档间切换。
void WebPreview(string strPathName);

void WebOutput(string strText);

Document NewDoc();

Document CreateDoc();

Document OpenDoc(string strPathName);

/**
    输出文本到输出窗口
    bClear:是否清除当前文
    bTerminate:是否终止当前正在运行的程序。
**/
void OutputText(string strText, bool bClear=false, bool bTerminate=false);

// 打印文本到输出窗口并自动添加一个换行符
void OutputLine(string strText);

// 打印文本到输出窗口
void Print(string text);

// 打印文本到输出窗口并自动添加一个换行符
void PrintLine(string text);

/**
    弹出文本输入框
    strPrompt:提示文字
    strTitle:对话框的标题
**/
string ShowInputBox(string strPrompt, string strTitle);

/**
执行一个命令行程序，并获取它的stdout的输出
initdir: 初始目录
encoding: 该程序的输出编码
**/
string GetResultFromExe(string cmdline, string initdir="", int encoding=0)。

// 打开代码管理窗口，并让其显示指定title的snippet文件
void OpenSnippetByTitle(string title);

// 获取当前显示器，平均字符宽度
int GetEmWidth(); 

// 运行自定义工具
void RunTool(string toolFile)

// 下载文件
void DownloadFile(string fileName, string url, string saveTo);

bool Unzip(string pathName, string unzipTo);

IniFile* GetIniFile(string pathName);

string md5file(string pathName);

string md5(string text);

string GetTempFolder()

// 状态栏闪烁文本, 5秒后停止闪烁
void Alert(string text)
```

```c
/**
    弹出消息对话框
    strText:对话框中的文本
    strTitle:对话框的标题
    buttons:对话框按钮的组合

    有以下几种组合(摘自MSDN):
    #define MB_OK                       0x00000000L
    #define MB_OKCANCEL                 0x00000001L
    #define MB_ABORTRETRYIGNORE         0x00000002L
    #define MB_YESNOCANCEL              0x00000003L
    #define MB_YESNO                    0x00000004L
    #define MB_RETRYCANCEL              0x00000005L
    #define MB_CANCELTRYCONTINUE        0x00000006L
    #define MB_ICONHAND                 0x00000010L
    #define MB_ICONQUESTION             0x00000020L
    #define MB_ICONEXCLAMATION          0x00000030L
    #define MB_ICONASTERISK             0x00000040L
**/
int ShowMsgBox(string strText, string strTitle, int buttons);

/**
显示chm或者hlp后缀的帮助文件
strPathName:帮助文件的全路径
strWord:打开帮助文件时自动查找的词汇。如果为空，则定位到起始页
**/
int ShowHtmlHelp(string strPath, string strWord);

// 创建临时文件。bAutoDelete:主程序结束时是否自动删除
string CreateTempFile(bool bAutoDelete)

// 显示打开或保存文件的对话框。
bool RemoveFile(string strPath);

string ShowFileDialog(bool bOpen, string strInitDir, string strDefExt);

OutputWindow CreateOutputWindow(string strTitle);

// 用于调试插件DLL。通过个简单的脚本(App.DebugLibrary "xxxx.dll")加载指定路径的DLL，
// 结合开发工具(比如Visual Studio)的附加到进程的功能，可以非常方便地达到调试DLL的目的。
void DebugLibrary(string strPathName);

void SetValue(string strPathName, string val);

string GetValue(string strPathName);

// 获取一个持久化存储的表，如不存在则创建
PersistentStringTable GetPersistentStringTable(string name); 

//创建字符串表
StringTable CreateStringTable();

// 创建一个html对话框, 里面可以调用ee的各种脚本。通过此函数创建之后，EverEdit的全局对象会放到window.external中, 可以通过var App=window.external获取到
HtmlDialog ShowHtmlDialog(bool modal, String url, int width, int height); 
```

### 属性
```c
// 获取当前的文档
Document ActiveDoc; //get

// 获取当前的hex文档
HexDoc ActiveHex; //get

// 等同ActiveDoc
Document document; //get

// 获取当前激活的输出窗口
OutputWindow OutputWindow; //get

// 获取当前的项目
Project Project; //get

// 获取主程序的路径
string AppPath; //get

// 获取命令窗口
string CommandBox; //get

ULONG Hwnd; //get

// 获取文件总数
ULONG FileCount; //get

// 获取版本
string Version; //get

// 获取或设置剪贴板
string ClipboardText; //get,set

// 获取语言
int Lang;//get

// 获取当前目录
string CurDir; //get,set
```

## Document
### 函数
```c
// 创建菜单。菜单的弹出位置将会跟随光标Caret。
Menu CreateMenu();

void SendCommand( int nCmd);

bool Activate();

// 强制刷新、重绘文档。
void Refresh();

long GetHwnd();

bool SetImageList(string path);

int GetLineImage(int line);

void SetLineImage(int line, int icon);

string GetLineTooltip(int line);

void SetLineTooltip(int line, string strtext);

void AppendLineTooltip(int line, string strtext);

// 当前文档是否含有普通选区。列选和多选会返回False。
bool HasSel();

// 清除选区包括多选和列选。
void ClearSel();

// 指定坐标处插入文本。
void InsertAt(int line, int col, string strText);

void Insert(string strText);

void Append(string text);

// 以光标位置为基准，把光标移动指定长度。注意：换行符也会被计算在内。
void MoveCaret(int nLength);

// 插入文本。被插入的文本从第二行开始按照首行的文本进行缩进。
void IndentInsert(string strText);

int GetSyntaxState(int line);

int GetStateStyle(int state);

void OpenOutlineFile1(string strText);

void OpenOutlineFile2(string strText, string strImg);

// 删除指定位置的文本。如果不带任何参数，相当与按一次delete键
void Delete(int sline, int scol, int eline, int ecol);

void Delete(Pos spos, Pos epos);

void Delete();

void SetCaretPos(int line, int col, bool bVisible);

void SetSel(int sline, int scol, int eline, int ecol);

// 设置普通选区。
void SetSel(Pos pos1, Pos pos2);

// 指定范围的文本。添加到选区。如果该范围和当前选区不重合，那么自动变成多选。
int AddSel(Pos pos1, Pos pos2);

Pos Offset2Pos(int nOffset);

int Pos2Offset(Pos pos);

// 偏移和行列位置之间的转换。
int Pos2Offset(int line, int col);

/**
    导出当前buffer到指定路径(如果文档被修改，则连同被修改的内容一起导出)
    strPathName:目的路径
    nEncoding:导出为指定编码的文件
    bBom:是否添加BOM(仅在UTF 16/8下有效)
    nEol:换行符的类型。换行符取值如下：
        WIN=1
        UNIX=2
        MAC=3
**/
bool ExportTo(string strPathName, int nEncoding, bool bAddBom, int nEol);
bool ExportTo(string strPathName, int nEncoding, bool bAddBom);
bool ExportTo(string strPathName);


void Wrap(int type);

void Wrap(int type, int val);
```

```c
// 替换全部。
int ReplaceAll(string strFind, string strReplace, bool bCase, bool bRegex, bool bWord, bool bExtended);
int ReplaceAll(string strFind, string strReplace, bool bCase, bool bRegex, bool bWord);
int ReplaceAll(string strFind, string strReplace, bool bCase, bool bRegex);
int ReplaceAll(string strFind, string strReplace, bool bCase);
int ReplaceAll(string strFind, string strReplace);

// 查找匹配的指定文本字符串，并显示到输出窗口。
int FindAll(string strFind, bool bCase, bool bRegex, bool bWord);
int FindAll(string strFind, bool bCase, bool bRegex);
int FindAll(string strFind, bool bCase);
int FindAll(string strFind);

// 以当前光标位置为基准，向下查找并高亮匹配的指定字符串，未找到则返回False。
bool FindNext(string strFind, bool bCase, bool bRegex, bool bWord);
bool FindNext(string strFind, bool bCase, bool bRegex);
bool FindNext(string strFind, bool bCase);
bool FindNext(string strFind);
```

```c
/**
依据flag获取光标处的文本。flag的取值如下:
#define GETWORD_LWORD           1
#define GETWORD_RWORD           2
#define GETWORD_WORD            GETWORD_LWORD|GETWORD_RWORD
#define GETWORD_LEDGE           4
#define GETWORD_REDGE           8
#define GETWORD_EDGE            GETWORD_LEDGE|GETWORD_REDGE
#define GETWORD_LSYNTAX         16
#define GETWORD_RSYNTAX         32
#define GETWORD_SYNTAX          GETWORD_LSYNTAX|GETWORD_RSYNTAX
#define GETWORD_LSYNTAX2        64
#define GETWORD_RSYNTAX2        128
#define GETWORD_SYNTAX2         GETWORD_LSYNTAX2|GETWORD_RSYNTAX2
**/
string GetWord(int flag);

string GetLineText(int);

int  GetLineLength(int);

// 获取指定行的子行数。

int  GetWrapCount(int);

// 插入snippet到光标处。
int  InsertSnippet(string strSnippet);

// 按照文档的语法模式注释、反注释当前行或者选区文本。
void CommentLine(string strCommentLine, bool bComment);
void CommentBlock(string strCommentOn, string strCommnetOff, bool bComment);

// 模拟js的操作。
void write(string strText);
void writeln(string strText);
void close();

// 幽灵打印，调用该函数，将会自动按照指定的速度在文档内键入文本，speed越小速度越快,最快每30毫秒键入一个字符
void GhostTyping(string text, int speed=100);

/**
    设定换行的方式, type的取值如下：
    0: 取消换行
    1: 窗口边界处换行
    2: 智能换行，智能判断单词边界和禁则字符
    3: 指定列换行(value)
    4: 指定列换行并扩展制表符(value)
    5: 保留
    6: 指定像素处换行(value)
**/
void Wrap(int type, int value=0)
```

### 属性
```c
Pos CaretPos;//get,set
Pos SelStartPos;//get
Pos SelEndPos;//get
long Hwnd;//get
bool Dirty;//get
int CaretLine;//get,set
int CaretCol;//get,set
int LineCount;//get
string SelText;//get,set
string PathName;//get
string Scope;//get
int Encoding;//get,set
int TabStop;//get,set
bool SoftTab;//get,set
string Text;//get,set
string EndOfLine;//get
string Syntax;//get,set
string ImageList;//set
int EditMode;//get
string Syntax;

```

### 光标位置:Pos
```c
int Line;//get,set
int Col;//get,set
```

## 输出窗口:OutputWindow
### 函数
```c
void OutputText(string strText);
void OutputLine(string strText);

/**
设置输出窗口的跳转模式
strText:用正则表达式描述的跳转模式
file/line/col:指定分组描述了文件名、行、列
**/
void SetJumpPattern(string strText, int file, int line, int col);

void ClearJumpPattern();

void Clear();

// 终止输出窗口中正运行的进程
void Terminate();

void Show();
void Hide();
```

## 菜单:Menu
```c
void AddItem(int nCommand, string strText);

void AddSubItem(string strText, Menu pSub);

void AddSeparator();

int Popup();

int Popup(bool bCaretPos);

string GetText(int nCommand);
```

## 持久化存储:PersistentStringTable
```c
// 必须要和EndAdd配对使用
void BeginAdd();
void EndAdd();

void AddItem(string key, string value);

void RemoveAt(int id);

int FindItemId(string key, bool case);

StringTable FindFirstItem(string key, bool case);

StringTable FindItems(string key, int limit, int sort, bool case);

int GetCount();

void Clear();
```

## 字符串表:StringTable
```c
void AddItem(string key, string value);
string GetKeyAt(int idx);
string GetValueAt(int idx);
void RemoveAt(int idx);
string FindFirstItem(string key, bool case);
int GetCount();
void Clear();
```

## HtmlDialog
```c
//设置对话框的标题
void SetTitle(string title);
```

## HexDoc
```c
void InsertText(int nOffset, string lpText);
void InsertText(string lpText);
void InsertBinary(int nOffset, string lpText);
void InsertBinary(string lpText);
void SetCaretPos(int nOffset);
void InsertChar( int ch );
void SendCommand(UINT uCommand);
void Clear();

int CaretPos;
string PathName;
bool Dirty;
int EditMode();
int Size;
```

## Hello World
下面看看在EverEdit中用脚本如何显示一个Hello，World。首先打开EverEdit，建立一纯文本文件，保存以下代码：

```js
var str = "hello, world";
App.OutputText(str);

var doc = App.ActiveDoc;
doc.Insert( str );
```

点击`主菜单→扩展→保存为脚本`，输入任意脚本名称就可以了（支持中文），因为我们是用js写的，所以后缀要输入ejs。然后点击同菜单下的刷新脚本菜单，看看你命名的脚本是不是出现了呢，点击它运行试试吧！