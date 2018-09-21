---
title: CSS 垂直居中
date: 2018-09-20 17:20:36
tags: [CSS]
categories: [CSS]
---

1. 显示方式设置成表格
html:
```
<div id="wrapper">
    <div id="cell">
        <div class="content">Content goes here.</div>
    </div>
</div>
```
css:
```
#wrapper {
    display: table;
    height: 100px;
    background: pink;
}
#cell {
    display: table-cell;
    vertical-align: middle;
}
```
效果：
![](https://capping.github.io/images/css/css-vertical-center-01.jpg)

2. 父元素相对定位，子元素绝对定位
html:
```
<div id="wrapper">
    <div class="content">Content goes here.</div>
</div>
```
css:
```
#wrapper {
    border: 1px solid #ccc;
    width: 600px;
    height: 400px;
    position: relative;
}
.content {
    background: pink;
    width: 300px;
    height: 200px;
    positioin: absolute;
    left: 50%;
    top: 50%;
    margin-left: -150px;
    margin-top: -100px;
}
```
效果：
![](https://capping.github.io/images/css/css-vertical-center-02.jpg)

3. CSS3 `transform` 代替 `margin`
html:
```
<div id="wrapper">
    <div class="content">Content goes here.</div>
</div>
```
css:
```
#wrapper {
    border: 1px solid #ccc;
    width: 600px;
    height: 400px;
    position: relative;
}
.content {
    background: pink;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}
```
效果：
![](https://capping.github.io/images/css/css-vertical-center-03.jpg)

4. `margin: auto` 实现
html:
```
<div id="wrapper">
    <div class="content">Content goes here.</div>
</div>
```
css:
```
#wrapper {
    border: 1px solid #ccc;
    width: 600px;
    height: 400px;
    position: relative;
}
.content {
    background: pink;
    width: 200px;
    height: 100px;
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```
效果：
![](https://capping.github.io/images/css/css-vertical-center-04.jpg)
