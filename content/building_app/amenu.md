## 一个应用菜单

就像窗口模板，在一个ui file 中我们指定了我们的应用程序菜单，然后作为资源向二进制文件中添加。

```xml
<?xml version="1.0"?>
<interface>
  <!-- interface-requires gtk+ 3.0 -->
  <menu id="appmenu">
    <section>
      <item>
        <attribute name="label" translatable="yes">_Preferences</attribute>
        <attribute name="action">app.preferences</attribute>
      </item>
    </section>
    <section>
      <item>
        <attribute name="label" translatable="yes">_Quit</attribute>
        <attribute name="action">app.quit</attribute>
      </item>
    </section>
  </menu>
</interface>
```

为了关联应用程序和应用菜单，我们必须调用`gtk_application_set_app_menu()`。y因为应用菜单被活动的GActions激活，所以必须为应用程序增加一个合适的设定。

所有这些任务最好在startup()函数中做完，因为startup()函数被保证在每个应用程序实例中只被调用一次。

```c
...

static void
preferences_activated (GSimpleAction *action,
                       GVariant      *parameter,
                       gpointer       app)
{
}

static void
quit_activated (GSimpleAction *action,
                GVariant      *parameter,
                gpointer       app)
{
  g_application_quit (G_APPLICATION (app));
}

static GActionEntry app_entries[] =
{
  { "preferences", preferences_activated, NULL, NULL, NULL },
  { "quit", quit_activated, NULL, NULL, NULL }
};

static void
example_app_startup (GApplication *app)
{
  GtkBuilder *builder;
  GMenuModel *app_menu;
  const gchar *quit_accels[2] = { "<Ctrl>Q", NULL };

  G_APPLICATION_CLASS (example_app_parent_class)->startup (app);

  g_action_map_add_action_entries (G_ACTION_MAP (app),
                                   app_entries, G_N_ELEMENTS (app_entries),
                                   app);
  gtk_application_set_accels_for_action (GTK_APPLICATION (app),
                                         "app.quit",
                                         quit_accels);

  builder = gtk_builder_new_from_resource ("/org/gtk/exampleapp/app-menu.ui");
  app_menu = G_MENU_MODEL (gtk_builder_get_object (builder, "appmenu"));
  gtk_application_set_app_menu (GTK_APPLICATION (app), app_menu);
  g_object_unref (builder);
}

static void
example_app_class_init (ExampleAppClass *class)
{
  G_APPLICATION_CLASS (class)->startup = example_app_startup;
  ...
}

...
```
([full source](https://git.gnome.org/browse/gtk+/tree/examples/application4/exampleapp.c))

菜单首选项如今并不能作任何事，但是Quit菜单选项的功能是正常的。**注意**它也可以被快捷键`Ctrl-Q`激活。这个快捷方式已经在`gtk_application_set_accels_for_action()`中被添加。

我们的应用菜单如下：

![getting-started-app4.png](../../images/getting-started-app4.png)
