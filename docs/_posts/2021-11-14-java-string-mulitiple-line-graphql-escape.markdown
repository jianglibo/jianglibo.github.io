---
layout: post
title:  "Graphql query string escape util for JAVA"
date:   2021-11-14 6:56:04 +0300
description: escape mulitple lines to java string.
categories: java
custom_js:
- multiple-line-escape
custom_css:
- multiple-line-escape

---

## 为什么要写这样一个工具

Java可以将Graphql的查询转换成强类型的Java类，最大限度的减少typo和其它错误。但有时候纯粹的String POST也会让事情变得简单，Java处理多行字符串是很痛苦的，容易出错。所以用elm写了这么一个小工具。[源代码](https://github.com/jianglibo/elm-mini-util)

<div id="myapp"><div id="elmapp"></div></div>
<script>
	var app = Elm.Main.init({
		node: document.getElementById('elmapp')
	});
</script>