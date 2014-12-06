# GTK+ 概览

GTK+是用来创造图形界面的库，它可以运行在许多类UNIX系统，Windows和OS X。GTK+按照GNU LGPL许可证发布，这个许可证对程序来说相对宽松。GTK+有一个基于C的面向对象的灵活架构，它有对于许多其他语言的版本 ，包括C++，Objective-C， Guile/Scheme, Perl, Python, TOM, Ada95, Free Pascal和Eiffel。
GTK+依赖于以下库：

>* GLib A general-purpose utility library, not specific to graphical user interfaces. GLib provides many useful data types, macros, type conversions, string utilities, file utilities, a main loop abstraction, and so on.
* GObject A library that provides a type system, a collection of fundamental types including an object type, a signal system.
* GIO A modern, easy-to-use VFS API including abstractions for files, drives, volumes, stream IO, as well as network programming and DBus communication.
* cairo Cairo is a 2D graphics library with support for multiple output devices.
* Pango Pango is a library for internationalized text handling. It centers around the PangoLayout object, representing a paragraph of text. Pango provides the engine for GtkTextView, GtkLabel, GtkEntry, and other widgets that display text.
* ATK ATK is the Accessibility Toolkit. It provides a set of generic interfaces allowing accessibility technologies to interact with a graphical user interface. For example, a screen reader uses ATK to discover the text in an interface and read it to blind users. GTK+ widgets have built-in support for accessibility using the ATK framework.
* GdkPixbuf This is a small library which allows you to create GdkPixbuf ("pixel buffer") objects from image data or image files. Use a GdkPixbuf in combination with GtkImage to display images.
* GDK GDK is the abstraction layer that allows GTK+ to support multiple windowing systems. GDK provides window system facilities on X11, Windows, and OS X.
* GTK+ The GTK+ library itself contains widgets, that is, GUI components such as GtkButton or GtkTextView.

