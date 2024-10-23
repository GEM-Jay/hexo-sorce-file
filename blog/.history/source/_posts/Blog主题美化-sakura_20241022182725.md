---
title: Blog主题美化-sakura
topic: id
tags:
  - 技术
categories:
  - 技术
repo: GEM-Jay/GEM-Jay.github.io
abbrlink: 4114488392
date: 2024-10-16 13:37:29
---

# sakura主题美化（本站作者前博客，已经不再使用）

## 本站正在积极优化中，优化完成后将贴出心得

### 优化参考链接：

* [Mashiro.本站原作者](https://2heng.xin/)
* [cungudafa.一个很厉害的小姐姐](https://cungudafa.gitee.io/)
* [hojun.从sakura主题魔改本站的大佬](https://moyi.ga/)

感谢以上大佬代码的开源，枣糕也备受开源精神的感染

### 感谢在本站优化过程中为枣糕提供帮助的小伙伴：

* [kang.我的好朋友兼本站优化指导](https://www.kangblogs.top/)

## 更新日志

### **2021/4/24 23:58** 关于-我？

今日更新:  
"关于—我"下的[botui](https://botui.org/)

* BOTUI:  
  一款自动回复文字、图片、视频的JS聊天机器人框架BotUI，可以自由设置多种选项、触发关键词、输入框等内容，聊天内容或范围也可以自由设置，回复内容可以是文字、图片（GIF亦可）、视频，我在博客中引用了此框架。

优化后图片:  
![这是图片](https://cdn.jsdelivr.net/gh/GEM-Jay/images/me.jpg)  
详情跳转至[关于—我](https://zaogao.top/about/)  
本次优化参考了cungudafa姐姐的教程[教程链接](https://blog.csdn.net/cungudafa/article/details/104291032)  
修改了`主目录\source\about\index.md`及`sakura/js/botui.js`, 具体修改内容参考cungudafa的教程链接

* 修改了各分类页的图片和内容框架，不过不是很满意，仍在施工中

### **2021/4/25 23:58** 添加视频

HEXO添加视频

* 添加了分类页的[播放器](https://zaogao.top/tags/%E6%82%A6%E8%AF%BB/)

### **2021/10/17 22:26** 给文字标题添加css动画

博客搁置了好久了，今天来更新一下主题美化  
首先看看效果：

![修改前](https://cdn.jsdelivr.net/gh/Ukenn2112/image/large/20200329100739.png)

* 修改前

![修改后](https://cdn.jsdelivr.net/gh/Ukenn2112/image/large/haaiii.gif)

* 修改后

## 1.给文章标题添加css动画横线

修改 `themes\Sakura\layout\_widget\common-article.ejs`  
编辑文章通用属性目录`themes\Sakura\layout\_widget\common-article.ejs` 找到如下部分  
![目标代码](https://cdn.jsdelivr.net/gh/GEM-Jay/images/css%E6%A0%87%E9%A2%98%E6%B5%AE%E5%8A%A8%E5%8A%A8%E7%94%BB.png)  
在`<p class="entry-census">`前添加：

```none
<span class="toppic-line"></span>
```

添加自定义CSS  
将添加到`style.css`里头的自定义 CSS 样式就好啦\~  
直接复制下列代码粘贴到`style.css`

```none
/*标题横线动画*/
.single-center header.single-header .toppic-line{position:relative;bottom:0;left:0;display:block;width:100%;height:2px;background-color:#fff;animation:lineWidth 2.5s;animation-fill-mode:forwards;}
@keyframes lineWidth{0%{width:0;}
100%{width:100%;}
}
```

## 2.给标题文字添加CSS动画

这部分比较简单直接添加自定义css即可

添加自定义CSS

```none
/*标题动画*/
.entry-title {
	-moz-animation: fadeInUp 2s;
    -webkit-animation:fadeInUp 2s;
	animation: fadeInUp 2s;
}
@-moz-keyframes fadeInUp {
	0% {
		-moz-transform: translateY(200%);
		transform: translateY(200%);
		opacity: 0
	}

	50% {
		-moz-transform: translateY(200%);
		transform: translateY(200%);
		opacity: 0
	}

	100% {
		-moz-transform: translateY(0%);
		transform: translateY(0%);
		opacity: 1
	}
}

@-webkit-keyframes fadeInUp {
	0% {
		-webkit-transform: translateY(200%);
		transform: translateY(200%);
		opacity: 0
	}

	50% {
		-webkit-transform: translateY(200%);
		transform: translateY(200%);
		opacity: 0
	}

	100% {
		-webkit-transform: translateY(0%);
		transform: translateY(0%);
		opacity: 1
	}
}

@keyframes fadeInUp {
	0% {
		-moz-transform: translateY(200%);
		-ms-transform: translateY(200%);
		-webkit-transform: translateY(200%);
		transform: translateY(200%);
		opacity: 0
	}

	50% {
		-moz-transform: translateY(200%);
		-ms-transform: translateY(200%);
		-webkit-transform: translateY(200%);
		transform: translateY(200%);
		opacity: 0
	}

	100% {
		-moz-transform: translateY(0%);
		-ms-transform: translateY(0%);
		-webkit-transform: translateY(0%);
		transform: translateY(0%);
		opacity: 1
	}
}
```