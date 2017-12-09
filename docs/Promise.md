
#### 1. 编写Promise代码

```js
function getURL(URL) {
    return new Promise(function (resolve, reject) {
        var req = new XMLHttpRequest();
        req.open('GET', URL, true);
        req.onload = function () {
            if (req.status === 200) {
                resolve(req.responseText);
            } else {
                reject(new Error(req.statusText));
            }
        };
        req.onerror = function () {
            reject(new Error(req.statusText));
        };
        req.send();
    });
}
// 运行示例
var URL = "http://httpbin.org/get";
getURL(URL).then(function onFulfilled(value){
    console.log(value);
}).catch(function onRejected(error){
    console.error(error);
});
```

#### 2. 静态方法**Promise.resolve(value)** 

- 可以认为是 new Promise() 方法的快捷方式。
    ```js
    比如 Promise.resolve(42); 可以认为是以下代码的语法糖。

    new Promise(function(resolve){
        resolve(42);
    });
    ```
    
- Promise.resolve 方法另一个作用就是将 thenable 对象转换为promise对象。

    这种将thenable对象转换为promise对象的机制要求thenable对象所拥有的 then 方法应该和Promise所拥有的 then 方法具有同样的功能和处理过程
    
    到底什么样的对象能算是thenable的呢，最简单的例子就是 jQuery.ajax()，它的返回值就是thenable的。

    ```js
    var promise = Promise.resolve($.ajax('/json/comment.json'));// => promise对象
    promise.then(function(value){
       console.log(value);
    });
    ```

#### 3. Promise.reject

```js
比如 Promise.reject(new Error("出错了")) 就是下面代码的语法糖形式。

new Promise(function(resolve,reject){
    reject(new Error("出错了"));
});
```
#### 4. Promise只能使用异步调用方式 

**不要对异步回调函数进行同步调用**

+  绝对不能对异步回调函数（即使在数据已经就绪）进行同步调用。

+ 如果对异步回调函数进行同步调用的话，处理顺序可能会与预期不符，可能带来意料之外的后果。

+ 对异步回调函数进行同步调用，还可能导致栈溢出或异常处理错乱等问题。

+ 如果想在将来某时刻调用异步回调函数的话，可以使用 setTimeout 等异步API。

将onReady 函数用Promise重写的话，代码如下面所示
```js
function onReadyPromise() {
    return new Promise(function (resolve, reject) {
        var readyState = document.readyState;
        if (readyState === 'interactive' || readyState === 'complete') {
            resolve();
        } else {
            window.addEventListener('DOMContentLoaded', resolve);
        }
    });
}
onReadyPromise().then(function () {
    console.log('DOM fully loaded and parsed');
});
console.log('==Starting==');
```

#### 5.  Promise#then 
- 不仅仅是注册一个回调函数那么简单，它还会将回调函数的返回值进行变换，创建并返回一个promise对象。

#### 6.  Promise#catch  *[IE8的兼容问题](http://liubin.org/promises-book/#ch2-promise-reject)*
- 只是 promise.then(undefined, onRejected); 方法的一个别名而已。 也就是说，这个方法用来注册当promise对象状态变为Rejected时的回调函数

#### 7. Promise.all

```js
Promise.all([request.comment(), request.people()]);

```
request.comment() 和 request.people() 会同时开始执行，而且每个promise的结果（resolve或reject时传递的参数值），和传递给 Promise.all 的promise数组的顺序是一致的。

 Promise.all 的promise并不是一个个的顺序执行的，而是同时开始、并行执行的。

#### 8. Promise.race

Promise.all 在接收到的所有的对象promise都变为 FulFilled 或者 Rejected 状态之后才会继续进行后面的处理， 与之相对的是 Promise.race 只要有一个promise对象进入 FulFilled 或者 Rejected 状态的话，就会继续进行后面的处理。

 Promise.race 在第一个promise对象变为Fulfilled之后，并不会取消其他promise对象的执行。


**参考链接**
[promises-book](http://liubin.org/promises-book/)










