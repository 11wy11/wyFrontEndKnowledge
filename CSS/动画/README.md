# CSS动画
 - 位置 - 平移
 - 方向 - 旋转
 - 大小 - 缩放
 - 透明度
 - 其他 - 线形变换


### 前端动画怎么做
 - animation过渡动画
 - transition过渡动画
 - JS原生控制DOM位置
 - canvas绘制动画


### transition 过渡动画
用来控制过渡的时间，使用过渡的属性，过渡效果曲线，过渡的延时

要求元素的状态必须有变化，即某CSS值变化时发生的动画

 - transition-property   
   - 规定设置过渡效果的 CSS 属性的名称。
 - transition-duration   
   - 规定完成过渡效果需要多少秒或毫秒。
 - transition-timing-function  	
   - 规定速度效果的速度曲线。
 - transition-delay      
   - 定义过渡效果何时开始


在chrome的调试窗口按下esc在下面出现一个框框，选择 animation就可以看到动画面板了
```css
div
{
  width:100px;
  height:100px;
  background:blue;
  transition:width 2s, background 1s;
}
div:hover
{
  background:orange;
  width:300px;
}
```

### transition-timing-function
 - ease  慢速开始，然后变快，然后慢速结束
 - ease-in 慢速开始
 - ease-out 慢结束
 - ease-in-out 
 - linear
 - cubic-bezier(a,b,c,d)

bezier曲线在线效果网址 [cubic-bezier.com](http://cubic-bezier.com)



### animation 关键帧动画
相当于多个补间动画组合到一起

与transition不同的是，他可以让元素自己动，而不要求某值的改变来触发动画

`animation: name duration timing-function delay iteration-count direction;`

 - animation-name
   - 规定需要绑定到选择器的 keyframe 名称。
 - animation-duration
   - 规定完成动画所花费的时间，以秒或毫秒计
 - animation-timing-function
   - 动画的速度曲线
 - animation-delay
   - 动画开始之前的延迟
 - animation-iteration-count
   - n | infinit
   - 动画应该播放的次数
 - animation-direction
   - normal | alternate
   - 是否应该轮流反向播放动画
 - animation-play-state
   - 可用于暂停动画
 - animation-fill-mode 
   - forwards 动画停了就保持最后的那个状态
   - backwards 动画停了还得反着做一遍回去
   - 在动画执行之前和之后如何给动画的目标应用样式。

```css
#one{
  width: 50px;
  height: 50px;
  background-color: orange;
  animation: run;
  animation-delay: 0.5s;
  animation-duration: 2s;
  animation-fill-mode: forwards;
}
@keyframes run {
  0%{
    width: 100px;
  }
  50%{
    width: 400px;
    background-color: blue;
  }
  100% {
    width: 800px;
  }
}
```


### 逐帧动画
关键帧之间是有补间的，会选一个效果过渡过去，而逐帧动画则是每个keyframe之间没有过渡，直接切换过去
参考[猎豹奔跑](./animal.html)
关键是使用下面这行CSS
`animation-timing-function: steps(1);`
这个step是指定关键帧之间需要有几个画面


### 过渡动画和关键帧动画的区别
 - 过渡动画需要有状态变化
 - 关键帧动画不需要状态变化
 - 关键帧动画能控制更精细


### CSS动画的性能
 - CSS动画不差
 - 部分情况下优于JS
 - JS可以做到更精细
 - 含高危属性，会让性能变差 (如box-shadow)

### CSS动画性能优化

**目前对提升移动端CSS3动画体验的主要方法有几点：**
**1.尽可能多的利用硬件能力，如使用3D变形来开启GPU加速**

```
-webkit-transform: translate3d(0, 0, 0);``-moz-transform: translate3d(0, 0, 0);``-ms-transform: translate3d(0, 0, 0);``transform: translate3d(0, 0, 0);
```

 一个元素通过translate3d右移500px的动画流畅度会明显优于使用left属性；

原因是因为：

  CSS动画属性会触发整个页面的重排relayout、重绘repaint、重组recomposite
  Paint通常是其中最花费性能的，尽可能避免使用触发paint的CSS动画属性，这也是为什么我们推荐在CSS动画中使用webkit-transform: translateX(3em)的方案代替使用left: 3em，因为left会额外触发layout与paint，而webkit-transform只触发整个页面composite（这也是为什么推荐在CSS动画中使用webkit-transform: translateX(500px)的方案代替使用left: 500px）；

如动画过程有闪烁（通常发生在动画开始的时候），可以尝试下面的Hack：

```
-webkit-backface-visibility: hidden;``-moz-backface-visibility: hidden;``-ms-backface-visibility: hidden;``backface-visibility: hidden;`` ` `-webkit-perspective: 1000;``-moz-perspective: 1000;``-ms-perspective: 1000;``perspective: 1000;
```

 **2.尽可能少的使用box-shadows与gradients**

box-shadows与gradients往往都是页面的性能杀手，尤其是在一个元素同时都使用了它们.

**3.尽可能的让动画元素不在文档流中，以减少重排**

```
position: fixed;``position: absolute;
```

现代浏览器在使用CSS3动画时，以下四种情形绘制的效率更高：

- 改变位置
- 改变大小
- 旋转
- 改变透明度

其实，对于每一帧动画，浏览器可能要做以下工作

1. 计算需要被加载到节点上的样式结果(样式重计算)
2. 为每个节点生成图形和位置(重排)
3. 将每个节点填充到图层中(重绘)
4. 组合图层到页面上(图层重组)

如果我们需要使得动画的性能提高，需要做的就是减少浏览器在动画运行时所需要做的工作。最好的情况是，改变的属性仅仅是图层的组合，变换（transform）和透明度（opacity）就属于这种情况

现代浏览器如Chrome，Firefox，Safari和Opera都对变换和透明度采用硬件加速，但IE10+不是很确定是否硬件加速

一些常用的改变时会触发重布局的属性：
 盒子模型相关属性会触发重布局：

- width
- height
- padding
- margin
- display
- border-width
- border
- min-height

定位属性及浮动也会触发重布局：

- top
- bottom
- left
- right
- position
- float
- clear

改变节点内部文字结构也会触发重布局：

- text-align
- overflow-y
- font-weight
- overflow
- font-family
- line-height
- vertival-align
- white-space
- font-size

这么多常用属性都会触发重布局，可以看到，他们的特点就是可能修改整个节点的大小或位置，所以会触发重布局

触发重绘的属性
 修改时只触发重绘的属性有：

- color
- border-style
- border-radius
- visibility
- text-decoration
- background
- background-image
- background-position
- background-repeat
- background-size
- outline-color
- outline
- outline-style
- outline-width
- box-shadow

这样可以看到，这些属性都不会修改节点的大小和位置，自然不会触发重布局，但是节点内部的渲染效果进行了改变，所以只需要重绘就可以了

结论：

动画给予了页面丰富的视觉体验。我们应该尽力避免使用会触发重布局和重绘的属性，以免失帧。最好提前申明动画，这样能让浏览器提前对动画进行优化。由于GPU的参与，现在用来做动画的最好属性是如下几个：

- opacity
- translate
- rotate
- scale