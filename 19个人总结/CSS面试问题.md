# 1.实现效果

### 画0.5px的线

https://segmentfault.com/a/1190000013998884?utm_source=tag-newest

1.scaleY，但chrome会变虚

```
.hr.scale-half {
  height: 1px;
  transform: scaleY(0.5);
}
```

2.background-image

```
background: linear-gradient(0deg, #fff, #000);
```

3.SVG  1px=1物理像素相当于高清屏的0.5px

```css
background: url("data:image/svg+xml;utf-8,<svg xmlns='http://www.w3.org/2000/svg' width='100%' height='1px'><line x1='0' y1='0' x2='100%' y2='0' stroke='#000'></line></svg>");
```

firefox的background-image如果是svg的话只支持命名的颜色，如"black"、"red"等，如果把上面代码的svg里面的#000改成black的话就可以显示出来，但是这样就很不灵活了。否则只能把svg转成base64的形式，我们把svg的内容转成base64（可以找一些在线的工具）

IOS下的Safari和Chrome表现一致，都是以直接设置0.5px的效果最好，而安卓浏览器则是以box-shadow的效果最好（试了5和7），而svg的方案在IOS和安卓的设备上都能完美支持。读者可以打开这个网页，看一下在你的设备是哪种效果最好。

4.采用meta viewport的方式

```
<meta name="viewport" content="width=device-width,initial-sacle=1">
```

### 实现垂直居中

(1)margin:auto法

```
css:
div{
width: 400px;
height: 400px;
position: relative;
border: 1px solid #465468;
}
img{
position: absolute;
margin: auto;
top: 0;
left: 0;
right: 0;
bottom: 0;
}
html:
<div>
<img src="mm.jpg">
</div>
```

定位为上下左右为0，margin：0可以实现脱离文档流的居中.

(2)margin负值法

```
.container{
width: 500px;
height: 400px;
border: 2px solid #379;
position: relative;
}
.inner{
width: 480px;
height: 380px;
background-color: #746;
position: absolute;
top: 50%;
left: 50%;
```

margin-top: -190px; /*height的一半*/

margin-left: -240px; /*width的一半*/

}

补充：其实这里也可以将marin-top和margin-left负值替换成，
transform：translateX(-50%)和transform：translateY(-50%)

(3)table-cell（未脱离文档流的）

设置父元素的display:table-cell,并且vertical-align:middle，这样子元素可以实现垂直居中。

```
css:
div{
width: 300px;
height: 300px;
border: 3px solid #555;
display: table-cell;
vertical-align: middle;
text-align: center;
}
img{
vertical-align: middle;
}
```

(4)利用flex

将父元素设置为display:flex，并且设置align-items:center;justify-content:center;

```css
css:
.container{
width: 300px;
height: 200px;
border: 3px solid #546461;
display: -webkit-flex;
display: flex;
-webkit-align-items: center;
align-items: center;
-webkit-justify-content: center;
justify-content: center;
}
.inner{
border: 3px solid #458761;
padding: 20px;
}
```

### 多行元素的文本省略号

```
display``: -webkit-box
-webkit-box-orient:vertical
-webkit-line-clamp:``3
overflow``:``hidden
```

### 清除浮动

方法一：使用带clear属性的空元素

在浮动元素后使用一个空元素如<div class="clear"></div>，并在CSS中赋予.clear{clear:both;}属性即可清理浮动。亦可使用<br class="clear" />或<hr class="clear" />来进行清理。

方法二：使用CSS的overflow属性

给浮动元素的容器添加overflow:hidden;或overflow:auto;可以清除浮动，另外在 IE6 中还需要触发 hasLayout ，例如为父元素设置容器宽高或设置 zoom:1。

在添加overflow属性后，浮动元素又回到了容器层，把容器高度撑起，达到了清理浮动的效果。

方法三：给浮动的元素的容器添加浮动

给浮动元素的容器也添加上浮动属性即可清除内部浮动，但是这样会使其整体浮动，影响布局，不推荐使用。

方法四：使用邻接元素处理

什么都不做，给浮动元素后面的元素添加clear属性。

方法五：使用CSS的:after伪元素

结合:after 伪元素（注意这不是伪类，而是伪元素，代表一个元素之后最近的元素）和 IEhack ，可以完美兼容当前主流的各大浏览器，这里的 IEhack 指的是触发 hasLayout。

给浮动元素的容器添加一个clearfix的class，然后给这个class添加一个:after伪元素实现元素末尾添加一个看不见的块元素（Block element）清理浮动。

