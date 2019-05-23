---
---
一键将 Word 转换为 Markdown
===========================

![96](image/media/bca5f1852bb1e5bce46123837d06d10d.png)

 

stata连享会 关注

2017.11.09 15:43\* 字数 1332 阅读 2327评论 1喜欢 8

李缘 \| \| Stata 连享会
([知乎](https://link.jianshu.com/?t=https%3A%2F%2Fzhuanlan.zhihu.com%2Farlion) \| [简书](https://www.jianshu.com/u/69a30474ef33) \| [码云](https://link.jianshu.com/?t=https%3A%2F%2Fgitee.com%2Farlionn) \| github)

### 方法一：Writage + Pandoc -- 双剑合璧！

1.  下载并安装 Writage，下载地址：[http://www.writage.com/](https://link.jianshu.com/?t=http%3A%2F%2Fwww.writage.com%2F)

-   打开 Writage网页，点击Download，再点击Download Now完成下载

![https://upload-images.jianshu.io/upload_images/7692714-0dea7d7aeeffba56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](/image/media/4464e9112ba9760beabb30953433d66b.png)

网页

![https://upload-images.jianshu.io/upload_images/7692714-6ce19b9714bb0336.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/53a8011e08db9de6d82fa7d3a3513db2.png)

下载

-   运行安装程序，一般按照默认选项安装就好啦

![https://upload-images.jianshu.io/upload_images/7692714-b10e96c9c32e0026.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/569](image/media/28bddb19b679e36a7807eff323094b7c.png)

安装

-   重启电脑，新建或打开任一 Word 文档，在 文件 菜单栏下选 另存为，查看 **【保存类型】** 中是否有 Markdown 格式。  
    （如果插件安装成功，就会自动出现Markdown选项；否则，重新安装一遍吧\~）

![https://upload-images.jianshu.io/upload_images/7692714-180fec4da16b8f9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/8954d86f0eaf75c55db0df779746dc85.png)

输入图片说明

1.  下载并安装 Pandoc：官方下载地址 ； 百度云下载地址

-   运行安装程序，一般按照默认选项安装就好啦

![https://upload-images.jianshu.io/upload_images/7692714-200a8ac8072f02c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/668](image/media/05806da6a6dd8a308b7afceed03ac316.png)

下载

1.  准备好工具啦，我们开始尝试将word文档转换为markdown文档吧！

-   **首先设置 word
    文档中的标准样式**，如一级、二级标题，项目符号或编号等，如此才能与 markdown
    的格式对应  
    （稍微有点繁琐的前期准备，如果文档一开始就是按照标准样式排版，就没有这个烦恼啦）

![https://upload-images.jianshu.io/upload_images/7692714-abb1b0c891b37773.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/image/media/45dc65b7f96501a945226b6e5416659e.png)

设置

-   Word 格式 **另存为** Markdown（这是最关键的一步\~）

![https://upload-images.jianshu.io/upload_images/7692714-783e08901ab25077.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/674](image/image/media/b72a9b30e0ed99b6d16845a52dce292d.png)

另存为

-   markdown 文档在 Word 中的显示效果：

![https://upload-images.jianshu.io/upload_images/7692714-e545bfbe49f09069.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/01b38e3f7d38cdfa12cc4c77456c8879.png)

image

由于markdown中的图片无法设置大小，因此在word中排布的图片格式不标准，需要人工调整

其他格式，如一级、二级标题，项目列表等基本没有问题，其中表格显示如下

![https://upload-images.jianshu.io/upload_images/7692714-80ae8d0c8d8f306d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/f21cff6f93207123aaf2b729cf08ad79.png)

image

-   将markdown文档上传至码云等平台并提交，对应的内容预览效果如下

![https://upload-images.jianshu.io/upload_images/7692714-570250456c4049f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/ee1def7469b1c43711b837a2e15e6124.png)

image

![https://upload-images.jianshu.io/upload_images/7692714-99c848242acc487a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/2c6f3d4f7d5d0d1a85fd759c76389cde.png)

image

4.大功告成啦！ :laughing:

#### 更加快捷（有时略坑）的方法二：Word to Markdown Converter在线转换网页！

1.  **设置word文档中的标准样式，如一级、二级标题，项目符号或编号等，如此才能与markdown的格式对应**

（稍微有点繁琐的前期准备，如果文档一开始就是按照标准样式排版，就没有这个烦恼啦）

![https://upload-images.jianshu.io/upload_images/7692714-9dfa2efd15ca0a63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/197f403c53f7bb48f5295d4cc2de56ff.png)

设置

1.  打开网页Word to Markdown
    Converter：[https://word-to-markdown.herokuapp.com/](https://link.jianshu.com/?t=https%3A%2F%2Fword-to-markdown.herokuapp.com%2F)

![https://upload-images.jianshu.io/upload_images/7692714-a8bc48ed32e7907e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/270040b94b9cdc496d5ee0cb64941818.png)

输入图片说明

1.  选择需要转换的Word文件,点击Convert

![https://upload-images.jianshu.io/upload_images/7692714-a2998ff04f5745ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/a7b571a41fc1cbef85f33eca68b72bc1.png)

输入图片说明

4.大功告成啦！ :yum:
页面左边显示的是Markdown文档的内容，右边显示的是预览出来的样子

![https://upload-images.jianshu.io/upload_images/7692714-692e722f34b3be99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700](image/media/26dfa0fa67bd996666ef026ee25836de.png)

输入图片说明

**那么坑在哪里呢？** 我们会发现，表格转换出来是这样子的 :disappointed:

Markdown文档

![https://upload-images.jianshu.io/upload_images/7692714-235cbd951c925c95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/563](image/media/201ec9e8fc1b81021cbf5b3a84b8a999.png)

输入图片说明

预览出来

![https://upload-images.jianshu.io/upload_images/7692714-ea29513995c11fea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/561](image/media/972eb6f24dc201130749a5819445c785.png)

输入图片说明

-   **因此还需要进一步手动编辑整理完善表格**

（如果，Word文档本身不包括表格，Word to Markdown
Converter使用体验可谓相当便捷稳妥啦！）

#### 图片的下载与存储

除了标准格式设置与表格调整问题， **图片的下载与存储**也会是我们可能会遇到的问题

-   **Markdown转换为Word** ：在Markdown文档中，图片以网络超链接的形式保存,如果markdown文档中有这一类图片，那么需要在网络连接的情况下，才能正常输出有图片的word文档。否则，图片处显示空白

-   **Word转换为Markdown** ：Word转换为Markdown文档之后，文档中的图片输出到本地文件夹下，将该文件夹与输出的Markdown文档在同一目录下，在Markdown文档中图片引用本地相对路径，也就是说，必须保证Markdown文档与存放图片的本地文件夹在一起，才能完整的在markdown编辑器中显示图片

参考资料：

[软件技能\|markdown与word相互转换的快捷方法](https://www.jianshu.com/p/f9c5da56e0cb)

[怎么把word转成markdown](https://www.jianshu.com/p/b0be43b03015)