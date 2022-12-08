EE4.0中支持弹出浏览器对话框，从而实现一些带有界面的插件制作，用法非常简单。

用法:
```
var dlg=App.ShowHtmlDialog(modal, url, width, height);
dlg.SetTitle(title)
```
参数说明:
```
modal: 是否是模态对话框
url: 要打开的本地或者远程网址
width: 高度
height: 宽度
```

通过这种方式打开的对话框，可以在网页里面直接操作ee脚本对象。
如何使用呢?简单至极! EE的脚本对象放到了window.external中了， 因此，

```
var App = window.external;
```

只要这一句话即可获得所有的脚本对象能力！
