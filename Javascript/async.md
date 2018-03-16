### async和 await 在干什么？
* 任意一个名称都是有意义的，先从字面意思来理解。async 是“异步”的简写，而 await 可以认为是 async wait 的简写。所以应该很好理解 async 用于申明一个 function 是异步的，而 await 用于等待一个异步方法执行完成。
* 另外还有一个很有意思的语法规定，await 只能出现在 async 函数中。然后细心的朋友会产生一个疑问，如果 await 只能出现在 async 函数中，那这个 async 函数应该怎么调用？
* 如果需要通过 await 来调用一个 async 函数，那这个调用的外面必须得再包一个 async 函数，然后……进入死循环，永无出头之日……
* 如果 async 函数不需要 await 来调用，那 async 到底起个啥作用？
### async 起什么作用？
 直接上代码
```javascript
	async function testAsync() {
      return "hello async";
    }

    const result = testAsync();
    console.log(result);
	//Promise{promiseValue:'hello async'}
```
看到输出就恍然大悟了——输出的是一个 Promise 对象。 
所以，async 函数返回的是一个 Promise 对象。从文档中也可以得到这个信息。async 函数（包含函数语句、函数表达式、Lambda表达式）会返回一个 Promise 对象，如果在函数中 return 一个直接量，async 会把这个直接量通过 Promise.resolve() 封装成 Promise 对象。