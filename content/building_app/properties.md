## 属性

部件和其他的对象有许多有用的属性。

这里我们展示一些灵活的新方法来使用它们，可以通过`GPropertyAction`包装在action中，也可以用`GBinding`来绑定它们。

着手干吧，我们在窗口模板头栏增加两个lable，分别为lines_label和lines，然后在一个私有结构体中将它们和结构体成员绑定，就像我们前2次做的一样。

我们在工具菜单上增加一个新的Lines菜单项，它负责触发show-lines 动作。

```xml
<?xml version="1.0"?>
<interface>
  <!-- interface-requires gtk+ 3.0 -->
  <menu id="menu">
    <section>
      <item>
        <attribute name="label" translatable="yes">_Words</attribute>
        <attribute name="action">win.show-words</attribute>
      </item>
      <item>
        <attribute name="label" translatable="yes">_Lines</attribute>
        <attribute name="action">win.show-lines</attribute>
      </item>
    </section>
  </menu>
</interface>
```
为了使这个菜单项起作用，我们为lines label的可见属性添加了一个属性动作，然后将它添加进了窗口动作。效果就是，每次lable一可见，该动作就被触发。

因为我们希望所有的label都能一起显示和消失，我们将lines-label部件的可见属性和lines部件相同属性绑定。

```c
...

static void
example_app_window_init (ExampleAppWindow *win)
{
  ...

  action = (GAction*) g_property_action_new ("show-lines", priv->lines, "visible");
  g_action_map_add_action (G_ACTION_MAP (win), action);
  g_object_unref (action);

  g_object_bind_property (priv->lines, "visible",
                          priv->lines_label, "visible",
                          G_BINDING_DEFAULT);
}

...
```
([full source](https://git.gnome.org/browse/gtk+/tree/examples/application9/exampleappwin.c))

我们需要一个计算当前活动标签行数的函数，然后更新lines label。如果你对细节感兴趣，请看[全部源代码](https://git.gnome.org/browse/gtk+/tree/examples/application9/exampleappwin.c)。

这使我们的范例程序如下所示：

![getting-started-app9.png](../../images/getting-started-app9.png)
