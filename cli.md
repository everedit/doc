EverEdit support several command line parameters, you can work with other programs easily on top of these parameters.


* -n: (-n10:20), Move cursor to specified line and column when opening a file, column is optional.
* -h: Don't add opened files to MRU (most recently used file)
* -e: Force encoding, example: `utf8: -e65001, Chinese: -e936, Japanese: -e932`
* -s: Syntax highlight, example: `-s"HTML"`
* -r: Read-only mode
* -i: Create a new instance of EverEdit
* -c: Clean mode (Don't load plugins and modes)
* -u: Update addons with `update mode`，example: `everedit.exe -u %1`
* -w: working directory, example: `-w"d:/"`

Open a file by relative path:
```bat
everedit ./test.txt
```