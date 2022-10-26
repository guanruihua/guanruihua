---
title: textare  
date: 2020-12-25 11:12:38  
tags:
- textare
- html
- front-end
---


# textAre
- 其他常用设置:　　
    - 去除右下角的三角形的图标 : `resize:none`

| 属性 | 值  | 描述   |
|---|---|---|
|  cols | number  | 文本区的可见宽度  |
|  rows | number | 文本可见行数   |
|  readonly | readonly  | 规定文本区为只读  |
|  disabled | disabled   | 禁用该文本区  |
|  form | form_id  | 规定文本区域所属一个活多个表单  |
|  maxlength | number  | 文本区域的最大字符数  |
|  placeholder | text  | 提示文字  |
|  required | required  | 文本域是必填  |
| autofocus | autofocus | 页面加载后文本域自动获得焦点
| wrap | hard sorf | 规定表单提交时, 文本区域中的文本如何换行

- wrap属性
    - wrap :文本区会包含一行文本，用户必须将光标移动到右边才能看到全部文本，这时将把一行文本传送给服务器。

    - virtual : 文本区会包含两行文本，并在单词 "makes" 后面换行。但是只有一行文本被传送到服务器：没有嵌入新行字符。
    - physical : 文本区会包含两行文本，并在单词 "makes" 后面换行，这时发送给服务器两行文本，单词 "makes" 后的新行字符将分隔这两行文本。