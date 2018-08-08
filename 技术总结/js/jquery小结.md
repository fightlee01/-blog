# jQuery的基础知识和主要的api
> 2018.8.8
## 前言 
    没有什么要说的。。。还是说点把，前段时间github将jQuery从前端全部移除，而是采用原生实现前端（并没有使用当前热门的框架），又掀起了对大家对jQuery的缅怀，我自己说实话没怎么用过jQuery，在学习前端的路线上人云亦云的学的，但是我觉得是该学的，以后的也是，不管前端框架如何改变，只要原生js不变，jQuery还是必须要学的。
## 正文
### 一、选择器
#### 1.基础语法：
$(cssSelector)、$(divDom)，选择器返回的结果是一个类似数组的jQuery对象，如果选择器没有匹配到DOM元素,则返回一个[]。
#### 2.与原生DOM的转换：
var divDom = &.get(index),通过get选择出数组中需要的Dom元素。
#### 3.过滤器（css3的伪类）:
first-child、:last-child、:nth-child(n)选出第n个元素、:nth-child(even)选出序号为偶数的序号、:nth-child(odd)选出序号为奇数的元素。
#### 4.和一些其他伪类选择器。。。
#### 5.find()
find(cssSelector)，从jQuery对象中继续查找需要的DOM元素。
#### 6.parent()
parent()，返回juqery对象的父级Dom元素，还是个jQuery对象。注意也可传入参数判断父级元素是否符合条件。
#### 7.next()和prev
next(cssSelector)和prev(cssSelector)，在jQuery对象的同级进行查找。
### 二、Dom操作
#### 1.text()和html()
text()和html()，获取Dom元素的文本和html原生内容的文本，使用：不传入参数时是获取内容，传入参数是设置内容。
#### 2.统一操作
一个jQuery对象可以包含多个Dom对象，在对jQuery对象进行内容/属性修改或者添加/删除监听器的时候会对所有的Dom元素进行修改。
#### 3.修改css
修改css，使用方法css(‘name’,’value’),或者css({‘name’:’value’})
#### 4.class操作
Class操作，addClass()、removeClass()、hasClass()。
#### 5.show()和hide()
显示隐藏Dom，show()和hide()
#### 6.获取DOM信息
获取Dom信息，wdth()和height()可以获取或设置Dom元素的宽高，attr(‘name’)和attr(‘name’,’value’)获取或者设置Dom节点的属性，removeAttr(‘name’)。
#### 7.表单操作
表单操作，获取或者设置表单元素的值统一使用val()。
#### 8.添加Dom：
- html()：直接暴力的传入hmtl字符进行修改。
- append()：类似appendChild，参数可以是Dom对象、jQuery对象、HTML字符
- Prepend()：类似insertBefore，参数同上
#### 9.删除Dom：
remove()，删除jQuery对象中所有的Dom节点
### 三、事件
#### 1.on()
为jQuery中的Dom节点添加监听器，参数为监听事件和处理函数。
#### 2.click()
为jQuery中的Dom节点添加点击事件，参数为处理函数，为on()方法的简写形式。
### 3.事件汇总
事件名称 | 触发时机
--- | ---
click | 鼠标点击时触发
dblclick | 鼠标双击时触发
mouseenter | 鼠标加入时触发
mouserleave | 鼠标移出时触发
mousemove | 鼠标移动时触发
hover | 鼠标进入和退出是触发两个函数，相当于mouseenter和mouseleave
keydown | 键盘按下时触发
keyup | 键盘松开时触发
keypress | 键盘按一次后触发
focus | Dom元素获得焦点时触发
blur | Dom元素失去焦点时触发
change | 当`<input>`、`<select>`和`<textarea>`内容变化是触发
submit | 当<form>提交时触发
ready | 当页面被载入并且Dom树完成初始化后触发，且该事件仅属于document对象并只被触发一次。

注意：在Dom树还没初始化的时候，代码中的选择器不会比配到Dom元素，则后面的工作也没有完成，因此一般都在ready事件里进行初始化代码，其有个简化显示：
$(function(){})
#### 4.取消绑定：
off(事件名称，函数名称)，注意函数绑定时如果是匿名函数，则只能通过off（事件名称）进行取消绑定（它会把所有绑定到改时间的函数取消绑定）。
### 四、Ajax
#### 1.
ajax(url,setting)：seting对象包含有：asyn（是否异步）、method、contentType（发送post的数据格式，默认值为'application/x-www-form-urlencoded; charset=UTF-8'）、data（数据，如果是get则以query的形式添加到ulr中，如果是post则根据contentType来进行序列化）、headers（额外的请求头必须是对象），dataType（接受数据格式）
#### 2.
Get(url，setting)和post(url，setting)
#### 五、自定义插件
基本语法：`$.fn.function()`，在function中定义自己的插件，在函数内部使用this来表示要操作的jQuery对象，注意：在函数末尾return this，以便实现链式操作，还可以在`$.fn.function.defaults` 对象上设置默认参数，在没有参数传入时可以使用默认参数。
### 六、参考文章
- [javaScript教程——廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/0014344992519683bcfa2e33760462fb5db8eb9430924be000)