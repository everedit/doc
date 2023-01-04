# Script Engine
VBScript/JScript are the built-in script engines of EverEdit. You could use VBS/JScript to manage your macros and define syntax highlights. Recommend JScript to write scripts for EverEdit. JScript is very simple and powerful, you could leverage various COM components in JScript/VBScript.

# Save Script
In order to manage scripts, all the files should be placed into `macro` folder. EverEdit will automatically build a menu for these scripts. After adding/deleting scripts, you'd better refresh the menu by clicking [Reload Macros...] from Addons menu.

VBScript files should be saved with `.mac` or `.evb` and JScript should be `.ejs`.

# How to bind a shortcut
Open shortcut manager and drag-drop a script file into the dialog or click `Bind` button to add a script file into Shortcut View. Then, just set the shortcut like normal commands.

# Built-in Objects and Functions

## Application
An application object means the EverEdit instance you are using.

```c
// functions
Menu CreateMenu();
void Sleep(DWORD dwMillisec);
void SendCommand(int nCmd);
void SendCommandEx(string strCommand);
void WebPreview(string strPathName);
Document NewDoc();
Document OpenDoc(string strPathName);
void OutputText(string strText, bool bClear=false, bool bTerminate=false);
string ShowInputBox(string strPrompt, string strTitle);
int ShowMsgBox(string strText, string strTitle, int buttons);
int ShowHtmlHelp(string strPathName, string strWord);
string CreateTempFile(bool bAutoDelete);
void DebugLibrary(string strPathName);
void OpenSnippetByTitle(string title);
string GetResultFromExe(string cmdline, string initdir="", int encoding=0)

// properties
Document ActiveDoc; //get
HexDoc ActiveHex; //get
Document document; //get
OutputWindow OutputWindow; //get
Project Project; //get
string AppPath; //get
string CommandBox; //get
ULONG Hwnd; //get
ULONG FileCount; //get
string Version; //get
string ClipboardText; //get,set
int Lang;//get
```

```c
// Create a menu that will show at your gloabl cursor position.
Menu CreateMenu();

// Send a command to main window
// nCmd: the ID of a menu item.
void SendCommand(int nCmd);

// Send a string command(cm_edit_find...), get more details by shortcut dialog.
void SendCommandEx(string strText);

// Open a file in a new tab by web view, and the current text document will be linked to this tab. 
// You could press Ctrl+B to switch between them.
void WebPreview(string strPathName);

// Output texts into OutputWindow.
// bClear: Clear the exist content?
// bTerminate: Terminate running process?
void OutputText(string strText, bool bClear=false, bool bTerminate=false);

// Popup an input box.
// strPrompt: promopt text
// strTitle: title of the input box.
string ShowInputBox(string strPrompt, string strTitle);

/**
Popup a message box.
strText: message text
strTitle: title of the message box
buttons: buttons and icons.

Ref the below values to set buttons:
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

// Open a .chm or .hlp file, find and locate strWord.
// strWord: If strWord is empty, start page will be displayed.
int ShowHtmlHelp(string strPathName, string strWord);

// Create a temporary file.
// bAutoDelete: delete this file after closing EverEdit?
string CreateTempFile(bool bAutoDelete);

// This function is used for debugging a plugin developed by C/C++.
// You could attach process of EverEdit first before invoking this function.,
void DebugLibrary(string strPathName);

// Show save/open dialog.
// bOpen:true:open dialog, false:save dialog
// strDefaultDir:default folder path
// strExts:default exts, format:*.png;*jpg;*.bmp
string ShowFileDialog(bool bOpen, string strDefaultDir, string strExts);
```

## Document
An document metans the active text tab.

```c
// functions
Menu CreateMenu();
void SendCommand(int nCmd);
void Refresh();
bool HasSel();
void ClearSel();
void InsertAt(int line, int col, string strText);
void Insert(string strText);
void MoveCaret(int nLength);
void IndentInsert(string strText);
void Delete(int sline, int scol, int eline, int ecol);
void Delete(Pos spos, Pos epos);
void Delete();
void SetCaretPos(int line, int col, bool bVisible);
void SetSel(int sline, int scol, int eline, int ecol);
void SetSel(Pos pos1, Pos pos2);
int AddSel(Pos pos1, Pos pos2);
Pos Offset2Pos(int nOffset);
int Pos2Offset(Pos pos);
int ReplaceAll(string strFind, string strReplace, bool bCase=true, bool bRegex=false, bool bWord=false);
int FindAll(string strFind, bool bCase=true, bool bRegex=false, bool bWord=false);
bool FindNext(string strFind, bool bCase=true, bool bRegex=false, bool bWord=false);
string GetWord(int flag);
string GetLineText(int nLine);
int  GetLineLength(int nLine);
int  GetWrapCount(int nLine);
int  InsertSnippet(string strSnippet);
void CommentLine(string strCommentLine, bool bComment);
void CommentBlock(string strCommentOn, string strCommnetOff, bool bComment);
void write(string strText);
void writeln(string strText);
void close();
bool ExportTo(string strPathName, int nEncoding=/*same as document*/, bool bBom=/*same as document*/, int nEol=/*same as document*/)
void GhostTyping(string text, int speed=100);

/**
type：
    0: cancel wrap
    1: wrap at window's edge
    2: smart wrap
    3: wrap by column
    4: wrap by column (extend tabs)
    5: N/A (reserved)
    6: wrap by pixel
**/
void Wrap(int type, int value=0);
```

