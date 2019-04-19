---
title: 个人在hexo遇到的坑
date: 2019-03-06 23:37:29
tags: hexo
categories: hexo
---
###1.下载百度提交链接模块遇到bug

整百度SEO的时候，有个向百度提交自己网站地址的操作，安装了 "hexo-baidu-url-submit --save" 这个插件，网站认证是通过了.

然而，我吃个晚饭回来准备继续整网站时,

hexo clean && hexo g && hexo s

一堆报错提示！当时内心简直崩溃！！！(才不会说是未保存就在本地调主题呢...

这个bug我也没整懂，翻译意思好像是"Count"未赋值，一堆js文件里提示error:"Count"undefined，简直抓狂啊，这要怎么找bug！

copy关键句怎样百度都没办法，甚至想到了克隆之前提交能跑的git仓库（结果没有克隆blog上传，git仓库里的是整合好的...)发现git仓库最近一次提交是19：22，于是只能用蠢办法翻chrome历史纪录查自己弄过什么插件了，照着历史纪录一个一个插件uninstall回去，改过的配置一一还原，最终通过报错的js文件，找到了源头"百度提交插件"的锅...

npm uninstall hexo-baidu-url-submit --save ，得嘞 你给我滚回去吧(怒

###2.fs.SyncWriteStream is deprecated

关于这个错误，参考了这篇博主的文章：https://www.leiyawu.com/2018/02/28/hexo-fs-SyncWriteStream-is-deprecated/

这里稍微贴下方法步骤吧

npm install hexo-fs –save&emsp;更新hexo-fs插件

问题出在：hexo-admin的hexo-fs
因hexo-admin作为后台管理，无法npm uninstall hexo-admin卸载,则找到对应文件，注释：

[root@server init]# grep -irn “SyncWriteStream” ./node_modules/hexo-admin/
./node_modules/hexo-admin/node_modules/hexo-fs/lib/fs.js:718:exports.SyncWriteStream = fs.SyncWriteStream;
[root@lywserver init]#

将对应的exports.SyncWriteStream = fs.SyncWriteStream;注释(前面 //)即可！

###3.蠢蠢的把网站下面运行时间的©打成了@,这个报错很好就找到了...

