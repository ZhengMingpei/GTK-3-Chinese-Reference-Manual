
>之前看到在[ Transifex ](https://www.transifex.com/)平台上有一个[ GTK+3 参考手册的翻译项目](https://www.transifex.com/organization/yetist/dashboard/gtk3-reference-manual)，但更新乏力，版本滞后，就产生了翻译新版参考手册的冲动。

>如果你对这个项目感兴趣，可以帮助完成这个项目,参与方式及详情见[项目主页](https://github.com/ZhengMingpei/GTK-3-Chinese-Reference-Manual)，目前的参与者已经有[ZhengMingpei](https://github.com/ZhengMingpei) 和[truehyp](https://github.com/truehyp)两人，本文最后更新于 2015/02/18。

# GTK+ 3 Chinese Reference Manual

该参考手册适用于GTK+3的3.14.5版本，最新版本的参考手册可以去[这里](http://developer.gnome.org/gtk3/)在线查看。如果你在寻找GTK2的手册，可以移步到[这里](http://developer.gnome.org/gtk2/)查看。

**本项目受[ The Swift Programming Language 中文版项目](https://github.com/numbbbbb/the-swift-programming-language-in-chinese)启发，决定也用[GitBook](https://www.gitbook.io/)完成这个翻译项目。请记住本中文手册的在线查看网址是[这里](http://zhengmingpei.github.io/GTK-3-Chinese-Reference-Manual/)，目前手册的Beta PDF版本已经更新，可以到[这里](http://pan.baidu.com/s/1pJ1bI1X)下载。**

### 目录结构

* [GTK+ 概览](content/README.md)
   * [开始使用GTK+](content/gtk+.md)
       * [基础](content/basic.md)
       * [填充](content/packing.md)
       * [绘制](content/drawing.md)
       * [构建用户界面](content/building_ui.md)
       * [构建应用程序](content/building_app.md)
           * [一个小应用](content/building_app/asmallapp.md)
		   * [填充窗口](content/building_app/paddingwindow.md)
		   * [打开文件](content/building_app/openfile.md)
		   * [一个应用菜单](content/building_app/amenu.md)
		   * [一个偏好对话框](content/building_app/dialog.md)
		   * [增加搜索条](content/building_app/searchbar.md)
		   * [增加侧边栏](content/building_app/sidebar.md)
		   * [属性](content/building_app/properties.md)
		   * [标题栏](content/building_app/headerbar.md)
* [邮件列表和bug报告](content/gtk-resources.md)
* [常见问题](content/gtk-questing-index.md)
* [GTK+ 绘图模型](content/chap-drawing-model.md)
* [GTK+ 输入处理模型](content/chap-input-handing.md)
* [GTK+ 小程序范例](content/AppExample/index.md)

### 参与流程

1. fork项目
2. 把fork过去的项目也就是你的项目clone到你的本地
3. `git branch dev` 创建dev分支
4. `git checkout dev`切换到dev分支
5. `git remote add upstream https://github.com/ZhengMingpei/GTK-3-Chinese-Reference-Manual.git` 将我的库添加到远程库。可以用`git remote -v`查看
6. `git remote update`更新远程库
7. `git fetch upstream master` 拉取库的更新到本地
8. `git rebase upstream/master`将更新合并到本地你的分支

修改之后，先push到你的库，然后登录GitHub,在你的库首页找到pull request按钮，点击填写说明，然后提交。


### 目前的两名参与者

* [ZhengMingpei](https://github.com/ZhengMingpei)  加入于2014年11月
* [truehyp](https://github.com/truehyp)  加入于2014年12月




