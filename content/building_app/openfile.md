## 打开文件

在这节，我们使我们的应用程序展示命令行传来的文件的正文。

在这后面，我们为我们的应用程序的窗口子类增加了一个私有的结构体，结构体内是一个指向GtkStack的指针。`gtk_widget_class_bind_template_child_private()`函数使得在实例化模板后，私有结构体中的stack成员会指向模板中的同名部件。

```c
...

struct _ExampleAppWindowPrivate
{
  GtkWidget *stack;
};

G_DEFINE_TYPE_WITH_PRIVATE(ExampleAppWindow, example_app_window, GTK_TYPE_APPLICATION_WINDOW);

...

static void
example_app_window_class_init (ExampleAppWindowClass *class)
{
  gtk_widget_class_set_template_from_resource (GTK_WIDGET_CLASS (class),
                                               "/org/gtk/exampleapp/window.ui");
  gtk_widget_class_bind_template_child_private (GTK_WIDGET_CLASS (class), ExampleAppWindow, stack);
}

...
```

([full source](https://git.gnome.org/browse/gtk+/tree/examples/application3/exampleappwin.c))

现在我们重新看一下在每个命令行参数中都会被调用的`example_app_window_open()`函数，然后构建`GtkTextView`，它在后来的stack中作为一页被添加。

```c
...

void
example_app_window_open (ExampleAppWindow *win,
                         GFile            *file)
{
  ExampleAppWindowPrivate *priv;
  gchar *basename;
  GtkWidget *scrolled, *view;
  gchar *contents;
  gsize length;

  priv = example_app_window_get_instance_private (win);
  basename = g_file_get_basename (file);

  scrolled = gtk_scrolled_window_new (NULL, NULL);
  gtk_widget_show (scrolled);
  gtk_widget_set_hexpand (scrolled, TRUE);
  gtk_widget_set_vexpand (scrolled, TRUE);
  view = gtk_text_view_new ();
  gtk_text_view_set_editable (GTK_TEXT_VIEW (view), FALSE);
  gtk_text_view_set_cursor_visible (GTK_TEXT_VIEW (view), FALSE);
  gtk_widget_show (view);
  gtk_container_add (GTK_CONTAINER (scrolled), view);
  gtk_stack_add_titled (GTK_STACK (priv->stack), scrolled, basename, basename);

  if (g_file_load_contents (file, NULL, &contents, &length, NULL, NULL))
    {
      GtkTextBuffer *buffer;

      buffer = gtk_text_view_get_buffer (GTK_TEXT_VIEW (view));
      gtk_text_buffer_set_text (buffer, contents, length);
      g_free (contents);
    }

  g_free (basename);
}

...
```
([full source](https://git.gnome.org/browse/gtk+/tree/examples/application3/exampleappwin.c))

**注意**我们不一定非要接触stack switcher。它从它属于的stack得到了自己所有的信息。在这里，我们传递`gtk_stack_add_titled()`函数的最后一个参数来显示每个文件的标签。

我们的程序打开后就像这样：

![getting-started-app3.png](../../images/getting-started-app3.png)
