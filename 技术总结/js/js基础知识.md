# js基础知识小结
> 2018.8.8
## 前言
    以前写得东西，觉得很够用了，有些地方不足的后续会改上，都是一些简单的知识点，没事可以看一看。
## 正文
### 一、数据分类
- 简单数据类型：Number、Null、Undefined、String、Boolean
- 复杂数据类型：Object、Array、Function、Date、Error、RegExp
- 全局数据类：Math
### 二、各个数据类型typeof结果
- Number、NaN: “number”
- String: “string”
- Object、Null、Error、Date、RegExp、Math、Array: “obeject”
- Boolean: “boolean”
- Function: “function”
- Undefinde: ‘undefind’
### 三、非全等下判定为false的情况
Nan、null、undefined、false、””、0
### 四、String和 Array的常用方法
#### 1.String
- toUpperCase： 将字符串全部置换为大写返回新字符串，对原字符串无影响。
- toLowerCase：将字符串全部置换为小写 str.toLowerCase（）上同，
- indexOf: 检索出字符串的位置，如果没有就返回-1 str.indexOf(str1)
- substring: 复制指定位置的字符串，str.substring(start,end) 以end截止不包含end,str.substring(index1)从index1到字符串结尾,且不填写参数的时候（substring（））直接返回原字符串一样的新的字符串。
- substr: substr(start,len) 提取一个以start开始的len长度的字符串。
- concat： 连接两个或多个字符串，str.concat(str1,...)返回新字符串，对原字符串无影响。
- charAt: 返回指定位置的字符串，str.chartAt(index)
- split: 把字符串分割成字符数组返回新数组，对原字符串无影响。
- search: 匹配符合正则表达式的第一个字符串，返回其位置，如果没有就返回-1
- slice：和substring的该功能用法一直
#### 2.Array
- indexOf检索出数组中的元素，返回其位置，如果没有就返回-1，arr.indexOf(12)
- slice 截取出数组中指定位置返回出一个新数组，arr.slice(1,4),arr.slice(5)，返回新数组，对原数组无影响，用法string的slice一致。
- push和pop,向数组末尾添加和删除一个元素，arr.push(),arr.pop()
- unshift和shift，先数组头部添加或删除一个元素，arr.unshift()，arr.shift()
- sort，排序可以传入函数进行自定义排序逻辑，对原数组进行操作，不返回新数组。
- reverse，翻转整个数组，对原数组进行操作，不返回新数组。
- concat，连接两个或多个数组，arr.concat(arr1...) 返回新数组，对原数组无影响。
- join，将每个数组元素通过传入的指定字符串连接起来，返回一个字符串，arr.join()
- splice()修改数组的方法，能够通过参数进行参数说明，splice(a,b,c..)a指定删除元素开始位置,b指定删除个数,c..则为添加的元素且添加位置为删除的开始位置,返回新数组，对原数组有影响。
### 五、循环
- for(var item of arr/object)
- for(var item in arr/object)
- forEach(function(element,index,array))
### 六、DOM操作
#### 1.基本概念
Dom中所有节点都是继承自Node类型，都共享着相同的基本属性和方法，Node的类型可以根据nodeType来判断。
#### 2.常见的node类型和api
1）Element：表示Dom中的标签元素，常见的有：div、p、a等；
- nodeType为1；
- nodeName和tagName都返回标签名（大写形式）
- parentNode可能是Document或者Element
- nodeValue为null
子节点可能为Element、Text、Comment(注释)等

2）Text：表示文本节点。包含纯文本内容，不包含html代码，可以放回转义后的html代码；
- nodeType为3；
- nodeName为”#text”
- nodeValue为文本内容
- parentNode是一个Element
- 没有子节点
  
3）Attr：表示元素的属性，相当于元素的attributes属性的节点；
- nodeType为2
- nodeName是属性名称
- nodeValue是属性值
- parentNode为null
- 没有子节点
  
