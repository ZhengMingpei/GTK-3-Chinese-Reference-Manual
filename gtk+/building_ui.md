## 构建用户界面


当我们构建一个更加复杂的带有成百控件的用户界面时，用C程序做这些控件的所有设置工作是非常麻烦的，而且也让做些调整变得几乎不可能。
谢天谢地， GTK+ 支持将用户界面布局从业务逻辑中分离。这是一种通过XML格式实现的UI描述，它可以通过`Gtkuilder` 类进行解析。

**Example 4. Packing buttons with GtkBuilder**

新建一个名为 `example-4.c` 的文件，写入如下内容：

```c
#include <gtk/gtk.h>

static void print_hello (GtkWidget *widget,
             gpointer   data)
{
    g_print ("Hello World\n");

}

int main (int   argc, char *argv[])
{
    GtkBuilder *builder;
    GObject *window;
    GObject *button;

    gtk_init (&argc, &argv);

    /* Construct a GtkBuilder instance and load our UI description */
    builder = gtk_builder_new ();
    gtk_builder_add_from_file (builder, "builder.ui", NULL);

    /* Connect signal handlers to the constructed widgets. */
    window = gtk_builder_get_object (builder, "window");
    g_signal_connect (window, "destroy", G_CALLBACK (gtk_main_quit), NULL);

    button = gtk_builder_get_object (builder, "button1");
    g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);

    button = gtk_builder_get_object (builder, "button2");
    g_signal_connect (button, "clicked", G_CALLBACK (print_hello), NULL);

    button = gtk_builder_get_object (builder, "quit");
    g_signal_connect (button, "clicked", G_CALLBACK (gtk_main_quit), NULL);

    gtk_main ();

    return 0;
}

```

新建一个名为`builder.ui`的文件，写入如下内容：

```XML
<interface>
    <object id="window" class="GtkWindow">
        <property name="visible">True</property>
        <property name="title">Grid</property>
        <property name="border-width">10</property>
        <child>
            <object id="grid" class="GtkGrid">
                <property name="visible">True</property>
                <child>
                    <object id="button1" class="GtkButton">
                        <property name="visible">True</property>
                        <property name="label">Button 1</property>
                    </object>
                    <packing>
                        <property name="left-attach">0</property>
                        <property name="top-attach">0</property>
                    </packing>
                </child>
                <child>
                    <object id="button2" class="GtkButton">
                        <property name="visible">True</property>
                        <property name="label">Button 2</property>
                    </object>
                    <packing>
                        <property name="left-attach">1</property>
                        <property name="top-attach">0</property>
                    </packing>
                </child>
                <child>
                    <object id="quit" class="GtkButton">
                        <property name="visible">True</property>
                        <property name="label">Quit</property>
                    </object>
                    <packing>
                        <property name="left-attach">0</property>
                        <property name="top-attach">1</property>
                        <property name="width">2</property>
                    </packing>
                </child>
            </object>
            <packing>
            </packing>
        </child>
    </object>
</interface>
```
然后在终端输入以下命令用GCC编译程序：

```bash
gcc `pkg-config --cflags gtk+-3.0` -o example-4 example-4.c `pkg-config --libs gtk+-3.0`
```

注意`GtkBuilder`也可以用来构建非控件的对象，例如树结构，调节器。这也是我们这里使用的方法叫做`gtk_builder_get_object()`并且返回值为`GObject*`而不是`GtkWidget*`的原因。
一般情况下，你将把一个完整路径传递给`gtk_builder_add_from_file()`使你的程序不依赖于当前路径运行。一个常用的放置UI描述和类似数据的目录是`/usr/share/appname`。

也可以将UI描述以字符串的形式嵌入到源代码中，然后使用`gtk_builder_add_from_string()`加载。但是将UI描述放置在一个单独的文件有几个好处：首先，这让我们在对UI进行调整时不需要重新编译程序，而且，更重要的是，一些UI编辑器比如`glade`可以加载这种文件并且允许你通过点击就能够创建和修改你的UI。
