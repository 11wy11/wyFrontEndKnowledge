### JavaScript基本原则

如何用好？

《JavaScript权威指南》

《JavaScript The Good Pass》

HTML：结构

CSS：表现

JavaScript：行为

#### **1.各司其职原则**

JavaScript直接操作了CSS属性

改进1：切换className，代码结构，简洁性，可维护性更好:

改进2：能否用HTML和CSS实现切换样式的功能:；修改HMTL：input CheckBox（display：none） 用label for绑定到input，但存在一定缺点（兄弟节点选择器，PC端要考虑兼容性，移动端可以使用）

#### **2.UI组件的设计思路**

引入：框架中没有的组件，如何扩展，如何维护

##### 以轮播图为例

思路：

- UI设计
  - 例子：轮播图
  - 列表，position：absolute
  - transition
- API设计
  - getSeletedItem
  - getSeletedItemIndex
  - slideTo
  - slideNext
  - slidePrevious

**BM命名方式**：组件太多层级，slider-list__item--seleted

面向对象的方法

**控制组件**

自定义事件 

querySelector

停止定时器，恢复定时器

slide

slideTo(){

new CustomEvent('slide',{buddles:true,detail})

this.container.dispatchEvent(event)

}

**基础版实现**

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title></title>
</head>
<style>
    #my-slider {
        position: relative; 
        width: 800px;
        height: 445px;     
    }

    .slide-list ul {
        list-style-type: none;
        position: relative;
        padding: 0;
        margin: 0;
        width: 100%;
        height: 100%;
        /* 父容器 */
    }

    .slide-list__item,
    .slide-list__item--selected {
        position: absolute;
        transition: opacity 1s;
        opacity: 0;
        text-align: center;
        left: 0;
        top: 0;
    }

    .slide-list__item--selected {
        opacity: 1;
    }

    .slide-list__item img,
    .slide-list__item--selected img {
        width: 800px;
        height: auto;
    }

    .slide-list__next,
    .slide-list__previous {
        display: inline-block;
        position: absolute;
        top: 50%;
        margin-top: -25px;
        width: 30px;
        height: 50px;
        text-align: center;
        font-size: 24px;
        line-height: 50px;
        overflow: hidden;
        border: none;
        background: transparent;
        color: white;
        background: rgba(0, 0, 0, 0.2);
        cursor: pointer;
        opacity: 0;
        transition: opacity .5s;
    }

    .slide-list:hover .slide-list__next,
    .slide-list:hover .slide-list__previous {
        opacity: 1;
    }

    .slide-list:hover .slide-list__previous {

        left: 0;
    }

    .slide-list:hover .slide-list__next {
        right: 0;
    }

    .slide-list__control {
        position: relative;
        display: table;
        background-color: rgba(255, 255, 255, 0.5);
        padding: 5px;
        border-radius: 12px;
        bottom: 30px;
        margin: auto;
    }

    .slide-list__control-buttons,
    .slide-list__control-buttons--selected {
        display: inline-block;
        width: 15px;
        height: 15px;
        border-radius: 50%;
        margin: 0 5px;
        background-color: white;
        cursor: pointer;
    }

    .slide-list__control-buttons--selected {
        background-color: red;
    }
</style>

<body>
    <div id="my-slider" class="slide-list">
        <ul>
            <li class="slide-list__item--selected"><img
                    src="https://uploadbeta.com/api/pictures/random/?key=BingEverydayWallpaperPicture&v=111" alt=""></li>
            <li class="slide-list__item"><img
                    src="https://uploadbeta.com/api/pictures/random/?key=BingEverydayWallpaperPicture&v=112" alt=""></li>
            <li class="slide-list__item"><img
                    src="https://uploadbeta.com/api/pictures/random/?key=BingEverydayWallpaperPicture&v=113" alt=""></li>
            <li class="slide-list__item"><img
                    src="https://uploadbeta.com/api/pictures/random/?key=BingEverydayWallpaperPicture&v=114" alt=""></li>
        </ul>
        <!-- controller 导航控制 -->
        <a class="slide-list__previous">&lt</a>
        <a class="slide-list__next">&gt</a>
        <div class="slide-list__control">
            <span class="slide-list__control-buttons--selected"></span>
            <span class="slide-list__control-buttons"></span>
            <span class="slide-list__control-buttons"></span>
            <span class="slide-list__control-buttons"></span>
        </div>
    </div>

</body>

