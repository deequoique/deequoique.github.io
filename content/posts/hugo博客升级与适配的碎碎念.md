+++
title = 'Hugo博客loveit主题升级与适配的碎碎念'
date = 2024-09-10T12:33:00+08:00
draft = false
author = "Deequoique"
hiddenFromHomePage = false
categories = ["码农日记"]
summary = "博客大升级，可喜可贺。"
tags = []
+++

先说重点：我终于做了博客**移动端**适配啦，主题也从**LoveIt**升级到了**FixIt**！

突然想起来这么个事是因为冲浪时看到了loveit的继承者fixit。这个n年没更新的主题居然有救了！于是怀着激动的心情移植了两个小时——宣布放弃。(推迟进行)

我以为移植是个简单的事情，却完全没想到，之前搞的jquery背景轮播+自定义css+hugo版本更新变动，导致需要在当前基础上做非常多的改动。否则博客就会变成一个丑陋的*乱码版*B4。

因为我如此轻敌以至于根本没切换分支，在两个小时后博客被我折腾成了四不像。只好灰溜溜的回滚，择日再战。

### 移动端适配
就在这个过程中我才想起之前的css在移动端自适应一直有问题。我调前端css的经验非常少，这次上手才知道不是让gpt output这么简单。又折腾了半个小时，终于把元素调到了一个满意的效果。

不说废话了上代码，把这个放到`assets/css/_custom.css`里（没有的话建一个），就能覆盖两个主题的css了。(LoveIt的暗黑模式改成：[theme=dark])
``` scss
.home-profile {
    margin-left: -1rem;
    margin-right: -1rem;
    background: rgb(87, 85, 84);
	opacity: .85;
    color: #ffffff;
}
[data-theme='dark'] .home-profile {
    background: #42444b;
	opacity: .8;
}
.summary {
	margin-bottom: .1rem;
    margin-left: -1rem;
    margin-right: -1rem;
	padding-left: 1rem;
	padding-right: 1rem;
    background: white;
	opacity: .95;
}
[data-theme='dark'] .summary {
    background: #3a3535;
}
.footer {
    display: block;
	border-top-width: 3px;
    border-top-style: solid;
    border-top-color: #6670ab;
    position: relative;
    z-index: -1;
    max-width: 800px;
    width: 60%;
    margin: .1rem auto 0 auto;
	padding-left: 1rem;
	padding-right: 1rem;
    background: white;
	opacity: .95;
}

[data-theme='dark'] .footer {
    background: #3a3535;
}
/* 目录 */
.toc {
    background: white;
	opacity: .95;
}
[data-theme='dark'] .toc {
    background: #3a3535;
}
[data-theme='dark'] #toc-auto {
    border-left-color: #6f6a6a;
}

.toc .toc-content {
    font-size: .8rem;
}

.toc .toc-content code {
    border: none;
    color: #f7ab01;
    font-size: 1em;
}

/* 归档、标签、分类、特殊页面 */
.page.archive, .page.single, .page.single.special {
	padding-left: 5rem;
	padding-right: 5rem;
    padding-bottom: 2rem;
    background: rgb(255, 255, 255);	
	opacity: .95;
}
[data-theme='dark'] .page.archive,
[data-theme='dark'] .page.single,
[data-theme='dark'] .page.single.special {
    background: #3a3535;
}

.archive-item-date2 {
    color: #a9a9b3;
}

[data-theme='dark'] .archive .archive-item-date {
    color: #a9a9b3;
}

.page.single.special .single-title.animated.pulse.faster {
    padding-right: 1rem;
}

.archive .card-item-title a sup {
    color: #a9a9b3;
	font-weight: initial;
}

.archive .group-title sup {
    color: #a9a9b3;
	font-weight: initial;
}

.archive .single-title.animated.pulse.faster sup {
	margin-left: .4rem;
    color: #a9a9b3;
	font-weight: initial;
}

[data-theme='dark'] .archive .tag-cloud-tags a sup {
    color: #a9a9b3;
}

/* 分类页面 */
.archive .categories-card {
    margin-top: 1rem;

    & .card-item {
        margin-top: 1rem;
    }
}
#header-desktop .header-wrapper {
    padding: 0 2rem 0 5vh;
}
#header-desktop .header-wrapper .menu .menu-item {
    margin: 0 .3rem;
}

.menu .menu-item i {
    margin-right: 2px;
}
/* 媒体查询，针对小屏幕调整样式 */
@media (max-width: 768px) {
    .footer {
        width: 90%; /* 在小屏幕上宽度调整为100% */
        padding-left: 1rem; /* 减少内边距 */
        padding-right: 1rem;
        margin: 0.5rem auto; /* 简化外边距设置 */
        border-top-width: 2px; /* 减少边框宽度 */
    }
    .page.archive, .page.single, .page.single.special {
        padding-bottom: 3rem;  /* 维持底部内边距 */
        padding-top: 1rem;   /* 减少顶部内边距 */
        padding-left: 1rem;  /* 减少左边内边距 */
        padding-right: 1rem; /* 减少右边内边距 */
        background: rgb(255, 255, 255);
        opacity: 1;
        max-width: 100%;
        width: auto;
    }
}
```
### 评论区复活
先前一直用的valine的评论服务，国内的网根本看不到，所以后来形同虚设。

现在换到了~~utteranc~~giscus，开源+GitHub，稳稳的很安心。

### 之后要做的
- [x]浏览量统计
- [x]过时博客警告横幅

~~图床升级~~(感觉没有必要)
- [x]profile美化
- [x]友链
- [x]社交账号
- [x]google/baidu/bing上站