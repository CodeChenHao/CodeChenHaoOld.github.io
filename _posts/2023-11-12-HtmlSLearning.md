---
layout: post
title: Html
description: Html相关的概念、初体验以及语法
tags: 前端相关技术
---

## 一、HTML初识

### 1.1 网页由什么组成
```
文字
图片
视频
音频
超链接
```

### 1.2 网页的本质
```
一个网页就是一个HTML文件，通过浏览器可以将HTML文件中的代码转化成网页
```

### 1.3 浏览器是什么
```
浏览器是网页展示、运行的平台
```

### 1.4  常见的浏览器
```
IE => Trident
火狐 => Gecko
Safari => Webkit
Opera => Blink
谷歌(推荐使用) => Blink

注：浏览器的内核用于渲染网页，如果内核不一样，则同一个网页通过不同内核渲染时，渲染速度、效果可能不一样。
```
### 1.5 Web规范
```
HTML => 负责网页结构/网页内容
CSS => 负责网页外观/网页样式
JS => 负责页面交互
```

## 二、HTML初体验

### 2.1 HTML简介

```
Hyper Text Markup Language,中文翻译为超文本标记语言
```

### 2.2 文字变粗案例
```html
<strong>需要加粗的文字</strong>
```

### 2.3 HTML骨架：
```html
<html>
	<head>
		<title>网页的标题</title>
	</head>
	<body>
		网页的主题内容
	</body>
</html>
```

### 2.4 开发工具
```
VSCode (推荐、体积小、免费、插件多)、WebStorm、Sublime、Dreamweaver、Hbuider
```

## 三、HTML语法

### 3.1 注释

```html
<!--注释的内容-->
```

### 3.2 HTML标签的结构
```
双标签：
	<开始标签>内容</结束标签>
单标签：
	<标签/>
```

### 3.3 标签之间的关系
```
父子关系：嵌套
兄弟关系：并列
```

### 3.4 HTML标签

#### 3.4.1 排版标签

**标题标签**


```html
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>

特点:
	文字都加粗、文字逐渐减小、独占一行
```

**段落标签**
```html
<p>我是一段文字</p>

特点:
	段落之间存在间隙、独占一行
```

**换行**

```html
<br/>
```

**水平分割线**

```html
<hr/>
```

#### 3.4.2 文本格式化标签

**加粗**

```html
<b>文本</b>
<strong>文本</strong>
```

**下划线**

```html
<u>文本</u>
<ins>文本</ins>
```

**倾斜**

```html
<i>文本</i>
<em>文本</em>
```

**删除线**

```html
<s>文本</s>
<del>文本</del>
```

#### 3.4.3 媒体标签

**图片**


```html
<img src="图片路径" alt="替换文本" title="提示文本" width="100" height="100">
```

**音频**

```html
<audio src="音频路径" controls autoplay loop>
```

**视频**

```html
<video src="视频路径" controls autoplay loop muted>
```

#### 3.4.4 链接标签

```html
<a href="跳转路径" target="_blank">文本</a>
```

#### 3.4.5 列表标签

**无序列表**

```html
<ul>
	<li>苹果</li>
	<li>香蕉</li>
	<li>西瓜</li>
</ul>
```

**有序列表**

```html
<ol>
	<li>张三</li>
	<li>李四</li>
	<li>王五</li>
</ol>
```

**自定义列表**

```html
<dl>
	<dt>自定义的主题</dt>
	<dd>自定义项</dd>
	<dd>自定义项</dd>
</dl>
```

#### 3.4.6 表格标签

**表格**

```html
<table border="1" width="20" height="20">
	<caption>表格标题<caption>
	<thead>
		<th>
            <td>1</td>
            <td>2</td>
            <td>3</td>
		</th>
	</thead>
	<tbody>
        <tr>
            <td>1</td>
            <td>2</td>
            <td>3</td>
        </tr>
        <tr>
            <td>4</td>
            <td>5</td>
            <td>6</td>
        </tr>
        <tr>
            <td>7</td>
            <td>8</td>
            <td>9</td>
        </tr>
	</tbody>
	<tfoot>
		<tr>
            <td>10</td>
            <td>11</td>
            <td>12</td>
        </tr>
	</tfoot>
</table>

跨行合并:rowspan
跨列合并:colspan
```

#### 3.4.7 表单标签

**表单**

```html
<form action="提交的路径">
</form>
```

#### 3.4.8 Input系列标签

**文本框**

```html
<input type="text" name="后台的key" palceholder="提示文本">
```

**密码框**

```html
<input type="password" name="后台的key" palceholder="提示文本" >
```

**单选框**


```html
<input type="radio" name="后台的key" checked>
```

**复选框**

```html
<input type="checkbox" name="后台的key" checked>
```

**文件**

```html
<input type="file" name="后台的key" multiple >
```

**提交按钮**

```html
<input type="submit" value="我是一个提交按钮">
```

**重置按钮**

```html
<input type="reset" value="我是一个重置按钮">
```

**普通按钮**

```html
<input type="button" value="我是一个按钮">
```

#### 3.4.9 Button系列标签

**提交按钮**

```html
<button type="submit">我是提交按钮</button>
```

**重置按钮**

```html
<button type="rest">我是重置按钮</button>
```

**普通按钮**

```html
<button type="button">我是普通按钮</button>
```

#### 3.4.10 Select系列标签

**下拉标签**

```html
<select name="后台的key">
	<option value="" selected></option>
	<option value="北京">北京</option>
	<option value="上海">上海</option>
	<option value="深圳">深圳</option>
</select>
```

#### 3.4.11 TextArea标签

**文本域**

```html
<textarea cols="20" rows="20" palceholder="提示文本"></textarea>
```

#### 3.4.12 Lable标签

```html
<lable for="id">标签</lable><input id="app" type="text">

<lable>标签<input type="text"></lable>
```

#### 3.4.13 语义化标签

**无语言的标签**

```html
<div></div>
	独占一行
<span></span>
	在一行显示
```
**有语义的标签(手机端)**
```
header =>头部
nav =>导航
footer =>底部
aside =>侧边栏
section =>区块
article =>文章
```

![](/images/posts/2023-11-12-HtmlSLearning/image-20230522234259019.png)

### 3.5 字符实体

```
空格: &nbsp;
<: &lt;
>: &gt;
": &quot;
': &apos;
```

