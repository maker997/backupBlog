---
title: hexo Yilia 主题如何添加相册功能
date: 2017-07-01 08:13:47
tags: 
    - Hexo
---
> 添加相册功能的思路

如果你跟我一样不懂前端的话,要增加想相册功能就去看看网上别人的已经实现的案例.然后高度模仿,模仿出来再订制自己的样式.
经过搜索我参考这两个地方.
[Yilia主题的作者 Litten的 github 仓库](https://github.com/litten/hexo-theme-yilia)
[这个哥们实现的效果看着很棒,我就看看他如何实现的](http://lawlite.me/2017/04/13/Hexo-Github%E5%AE%9E%E7%8E%B0%E7%9B%B8%E5%86%8C%E5%8A%9F%E8%83%BD/#more)

<!---more--->

看完别人的文章之后,整理了下思路.
1.在主页上必须有一个可供点击的[相册](http://maker997.com/photos/)连接
2.要用 hexo 生成一个 photos.html 文件
3.photos.html 中的图片数据来源?因为这是一个静态页面所有要有一个 json 文件
4.json 文件中有含有信息,图片的文件名.
5.图片要有一个完整的路径,把图片放到哪呢?七牛的图床,github 的空间,自己的服务器都可以.
6.怎么上传呢?大量图片当然写脚本传了.不会写咋办?很多人都写好了,改改就是了,脚本也有很多个版本.多数用 nodeJS写的,hexo 就用的 nodeJS.Python也是很不错的选择.
7.加载图片太慢怎么办?准备两套图,一套缩略图,一套高清大图.缩略图怎么来?写脚本裁剪图片.
不多说废话了,顺着思路逐一解决问题吧
> 1.在主页上必须有一个可供点击的连接

1. 进入到你的博客目录下执行 hexo new page "photos",就会出现一个这样的新目录.
 ![new_photos](http://7xtc4k.com1.z0.glb.clouddn.com/new_photos-2.png)
2. 配置 Yilia 主题让其显示出来.
   yourBlog/themes/yilia/_config.yml文件中这样设置
   ![show_photos](http://7xtc4k.com1.z0.glb.clouddn.com/show_photos-1.png)

> 2.如何生成 photos.html 文件来

1. 我也不知道他是如何生成,不知道就模仿作者的.[下载作者的备份博客](https://github.com/litten/BlogBackup)
2. 模仿他的文件目录结构
Litten的photos文件夹下的模板文件,样式文件, js文件 
![litten](http://7xtc4k.com1.z0.glb.clouddn.com/litten_photos.png)
我的photos文件夹下的模板文件,样式文件, js文件
![](http://7xtc4k.com1.z0.glb.clouddn.com/maker_photos.png)
复制他的到你博客相应的目录下.
ejs 文件是以后要hexo 文件渲染的文件.
assets 目录是放默认图片的也要有.
3. 修改 ejs 模板文件,ins.js 文件设置自己的东西.
3.1 index.ejs文件可以不用修改
3.2 修改 ins.js 文件的 render()函数.这个函数是用来渲染数据的
  修改图片的路径地址.minSrc 小图的路径. src 大图的路径.修改为自己的图片路径(七牛的, github的路径).
  ![ins_js](http://7xtc4k.com1.z0.glb.clouddn.com/ins_js.png)
  
> 3.生成 json 文件.

这一步是关键的一步,也是最后一步.先用脚本把图片处理成一套大图和一套小图,
然后上传的七牛或者 github 上再回头生成这个 json文件.
每次更新图片都要执行脚本重新生成 json 文件.这个json 文件会出现在 
yourBlog/source/photos/data.json

> 4.处理图片

我用的别人写好的脚本,看他写得已经很全了,我也懒得再写了.改改就好了.脚本[下载地址](https://github.com/maker997/backupBlog/)
.py 文件就是 Python 的脚本.
1. tool.py 文件中几个方法
![tools](http://7xtc4k.com1.z0.glb.clouddn.com/tools.png)
2. 放图的文件夹和 tool.py 的文件目录结构
 ![makerBlog_di](http://7xtc4k.com1.z0.glb.clouddn.com/makerBlog_dir-1.png)
3. 需要修改的地方
  3.1 git_operation()方法: 
  如果你把图片上传到你的 github上这个方法就不用更改了.但是要确保你这个目录能否 push 到 GitHub.
  3.2 如果你打算将图片上传到七牛上.这个方法就要删除掉了,重新写一个上传到七牛的方法.[参考链接](http://lovenight.github.io/2015/11/17/%E6%8B%96%E6%9B%B3%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E5%88%B0%E4%B8%83%E7%89%9B%E7%9A%84%E8%84%9A%E6%9C%AC/)
  3.3 handle_photo()方法:
  生成 json 文件最关键的一步.很好非常好,马上要成功了.
  注意: 该脚本对图片的命名规则有要求.
  图片应该这样命名: 2016-10-12_xxx.jpg/png
  
  
  > 5.会出现的坑
  
1. python too.py 命令的时候出现`no module named PIL`错误
   出现这错误的原因是: PIL 模块找不到,PIL 模块已经过时了.
   解决方案: pip install pillow
   我的是Mac 系统, python 版本是3.5.2, pip的版本是9.0.1.
   其他环境的暂时没有尝试.
2. mac系统如何切换 Python 的版本
    [python3.5.2连接下载连接](https://www.python.org/downloads/release/python-352/)
    ![downloadPython](http://7xtc4k.com1.z0.glb.clouddn.com/downloadPython.png)
*   一路 next 安装完毕
*   设置 python 的环境变换切换版本
*   open ~/.bash_profile 打开设置保存用户环境变量的文件
*   在 export PATH 之后添加 alias python="/Library/Frameworks/Python.framework/Version/3.5/bin/python3.5"(注意引号的是否为英文)
*   设置完成的效果
    ![pythonShortCut](http://7xtc4k.com1.z0.glb.clouddn.com/pythonShortCut.png)
*   保存退出后,让文件生效. source ~/.bash_profile
  
> 6.总结

走到这里所有的所有的准备工作都做好了.
进入到你博客目录, 执行 python tool.py(处理图片,上传图片,生成 json 文件)
hexo clean (清理之前的 HTML 等)
hexo g (生成 HTML 文件)
hexo s (看看效果如何)
最后部署到你的博客上.
网上大致文章都是这么的写,找到一个差不多就开始实践吧,如果你看我的去实践遇到问题可以找我,我只要有时间定会回复你.如果遇见更多的坑欢迎补充(github提问,博客评论, QQ 加好友都可以哦)✌️
  
  







