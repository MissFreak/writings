---
title: "Hexo博客Next主题搭建全过程"
tags:
- hexo
- markdown
---

经过三次不明觉厉的error崩盘导致我重新初始化博客（不知道哪里出错所以只有推倒重来），2021年3月18日博客终于配置完毕。在此记录全过程。最后一次配置，我只修改了三个文件：_config.yml和next/_config.yml以及custom_file_path（当然还有css/images和source）。其中custom_file_path设置为styles.styl，用于**修改文章内链接文本样式**、**文章内单行代码的样式设置**等等，需要在custom_file_path中加入这个地址。我把这些文件备份在github地址：_config.yml，next/_config.yml，next/source/css/images和source（source中有styles.styl和我写的文章）。
<!-- more -->

# 博客的初始化

关于怎么部署参考：https://zhuanlan.zhihu.com/p/26625249

```
cd hexo-final
npm install -g hexo-cli
hexo init
hexo g
hexo s
git clone https://github.com/theme-next/hexo-theme-next themes/next
```
完成以上步骤后，我们看到目前的插件如下：

```
+-- hexo-generator-archive@1.0.0
+-- hexo-generator-category@1.0.0
+-- hexo-generator-index@2.0.0
+-- hexo-generator-tag@1.0.0
+-- hexo-renderer-ejs@1.0.0
+-- hexo-renderer-marked@4.0.0
+-- hexo-renderer-stylus@2.0.1
+-- hexo-server@2.0.0
+-- hexo-theme-landscape@0.0.3
`-- hexo@5.4.0
```
我们需要安装其他插件：

```
+-- hexo-browsersync@0.3.0
+-- hexo-deployer-git@3.0
+-- hexo-generator-index-pin-top@0.2.2
+-- hexo-generator-searchdb@1.3.3
+-- hexo-related-popular-posts@5.0.1
+-- hexo-renderer-pandoc@0.3.0
+-- hexo-symbols-count-time@0.7.1
```
安装方式：
```
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
npm install 插件 --save
```
安装完毕后，我们把_config.yml和next/_config.yml以及css/images和source中的文件（注意有_data/styles.styl）复制粘贴过来。

## 网上的教程

图片操作：
https://davidwells.io/snippets/how-to-align-images-in-markdown
https://www.w3schools.com/html/html_images.asp

其他美化：
https://huangpiao.tech/2019/01/24/Hexo%E5%8D%9A%E5%AE%A2%E4%BC%98%E5%8C%96%E4%B9%8BNext%E4%B8%BB%E9%A2%98%E7%BE%8E%E5%8C%96/

Markdown进阶：

https://blog.csdn.net/thither_shore/article/details/52181464

https://unnamed42.github.io/2015-12-02-Markdown%E7%9A%84%E6%AD%A3%E7%A1%AE%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F.html

https://blog.csdn.net/m0_37925202/article/details/80461714

Markdown使用html
https://www.wuchenxu.com/2015/12/30/Markdown-html-compare/
