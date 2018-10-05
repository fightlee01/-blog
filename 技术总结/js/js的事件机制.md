# js的事件机制
> 2018.8.11
## 前言
    以前没有注意这个问题，觉得很简单，现在觉得不是的，自己事件代理完全是理解错了，幸好自己发现了。
## 正文
### 一、标准的事件机制
EMCAScript定义的标准事件主要有三个阶段：1）事件捕获：从docment到目标Dom由上到下传播，期间如果注册了对应的监听事件都会触发；2）目标阶段；3）冒泡阶段：从目标Dom到document由下到上传播。注册事件时可以选择在捕获阶段触发还是冒泡阶段触发。

注册事件    
```javascript
target.addEventListener(type,listener,useCapture)
// target 对应dom节点
// type 事件种类
// listener 事件触发时调用的函数
// useCapture Boolen 表示注册的事件是在捕获阶段触发还是冒泡阶段，默认为default
```
移除事件
```javascript
target.removeEventListener(type,listener、userCapture)
// 注意：listener必须与注册时是同一个引用，不然不会移除成功。
```
![image](/img/09.jpg)
### 二、特殊的事件机制
IE比较特殊，它的事件机制只有两个阶段：1）处于目标阶段；2）冒泡阶段（万恶的IE）
![image](/img/10.jpg)
### 三、阻止事件传播
在实际运用中，有时只是为了让某一个节点触发事件，但是其父节点注册了对应的事件，那么在捕获/冒泡阶段他们也会触发，为了防止他们触发可以在触发函数中使用`event.stopPropagation()`来阻止传播。
### 三、事件代理
原理：在父节点注册事件，在冒泡阶段父节点触发时根据event.target来判断是哪个子节点触发的该事件，一般使用事件代理来对有动态节点进行事件监听。优点：维护一个父节点就可以对所以子节点进行监听。

    ps:为什么是动态节点：由于是动态生成的节点，是没有经历注册事件的阶段的，你也可以在生成的时候为他添加事件监听，这样消耗性能且代码冗余，有1000个节点动态生成你就要注册1000次事件监听，性能消耗严重，通过事件代理可以通过event.target来判断是哪个节点触发来完成想要做的后续逻辑。
## 参考文章
- [InterviewMap](https://yuchengkai.cn/docs/zh/frontend/browser.html#%E4%BA%8B%E4%BB%B6%E6%9C%BA%E5%88%B6)
- [浅谈前端和移动端的事件机制](https://juejin.im/post/59ede91ef265da43143fde87)

