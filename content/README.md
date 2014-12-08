# GTK+ 概览

GTK+是用来创造图形界面的库，它可以运行在许多类UNIX系统，Windows和OS X。GTK+按照GNU LGPL许可证发布，这个许可证对程序来说相对宽松。GTK+有一个基于C的面向对象的灵活架构，它有对于许多其他语言的版本 ，包括C++，Objective-C， Guile/Scheme, Perl, Python, TOM, Ada95, Free Pascal和Eiffel。
GTK+依赖于以下库：

>* GLib 是一个多方面用途的库,  不仅仅针对图形界面。GLib提供了有用的数据类型、宏、类型转换，字符串工具，文件工具，主循环抽象等等。
* GObject 是一个提供了类型系统、包括一个元类型的基础类型集合、信号系统的库。
* GIO 是一个包括文件、设备、声音、输入输出流、网络变成和DBus通信的现代的易于使用的VFS 应用程序变成借口。
* cairo Cairo 是一个支持复杂设备输出的2D图形库。
* Pango Pango 是一个国际化正文布局库。它围绕一个表现正文段落的PangoLayout object。Pango提供GtkTextView、GtkLabel、GtkEntry和其他表现正文的引擎。
* ATK ATK 是一个友好的工具箱。它提供了一个允许技术和图形用户界面交互的界面的集合。例如，一个屏幕阅读程序用ATK去发现界面上的文字并为盲人用户阅读。GTK＋部件已经被制作方便支持ATK框架。
* GdkPixbuf 是一个允许你从图像数据或图像文件创建GdkPixbuf("pixel buffer")的小的库。用GdkPixbuf与显示图像的GtkImage的结合。
* GDK GDK 是一个允许GTK＋支持复杂图形系统的抽象层。GDK支持X11、Windows和OS X的图形系统工具。
* GTK+ 是GTK+库本身包含的部件，确切的说是GUI零件，比如GtkButton或者GtkTextView。

