## 填充


当创建一个应用时，你将会想将多个控件放入一个窗口控件。我们的第一个 helloworld 范例仅仅使用了一个控件，因而我们可以只是简单地调用一个`gtk_container_add()`将控件填充到一个窗口控件。但是当你想要向窗口控件中放置超过一个控件时，控制每一个控件的位置和大小就变得很重要了。这就是接下来要讲的填充。

GTK+自带了大量各种布局的容器，这些容器的目的是控制被添加到他们的子控件的布局。具体可以参考布局容器的概述。
下面的示例显示了GtkGrid容器如何让你如何安排几个按钮：

![grid_packingpng](../images/grid_packing.png)

**Example 2. Packing buttons**

新建一个名为 `example-2.c` 的文件，写入如下内容:

```c
#include <gtk/gtk.h>

static void
print_hello (GtkWidget *widget,
             gpointer   data)
{
    g_print ("Hello World\n");

}

int main (int   argc, char *argv[])
{
    GtkWidget *window;
    GtkWidget *grid;
    GtkWidget *button;

    /* This is called in all GTK applications. Arguments are parsed
    * from the command line and are returned to the application.
    */
    gtk_init (&argc, &argv);

    /* create a new window, and set its title */
    window = gtk_window_new (GTK_WINDOW_TOPLEVEL);
    gtk_window_set_title (GTK_WINDOW (window), "Grid");
    g_signal_connect (window, "destroy", G_CALLBACK (gtk_main_quit), NULL);
    gtk_container_set_border_width (GTK_CONTAINER (window), 10);

    /* Here we construct the container that is going pack our buttons */
    grid = gtk_grid_new ();

    /* Pack the container in the window */
    gtk_container_add (GTK_CONTAINER (window), grid);

    button = gtk_button_new_with_label ("Button 1");
    g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);

    /* Place the first button in the grid cell (0, 0), and make it fill
    * just 1 cell horizontally and vertically (ie no spanning)
    */
    gtk_grid_attach (GTK_GRID (grid), button, 0, 0, 1, 1);

    button = gtk_button_new_with_label ("Button 2");
    g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);

    /* Place the second button in the grid cell (1, 0), and make it fill
    * just 1 cell horizontally and vertically (ie no spanning)
    */
    gtk_grid_attach (GTK_GRID (grid), button, 1, 0, 1, 1);

    button = gtk_button_new_with_label ("Quit");
    g_signal_connect (button, "clicked", G_CALLBACK (gtk_main_quit), NULL);

    /* Place the Quit button in the grid cell (0, 1), and make it
    * span 2 columns.
    */
    gtk_grid_attach (GTK_GRID (grid), button, 0, 1, 2, 1);

    /* Now that we are done packing our widgets, we show them all
    * in one go, by calling gtk_widget_show_all() on the window.
    * This call recursively calls gtk_widget_show() on all widgets
    * that are contained in the window, directly or indirectly.
    */
    gtk_widget_show_all (window);

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
gcc `pkg-config --cflags gtk+-3.0` -o example-2 example-2.c `pkg-config --libs gtk+-3.0`
```