### 硬币翻转

<style type="text/css">
  .coin{
      width: 100px;
      height: 100px;
      border-radius: 100%;
      animation:spin 2.5s linear;
      position: relative;
      transform-style: preserve-3d;
      text-align: center;
      color:#fff;
  }
  .back{
      width: 100%;
      height: 100%;
      background-image: radial-gradient(rgb(136, 117, 151),rgb(28, 0, 128));
      border-radius: 100%;
      transform: translateZ(1px);
  }
  .front{
      width: 100%;
      height: 100%;
      background-image: radial-gradient(rgb(117, 144, 151),rgb(0, 128, 122));
      border-radius: 100%;
      position: absolute;
      left: 0;
      top: 0;
      transform: translateZ(1px);
  }
  .coin:hover{
     transition: rotate 2.5s linear;
  }
  .coin:hover .front{
      transform: rotateY(180deg);
  }
  .coin:hover .back{
      transform: rotateY(180deg);
  }
  @keyframes spin {
0% {
transform: rotateY(0deg);
}
100% {
transform: rotateY(360deg);
}
}
</style>
   <div class="coin">
       <div class="front">前</div>
       <div class="back">后</div>
   </div>
# 2.属性概念相关

#### 重排重绘



###  transition和animation的区别

Animation和transition大部分属性是相同的，他们都是随时间改变元素的属性值，他们的主要区别是transition需要触发一个事件才能改变属性，而animation不需要触发任何事件的情况下才会随时间改变属性值，并且transition为2帧，从from .... to，而animation可以一帧一帧的。

### BFC块级格式上下文

直译成：块级格式化上下文，是一个独立的渲染区域，并且有一定的布局规则。

BFC区域不会与float box重叠

BFC是页面上的一个独立容器，子元素不会影响到外面

计算BFC的高度时，浮动元素也会参与计算

那些元素会生成BFC：

根元素

float不为none的元素

position为fixed和absolute的元素

display为inline-block、table-cell、table-caption，flex，inline-flex的元素

overflow不为visible的元素

### position属性 比较

固定定位fixed：

元素的位置相对于浏览器窗口是固定位置，即使窗口是滚动的它也不会移动。Fixed定位使元素的位置与文档流无关，因此不占据空间。 Fixed定位的元素和其他元素重叠。

相对定位relative：

如果对一个元素进行相对定位，它将出现在它所在的位置上。然后，可以通过设置垂直或水平位置，让这个元素“相对于”它的起点进行移动。 在使用相对定位时，无论是否进行移动，元素仍然占据原来的空间。因此，移动元素会导致它覆盖其它框。

绝对定位absolute：

绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于<html>。 absolute 定位使元素的位置与文档流无关，因此不占据空间。 absolute 定位的元素和其他元素重叠。

粘性定位sticky：

元素先按照普通文档流定位，然后相对于该元素在流中的flow root（BFC）和 containing block（最近的块级祖先元素）定位。而后，元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。

默认定位Static：

默认值。没有定位，元素出现在正常的流中（忽略top, bottom, left, right 或者 z-index 声明）。

inherit:

规定应该从父元素继承position 属性的值。

# 3.布局相关

### Flex布局

Flex是Flexible Box的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。
布局的传统解决方案，基于盒状模型，依赖display属性 + position属性 + float属性。它对于那些特殊布局非常不方便，比如，垂直居中就不容易实现。

简单的分为容器属性和元素属性
容器的属性：

flex-direction：决定主轴的方向（即子item的排列方法）
.box {
flex-direction: row | row-reverse | column | column-reverse;
}

flex-wrap：决定换行规则
.box{
flex-wrap: nowrap | wrap | wrap-reverse;
}

flex-flow：
.box {
flex-flow: <flex-direction> || <flex-wrap>;
}

justify-content：对其方式，水平主轴对齐方式

align-items：对齐方式，竖直轴线方向

项目的属性（元素的属性）：

order属性：定义项目的排列顺序，顺序越小，排列越靠前，默认为0

flex-grow属性：定义项目的放大比例，即使存在空间，也不会放大

flex-shrink属性：定义了项目的缩小比例，当空间不足的情况下会等比例的缩小，如果定义个item的flow-shrink为0，则为不缩小

flex-basis属性：定义了在分配多余的空间，项目占据的空间。

flex：是flex-grow和flex-shrink、flex-basis的简写，默认值为0 1 auto。

