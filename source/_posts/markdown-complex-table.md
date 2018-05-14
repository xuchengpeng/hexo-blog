---
title: "Markdown 复杂表格"
date: 2018-05-14 19:46:01
udpated: 2018-05-14 19:46:01
comments: true
categories:
 - Technology
tags:
 - Markdown
---

由于Markdown语法本身不包含复杂表格的插入，但是可以使用html语法来实现，一般的Markdown编辑器都是支持html语法的。

复杂表格与简单表格最大的差异有两点：水平单元格的合并和纵向单元格的合并，通过html语法实现这两个操作本质就是：删除多余的空白单元格，然后使用某些单元格的colspan和rowspan属性进行扩展填充。

* 水平单元格的合并：基于colspan属性，即使一个单元格占多列的空间。
* 纵向单元格的合并：基于rowspan属性，即使一个单元格占多行的空间。

可以从Word或者Excel中复制表格，通过网站 [No-Cruft Excel to HTML Table Converter](http://pressbin.com/tools/excel_to_html_table/index.html) 转换获得html代码。
<!-- more -->

## 合并行

```html
<table>
    <tr>
        <td>列一</td>
        <td>列一</td>
   </tr>
    <tr>
        <td colspan="2">合并行</td>
    </tr>
    <tr>
        <td colspan="2">合并行</td>
    </tr>
</table>
```

显示效果：

<table>
    <tr>
        <td>列一</td>
        <td>列一</td>
   </tr>
    <tr>
        <td colspan="2">合并行</td>
    </tr>
    <tr>
        <td colspan="2">合并行</td>
    </tr>
</table>

## 合并列

```html
<table>
    <tr>
        <td>列一</td>
        <td>列二</td>
   </tr>
    <tr>
        <td rowspan="2">合并列</td>
        <td >行二列二</td>
    </tr>
    <tr>
        <td >行三列二</td>
    </tr>
</table>
```

显示效果：

<table>
    <tr>
        <td>列一</td>
        <td>列二</td>
   </tr>
    <tr>
        <td rowspan="2">合并列</td>
        <td >行二列二</td>
    </tr>
    <tr>
        <td >行三列二</td>
    </tr>
</table>

## 合并行列

```html
<table>
    <tr>
        <td>列一</td>
        <td>列二</td>
   </tr>
   <tr>
        <td colspan="2">合并行</td>
   </tr>
   <tr>
        <td>列一</td>
        <td>列二</td>
   </tr>
    <tr>
        <td rowspan="2">合并列</td>
        <td >行二列二</td>
    </tr>
    <tr>
        <td >行三列二</td>
    </tr>
</table>
```

显示效果：

<table>
    <tr>
        <td>列一</td>
        <td>列二</td>
   </tr>
   <tr>
        <td colspan="2">合并行</td>
   </tr>
   <tr>
        <td>列一</td>
        <td>列二</td>
   </tr>
    <tr>
        <td rowspan="2">合并列</td>
        <td >行二列二</td>
    </tr>
    <tr>
        <td >行三列二</td>
    </tr>
</table>

