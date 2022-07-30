---
title: Markdown文件的相关语法
date: 2020/10/29
keywords: Makedown
index_img: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1603980235897&di=18bf19228133a15cc5899c03f6efabfc&imgtype=0&src=http%3A%2F%2Fb-ssl.duitang.com%2Fuploads%2Fitem%2F201804%2F07%2F20180407083800_MBz2r.thumb.700_0.jpeg
cover: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1603983928098&di=79fabf49d6e8ab3cc8a4013e35915e5c&imgtype=0&src=http%3A%2F%2Fc2.hoopchina.com.cn%2Fuploads%2Fstar%2Fevent%2Fimages%2F200105%2Fab31082ad6c8121fba58220ea18cdb4f003242ef.jpg
toc_number: true
toc: true
---

# Makedown 文件的相关语法

## 如何添加多级标题

    在md文件中可以使用#号表示1-6级标题，一级标题对应一个#号，二级标题对应两个#号

---

## 段落格式

    Markdown 段落没有特殊的格式，直接编写文字就好，段落的换行是使用两个以上空格加上回车。Markdown可以使用以下几种字体
    *斜体文本*
    _斜体文本_
    **粗体文本**
    __粗体文本__
    ***粗斜体文本***
    ___粗斜体文本___

---

## 添加分隔线的几种方式

    你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号—
    或是减号中间插入空格。下面每种写法都可以建立分隔线：
    ***
    * * *
    *****
    - - -
    --------

## 添加下划线

    <u>asdfasfasdfsdf</u>

## Makedown 列表

    无序列表通过*,+,-作为列表标记，这些标记后面都需要添加一个空格，然后在填写内容
    有序列表使用数字加上.号来表示，如
    1. 第一项
    2. 第二项
    3. 第三项

---

## 列表嵌套

    列表嵌套只需在子列表中的选项前面添加四个空格即可:
        1. 第一项：
            - 第一项嵌套的第一个元素
            - 第一项嵌套的第二个元素
        2. 第二项：
            - 第二项嵌套的第一个元素
            - 第二项嵌套的第二个元素

---

## Makedown 区块

1.  Markdown 区块引用是在段落开头使用 > 符号 ，然后后面紧跟一个空格符号;  
    区块的嵌套

    > 最外层
    >
    > > 第一层嵌套
    > >
    > > > 第二层嵌套

2.  列表中使用区块
    如果要在列表项目内放进区块，那么就需要在 > 前添加四个空格的缩进。
    区块中使用列表实例如下：
    > - 第一项
    >   > 菜鸟教程
    >   > 学的不仅是技术更是梦想
    > - 第二项

---

## Makedown 代码

如果是段落上的一个函数或片段的代码可以用反引号把它包起来（`），例如:

     `print()`函数

显示结果如下
`print()`函数

你也可以用` ``` `包裹一段代码，并指定一种语言:

>     ```javascript
>     $(document).ready(function () {
>      alert('RUNOOB');
>     });
>     ```

## Makedown 链接

链接使用方法如下:

>     【链接名称】(链接地址)
>
>      或者
>
>     <链接地址>
>
> 访问百度网址:<https://www.baidu.com> >

## Makedown 图片

Makedown 图片语法格式如下:
![alt 属性文本](图片地址)

{% asset_img image-20200720144654863.png hello %}
