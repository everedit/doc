# Colors
When you are writing a syntax highlight, you are supposed to distinguish colors of data types. EverEdit provides a couple of built-in colors.

```
COLOR_DEFAULT: Normal text without any syntax state
COLOR_COMMENT1: Single line comment, example:// in C/C++
COLOR_COMMENT2: Block line comment, example:/your comment/ in C/C++
COLOR_STRING1: Single quoted string, example:'your single quoted string' in PHP
COLOR_STRING2: Double quoted string, example:"your double quoted string" in PHP
COLOR_TAG: Use for HTML/XML tag match, EverEdit could find the matched tag automatically
COLOR_MACRO: Macro define, example:#define/Import/Range VALUE 100 in C/C++/C#/Java
COLOR_URL: URL, example:http://www.everedit.net
COLOR_EMAIL: Email address, example:support@everedit.net
COLOR_NUMBER: Numbers, example:10,0x20,1.35
COLOR_FOUND: When you search something, EverEdit will use this state to draw the found text
COLOR_PAIR: Paired strings, example:()[]{}<>""''
COLOR_FUNCTION: Function name
COLOR_VAR: Variables
COLOR_SUBLAN: Sub-language, example: in HTML
COLOR_OPERATOR: Operators, example:+-*/ ++ --
COLOR_WORD1: Key word 1, built-in keywords. example:int bool double do while in C/C++/Java
COLOR_WORD2: Key word 2, framework keywords. example:CString CMap CArray in MFC
COLOR_WORD3: Key word 3
COLOR_WORD4: Key word 4
COLOR_HIGHLIGHT1~COLOR_HIGHLIGHT8: User defined highlight, used for custom marker
COLOR_IGNORE: The foreground color of text will be same as the background. It will show on selecting
COLOR_CONCEAL: The text will not be displayed expect selecting
```

# Objects

## SyntaxItem
```c
// Functions
void Capture(int group, int state);

// Properties
string Name;//get,set
```

## WordItem
```c
// Properties
bool AutoCase;//get,set
string Name;//get,set
```

## SyntaxRegion
```c
// Functions
bool AddSnippet(string strPathName, bool bCase)
void AddItem(SyntaxItem item );
void AddWord(WordItem item );
void AddRegion(SyntaxRegion child );
void FoldText(string strFold, bool bFCase, string strUnFold, bool bUFCase);
void CommentBlock(string strOn, string strOff);
void CommentLine(string strText);
void SetPairs(string strText);
void IndentText(string strIndnet, bool c1, string strUnIndnet, bool c2);
void AddCallTip( string strPathName, bool bCase, string strWord="", string strBegin="(", string strSep=",", string strEnd=")", bool bRemoveSpace=false );
void AddSnippet( string title, string trigger, string text, bool script=false);

// Properties
void TransRegion;
string Name;//get;set
```

## Parser

```c
// Functions
bool AddSnippet(string strPathName );
void AddItem(SyntaxItem item );
void AddWord(WordItem item );
void AddRegion(SyntaxRegion child);
SyntaxRegion CopyRegion(SyntaxRegion pCopy);
SyntaxItem CreateItem(int state, string strMatch, bool bCase, bool bToRight=false);
WordItem CreateWord(int state, string strMatch, bool bCase, string strDelimiters="");
SyntaxRegion CreateRegion(int state, string strBegin, string strEnd, bool bCase, bool bToRight=false );
SyntaxRegion CreateStringRegion(int state, string strChar, string strEscape, bool mline, string strContinueChar );
void FoldText(string strFold, bool bFCase, string strUnFold, bool bUFCase);
void IndentText(string strIndnet, bool c1, string strUnIndnet, bool c2);
void FoldAnyText(int nLevel, string strText);
void SetPairs(string strText);
void CommentBlock(string strOn, string strOff);
void CommentLine(string strText);
void AddCallTip( string strPathName, bool bCase, string strWord="", string strBegin="(", string strSep=",", string strEnd=")", bool bRemoveSpace=false );
void SetFont( string font_name, int font_size, int base_line, bool bold);
void SetCJKFont( string font_name, int font_size, int base_line, bool bold);
void AddSnippet( string title, string trigger, string text, bool script=false);

// Properties
string Name;//get,set
string FoldingMethod;//get,set
string WordChars;//get,set
SyntaxRegion DefaultRegion;//get
```

# Sample 1 (create a syntax highlight for C/C++)
This sample will guide you to define a new syntax highlight for EverEdit! let's have a look at some key syntax elements of C/C++:

* Single line comment
* Multiple line comment
* String
* Key word

The above elements are key parts for most of syntax highlights.

## 1. Create a file
Create a mycpp.mac and put it into (syntax) folder;

## Include color define file
EverEdit provides predefined colors, so let's include it first.
```c
Include( ".\const.mac" )
```

## 3. Create a parser object
```
Set cpp=Parser.CreateParser()
```

## 4. Highlight single line comment
Single line comment is be a region which starts from `//` and ends with End of Line(`EOL`).

```c
Set regionLine=cpp.CreateRegion( COLOR_COMMENT1, "+//+", "$", True )
```

**Note**: The text surrounded by plus(+) means this is a normal string match (simple&fast), not regular expression.(This kind of usage is available in `CreateRegion` function only).

## 5. Highlight multiple line comment 
```c
regionBlock=cpp.CreateRegion( COLOR_COMMENT1, "+/*+", "+*/+", True )
```

## 6. Highlight string
```c
Set regionString=cpp.CreateStringRegion( COLOR_STRING1, """", "\", False )
```
The last param of CreateStringRegion means whether the match will be continue to next line!

## 7. Highlight key words
```c
Set itemWord2=cpp.CreateWord(COLOR_WORD1, "int float double char void for while if else return break continue", True)
```

There are 4 params in CreateWord function. The last one is a string which contains the delimiters that can be treated as word, example:
```c
cpp.CreateWord(COLOR_WORD1, "bottom-top bottom-right", True, "-")
```

## 8. Add matches into Parser
```c
cpp.AddRegion( regionLine )
cpp.AddRegion( regionBlock )
cpp.AddRegion( regionString )
cpp.AddItem( itemWord )
```

## 9. Overview
```c
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

# Sample 2 (Enhance C++ syntax highlight)
## 1. Add todo match in comment region
Create a todo match and add it into single/multiple line comment:
```c
Set itemTodo=cpp.CreateItem(COLOR_HIGHLIGHT1, "\bTODO\b", True)

regionLine.AddItem( itemTodo )
regionBlock.AddItem( itemTodo )
```

## 2. Code folding
C++ source files are normally folded begin with `{` and `}`
```c
cpp.FoldText "\{", False, "\}", False
```

## 3. Auto Indent
```c
cpp.IndentText "\{$", False, "^\s*}$", False
```

## 4. Add auto pair complete
```
cpp.SetPairs "[]{}()""""''"
```

## 5. Add quick line comment
```
cpp.CommentLine "//"
cpp.CommentBlock "/*", "*/"
```

Now you can use `Ctrl+/` to comment your selection or active line quickly.