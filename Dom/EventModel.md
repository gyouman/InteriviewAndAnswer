### DOM 事件模型？
* DOM全称是Document Object Model，即文档对象模型
DOM事件模型分为两类：
 - 一类是IE所使用的冒泡型事件（Bubbling）；
 - 另一类是DOM标准定义的冒泡型与捕获型（Capture）的事件。除IE外的其他浏览器都支持标准的DOM事件处理模型。
 ## 冒泡型事件处理模型（Bubbling）

　　冒泡型事件处理模型在事件发生时，首先在最精确的元素上触发，然后向上传播，直到根节点。反映到DOM树上就是事件从叶子节点传播到根节点。


## 捕获型事件处理模型（Captrue）

　　相反地，捕获型在事件发生时首先在最顶级的元素上触发，传播到最低级的元素上。在DOM树上的表现就是由根节点传播到叶子节点。

### 标准的DOM事件处理模型

* 标准的事件处理模型分为三个阶段：

 - 父元素中所有的捕捉型事件（如果有）自上而下地执行
 - 目标元素的冒泡型事件（如果有）
 - 父元素中所有的冒泡型事件（如果有）自下而上地执行
 ```html
    <html xmlns="http://www.w3.org/1999/xhtml">
        <head>
            <title></title>
            <meta charset="utf-8" />
            <style type="text/css">
                #p { width: 300px; height: 300px; padding: 10px;  border: 1px solid black; }
                #c { width: 100px; height: 100px; border: 1px solid red; }
            </style>
        </head>
        <body>
        <div id="p">
            parent
            <div id="c">
                child
            </div>
        </div>
        <script type="text/javascript">
            var p = document.getElementById('p'),
                c = document.getElementById('c');
            c.addEventListener('click', function () {
                alert('子节点捕获')
            }, true);

            c.addEventListener('click', function (e) {
                alert('子节点冒泡')
            }, false);

            p.addEventListener('click', function () {
                alert('父节点捕获')
            }, true);

            p.addEventListener('click', function () {
                alert('父节点冒泡')
            }, false);
        </script>
        </body>
        </html>

 ```
 　这个依次会打出父节点捕获,子节点捕获,子节点冒泡和父节点冒泡,(注意：如果你在目标元素上改变绑定事件的顺序, 这些事件可能就不按照捕获和冒泡的顺序来了，而是根据捕获和冒泡的顺序进行触发 , 这个有解决方法,参考:) ==>>(叶小钗的东东)http://www.cnblogs.com/yexiaochai/p/3567597.html );

　　捕获的事件是按照顺序执行的, 而冒泡的事件在有的浏览器中的按照顺序执行有的按照相反的顺序执行; 
　　说实话啊，事件捕获没啥用处