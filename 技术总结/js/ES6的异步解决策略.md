# js的异步解决方案
> 2018.8.14
## 前言
    现在异步方案已经趋于成熟，现在较多使用的应该是Promise和Async了，做个小小的中介（其实是有很多细节都忘了，现在再去补补）
## 正文

### 一、Promise

Promise是一种比较特殊的异步解决方案，首先一个Promise对象表示一个异步操作，通过三个状态表示异步操作的状态：pending(进行中)、fulfilled（已成功）、rejected（以失败），故promise只有两种情况或者结果：1）pending-->fulfilled(表示异步操作成功)；2）pending-->rejected（表示异步操作失败）。Promise对象一创建就立即执行了，然后Promise对象执行完成后，状态就凝固了，所以Promise的状态是不可逆的且不可改变的。

语法： Promise对象提供两个方法resolve和reject,一是用来在异步操作结束的时候转换Promise的状态，二是用来将数据传递出去给后面的then或者catch使用。故必须使用这两个方法，不然Promise状态无法转换，异步操作就无法执行。

    const promise = new Promise(function(resolve, reject) {
    // ... some code

    if (/* 异步操作成功 */){
        resolve(value);
    } else {
        reject(error);
    }
    });

.then(resolve,reject)方法可以在Promise对象状态凝固以后触发，resolve表示状态成功，reject表示状态失败，这两个函数需要自己定义（只有一个会触发，因为状态只可能是一个），两个函数都提供一个参数，表示在从异步操作传递过来的数据。由于then考虑到then返回的可能会是一个promise对象，所以支持链式调用。

    getJSON("/posts.json").then(function(json) {
    return json.post;
    }).then(function(post) {
    // ...
    });
.catch()是.then(null,reject)的别名，所以一般then只处理成功的状态，catch负责失败的状态。

    getJSON('/posts.json').then(function(posts) {
    // ...
    }).catch(function(error) {
    // 处理 getJSON 和 前一个回调函数运行时发生的错误
    console.log('发生错误！', error);
    });
### 二、Generator
Generator一个更加奇葩的异步结局方案，通过`*`来声明函数为Generator函数和然后使用`yield`关键来表明那些是`异步`操作，然后这个Generator函数不会立即执行而是返回一个遍历对象，通过调用next()方法执行内部的逻辑，执行next()会从开头或者上一个yield语句为开头开始执行，执行到yield或者return结束,会返回一个结果{value,done},value表示yield后面语句执行的结果，done为布尔值表示是否Generator函数执行完毕，只有遇到return才会是true；yield没有返回值（undefined），可以使用next(value)提供返回值，这个值是上一个yield的结果，所以第一个next()添加参数默认不会生效。

这里就用代码解释了，这个异步解决方案的核心是想将异步操作封装到一个`同步`函数里面，函数里面可以通过yiled来将整个函数的变成同步执行，只不过需要人为的调用next(),现在应该没有人用这个方案来解决异步操作了，毕竟实在太繁琐，操作逻辑也很怪；不过还是要了解，毕竟它为后面的Async铺路。

### 三 、Async/await

Async应该是现在最成熟也是最实用的异步解决方案了，它其实本质上讲就是Generator语法糖，封装了一下，让整个异常操作变的前所未有的那么简单；对比Generator讲一下优势：1）更好的语义，Async只有两个关键之：asyncf负责声明（这点和Generator的*一样）、await表示后面的语句是一个异步操作（对比yiled明显后更好的语义）；2）内置执行器：Async不需要手动的调用next()来完成函数的执行，其内部就是一个同步操作，遇到await后会先返回函数，执行函数后面的逻辑，等到该异步操作完成后就继续完成函数内部的语句，所以就函数内部而言它是个同步操作，而对函数外部而言它是个异步操作；3）返回值是一个Promise对象：通过then()方法来完成异步数据的获取，这里就利用了Promise的有点。

### 四、参考
- [ECMAScript 6 入门](http://es6.ruanyifeng.com/)
- [Generator函数语法解析](https://juejin.im/post/5a6db41351882573351a8d72)