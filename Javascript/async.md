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
	 // get something  async get something
```
### awati 等到Promise 对象之后，做了什么？
* await  等到了 一个 Promise 对象，或者其他值，然后呢？首先 await 是一个运算符，用于组成表达式，await表达式的
运算结果取决于它等的东西
* 如果 它等到的不是一个 Promise 对象，那么awati 表达式的运算结果就是它等到的东西
* 如果它等到的是一个 Promise 对象，await 就忙起来了，它会阻塞后面的代码，等着 Promise 对象resolve，然后得到resole 的值
作为 await 表达式的运算结果
- 看到上面的阻塞一词，心慌了吧……放心，这就是 await 必须用在 async 函数中的原因。async 函数调用不会造成阻塞，它内部所有的阻塞都被封装在一个 Promise 对象中异步执行。
### async/await 帮我做了什么？
* 现在举例，用 setTimeout 模拟耗时的异步操作，先来看看不用 async/await 会怎么写
```javascript
    function takeLongTime(){
        return new promise((resolve)=>{
            setTimeout(()=>resolve('long_time_value'),1000)
        })
    }
    takeLongTime().then((v)=>{
        console.log("got",v)
    })
```
* 如果改用 async /await，会是下面这样
```javascript
    function takeLongTime(){
        return new Promise((resolve)=>{
            setTimeout(()=>resolve('long_time_value'),1000)
        })
    }
    async function test(){
        const v = await takeLongTime()
        console.log(v)
    }
    test()
```
* 眼尖的同学已经发现 takeLongTime 没有申明为 async    ，实际上，takeLongTime 本上返回的就是一个 Promise 对象，加不加
async 结果都一样，如果没明白，请回过头再去看看上面的 async 起到什么作用
* 又一个疑问产生了，这两段代码，两种方式对异步调用的处理（实际就是对 Promise 对象的处理）差别并不明显，甚至使用 async/await 还需要多写一些代码，那它的优势到底在哪？
* async/awati 的优势在于处理 then 链
 - 单一的 Promise 链并不能发现 async/await 的优势，但是，如果需要处理多个Promise组成 then的时候，优势就能体现出来了
* 假设一个业务，分多个步奏完成，每个步骤都是异步的，而且依赖上一个步骤的结果，我们仍然用 setTimeout来模拟异步操作
```javascript
    /**
        传入参数 n,表示这函数的执行时间  毫秒
        执行结果是 n + 200，这个值将用于下一步
    
    */
    function takeLongTime(n){
        return new Promise((resolve)=>{
            setTimeout(()=>resolve(n+200),n)
        })
    }
    function step1(n){
        console.log(`steop1 with ${n}`)
        return takeLongTime(n)
    }
    function step2(n){
        console.log(`step2 with ${n}`)
        return takeLongTime(n)
    }
    function step3(n){
        console.log(`step3 with ${n}`)
        return takeLongTime(n)
    }
     /*现在用 Promise 方式来实现这三个步骤的处理*/
     function doIt(){
         console.time('doIt')
         const time1 = 300
         step(time1)
            .then(time2 => step2(time2))
            .then(time3 => step3(time3))
            .then(result=>{
                console.log(`result is ${result}`)
                console.timeEnd('doIt')
            })
     }
     doIt()
     /*
        输出结果 是 step3的参数 700 + 200  = 900
        doIt 顺序执行了3个步骤 一共 用了 300 + 500 + 700  = 1500ms
     */
```
* 如果用 async/await 
```javascript
    async function doIt(){
        console.time('doIt')
        const time1 = 300
        const time2  = await step1(time1)
        const time3 = await step2(time2)
        const result = await step3(time3)
        console.log(`result is ${result}`)
        console.timeEnd("doIt")
    }
    doIt()
```

