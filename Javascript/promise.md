### Promise 怎么使用
``` javascript
    function runAsync(){
        var p = new Promise(function(resolve,reject){
            setTimeout(function(){
                console.log('执行完成')
                resove('随便什么数据')
            }，2000)
        })
        return p;
    }
    runAsync()
```
## 包装这个函数有什么用？
## resolve 是干嘛的？
 调用 runAsync 会 return Promise 对象，Promise 对象 有 all,resolve,reject,race,then、catch 等方法
 ```
    runAsync().then(function(data){
        console.log(data)
        // 后面可以用传过来的数据做其他操作
    })
 ```
 在 runAsync()的返回上直接调用 then 方法，then接受一个参数，是函数，并且我们会拿到我们在 runAsync中调用 resove时传递的参数，运行这段代码 2s 中后输出 『执行完成』，紧接着输出『随便什么数据』  
 all的 用法
 ```javascript
    runAsync1(){
        var p = new Promise(function(resolve,reject){
            setTimeout(function(){
                resolve('async1')
            },2000)
        })
        return p
    }
    runAsync1(){
        var p = new Promise(function(resolve,reject){
            setTimeout(function(){
                resolve('async1')
            },2000)
        })
        return p
    }
    runAsync2(){
        var p = new Promise(function(resolve,reject){
            setTimeout(function(){
                resolve('async2')
            },2000)
        })
        return p
    }
    runAsync3(){
        var p = new Promise(function(resolve,reject){
            setTimeout(function(){
                resolve('async3')
            },2000)
        })
        return p
    }
    Promise.all([runAsync1(),runAsync2(),runAsync3()]).then(function(results){
     console.log(results)
    })
 ```
 用Promise.all来执行，all接收一个数组参数，里面的值最终都算返回Promise对象。这样，三个异步操作的并行执行的，等到它们都执行完后才会进到then里面。那么，三个异步操作返回的数据哪里去了呢？都在then里面呢，all会把所有异步操作的结果放进一个数组中传给then，就是上面的results。所以上面代码的输出结果就是：
 ```javascript
    ['async1','async2','async3']

 ```
 Promise.race 的用法
 ```javascript
    //请求某个图片的资源
    function requestImg(){
        var p = new Promise(function(resolve,reject){
            var img = new Image()
            img.onload = function(){
                resolve(img)
            }
            img.src = 'xxx'
        })
        return p
    }
    // 延时函数，用于给请求记时
    function timeout(){
        var p = new Promise(function(resolve,reject){
            setTimeout(function(){
                reject('图片请求超时')
            },5000)
        })
        return p;
    }
    Promise.race([requestImg(),timeout()]).then(function(results){
        console.log(results)
    }).catch(function(reason){
        console.log(reason)
    })
 ```
 requestImg函数会异步请求一张图片，我把地址写为"xxxxxx"，所以肯定是无法成功请求到的。timeout函数是一个延时5秒的异步操作。我们把这两个返回Promise对象的函数放进race，于是他俩就会赛跑，如果5秒之内图片请求成功了，那么遍进入then方法，执行正常的流程。如果5秒钟图片还未成功返回，那么timeout就跑赢了，则进入catch，报出“图片请求超时”的信息。