### 事件委托是什么？
* 什么是事件委托：通俗的讲，事件就是onclick，onmouseover，onmouseout，等就是事件，委托呢，就是让别人来做，这个事件本来是加在某些元素上的，然而你却加到别人身上来做，完成这个事件。

也就是：利用冒泡的原理，把事件加到父级上，触发执行效果。
## 好处是？
#  1: 提高性能
```javascript
    // 我们可以看一个例子：需要触发每个li来改变他们的背景颜色。
    <ul id="ul">
        <li>aaaaaaaa</li>
        <li>bbbbbbbb</li>
        <li>cccccccc</li>
    </ul>
    window.onload = function(){
        var oul = document.getElementById('ul')
        var aLi = oul.getElementByTagName('li')
        for(var i=0;i<aLi.length;i++){
            aLi.onmouseover = function(){
                this.style.background = 'red'
            }
            aLi.onmouseout = function(){
                this.style.background  = ''
            }
        }
    }
```
这样我们就可以为li 上面添加上鼠标事件
但是如果说我们可能有很多个li用for循环的话就比较影响性能。而且如果通过 js 动态添加进去的 li，是不会绑定鼠标事件的
下面我们可以用事件委托的方式来实现这样的效果。html不变
```javascript
    window.onload = function(){
        var oUl = document.getElementById('ul')
        var aLi = oUl.getElementByTagName('li')
        /*
            这里要用到事件源：event 对象，事件源，不管在哪个事件中，只要你操作的那个元素就是事件源。
            ie：window.event.srcElement
            标准下:event.target
            nodeName:找到元素的标签名
        */
        oUl.onmouseover = function(ev){
            var ev = ev || window.event
            var target = ev.target || ev.srcElement;
            //alert(target)
            if(target.nodeName.toLowerCase()=='li'){
                target.style.background = 'red'
            }
        }
        oUl.onmouseout = function(ev){
            var ev = ev || window.event
            var target = ev.target || ev.srcElement;
            //alert(target)
            if(target.nodeName.toLowerCase()=='li'){
                target.style.background = ''
            }
        }
    }
```
# 2:新添加的元素还会有之前的事件

###  阻止默认事件
```javascript
    var $a = document.getElementsByTagName("a")[0];
    $a.onclick = function(e){
        alert("跳转动作被我阻止了")
        e.preventDefault();
        //return false;//也可以
    }
```
默认事件没有了。

既然return false 和 e.preventDefault()都是一样的效果，那它们有区别吗？当然有。

仅仅是在HTML事件属性 和 DOM0级事件处理方法中 才能通过返回 return false 的形式组织事件宿主的默认行为。

###  组织冒泡事件：

```javascript
    function stopBubble(e){
        if(e&&e.stopPropagation){//非IE
            e.stopPropagation();
        }else{//IE
            window.event.cancelBubble=true;
        }
    } 
```
