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
async 返回的是一个promise对象，所以在不用awati的情况下，我们就可以使用then（）来处理这个Promise 对象，如下
```javascript
	async function testAsync(){
    return "test jack"  
    }
 	const result = testAsync()
    result.then((res)=>{
		console.log(res)
    })
```
如果testAsync 函数没有返回值，那么res又是什么呢，很明显是，Promise.resolve(undefined)
联想一下promise的特点，无等待，立即执行，那么在没有awati的情况下，他会立即执行返回一个Promise对象，并且不会阻塞后面的语句，他和普通返回promise对象的函数并无二致，那么下面我们来看看awati关键字
### awati 到底在等什么？
* 一般认为await都在等待 async 函数的完成，所以可以用awati等待一个async函数的返回值----也可以理解为awati在等待一个async函数，不过按照语法说明，awati等待的是一个表达式，这个表达式的计算结果是一个Promise对象或者是其他值（换句话说就是没有特殊限定）
* 但是awati不仅仅等的是promise对象，他可以等任意表达式的结果，所以awati后面实际是可以接普通函数调用或者直接量
```javascript
	function getSth(){
    return "get something"
    }
	async function asyncGetSth(){
      return　"async get something"
    }

	async function test(){
    	const v1 = await getSth()
      const v2 = await asyncGetSth()
      console.log(v1,v2)
    }
	 test()
```