---
title: javaScript--select控件
tags:
- js
- select
---
### length长度
获取select下拉菜单中选项的总数
```html
<select name="select" id="select">
    <option value="1">a</option>
    <option value="2">b</option>
    <option value="3">c</option>
    <option value="4">d</option>
    <option value="5">e</option>
</select>
```
<!--more-->
```javaScript
var sel = document.getElementById("select");
console.log(sel.length)//5
```
### multiple
设置或返回下拉列表是否可以选择多个项目
```javaScript
var sel = document.getElementById("select");
sel.multiple = true;
```
### selectedIndex
设置或返回下拉列表中被选项目的索引值
```javaScript
var sel = document.getElementById("select");
console.log(sel.selectedIndex);//0
```
### size属性
设置或返回下拉列表中的可见选项数目；
```javaScript
var sel = document.getElementById("select");
sel.size = 2;
```
### value属性
设置或者返回select下拉菜单的value属性值
```javaScript
var op3 = document.getElementById("op3");
console.log(op3.value);//3
```
### test属性
设置或者返回select下拉菜单的文本内容；
```javaScript
var op3 = document.getElementById("op3");
console.log(op3.text); //c
```
### options集合
此属性返回包含下拉列表中的所用选项的一个集合；
```javaScript
var sel = document.getElementById("select");
console.log(sel.options); //[option, option, option#op3, option, option]
```
### add()方法
向下拉列表添加一个下拉列表项目
语法结构：`select.add(option,before)`
option：必需，要添加选项元素；
before：必需，在指定项之前添加；
```javaScript
var newoption = document.createElement("option");
newoption.text = "新添加项目";
newoption.value = 6;
sel.add(newoption, sel.options[5]);
```
### remove()方法
从下拉列表中删除一个选项
```javaScript
var sel = document.getElementById("select");
var newoption = document.createElement("option");
newoption.text = "新添加项目";
newoption.value = 6;
sel.add(newoption, sel.options[5]);

sel.remove(5)
```
可以看到之前添加进去的option已经不存在了；




