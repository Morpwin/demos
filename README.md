# demos

### 目录

1. <a href="#分页">分页</a>
2. <a href="#导航浮窗">导航浮窗</a>
3. <a href="#返回顶部">返回顶部</a>
4. <a href="#懒加载">懒加载</a>
5. <a href="#上拉加载">上拉加载</a>
6. <a href="#观察者模式">观察者模式</a>
7. <a href="#轮播图">轮播图</a>
8. <a href="#瀑布流">瀑布流</a>

### [分页](https://morpwin.github.io/demos/%E5%88%86%E9%A1%B5/)

#### 使用

    <script src="js/index.js"></script>
    
#### 示例

Html

    <div class="container">
        <div class="pagination"></div>
    </div>
    <div class="content">
        <p>1</p>
    </div>
    
Javascript
    
    let page = new Page(el, options, callback)
    page.init()

#### 参数

    el: html元素,如这里为document.querySelector(".pagination")
    //options默认为以下参数
    options: {
        pageSize: 2,  //每页显示数量
        pageNum: 1,  //页码
        allNum: 1,  //总页数
        selectValue: [1, 2, 5, 10, 15, 20],  //可选页码数，需要select为true才会生效
        select: true,  //是否有选择页码框
        first: true,  //是否有首页
        prev: true,  //是否有上一页
        next: true,  //是否有下一页
        last: true,  //是否有尾页
        jump: true  //是否有跳转指定页
    }
    callback: 跳转页码后触发回调函数，返回一个对象，里面有data

    let el = document.querySelector(".pagination")
    let content = document.querySelector(".content")
    let page = new Page(el, {}, function(res) {
        let insertContent = ""
        res.data.forEach((v, i) => {
            let str = `
                <p>${v.title}</p>
            `
            insertContent += str
        })
        content.innerHTML = insertContent

### [导航浮窗](https://morpwin.github.io/demos/%E5%AF%BC%E8%88%AA%E6%B5%AE%E7%AA%97/)

#### 使用

    <script src="js/index.js"></script>
    
#### 示例

Html

    <header>
        <div class="container">
            <div class="logo">LOGO</div>
            <ul>
                <li>首页</li>
                <li>A页</li>
                <li>B页</li>
                <li>B页</li>
                <li>D页</li>
            </ul>
            <div class="search">
                <input type="text">
            </div>
        </div>
    </header>
    
Javascript
    
    let scroll = new scrollFun(el, distance, time)
    scroll.init()

#### 参数

    el: html元素,如这里为document.querySelector("header")
    //options默认为以下参数
    distance: 导航条移动的距离，这个要根据个人的header来设置，默认60
    time: 淡入淡出动画时间，单位是秒

    let scroll = new scrollFun(document.querySelector("header"), 60, 0.5)
    scroll.init()
        
### [返回顶部](https://morpwin.github.io/demos/%E8%BF%94%E5%9B%9E%E9%A1%B6%E9%83%A8/)

#### 使用

    <script src="js/index.js"></script>
    
#### 示例

Html

    <div class="goTop">
        <img src="Top.jpg" alt="返回顶部">
    </div>
    
Javascript
    
    var scroll = new Scroll(el, options, callback)
    scroll.init()

#### 参数

    el: html元素,如这里为document.querySelector(".goTop")
    options: {
        top: 100,  //距离顶部100px显示，默认100
        fadeSpeed: 10,  //淡入淡出的速度，默认10
        speed: 10  //滚动到顶部的速度，默认10
    }
    callback: 触顶回调函数

    var scroll = new Scroll(document.querySelector(".goTop"), {top: 200, fadeSpeed: 20, speed: 20}, function() {
        console.log("到顶了")
    })
    scroll.init()
    
### [懒加载](https://morpwin.github.io/demos/%E6%87%92%E5%8A%A0%E8%BD%BD/)

这个懒加载使用了比较新的API **[IntersectionObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)**，用其代替传统的scroll事件


#### 使用

    <script src="js/index.js"></script>
    
#### 示例

Html

    <div class="img-box">
        <img src="./images/loading.gif" data-lazy-url="./images/1.jpg">
    </div>
    
src里为默认图片，data-lazy-url里为真正要加载的图片
    
Javascript

    let lazyLoad = new LazyLoad(el, options, callback)
    lazyLoad.init()
    
#### 参数

    el: html元素,如这里为document.querySelectorAll(".img-box")
    options为IntersectionObserver API所需的参数，这里列出几个，{
        threshold: [0, 0.25, 0.5, 0.75, 1] , //决定了什么时候触发回调函数,和图片交叉的比例
        root: document.querySelector('.container'), //指定目标元素所在的容器节点,不指定则为浏览器窗口
        rootMargin: "500px 0px"  //交叉区域的大小
    }
    callback: 加载完一张图片则会触发回调函数

    let lazyLoad = new LazyLoad(document.querySelectorAll(".img-box"), {}, function() {
        console.log("加载完了")
    })
    lazyLoad.init()
    
### [上拉加载](https://morpwin.github.io/demos/%E4%B8%8A%E6%8B%89%E5%8A%A0%E8%BD%BD/)

上拉加载和懒加载相同，使用了
**[IntersectionObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)** API

以最下面的loading为目标，当出现loading，则进行加载

#### 使用

    <script src="js/index.js"></script>

#### 示例

Html

    <article class="article">
        <p>There are moments in life when you miss someone so much that you just want to pick them from your dreams and hug them for real! Dream what you want to dream;go where you want to go;be what you want to be,because you have only one life and one chance to do all the things you want to do.</p>
    </article>
    <div class="loading">
        <img src="./images//loading.gif" alt="">
    </div>
    
Javascript

    let pullToLoad = new PullToLoad(container, target, options, callback) 
    pullToLoad.init()
    
#### 参数

    container: 容器元素，加载的元素会向这个容器添加
    target: 目标元素，使用container.insertBefore(item, target)插入元素
    options: IntersectionObserver API的参数
    callback: 加载完触发的回调函数
    
### 观察者模式

#### 使用

    <script src="js/index.js"></script>
    
Javascript

    let emitter = new EventEmitter()

    
#### addEvent

添加一个事件监听器，支持链式调用

    emmiter.addEvent(eventName, function)
    
- eventName 事件名称
- function 监听器函数
    
#### once

添加一个只触发一次的事件监听器，支持链式调用

    emmiter.once(eventName, function)
    
- eventName 事件名称
- function 监听器函数
- 
#### removeEvent

删除一个事件监听器，支持链式调用

    emmiter.removeEvent(eventName, function)
    
- eventName 事件名称
- function 要删除的事件，如果想全部删除则填null

#### emit

触发事件，支持链式调用

    emitter.emit(eventName, function, args)
    
- eventName 事件名称
- function 要触发的事件，如果想全部触发则填null
- args 参数
    
#### 示例

    let emitter = new EventEmitter()
    function fun1(arg) {
        console.log(`fun1 = ${123 + arg}`)
    }
    function fun2(arg) {
        console.log(`fun2 = ${456 + arg}`)
    }
    function fun3(arg) {
        console.log(`fun3 = ${789 + arg}`)
    }
    emitter.addEvent("add", fun1)
    emitter.addEvent("add", fun2)
    emitter.once("add", fun3)
    emitter.emit("add", null , 123)
        
### [轮播图](https://morpwin.github.io/demos/%E8%BD%AE%E6%92%AD%E5%9B%BE/)

### <a id="瀑布流">[瀑布流加载图片](https://morpwin.github.io/demos/%E7%80%91%E5%B8%83%E6%B5%81%E5%8A%A0%E8%BD%BD%E5%9B%BE%E7%89%87/)(这个图片较多，加载比较慢)</a>