```c
// properties
Pos SelStartPos;
Pos SelEndPos;
bool Dirty;
int CaretLine;
int CaretCol;
int LineCount;
string PathName;
string Scope;
string EndOfLine;
void GroupUndo;//set
Pos CaretPos;//get,set
string SelText;//get,set
int Encoding;//get,set
int TabStop;//get,set
bool SoftTab;//get,set
string Text;//get,set
string Syntax;//get,set
int Hwnd;//get
```

```c
// Create a menu and this menu will follow the caret of the active document.
Menu CreateMenu();void SetSel(int sline, int scol, int eline, int ecol);
void SetSel(Pos pos1, Pos pos2);

Set normal selection.
int AddSel(Pos pos1, Pos pos2);

Add region into selection.
Pos Offset2Pos(int nOffset);
int Pos2Offset(Pos pos);

// Force to redraw current document.
void Refresh();

// Does current document contain a normal selection?
// Normal selection: True
// Multiple Selection/Column Selection: False
bool HasSel();

// Clear all selection(Normal/Multiple/Column)
void ClearSel();

// Insert text by line and column.
void InsertAt(int line, int col, string strText);

// Insert text at current caret position
void Insert(string strText);

// Move caret to next position with specified nLength.
// Note: Line breaks are also included.
void MoveCaret(int nLength);

// Insert text at caret position and the text will be used same indent as first line.
void IndentInsert(string strText);

// Delete text. Delete() is equal to Press Delete Key once.
void Delete(int sline, int scol, int eline, int ecol);
void Delete(Pos spos, Pos epos);
void Delete();

// Set syntax highlight for current document.
// strText: syntax title, ref Syntax Dialog to get more details about syntax titles.
void SetSyntax(string strText);

// normal selection.
void SetSel(int sline, int scol, int eline, int ecol);
void SetSel(Pos pos1, Pos pos2);

// Add a region into selection.
int AddSel(Pos pos1, Pos pos2);

// Convert between offset and position.
Pos Offset2Pos(int nOffset);
int Pos2Offset(Pos pos);


/**
    strFind: find what
    strReplace: replace to
    bCase: case sensitive?
    bRegex: use regular expression?
    bWord: word only?
**/
int ReplaceAll(string strFind, string strReplace, bool bCase=true, bool bRegex=false, bool bWord=false);

// Find and output all match places into OutputWindow.
// Note: This function doesn't support multiple lines search.
int FindAll(string strFind, bool bCase=true, bool bRegex=false, bool bWord=false);

/**
Find from caret position and select it if found. return True If result exists otherwise return False.
bCase: case sensitive?
bRegex: use regular expression?
bWord: word only?
    strFind: find what
    bCase: case sensitive?
    bRegex: use regular expression?
    bWord: word only?
**/
bool FindNext(string strFind, bool bCase=true, bool bRegex=false, bool bWord=false);

/**
Get the word from caret position by flag.

Flag values as below:
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

// Get wrapped line's (sub line) count.
int  GetWrapCount(int nLine);

// Insert a snippet at caret position.
int  InsertSnippet(string strSnippet);

// Line Comment or Block Comment.
// bComment: comment or uncomment?
void CommentLine(string strCommentLine, bool bComment);
void CommentBlock(string strCommentOn, string strCommnetOff, bool bComment);

// simulate some javascript functions.
void write(string strText);
void writeln(string strText);
void close();

/**
Export current document into a file.
    strPathName: dest folder
    nEncoding: encoding, utf-8 is 65001 native encoding is 0.
    bBom: should add bom?
    nEol: formant of line break(WIN=1,UNIX=2,MAC=3)
**/
bool ExportTo(string strPathName, int nEncoding=/*same as document*/, bool bBom=/*same as document*/, int nEol=/*same as document*/)

// Get the unique windows handle of document.
doc.Hwnd
```

## Menu
```c
void AddSeparator();

// Add item into menu, nCommand must >0
void AddItem(int nCommand, string strText);

// Popup this menu
int Popup();

// Get menu text by nComand
string GetText(int nCommand)
```

## Pos

```c
int Line;//get,set
int Col;//get,set
```

## OutputWindow

```c
void OutputText(string strText);
void OutputLine(string strText);

/**
Set the jump pattern of output window with a regular expression.
Jump pattern: OutputWindow will navigate to proper files by clicking output window 
Example:
    SetJumpPattern "(.*?):(\d+):(\d+):", 0, 1, 2
**/
void SetJumpPattern(string strText, int file, int line, int col);

// Clear jump patterns.
void ClearJumpPattern();

// Clear the content of OutputWindow
void Clear();

// Terminate running process.
void Terminate();

// Show or hide OutputWindow
void Show();
void Hide();
```

## HexDoc

```c
// Send command.
void SendCommand(int uCommand);
```

```c
// You can't use the below functions right now (Not ready, in developing)!
void InsertText(int nOffset, string strText);
void InsertText(string strText);
void InsertBinary(int nOffset, string strText);
void InsertBinary(string strText);
void SetCaretPos(int nOffset);
int GetSize();
void InsertChar( int ch );

// properties
int CaretPos;//get,set
```