align-self：允许单个项目与其他项目不一样的对齐方式，可以覆盖align-items，默认属性为auto，表示继承父元素的align-items

比如说，用flex实现圣杯布局

### 三栏布局的实现方式，尽可能多写，浮动布局时，三个div的生成顺序有没有影响

三列布局又分为两种，两列定宽一列自适应，以及两侧定宽中间自适应

#### 两列定宽一列自适应：

1、使用float+margin：

给div设置float：left，left的div添加属性margin-right：left和center的间隔px,right的div添加属性margin-left：left和center的宽度之和加上间隔

2、使用float+overflow：

给div设置float：left，再给right的div设置overflow:hidden。这样子两个盒子浮动，另一个盒子触发bfc达到自适应

3、使用position：

父级div设置position：relative，三个子级div设置position：absolute，这个要计算好盒子的宽度和间隔去设置位置，兼容性比较好，

4、使用table实现：

父级div设置display：table，设置border-spacing：10px//设置间距，取值随意,子级div设置display:table-cell，这种方法兼容性好，适用于高度宽度未知的情况，但是margin失效，设计间隔比较麻烦，

5、flex实现：

parent的div设置display：flex；left和center的div设置margin-right；然后right 的div设置flex：1；这样子right自适应，但是flex的兼容性不好

6、grid实现：

parent的div设置display：grid，设置grid-template-columns属性，固定第一列第二列宽度，第三列auto，

#### 两侧定宽中间自适应

对于两侧定宽中间自适应的布局，对于这种布局需要把center放在前面，可以采用双飞翼布局：圣杯布局，来实现，也可以使用上述方法中的grid，table，flex，position实现

1、双飞翼布局

<style >
 /* 双飞翼布局 */
   .method7{
       overflow: hidden;
       height: 150px;
   }
   .method7 .left {
        margin-right: 10px;
        margin-left: -100%;
        float: left;
    }
    .method7 .center {
       float: left;
       width: 100%;
    }
    .method7 .center .content {
       margin-left:110px;
       margin-right: 110px;
       background-color: antiquewhite;
    }
    .method7 .right{
        float: left;
        width: 100px;
        margin-left:-100px
    }
</style>
<div class="method7 method">
            <div class="center">
                <div class="content">
                    基本要求：中间部分的高度是三栏中最高的区域的高度。<br \>
                    先加载中间，三个div生成顺序center，left，right<br \>
                    left、center、right三种都设置左浮动
                    设置center宽度为100%<br \>
                    设置负边距，left设置负边距为100%，right设置负边距为自身宽度<br \>
                    设置content的margin值为左右两个侧栏留出空间，margin值大小为left和right宽度<br \></div>
            </div>
            <div class="left">method7</div>          
            <div class="right">method7</div>
        </div>

2.圣杯布局

【1】浮动

先定义好header和footer的样式，使之横向撑满。
在container中的三列设为浮动和相对定位(后面会用到)，center要放在最前面，footer清除浮动。
三列的左右两列分别定宽200px和150px，中间部分center设置100%撑满
这样因为浮动的关系，center会占据整个container，左右两块区域被挤下去了
接下来设置left的 margin-left: -100%;，让left回到上一行最左侧
但这会把center给遮住了，所以这时给外层的container设置 padding-left: 200px;padding-right: 150px;，给left和right空出位置
这时left并没有在最左侧，因为之前已经设置过相对定位，所以通过 left: -200px; 把left拉回最左侧
同样的，对于right区域，设置 margin-right: -150px; 把right拉回第一行
这时右侧空出了150px的空间，所以最后设置 right: -150px;把right区域拉到最右侧就行了。

上述grid,table,flex,position实现类似

# 4.综合对比

### 关于js动画和css3动画的差异性

渲染线程分为main thread和compositor thread，如果css动画只改变transform和opacity，这时整个CSS动画得以在compositor trhead完成（而js动画则会在main thread执行，然后出发compositor thread进行下一步操作），特别注意的是如果改变transform和opacity是不会layout或者paint的。

区别：

功能涵盖面，js比css大

实现/重构难度不一，CSS3比js更加简单，性能跳优方向固定

对帧速表现不好的低版本浏览器，css3可以做到自然降级

css动画有天然事件支持

css3有兼容性问题

###  双边距重叠问题（外边距折叠）

多个相邻（兄弟或者父子关系）普通流的块元素垂直方向marigin会重叠

折叠的结果为：

两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
两个外边距一正一负时，折叠结果是两者的相加的和。