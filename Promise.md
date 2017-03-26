# Promise对象

## Promise的由来
* Promise适用于解决异步编程，比传统的使用**``函数回调和事件``**作为解决方案，Promise更像是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。
* 从语法上说，``Promise`` 是一个对象，从它可以获取异步操作的消息。``Promise`` 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

## Promise的两个特点
* Promise对象的状态不受外界影响
	* Promise对象代表一个异步操作，有三种状态：``Pending（进行中）``、``Resolved（已完成，又称Fulfilled）``和``Rejected（已失败）``。
	* 只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。
* Promise的状态只会改变一次，然后就不会再改变了。
	* `` Promise``对象的状态改变，只有两种可能：从``Pending``变为``Resolved``和从``Pending``变为``Rejected``。
	* 只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。
	* 相对于从前会用的事件回调方式，若是不进行状态捕获，事件结束后便不再能够捕获到这个事件的结果状态。

## Promise的优缺点
### 优点
* 有了``Promise``对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，``Promise``对象提供统一的接口，使得控制异步操作更加容易。

### 缺点
* ``Promise``一旦新建它就会立即执行，无法中途取消。
* 如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
* 第三，当处于``Pending``状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

### 建议
* 如果某些事件不断地反复发生，一般来说，使用 stream 模式是比部署Promise更好的选择。


## Promise的基本用法
* 创建Promise对象<pre><code>var promise = new Promise(function(resolve, reject) {
  // ... some code
  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
</code></pre>


## 使用Promise模拟ajax请求

<pre><code>var getJSON = function(url) {
  var promise = new Promise(function(resolve, reject){
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();
    function handler() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
  });
  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
</code></pre>


## Promise 的嵌套
<pre><code>var p2 = new Promise(function (resolve, reject) {
  // ...
  resolve(p1);
})</code></pre>
* 上面代码中，p1和p2都是Promise的实例，但是p2的resolve方法将p1作为参数，即一个异步操作的结果是返回另一个异步操作。
* 这时p1的状态就会传递给p2，也就是说，p1的状态决定了p2的状态。
