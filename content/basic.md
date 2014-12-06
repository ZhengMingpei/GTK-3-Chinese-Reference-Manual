## 基础

我们用一个最简单的程序来开始对GTK的介绍，下面的程序将创造一个200×200像素的窗体。
![window_default](../images/window_default.png)

新建一个名为 `example-0.c` 的文件，写入如下内容：

```c
#include <gtk/gtk.h>

int main (int   argc, char *argv[])
{
    GtkWidget *window;
    gtk_init (&argc, &argv);
    window = gtk_window_new (GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title (GTK_WINDOW (window), "Window");
    g_signal_connect (window, "destroy", G_CALLBACK (gtk_main_quit), NULL);
    gtk_widget_show (window);
    gtk_main ();
    return 0;
}
```
然后在终端输入以下命令用GCC编译程序：

```bash
gcc `pkg-config --cflags gtk+-3.0` -o example-0 example-0.c `pkg-config --libs gtk+-3.0`
```
>注：要查找更多编译GTK程序的信息，请查看手册中编译GTK+应用的部分。

所有的GTK+程序必须包括`gtk/gtk.h`，这个头文件声明了GTK+程序需要的函数、类和宏。

>注：即使GTK+安装了多种头文件，只有顶层的`gtk/gtk.h` 能被第三方代码直接引入。如果引入任意一个其他的头文件，编译器都会报错。

我们接下来进入main函数，将会声明一个`GtkWidget`类型的指针变量`window`。
下面一行将会调用`gtk_init()`函数，这个函数是GTK+程序的初始化函数，它将设置GTK+、类系统和与窗口环境的连接。

>注:要查找更多GTK+程序的命令参数，请查看手册中运行GTK+程序的部分。

调用`gtk_window_new()`函数将会创造一个新的`GtkWindow`并将其储存在`window`变量中。并且，这个窗体的类型是`GTK_WINDOW_TOPLEVEL`，这也就意味着这个`GtkWindow`将会被当前的系统管理：这个窗体将会根据不同的系统平台产生一个框架、一个标题栏和窗口控件。

当`GtkWindow`被破坏时，我们将`“destroy”`信号连接到`gtk_main_quit()`函数以终止这个程序。这个函数将会在之后终止由`gtk_main()`函数启动的GTK+程序的主循环。`“destroy”`信号会在一个窗口部件被破坏时触发，也会是在调用`gtk_widget_destroy()`或者在这个窗口部件失去母体控件时触发。最顶端的GtkWindows会在关闭按钮被点击时被破坏。

GtkWidgets默认是隐藏的，通过在一个控件上调用`gtk_widget_show()`，我们将能设置其为可见。所有这些工作都将在主循环开始后被完成。

最后一行调用了`gtk_main()`。这个函数就会启动GTK+程序的主循环并且在`gtk_main_quit()`函数被调用之前都阻止`main()`的控制流。

当程序运行时，GTK+一直接收事件。有一些输入事件是由用户与程序互动时产生的，但也有一些事件，比如来自窗口管理器或者其他程序的信息。GTK+处理这些事件和信息，然后触发信号。为这些信号连接handles就是让你的程序为用户输入做出正确响应的方法。

下面这个例子有点复杂，它将展示GTK+的能力。按照程序设计语言和库的古老传统，这个程序也叫`Hello，World`。

![hello_worldpng](../images/hello_world.png)

**Example 1. Hello World in GTK+**

新建一个名为 `example-1.c` 的文件，写入如下内容：

```c
#include <gtk/gtk.h>

/* This is a callback function. The data arguments are ignored
 * in this example. More on callbacks below.
 */
static void print_hello (GtkWidget *widget,
		gpointer   data)
{
    g_print ("Hello World\n");
}

static gboolean on_delete_event (GtkWidget *widget,
		GdkEvent  *event,
		gpointer   data)
{
    /* If you return FALSE in the "delete_event" signal handler,
     * GTK will emit the "destroy" signal. Returning TRUE means
     * you don't want the window to be destroyed.
     *
     * This is useful for popping up 'are you sure you want to quit?'
     * type dialogs.
     */
    g_print ("delete event occurred\n");
    return TRUE;
}

int main (int  argc, char *argv[])
{
    /* GtkWidget is the storage type for widgets */
    GtkWidget *window;
    GtkWidget *button;

    /* This is called in all GTK applications. Arguments are parsed
     * from the command line and are returned to the application.
     */
    gtk_init (&argc, &argv);

    /* create a new window, and set its title */
    window = gtk_window_new (GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title (GTK_WINDOW (window), "Hello");

    /* When the window emits the "delete-event" signal (which is emitted
     * by GTK+ in response to an event coming from the window manager,
     * usually as a result of clicking the "close" window control), we
     * ask it to call the on_delete_event() function as defined above.
     *
     * The data passed to the callback function is NULL and is ignored
     * in the callback function.
     */
    g_signal_connect (window, "delete-event", G_CALLBACK (on_delete_event), NULL);

    /* Here we connect the "destroy" event to the gtk_main_quit() function.
     * This signal is emitted when we call gtk_widget_destroy() on the window,
     * or if we return FALSE in the "delete_event" callback.
     */
    g_signal_connect (window, "destroy", G_CALLBACK (gtk_main_quit), NULL);

    /* Sets the border width of the window. */
    gtk_container_set_border_width (GTK_CONTAINER (window), 10);

    /* Creates a new button with the label "Hello World". */
    button = gtk_button_new_with_label ("Hello World");

    /* When the button receives the "clicked" signal, it will call the
     * function print_hello() passing it NULL as its argument.
     * The print_hello() function is defined above.
     */
    g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);

    /* The g_signal_connect_swapped() function will connect the "clicked" signal
     * of the button to the gtk_widget_destroy() function; instead of calling it
     * using the button as its argument, it will swap it with the user data
     * argument. This will cause the window to be destroyed by calling
     * gtk_widget_destroy() on the window.
     */
    g_signal_connect_swapped (button, "clicked", G_CALLBACK (gtk_widget_destroy), window);

    /* This packs the button into the window. A GtkWindow inherits from GtkBin,
     * which is a special container that can only have one child
     */
    gtk_container_add (GTK_CONTAINER (window), button);

    /* The final step is to display this newly created widget... */
    gtk_widget_show (button);

    /* ... and the window */
    gtk_widget_show (window);

    /* All GTK applications must have a gtk_main(). Control ends here
     * and waits for an event to occur (like a key press or a mouse event),
     * until gtk_main_quit() is called.
     */
    gtk_main ();
    return 0;
}

```
然后在终端输入以下命令用GCC编译程序：

```bash
gcc `pkg-config --cflags gtk+-3.0` -o example-1 example-1.c `pkg-config --libs gtk+-3.0`
```




