# VUE小结
>2018.7.31

## 前言
前段时间一直在写React，边学边写，发现并没有想象中的那么难，还是要克服自己的心理障碍；这人学什么忘什么，Vue快要被我丢完了，不是要写Vue的的任务，我可能又要忘光，趁机写一个小结做一下复习。

## 正文
### 一、Vue生命周期

#### 1.概览
整个生命周期可以分为三个部分：初始化-->更新-->消亡，而最重要的是初始化和更新。初始化过程主要分三个阶段：1）初始化：在beforeCreate之前完成生命周期和事件的初始化，在created之前完成数据的挂载和事件的绑定，在created中就可以对数据（props和data）进行处理，一般在这个时候初始化需要的数据；2）编译虚拟DOM：Vue将模板编译为虚拟DOM，这个时候beforeMount钩子触发；3）渲染DOM：将虚拟DOM放入render()中渲染出真实的DOM，此时mounted可以操作DOM。
更新主要涉及diff，根据Vue的数据（data、props）变更，会进行变更，在数据变更之前会触发beforeUpdate，然后进行diff，得到需要重新渲染的部分，最后对Virtual DOM进行渲染,完成更新，之后updated钩子触发，这时可以操作新的DOM。
#### 2.父子组件间的生命周期
（记得面试的时候吃过亏，现在又忘了，哎！这人）一句话总结就是先有父后有子，血脉相连。
- 初始化时候父子组件钩子的触发顺序：beforeCreate父-->created父-->beforeMount父-->beforeCreate子-->created子-->beforeMount子-->Mounted子-->Mounted父
- 子组件更新：beforeUpdate父-->beforeUpdate子-->updated子-->updated父
- 父组件更新：beforeUpdate父-->updated父



### 二、 虚拟DOM
react和vue都要虚拟DOM，这里只说vue的，关键部分：virtual Dom是真实DOM的一种抽象，就是通过一个对象里面的字段来描述真实的DOM对象，比如：tag代表标签名（String）、children代表子节点（Array）、data代表一些属性（Object）等；主要目的：原生的api产生和操作Dom的虚拟消耗太大，通过虚拟DOM来完成对真实DOM进行抽象，在虚拟DOM上完成对DOM的一系列操作（增、删、改）和后面的diff算法，最后再对虚拟DOM进行渲染，这样可以大大对原生api的依赖，一句话就是优化性能。

### 三、diff细节
#### 1.概述
(看了半天有点晕，把理解好的写下来)Vue的diff和React的diff算法大致思想一致，基本思想：1）同级比较：只有同一层级才进行比较；2）组件比较：如果是不同组件，由于组件间dom结构差异很大，直接替换的性能基本大于优化dom结构的性能，故直接替换不进行继续比较；3）子节点比较：React通过子节点的key值来进行优化，vue则要复杂很多（后面详解），当然也有key来判断是否是相同的节点，来完成性能优化。

#### 2.详细过程
##### 2.1 patch 过程
核心部分在**patch**函数中进行，首先判断oldVnode和Vnode是否是相同的节点，如果不是则直接渲染出Vnode的DOM,并插入到oldVnode的真实DOM的父节点中；如果是则调用**patchVnode**进行进一步比较；首先执行Vnode.el=oldVnode.el,将oldVnode渲染出的真实DOM赋值给Vnode的el（el最开始的时候为null） 首先比较oldVnode和Vnode是否是同一个引用，如果是则不需要更改，如果不是则对其子节点进行比较，先判断两者是否是文本节点，如果是则根据文本内容判断，如果两个内容不同则将Vnode.text替换到真实DOM上；如果不是则对其子节点进行比较，首先判断是否有子节点的情况：1）oldVnode没有子节点，Vnode有子节点，则直接添加子节点到DOM上；2）oldVnode有子节点，Vnode没有子节点，则直接在DOm上删除子节点；3）都有子节点且子节点不相等，则调用**updateChildren**进行子节点的进一步比较；4）其他情况，则DOM不变.

![image](/img/01.jpg)

##### 2.2 updateChildren 过程
这个过程的算法很巧妙，也很简单；首先会为oldVnode和Vnode的子节点上设置oldVSIdx、oldVEIdx和VSIdx、VEIdx，进行两两比较,一共比较四次，如果匹配上了，就对DOM节点进行相应的移动，然后对应的指针向中间靠，如果没有匹配上，则判断是否设置了key,如果有则oldVnode的子节点会生成hash，然后Vnode的VSIdx和VEIdx位置的key会进行匹配，这样就会最大化的利用到oldVnode的DOM,而不是去产生DOM；当其中的一对SIdx>EIdx时说明其中一个Vnode的子节点扫描完毕，不用进行比较了，后面的操作可想而知是查漏补缺的过程。

##### 2.3.总结
总体看下来，Vue的diff对于不同的虚拟DOM会不去比较直接用新的DOM替换旧的DOM。而对相同的虚拟DOM会尽量不去产生新的DOM，尽量利用原先的DOM结构，所以整个过程是在对DOM进行打补丁，边比较边打补丁。

#### 四、响应式的原理
(又看的我有点蒙蔽)vue的响应式的设计模式其实就是观察者；在开始的初始化阶段，通过Obeject.definePropoty()来对vue的数据对象（这里应该是data）的每个属性添加get和set的钩子，而每个属性都有一个dep负责依赖的管理，get钩子被调用的时候（执行render()的时候）会进行依赖收集，dep对使用了属性的视图进行watcher实例收集，然后在set调用的时候，dep会通知所有收集的watcher进行视图更新。（每个vue实例都有一个watcher实例，可以理解为一个一个视图）。
#### 五、nextTick()的原理
由于上面说的响应式的特性会有一个bug,就是每次数据一更新就会调用set然后会update，假如在一个密集的操作里（比如对一个for循环里操作数据），岂不是要更新很多次，这性能消耗非常严重，这个就是nextTick()要做的事情了，因为每次更新都有set-->dep-->watcher-->upDate过程，在wacher要执行更新的这一步骤做点更改，使用一个queue来暂时存储要执行upDate的watcher对象，在下一个tick的时候再讲queue取出同一upDate,而在一个tick的时间段内，一个vue实例数据的set多次被调用，这个vue实例的watcher对象只会被进队列一次，意思这里面有个过滤功能；因为在执行更新的时候，数据都已经是被更新过的，视图只需要更新一次就能满足这个需求，所以只需要将对应的watcher对象的update执行一次就行了。而这个tick时间，是通过setTimeout来实现的。

## 参考文章
- [详解vue的diff算法](https://juejin.im/post/5affd01551882542c83301da)
- [vue 父子组件的生命周期顺序](https://www.cnblogs.com/status404/p/8733629.html)
- [vue 生命周期 详解](https://www.cnblogs.com/happ0/p/8075562.html)
- [剖析 Vue.js 内部运行机制](https://juejin.im/book/5a36661851882538e2259c0f)