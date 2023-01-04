# Format
You can customize your own snippets for EverEdit. All the snippets are saved as pure text format(ini), you can modify it directly with any text editor or Snippet Manager of EverEdit (Recommend).

**sample**:
```c
for (${1:unsigned int} ${2:i} = ${3:0}; $2 ${4:&lt;} ${5:count}; $2${6:++}) {
    $0
}
```

# Variables
It’s easy to define a variable (placeholder) in snippet, for example: `${1:myvar}`. `1` means it’s the first var, the value should be `1~9`, `0` is the last edit point. You’d better define vars with `asc order`.

# Use variables
We can make a reference for a defined variable. fox example `$1`, it means we ref the first var that has been defined above. Double `$$` can escape single `$` and it will just insert a pure `$` into document.

# Jump
Press `Tab` or `Shift+Tab` to jump between placeholders.

# Last edit point
`$0` is the last edit point. Cursor will not move anymore after reaching `$0`.

# Snippet manager
Click `Main menu->View->Snippet` to open the snippet manager. right click the snippet item to edit a selected item. It will display as:

```c
@Title:For loop
@Trigger:for
@Snippet:Input content from next line!
for (${1:unsigned int} ${2:i} = ${3:0}; $2 ${4:&lt;} ${5:count}; $2${6:++}) {
  $0
}
```
# Enable your snippet
A snippet file will not be available until you bind it in `Syntax Files`. You can bind it:
```
Cpp.AddSnippet “my.snippet”
```

# Predefined Snippet Vars
* `${SELECTED}`: selected text
* `${CLIPBOARD}`: clipboard text

**Note**: EverEdit doesn’t support nested vars, example`${1:data ${2:text}}`.
