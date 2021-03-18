---
layout: "post"
title: "从小白到起飞，一站解决Atom编辑器各种骚操作"
categories:
- 代码
- 编辑器
tags:
- atom
- editor
---

> 一站到底解决IDE搭建问题！从基础设置、插件安装、snippets填充、到github版本控制、代码调试……本文将不断更新以求完善！

## 安装文本编辑器Atom

入坑Sublime、VSCode无数次后，还是回头选择了Atom IDE，因为它颜值惊艳、操作便捷、界面简单，除了许多编辑器都有的**代码折叠**和**自动补全**，还自带原生Markdown支持！！！这漂亮的实时预览和代码高亮真让人春心荡漾！并且，插件非常丰富！
<!-- more -->
于是，果断选择Atom作为本博主的**创作神器**，为Python开发之旅保驾护航！

官网一键安装：[AtomSetup-x64](https://atom.io/)

确认操作系统无误，点击download，打开下载好的AtomSetup-x64.exe，极速体验！

![img](https://pic2.zhimg.com/v2-e2e305deea69303a19d71da452138945_b.png)

*缺点是启动速度不如sublime*

## 安装插件：失足卡顿的惨痛经历

此时你一定急不可耐地冲向Install a Package，风风火火地下载了一堆插件：minimap用来预览全貌，atom-beautify用来格式化（想到令人头痛的html），file-icons小图标好可爱呀，material主题貌似很热门吼，markdown-preview-enhanced吊打我的Typora呢（Typora是我常用的md编辑器），script可以运行代码嗷，autocomplete-python自动补全呢，python-autopep8调格式也不错哟，linter-flake8检查语法错误呢，Hydrogen简直是jupyter的孪生姐妹……你在界面乐此不疲地倒腾……

A few hours later...

突然，你意识到此时的atom一片混乱卡顿缓慢，再也不是当初清纯活泼的模样！你蹙起眉，两行清泪润湿了乌黑的下眼眶！

一怒之下，你卸载了atom，并剿杀了一切软件残留：[如何彻底删除atom](https://cn.compbs.com/how-uninstall-atom-windows)，[如何更彻底地删除](https://www.coder.work/article/552966)！

## 正确的打开方式是什么？

正文从这里开始！

那么，配置环境和安装插件的正确方式是什么呢？

1. 打开Editor Settings，**勾选Scroll Past End和Show Indent Guide，设置Tab Length为4，Font Size为20**，护眼第一啦！
2. 主题我喜欢atom自带的**One Light，**打开themes->One Light UI->Settings->**Font Size设置为15**，语法主题我选择**Monokai**。还是为了护眼！码字时还可以随时ctrl+和ctrl-调整字体大小。
3. 下载插件：**minimap、file-icons、atom-beautify、markdown-preview-enhanced、python-autopep8、markdown-writer**。大道至简，这是我目前选择的基础组件，可能以后会更新！进入python-autopep8->Settings，勾选Format On Save。

![img](https://pic3.zhimg.com/v2-62a1e257adc2ca9978dee539798d3342_b.png)

*atom-material-syntax-dark语法主题也好看，只是Python语法高亮略丑，就不放了*

个人认为，**编辑器只需提供高效、舒适的代码体验即可**，各种花里胡哨的功能比较鸡肋，反而会加重的负担。

[atom热门主题排行榜](https://atom.io/themes/list?direction=desc&sort=downloads)：material、monokai、seti。

[atom热门插件排行榜](https://atom.io/packages/list?direction=desc&sort=stars)：minimap、file-icons、atom-beautify、linter、script。

**下面重点来了，下载插件的方式！！！**

打开cmd，执行以下命令（apm是Atom Package Manager的缩写）：

```
pip install autopep8
apm install monokai, minimap, file-icons, atom-beautify, markdown-preview-enhanced, python-autopep8
```

如果一起下载速度太慢，也可以apm install <package_name>分开下载。

晃悠了一杯茶的时间，已经全部done啦！Voila！

重启Atom，Ctrl+Shift+P打开Settings->Packages，是不是整齐陈列着我们要的插件呢？（不知为什么ctrl+逗号不能打开settings）

码上行动叭！！！

![img](https://pic3.zhimg.com/v2-873f85f52366e6ab44b3eda548930102_b.png)

*这是最终的windows界面*

另外，有童鞋选择github上面的源码git clone，然后cd进文件夹、npm install来安装，也不错呢~

对了，卸载插件的话，apm uninstall <package_name> 就好啦！

----------2021-03-04更新---------

## 代码填充功能Snippets

当我们需要重复使用一套模板时，不如试试[Atom自带的Snippets代码块功能](https://www.jianshu.com/p/2ee34d8da142)，这种快捷填充，省去了重复码字的时间。打开终端输入下面指令，用atom打开snippets.cson：

```
atom C:\Users\用户名\.atom\snippets.cson
```

可以看到目前空空如也，在这个文件里面打“snip”，然后敲下tab键，会跳出来用于创建snippets的snippets（像套娃一样耶）。换上你想存储的代码块吧，示例如下：

![img](https://pic1.zhimg.com/v2-d5d6ec618868382045aa1044eae6e988_b.png)

以后每次想插入代码块，直接输我们预设的“暗号”，然后按tab键即可，迅如闪电呀！

**注意**：snippets只在相应语言中有效！并且，除了常见的Python、Java、JS等，**其他有些语言是不支持snippets功能的**，此时有两种方法：

1. 通过apm install language-语言，安装相应语言的支持包；
2.  将.source.语言 改成* 。

我比较建议第二种方法。

另外，mardown中输入table、img、L等按下tab键，可以快捷插入表格、图片、链接等。

![img](https://pic2.zhimg.com/v2-4d43fef791abc2195246b9621b37f4b9_b.gif)

成功啦，美滋滋~

## 代码调试

暂时用不到，先占个坑，下次完善！

## 使用Github远程版本控制

又解锁atom连接github和git啦！赶紧更新日志！

话说，atom本来就是github开发的编辑器好嘛！我们来测试下好用吗！

Github注册无须多言吧，我创建了一个新的repository，命名blog，用来云端存储我的写作素材与稿件。这时我们看到页面提供了一系列指令：

![img](https://pic1.zhimg.com/v2-6daf82cf178c578fd4cf4c19da68cb88_b.png)

*这里其实是blog仓库，但我为了演示又重新建了名为test的仓库*

我们只需要打开windows系统的git bash，一句一句输入。我输入的是以下指令：

```
cd blog
echo "#blog" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/MissFreak/blog.git
git push -u origin main
```

成功后，在Atom打开blog这个文件夹（也就是你想要进行版本控制的项目），我们看到右侧显示github登录界面，提示我们打开github.atom.io，复制GitHub token到Atom的登录表单。Success！

![img](https://pic1.zhimg.com/v2-6e1aa37602728c2eb6bdc8701b013270_b.png)

接下来就随心所欲地增改文件，然后stage all->commit to main->pull 吧！

终于不用在终端敲指令，也可以和Github Desktop说拜拜啦！

![img](https://pic4.zhimg.com/v2-713ea328565e6babf7e9559c20bc2e8b_b.png)

**参考资料：**[Atom Documentation-GitHub package](https://flight-manual.atom.io/using-atom/sections/github-package/)

## 你可能不知道的快捷键

| 功能                           | 快捷键           |
| ------------------------------ | ---------------- |
| 在所有项目文件中查找字符串     | Ctrl + Shift + f |
| 打开导航栏的File、Edit、View等 | Alt+导航栏首字母 |
| 向上/下移动该行                | Ctrl + up/down   |

[Windows环境下的Atom快捷键](https://yanyinhong.github.io/2017/07/23/Atom-keyboard-shortcuts/)

[mac下Atom编辑器快捷键大全](https://www.itread01.com/content/1549978931.html)

![img](https://pic4.zhimg.com/v2-e0ed2bebdccf67e44a7c2bb61ff7b26b_b.gif)

*在项目所有文件中搜索“正则”*

----------2021-03-04更新---------

## HTML神器

又屁颠屁颠跑来更新啦，虽然只有我一个人自娱自乐。

Python开发网站离不开的插件，亲测好用：

```
apm install atom-html-preview, pigments, highlight-selected, autoclose-html-plus, color-picker, color-tabs
```

效果如下：

![img](https://pic3.zhimg.com/v2-692783c713218dc64a68c8faca88c096_b.png)

## Markdown神器

还有，大家赶紧把这款神器mardown-preview-enhanced用起来吧，功能齐全到不可想象！**可以折叠一级、二级、三级标题，专注于当前标题下的内容！（但是我刚用了貌似会卡）**

可以运行代码，并渲染运行结果：

![img](https://pic3.zhimg.com/v2-12f773fb3a40b67afa0a432aa3dbc9b2_b.png)

各种流程图、结构图：

![img](https://pic2.zhimg.com/v2-9c4fd8ff9abd02d7cbb3f6566bf0549d_b.png)

甚至制作幻灯片：

![img](https://pic4.zhimg.com/v2-cdcf68324e69b094ea314250405b9e9b_b.gif)

还有太多亮瞎眼的操作不一一列举，详情请戳：https://shd101wyy.github.io/markdown-preview-enhanced/#/zh-cn/

VScode也可以用~真的很想把sublime扔进垃圾桶了！虽然sublime打开速度确实快！

## Terminal 神器

我又发现了一款终端程序包，terminal-plus！可以更改主题、查看终端当前运行的命令进程！用颜色标记状态图标和排序！从文本编辑器插入并运行文本！还有太多功能大家自己查阅！再次挖到宝贝啦！

[jeremyramin/terminal-plus](https://github.com/jeremyramin/terminal-plus)

![img](https://pic1.zhimg.com/v2-a579e162c502f89da0d9036ac21abfb8_b.png)

总结：我目前安装的插件和主题如下：

```
├── atom-beautify@0.33.4
├── atom-html-preview@0.2.6
├── autoclose-html-plus@0.27.2
├── color-picker@2.3.0
├── color-tabs@0.1.8
├── file-icons@2.1.46
├── highlight-selected@0.17.0
├── markdown-preview-enhanced@0.18.8
├── markdown-writer@2.11.11
├── minimap@4.39.9
├── monokai@0.27.0
├── pigments@0.40.6
├── python-autopep8@0.1.3
└── python-tools@0.6.9
```
