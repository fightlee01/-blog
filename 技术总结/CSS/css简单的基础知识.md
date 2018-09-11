# css简单的基础知识
> 2018.8.12
## 前言
    学到现在，接触最多的还是js，css用的都很少了，大家都在使用UI框架，能够要求写的css代码量少之又少，但是这些基础知识不能丢啊！
## 正文
### 一、基础
#### 1.不同display的区别
- block
    - 独占一行，可以设置宽高，默认情况下充满父级元素的宽度
    - 可以设置padding和margin
    - 常见的块级元素有: div、p、h1~h5、ul、ol、dl、li、table等
- inline
    - 不占一行，与其他行内或者inline-block元素在同一行，如果一行不够显示会换行(换行时会以内容换行，不会以整个元素换行)，不能设置宽高，默认宽度为内容宽度
    - 可以设置水平方向的padding-left、padding-right和margin-left、margin-right
    - 常见的行内元素有: span、a、img、input等
- inline-block
    - 不占一行，与其他行内或者inline-block元素在同一行，如果一行不够显示会换行（换行时以整个元素换行），可以设置宽高，默认宽度为内容宽度
    - 可以设置padding和margin
- none
    - 直接不渲染
- flex(见下面)
#### 2.animation

#### 3.transition

#### 4.flex布局

#### 5.float
    浮动：比较老的布局方法，但是还是理解一下，不然一问三不知
float属性可以将元素脱离文 档流，而脱离后的位置上下位置是在其元素的上一个元素的底部（正常情况下，挡住后面的文档流），左右位置是根据float的属性值决定，若果后面跟的元素也设置了浮动且属性值一样，则会按照float设置的顺序排列，如果一行不够显示则会换行显示。

正常情况下

![image](/img/float.jpg)

B元素设置float: left，遮挡了后续的文档流中的C元素

![image](/img/float1.jpg)

B元素设置float: right

![image](/img/flaot2.jpg)

为了避免上面设置了浮动后会出现遮挡的情况发生，可以通过设置clear来清除后续浮动带来的影响，注意：设置清除浮动时只会影响到自身，如上面图二要清除浮动带来的效果应该设置C元素clear: left。

clear的属性值
- left
- right
- both
- none
#### 6.em和rem
- em: 相对长度单位，基于父级元素的字体大小计算，如果父级没有设置字体大小，则以浏览器默认字体大小计算
- rem: 相对长度单位，基于页面（<htm>）根元素的字体大小进行计算。
#### 7.position的不同区别
- static: 无定位，正常显示在文档流中
- relative: 先对定位，不脱离文档流，基于原本自身位置定位
- absolute: 绝对定位，脱离文档流，基于第一个postion为非static的祖先进行定位
- inherit: 继承
- fixed: 绝对定位，脱离文档流，基于浏览器窗口进行定位
#### 8.几种常用的居中写法

#### 9.关于盒模型
盒模型主要分为标准盒模型和IE盒模型（万恶的IE）
盒模型基本内容为
- content: 内容
- padding: 内边距
- border: 边框
- margin: 外边距
  
标准盒模型：width = content（默认的box-sizing：content-box),总宽度 = padding + margin + border + width

IE盒模型: width = content + padding + borde（这个现在可以通过设置box-sizing: border-box实现),总宽度  = width + margin

![image](/img/17.jpg)
### 二、css渲染的步骤
如图可以看出，首先浏览器通过获取的到的HTML解析成DOM，然后将CSS解析成CSSOM(CSS Object Model),然后将DOM和CSSOM结合生产Render Tree(渲染树)，接着通过Layout步骤，依照盒子模型，计算出每个节点在屏幕中的位置及尺寸，最后通过Paint,按照算出来的规则，通过显卡，把内容画到屏幕上。
![image](/img/16.jpg)
### 三、回流和重绘

### 四、渐进增强和优雅降级
### 参考文章