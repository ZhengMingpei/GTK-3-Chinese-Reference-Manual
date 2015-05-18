

## 构建应用程序


一个普通的应用程序由以下文件组成：

* 二进制文件
>这个安装在 `/usr/bin`。
* 一个桌面文件
>这个桌面文件向shell提供关于这个程序的重要信息,例如名称、图标、D-Bus名称，启动的命令行。安装在 `/usr/share/applications`.
* 一个图标
>这个图标安装在 `/usr/share/icons/hicolor/48x48/apps`, 无论当前背景是什么系统都会到这里查找图标。
* 一个设置框架
>如果应用使用了GSettings, 它会将它的schema安装在 ` /usr/share/glib-2.0/schemas`, 这样dconf-editor之类的工具就能够找到它。
* 其他资源
>其他文件,例如GtkBuilder ui文件, 最好从应用二进制文件自身储存的资源中加载。如果有需要，许多文件会按照惯例放置在`/usr/share`。

GTK+ includes application support that is built on top of GApplication. 在这篇教程中，我们从头开始构建一个简单的应用，然后逐渐一点一点增加功能。在这个过程中, 我们将会了解到 GtkApplication, templates, resources, application menus, settings, GtkHeaderBar, GtkStack, GtkSearchBar, GtkListBox和更多东西。

完整的源文件可以在 GTK+ source distribution的范例根目录下找到，或者可以在GTK+的git仓库在线查看。

1. ###[一个小应用](building_app/asmallapp.md)
* ###[填充窗口](building_app/paddingwindow.md)
* ###[打开文件](building_app/openfile.md)
* ###[一个应用菜单](building_app/amenu.md)
* ###[一个偏好对话框](building_app/dialog.md)
* ###[增加搜索条](building_app/searchbar.md)
* ###[增加侧边栏](building_app/sidebar.md)
* ###[属性](building_app/properties.md)
* ###[标题栏](building_app/headerbar.md)