4）Document: 表示文档，document对象是HTMLDoucment的一个实例，表示整个页面，它同时也是window对象的一个属性;
- nodeType为11
- nodeName是”#doucment”
- nodeValue是null
- parentNode为null
- 子节点可能是一个DocumentType或者Element
#### 3.创建节点型api
1）createElement，创建一个Element元素，使用document.createElement("div")。

2）createTextNode，创建一个Text节点，使用document.createTextNode("一个TextNode");

3）cloneNode，复制一个节点，使用Element.cloneNode(true)，参数表示是否要是要复制节点中的子元素。注意：
- 复制的节点包含id，副本也会有相同的id，需要修改；
- 复制的节点绑定了事件，如果是一内联的发生绑定如：```<div onclick="showParent()"></div>```，则副本也会有触发事件，其他方式则不会；
  
- 在使用时最好传入参数，因为不同浏览器的默认参数不一致。
  
4）注意：上述方法创建的节点都处于游离态，需要添加到文档流中才能显示，需要使用appendChild或者insertBefore来添加。

#### 4.修改页面型api
1）appendChild：添加节点到该节点的子元素的末尾，使用parent.appendChild (child);注意：如果添加的节点是存在文档流的，则会以移动节点的形式添加而不是副本。

2）insertBefore：添加节点到该节点的前面，使用parentNode.insertBefore(添加节点, 参照节点);注意于上同。

3）removeChild：删除指定的节点，使用var deletedChild = parent.removeChild(node
)，注意删除的节点已经处于游离态，但还在内存中，可以进行操作。

4）replaceChild：用于替换另一个节点，使用parent.replaceChild (newChild, oldChild) ;

5）注意：
- 如果修改的节点存在于文档流中，则会以移动节点的形式添加而不是副本；
- 节点本身绑定的事件不会在移动或者添加过程中改变。

#### 5.检索类型api
1）getElementById：根据id查询文档流中的Dom元素，返回值是Element类型，如果不存在返回null，使用document.getElementById(id)。

2）getElementByTagName：根据标签名查询文档流中的Dom元素，返回值是及时（随着内容的变化一些属性也会随之变化）的HTMLCollection（类似数组）类型，如果不存在返回一个空的HTMLColloction。

3）getElementsByClassName:根据class出现Dom元素，返回值是及时的HTMLCollection类型，语法document.getElementsByClassName(names)，可以传入多个class,以空格分开。

4）querySelector：通过使用css选择器的语法进行检索，检索返回第一个符合条件的元素，检索的策略是按照深度优先来获取元素；如果不存在则放回null。

5）querySelectorAll：同上，不同则是检索结果返回，是一个非即时的NodeList，也就是说结果不会随着文档树的变化而变化。

#### 6.节点关系型api
1）parentNode和parentElement：都返回父节点,不同之处后一个返回必须是Element。

2）previousSibling：返回节点的前一个节点，如果自己是第一个节点则返回null，返回值可能是Text、Element、Commont等，注意判断nodeType；previousElementSilbing：返回前一个元素节点，且必须是Element。

3）nextSibling：返回后一个节点，如果自己是最后一个节点则返回nul，返回值能是Text、Element、Commont等，注意判断nodeType；nextElementSibling：返回后一个元素节点，且必须是Element。

4）childNodes：返回一个即时的NodeList，内容可能会是Text、Element、Commont等，注意判断nodeType；

5）children返回一个即时HTMLCollection，内容必须是Element类型。

6）firstNode：第一个节点；lastNode：最后一个节点；hasChildNodes：判断是否包含子节点。

#### 7.节点属性api
1）setAttribute: 语法element.setAttribute(name,value);

2）getAttribute：语法element.getAttribute(name);
## 参考文章
- [javaScript教程——廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/0014344992519683bcfa2e33760462fb5db8eb9430924be000)
- [Javascript操作DOM常用API总结](http://luopq.com/2015/11/30/javascript-dom/)