</html>
<script src="http://code.jquery.com/jquery-latest.js"></script>
<script>
    // 随机获得一些图片
    // $.ajax({
    //     async: false,
    //     url: "http://shibe.online/api/shibes?count=4&urls=true&httpsUrls=true",
    //     type: "GET",
    //     dataType: 'jsonp',
    //     jsonp: 'jsoncallback',
    //     data: {},
    //     timeout: 5000,
    //     success: function (json) {
    //         const container = document.getElementById('my-slide');
    //         const items = this.container.querySelectorAll(".slide-list__item,.slide-list__item--selected");
    //         const res = JSON.parse(json);
    //         for (let i = 0, len = items.length; i < len; i++) {
    //             items[i].src = res[i]
    //         }
    //     },
    //     error: function (xhr) {
    //     }
    // });

    class Slide {
        constructor(id, cycle = 3000) {
            this.container = document.getElementById(id);
            this.items = this.container.querySelectorAll(".slide-list__item,.slide-list__item--selected");
            this.cycle = cycle

            const controller = this.container.querySelector(".slide-list__control");
            if (controller) {
                const buttons = this.container.querySelectorAll(".slide-list__control-buttons,.slide-list__control-buttons--selected")
                //通过自定义事件来处理导航圆点的操作

                controller.addEventListener('mouseover', evt => {
                    const idx = Array.from(buttons).indexOf(evt.target);
                    if (idx >= 0) {
                        this.slideTo(idx);
                        this.stop();//停止计时器，避免出现刚一切换，轮播就结束了
                    }
                })

                controller.addEventListener('mouseout', evt => {
                    this.start();
                })

                this.container.addEventListener('slide', evt => {
                    const idx = evt.detail.index
                    const selected = controller.querySelector('.slide-list__control-buttons--selected')
                    if (selected) {
                        selected.className = 'slide-list__control-buttons'
                    }
                    buttons[idx].className = 'slide-list__control-buttons--selected'
                })

                const previous = this.container.querySelector('.slide-list__previous')
                if (previous) {
                    previous.addEventListener('click', evt => {
                        this.stop();
                        this.slidePrevious();
                        this.start();
                        event.preventDefault();
                    });
                }

                const next = this.container.querySelector('.slide-list__next')
                if (next) {
                    next.addEventListener('click', evt => {
                        this.stop();
                        this.slideNext();
                        this.start();
                        event.preventDefault();
                    });
                }
            }

        }

        getSelectedItem() {
            const selected = this.container.querySelector('.slide-list__item--selected');
            return selected;
        }

        getSelectedItemIndex() {
            return Array.from(this.items).indexOf(this.getSelectedItem());
        }

        slideTo(idx) {
            const selected = this.getSelectedItem();
            if (selected) {
                selected.className = 'slide-list__item'
            }
            const item = this.items[idx]
            if (item) {
                item.className = 'slide-list__item--selected'
            }
            const detail = { index: idx }
            const event = new CustomEvent('slide', { bubbles: true, detail })
            this.container.dispatchEvent(event);
        }

        slideNext() {
            const currentIdx = this.getSelectedItemIndex();
            const nextIdx = (currentIdx + 1) % this.items.length
            this.slideTo(nextIdx);
        }

        slidePrevious() {
            const currentIdx = this.getSelectedItemIndex();
            const previousIdx = (this.items.length + currentIdx - 1) % this.items.length
            this.slideTo(previousIdx);
        }

        start() {
            this.stop();//如果忘记，会有问题
            this.interval = setInterval(() => {
                this.slideNext()
            }, this.cycle)
        }

        stop() {
            clearInterval(this.interval);
        }
    }
    const slide = new Slide('my-slider');
    slide.start();
</script>
```

**思考：构造函数很长，将导航控制按钮的逻辑与轮播图切换耦合在了一起。**

###### 改进1：插件机制，JS代码独立

```
  class Slide {
        constructor(id, cycle = 3000) {
            this.container = document.getElementById(id);
            this.items = this.container.querySelectorAll(".slide-list__item,.slide-list__item--selected");
            this.cycle = cycle
        }
        registerPlugins(...plugins) {
            plugins.forEach(plugin => {
                plugin(this)
            })
        }
        //省略...
        }
 function pluginController(slider) {
        //
        const controller =slider.container.querySelector(".slide-list__control");
        if (controller) {
            const buttons = slider.container.querySelectorAll(".slide-list__control-buttons,.slide-list__control-buttons--selected")
            //通过自定义事件来处理导航圆点的操作

            controller.addEventListener('mouseover', evt => {
                const idx = Array.from(buttons).indexOf(evt.target);
                if (idx >= 0) {
                    slider.slideTo(idx);
                    slider.stop();//停止计时器，避免出现刚一切换，轮播就结束了
                }
            })

            controller.addEventListener('mouseout', evt => {
                slider.start();
            })

            slider.container.addEventListener('slide', evt => {
                const idx = evt.detail.index
                const selected = controller.querySelector('.slide-list__control-buttons--selected')
                if (selected) {
                    selected.className = 'slide-list__control-buttons'
                }
                buttons[idx].className = 'slide-list__control-buttons--selected'
            })
        }

    }

    function pluginPrevious(slider) {
        const previous = slider.container.querySelector('.slide-list__previous')
        if (previous) {
            previous.addEventListener('click', evt => {
                slider.stop();
                slider.slidePrevious();
                slider.start();
                event.preventDefault();
            });
        }

    }

    function pluginNext(slider) {
        const next = slider.container.querySelector('.slide-list__next')
        if (next) {
            next.addEventListener('click', evt => {
                slider.stop();
                slider.slideNext();
                slider.start();
                event.preventDefault();
            });
        }
    }

    const slide = new Slide('my-slider');
    slide.registerPlugins(pluginController,pluginPrevious,pluginNext)
    slide.start();
