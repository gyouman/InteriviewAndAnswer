### 什么是BFC
* BFC(Block Formatting Context)是Web页面中盒模型布局的CSS渲染模式。它的定位体系属于常规文档流

- 浮动，绝对定位元素，inline-blocks, table-cells, table-captions,和overflow的值不为visible的元素，（除了这个值已经被传到了视口的时候）将创建一个新的块级格式化上下文。著作权归作者所有。
### 创建一个BFC
一个BFC可以被显式的触发。如果想要创建一个新的BFC，只需要给它添加上面提到的任何一个CSS样式就可以了。

例如，请看下面的HTML：
```html
    <style>
        div.container { overflow: hidden; }
    </style>
    <div class="container"> Some Content here </div>
```
