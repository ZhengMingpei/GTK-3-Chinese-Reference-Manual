## 常见问题
常见问题——寻找GTK+手册中常见问题的答案

## 问题和答案
这是一个用常见“How do I...“组织的参考手册”索引“。如果你不确定去读哪个手册来解决你的问题，这个列表是个开始的好地方。

##### 常规
###1.1
我如何在GTK+上起步？

GTK+[网站](www.gtk.org)提供了一些[教程](www.gtk.org/documentation.php)和其他文档（大部分是关于GTK+ 2.x的，但大部分依然可用）。更多的从文本资源到在线书籍可以在[GNOME developer's site](https:/developer.gnome.org)中找到。在学习这些材料后，你应该准备回到这个索引来了解细节。
###1.2
我可以在哪里得到GTK+的帮助，提交bug报告或者制作一个特性请求？

参阅[此文档顶部](gtk-resources.md)
###1.3
我如何从一个GTK+版本迁移到另一个？

参阅[Migrating from GTK+2.x to GTK+ 3](https://developer.gnome.org/gtk3/3.14/gtk-migrating-2-to-3.html)。你也许能在这个文档里得到关于某些部件和函数的有用信息。
如果你有一个手册里所没涵盖的问题，请在邮件列表上提问然后对于文档在[提交一个bug报告](https://bugzilla.gnome.org)。
###1.4
GTK+中的内存管理如何工作？程序返回时我是否要释放数据？

查看[GObject](https://developer.gnome.org/gobject/unstable/gobject-The-Base-Object-Type.html#GObject)和[GInitiallyUnowned]((https://developer.gnome.org/gobject/unstable/gobject-The-Base-Object-Type.html#GInitiallyUnowned)文档。对于GObject特别主义g_object_ref()和g_object_unref()。GInitiallyUnowned是GObject的子类，所以同样注意，特别对于有"floating“状态的（相应文档中有解释）。
对于函数返回的strings，如果不应该被释放它们要被声明为"const"。非"const"的strings应该被g_free()释放。Arrays遵从同样的规则。如果你发现一个没被记录的特别是规则，请向[http://bugzilla.gnome.org](http://bugzilla.gnome.org)提交bug。
###1.5
###1.6
###1.7
###1.8
###1.9
###1.10
###1.11
###1.12
###1.13
###1.14
###1.15
##### 我该用哪个部件……
###2.1
###2.2
###2.3
###2.4
##### GtkWidget
###3.1
###3.2
###3.3

##### GtkTextView
###4.1
###4.2
###4.3

##### GtkTreeView
###5.1
###5.2
###5.3
###5.4
###5.5
##### GTK+中使用cairo
###6.1
###6.2
###6.3