```

依赖注入的插件方式：不是直接传类或者类对象，通过传参,实现依赖传递

插件功能和插件结构是分开的，如果去掉一个插件，插件结构还需额外维护

改进2：HMTL模板化，

构造函数中：添加render（）渲染模板

render，action

标准的组件封装：数据驱动，行为优化驱动，可维护性高

讨论：模板化

封装HMTL，封装CSS，封装JS

优化3，组件模型抽象，

可能不光只有图片，通用的Component，

UI组件的抽象思路

设计思路，抽象思路

UI组件框架

#### **3.局部细节控制原则**

点击只允许回调函数执行一次

block.addEventLister('click',()=>{},{once:true};

原生js实现：点击一次后block.onclick=null;

如果是异步处理数据，点击提交，行为只被执行一次。

##### High-ordered functions高阶函数

自身输入函数或返回函数，被称为高阶函数,例如：once、throttle、debounced,consumer,iterative

统一的抽象：过程抽象，一般考虑数据抽象

###### **once**

```
function once(fn){
    return function(...args){
        if(fn){
            let ret = fn.apply(this,args);
            fn=null;
            return ret;
        }
    }
}
//高阶函数
```

###### **节流**throttle

```js
function throttle(fn,time=500){

  let timer;

  return function(...args){

    if(timer==null){

      fn.apply(this,args);

     timer=setTimeout(()=>{

        timer=null;

      },time)

​    }

  }

}
```

应用场景：mousemove，scroll

###### **防抖**debouce

需要最终状态用debouce，可以避免重复提交

```
function debounce(fn,dur=100){
    var timer;
    return function(...args){
        clearTimeout(timer);
        timer=setTimeout(()=>{
            fn.apply(this,args);
        },dur)
    }
}
```

###### **消费者consumer**

同步执行操作变成异步操作，实现连击，新动作会记录在队列里，在之后的时间里逐渐执行。

```js
function consumer(fn,time){
    let tasks=[]
    let timer;
    return function(...args){
        tasks.push(fn.bind(this.args));
        if(timer==null){
            timer=setInterval(()=>{
                tasks.shift.call(this);
                if(tasks.length<=0){
                    clearInterval(timer);
                    timer=null;
                }
            },time)
        }
    }
}
```

###### **reduce**

```js
function add(x,y){
    return x+y;
}
function sub(x,y){
    return x-y;
}
console.log([1,2,3,4].reduce(add));
console.log([1,2,3,4].reduce(sub));
function addMany(...args){
    return args.reduce(add)
}
function subMany(...args){
    return args.reduce(sub)
}
console.log(addMany(1,2,3,4));
console.log(subMany(1,2,3,4));
//更通用的方式
function iterative(fn){
    return function(...args){
        return args.reduce(fn.bind(this))
    }
}
const addI=iterative((x,y)=>x+y)
const subI=iterative((x,y)=>x-y)
console.log(addI(1,2,3,4))
console.log(subI(1,2,3,4))
```

**toggle（imperative）**

// 声明式编程

```js
 switcher.onclick=function(evt){
  if(evt.target.className==='on'){
     evt.target.className='off'
   }else{
    evt.target.className='on' 
  }
 }
```

通用：toggle（declarative）

```js
function toggle(...actions){
  return function(...args){
   let action=actions.shift();
   actions.push(action);
    return action.apply(this.args);
  }
}
switcher.onclick=toggle(evt=>evt.target.className='off',evt=>evt.target.className="on") //指令式编程
```

当需要扩展其他状态时，不需要修改js功能函数的代码，只需要添加新的样式，扩展传入的参数即可，更为通用。

实现三态等

**使用生成器**

```js
function *loop(list,max=Infinity){
    let i=0;
    while(i<max){
        yield list[i++%list.length];
    }
}
function toggle(...actions){
    let action=loop(actions);
    return function(...args){
        return action.next().value.apply(this,args);
    }
}
```

### Declarative v.s. Imperative

声明式编程&指令式编程

How to do? v.s. What to do

（逻辑式，函数式）（面向对象，面向过程抽象的思路）

### 总结

1.遵循各司其职原则，JavaScript尽量只做状态管理

2.结构、API、控制流分离设计UI组件

3.插件和模块化，并抽象出组件模型

4.运用过程抽象的技巧来抽象并优化局部API

**todo: 根据视频思路自己实现一下轮播图，参照组件模块化的思想，尝试改造项目中的组件**
**编程范式进一步了解，声明式和指令式有点混淆**