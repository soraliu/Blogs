# javascript
## 内置函数
### `setTimeout(func, 0)`表示什么意思？
> 由于JavaScript是单线程处理任务的，而setTimeout是异步事件，延时时间为0的时候，JavaScript引擎会把异步事件立刻放到任务队列里，而不是立刻执行，需要等到前面处于等待状态的事件处理程序全部执行完成后再调用它。`[code](code/js/setTimeout.html)`

作用
* 拆解循环
    > 循环体中包含太多的操作和循环的次数过多都会导致循环执行时间过长，并直接导致锁死浏览器。如果循环之后没有其他操作，每次循环只处理一个数值，而且不依赖于上一次循环的结果则可以对循环进行拆解。`[code](code/js/setTimeoutRecycle.html)`

* 改变冒泡事件顺序
    > 让子元素的事件在父元素触发之后才能触发`[code](code/js/setTimeoutBubble.html)`

* 页面渲染动画(jQuery)
    > 浏览器中GUI渲染线程与JavaScript引擎是互斥的，所以当JavaScript执行时，浏览器就不会做任何的页面渲染。`[code](code/js/setTimeoutAnime.html)`
