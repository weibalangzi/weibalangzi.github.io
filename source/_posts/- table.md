---
title: javaScript--table表格
tags:
- js
- table
---
### cells属性
cells可以获取表格中某一行所有单元格的集合；
```html
<!--css部分-->
    <table id="theTable">
        <thead>
            <th>编号</th>
            <th>姓名</th>
            <th>年龄</th>
        </thead>
        <tbody>
            <tr>
                <td>01</td>
                <td>a</td>
                <td>18</td>
            </tr>
            <tr id="tr2">
                <td>02</td>
                <td>b</td>
                <td>19</td>
            </tr>
            <tr>
                <td>03</td>
                <td>c</td>
                <td>20</td>
            </tr>
        </tbody>
    </table>
```
<!--more-->
```javaScript
//js部分
var table = document.getElementById("theTable");
console.log(table.rows[0].cells);//HTMLCollection(3) [th, th, th]
```
以上代码可以获取表格中某一行所有单元格的集合；
### rowIndex属性
此属性可以获取当前tr行在table表格tr行集合中的索引位置，索引值从0开始；
```javaScript
var tr2 = document.getElementById("tr2");
console.log(tr2.rowIndex);//2
```
### rows属性
rows可以获取表格中所有行的集合；
```javaScript
var table = document.getElementById("theTable");
console.log(table.rows.length); //4
```
如上，可以huo获取表格中所有行的数量；
### insertRow()
此方法可以在table表格的指定位置插入一个新行tr；
```javaScript
var table = document.getElementById("theTable");
var tr2 = document.getElementById("tr2");
var newTr = table.insertRow(1);
var td1 = newTr.insertCell(0);
var td2 = newTr.insertCell(1);
var td3 = newTr.insertCell(2);
td1.setAttribute("class", "td1")
td1.innerHTML = "00";
td2.innerHTML = "zero";
td3.innerHTML = "17";
console.log(table.rows[1].cells);//HTMLCollection(3) [td.td1, td, td]
```
### deleteRow()
此方法可以删除表格中指定的行；
```javaScript
var table = document.getElementById("theTable");
var tr2 = document.getElementById("tr2");
var newTr = table.insertRow(1);
var td1 = newTr.insertCell(0);
var td2 = newTr.insertCell(1);
var td3 = newTr.insertCell(2);
td1.setAttribute("class", "td1")
td1.innerHTML = "00";
td2.innerHTML = "zero";
td3.innerHTML = "17";
console.log(table.rows[1].cells); //HTMLCollection(3) [td.td1, td, td]
table.deleteRow(1)
console.log(table.rows[1].cells);//HTMLCollection(3) [td, td, td]
```
可以看到之前添加进去的一行被删除了；
### insertCell()
在表格中行的指定位置插入一个td单元格
### deleteCell()
删除table表格中指定的td单元格；
