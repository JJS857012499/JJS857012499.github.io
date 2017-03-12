---
title: file控件的onchang事件只触发一次
date: 2017-03-12 20:33:42
categories: javascript
tags:
 - javascript
 - onchang
---
在html中，一个file控件，第一次选中一张图片，会触发该控件的onchang事件。
如果继续选中同一张图片，则不会触发onchang事件，所以我在另一个js方法中（删除方法）
对file控件的value置为空。测试通过。
