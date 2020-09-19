## **二**、HTML和CSS

### HTML

什么是前端？使用Web技术栈解决多端的图形界面下人机交互问题。
前端应关注哪些方面？功能，美观，无障碍性，安全，性能，兼容性，体验
前端的边界：nodejs,electron,react Native,
webRTC,webGL,webAssembly
开发环境：IE/Edge,firfox,chrome,safari
vscode
HTML:HyperText Markup Language
HyperText超文本 图片视频表格等
Markup Language 标记语言 <h1></h1>

<!doctype html>渲染模式和HTML版本

HTML语法：

- 标签和属性不区分大小写，推荐小写，

- 可以不闭合，image />,

- 属性值用双引号包裹，

- 属性值有些可省略 

文本标签
引用：

- blockquote“长引用,
- cite：作品名等短引用，
- q：前文引用

强调：

- strong ：强调突出，重要严重紧急，

- em：重音，突出的词

内容划分：head nav main article aside(导航链接，推荐，广告等，不属于页面主要内容) footer 
**语义化**

HTML中元素，属性，属性值都拥有某些含义。
**谁在使用HTML及语义化好处**

- 开发者：修改。维护页面-代码可读性，可维护性
- 浏览器：渲染页面
- 搜素引擎：提供关键词，排序-搜素引擎优化

- 屏幕阅读器：给盲人获得页面-提升无障碍性

传达内容，而不是样式
**如何做到语义化**

了解标签属性含义，思考什么标签最适合描述这个内容，尽量不用可视化工具生成代码而是手写代码

#### 1.从规范的角度理解`HTML`，从分类和语义的角度使用标签

HTML5语义标签：aside(定义页面内容之外的内容)，footer,header,nav,progress,section,time,track,article,video,audio，bdi,datalist,details,dialog,embed

#### 2.常用页面标签的默认样式、自带属性、不同浏览器的差异、处理浏览器兼容问题的方式

**浏览器默认样式**

1.页边距
IE默认为10px，通过body的margin属性设置
FF默认为8px，通过body的padding属性设置
要清除页边距一定要清除这两个属性值代码如下:

```css
body {
margin:0;
padding:0;
}
```

2.段间距
IE默认为19px，通过p的margin-top属性设置
FF默认为1.12em，通过p的margin-bottom属性设
p默认为块状显示，要清除段间距，一般可以设置代码如下:

```css
p {
margin-top:0;
margin-bottom:0;
}
```

3.标题样式

```css
h1 {font-size: 2em; margin: .67em 0 } 
h2 {font-size: 1.5em; margin: .75em 0 } 
h3 {font-size: 1.17em; margin: .83em 0 } 
h4, p,blockquote, ul, fieldset, form, ol, dl, dir, menu { margin: 1.12em 0 } 
h5 {font-size: .83em; margin: 1.5em 0 } 
h6 {font-size: .75em; margin: 1.67em 0 }
```

要清除标题样式，一般可以设置代码如下:

```css
hx {
font-weight:normal;
font-size:value;
}
```

4.列表样式
IE默认为40px，通过ul、ol的margin属性设置
FF默认为40px，通过ul、ol的padding属性设置
dl无缩进，但起内部的说明元素dd默认缩进40px，而名称元素dt没有缩进。
要清除列表样式，一般可以设置代码如下:

```css
ul, ol, dd{
list-style-type:none;/*清楚列表样式符*/
margin-left:0;/*清楚IE左缩进*/
padding-left:0;/*清楚非IE左缩进*/
}
```

5.元素居中
IE默认为text-align:center;
FF默认为margin-left:auto;margin-right:auto;
6.超链接样式
a 样式默认带有下划线，显示颜色为蓝色，被访问过的超链接变紫色，要清除链接样式，一般可以设置代码如下:

```css
a {
text-decoration:none;
color:#colorname;
}
```

7 鼠标样式
IE默认为cursor:hand;
FF默认为cursor:pointer;。该声明在IE中也有效
8 图片链接样式
IE默认为紫色2px的边框线
FF默认为蓝色2px的边框线
要清除图片链接样式，一般可以设置

```css
img {
border:0;
}
```

**常见浏览器兼容性问题和解决方案**

1.浏览器兼容问题一: 不同浏览器的标签默认的外补丁和内补丁不同

解决方案：CSS里    `*{margin:0;padding:0;}`     

2.浏览器兼容问题二: IE6 的横向margin加倍

产生原因：块属性 float 有横向margin


解决方案：`display:inline;`

3.浏览器兼容问题三：IE6 下有默认的行高

解决方案：`overflow:hidden` 或者用`font-size:0   line-height: xxpx;`

4.浏览器兼容问题四：在各浏览器下img有间隙

解决方案：float  浮动;

5.浏览器兼容问题五：IE6 不识别最大宽度和最小高度 意味`min-width/height` 和Max-width/height 在IE6中没效果

解决方案：(1)：`.abc{border:1px blue solid;width:200px;height:200px;}`

 `html>body .abc{width:auto;height:auto;min-width:200px;min-height:200px;}`

(2)：`.abc{width:200px;height:200px;_width:200px;_height:200px;}（`因为ie6有一个特征，当定义一个高度时，如果内容超过高度，元素会自动调整高度。）

6.浏览器兼容问题六：li之间的间距

解决方案:li 设置 `vertical-align:middle;`

7.浏览器兼容问题七：优先级`!important`  在IE6中!important 具有bug 在同一组属性中 !important 起不了作用

8.浏览器兼容问题八：IE6 不支持 fixed

#### 3.元信息类标签(`head`、`title`、`meta`)的使用目的和配置方法

**head：**

定义语言等，引入css（link charset规定被链接文档的字符编码方式）等

**title：**

浏览器会以特殊的方式来使用标题，当把文档加入链接列表或收藏夹时，文档标题将成为该链接的默认名称。

**1.dir**
属性为rtl和ltr规定元素中内容的文本方向-正向和反向。
**2.lang**
规定元素中内容的语言代码
**3.xml:lang**
规定 XHTML 文档中元素内容的语言代码

**meta**

**name属性**
主要用于描述网页，与之对应的属性值为content,content中的内容主要是便于搜索引擎机器人查找信息和分类信息用的.

```
<meta name="参数"content="具体的参数值">
```

1.keywords 关键字
keywords用来告诉搜索引擎你网页的关键字是什么

```
<meta name="keywords"content="science,education,culture,politics,ecnomics，relationships,entertaiment,human">
```

2.description 网站内容描述

```
<meta name="description"content="这是一个学习的网站">
```

3.robots 机器人向导
robots用来告诉搜索机器人哪些页面需要索引，哪些页面不需要索引
content的参数有all,none,index,noindex,follow,nofollow。默认是all

```
<meta name="robots"content="none">
```

4.author 作者
标注网页的作者

```
<meta name="author" content="root,root@xxxx.com">
```

5.rendere 渲染模式

```
<meta name="renderer" content="webkit">
```

6.viewport(视图模式)

```html
<meta name="viewport" 
content="
width=device-width,  //viewport的高度
initial-scle=1.0,  //初始的缩放比例
maximum-scale=1.0,  //允许用户缩放到的最小比例
minimum-scale=1.0, //允许用户缩放到的最大比例
user-scalable=no" //用户是否可以手动缩放
/>
```

**http-equiv属性**
http-equiv顾名思义，相当于http的文件头作用，它可以向浏览器传回一些有用的信息，以帮助正确和精确地显示网页内容，与之对应的属性值为content，content中的内容其实就是各个参数的变量值。

meta标签的http-equiv属性语法格式是：

```html
<meta http-equiv="参数"content="参数变量值">
```

1.X-UA-Compatible 浏览模式

```html
<meta http-equiv="X-UA-Compatible" content="IE=5" />
```

像是使用了 Windows Internet Explorer 7 的 Quirks 模式，这与 Windows Internet Explorer 5 显示内容的方式很相似。

```html
<meta http-equiv="X-UA-Compatible" content="IE=7" />
```

无论页面是否包含 <!DOCTYPE> 指令，均使用 Windows Internet Explorer 7 的标准渲染模式。

```html
<meta http-equiv="X-UA-Compatible" content="IE=8" />
```

开启 IE8 的标准渲染模式，但由于本身 X-UA-Compatible 文件头仅支持 IE8 以上版本，因此等同于冗余代码。

```html
<meta http-equiv="X-UA-Compatible" content="edge" />
```

Edge 模式通知 Windows Internet Explorer 以最高级别的可用模式显示内容，这实际上破坏了“锁定”模式。即如果你有IE9的话说明你有IE789，那么就调用高版本的那个也就是IE9。

2.Expires 期限
可以用于设定网页的到期时间，一旦网页过期，必须到服务器上重新传输

```html
<meta http-equiv="expires"content="Fri,01Jan201618:18:18GMT">
```

3.Pragma cache模式
说明：禁止浏览器从本地计算机的缓存中访问页面内容。
注意：这样设定，访问者将无法脱机浏览。

```html
<meta http-equiv="Pragma" content="no-cache">
```

4.Refresh 刷新
说明：自动刷新并指向新页面
注意：其中的2是指停留2秒钟后自动刷新到URL网址。

```html
<meta http-equiv="Refresh" content="2;URL=http://www.jb51.net">
```

5.Set-Cookie cookie设定
说明：如果网页过期，那么存盘的cookie将被删除
注意：必须使用GMT的时间格式。

```html
<meta http-equiv="Set-Cookie"content="cookievalue=xxx;expires=Friday,12-Jan-200118:18:18GMT；path=/">
```

6.Window-target 显示窗口的设定
说明：强制页面在当前窗口以独立页面显示。
注意：用来防止别人在框架里调用自己的页面。

```html
<meta http-equiv="Window-target" content="_blank">
```

7.content-Type 显示字符集的设定
说明：设定页面使用的字符集

```html
<meta http-equiv="content-Type" content="text/html;charset=gb2312">
```

8.content-Language 显示语言的设定

```html
<meta http-equiv="Content-Language" content="zh-cn"/>
```

#### 4.`HTML5`离线缓存原理

HTML5的离线存储是基于一个新建的`.appcache`文件的，通过创建 cache manifest 文件，通过这个文件上的`解析清单`离线存储资源，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。

manifest 文件是简单的文本文件，它告知浏览器被缓存的内容（以及不缓存的内容）。

manifest 文件可分为三个部分：

- CACHE MANIFEST - 在此标题下列出的文件将在首次下载后进行缓存
- NETWORK - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
- FALLBACK - 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）

在线的情况下,用户代理每次访问页面，都会去读一次manifest.如果发现其改变, 则重新加载全部清单中的资源

**下载/更新缓存的详细步骤**

(1)当浏览器访问一个包含 manifest 特性的文档时，如果应用缓存不存在，浏览器会加载文档，然后获取所有在清单文件中列出的文件，生成应用缓存的第一个版本。

(2)对该文档的后续访问会使浏览器直接从应用缓存(而不是服务器)中加载文档与其他在清单文件中列出的资源。此外，浏览器还会向 window.applicationCache 对象发送一个 checking 事件，在遵循合适的 HTTP 缓存规则前提下，获取清单文件。

(3)如果当前缓存的清单副本是最新的，浏览器将向 applicationCache 对象发送一个 noupdate 事件，到此，更新过程结束。注意，如果你在服务器修改了任何缓存资源，同时也应该修改清单文件，这样浏览器才能知道它需要重新获取资源。

(4)如果清单文件已经改变，文件中列出的所有文件—也包括通过调用 applicationCache.add() 方法添加到缓存中的那些文件—会被获取并放到一个临时缓存中，遵循适当的 HTTP 缓存规则。对于每个加入到临时缓存中的文件，浏览器会向 applicationCache 对象发送一个 progress 事件。如果出现任何错误，浏览器会发送一个 error 事件，并暂停更新。

(5)一旦所有文件都获取成功，它们会自动移送到真正的离线缓存中，并向 applicationCache 对象发送一个 cached 事件。鉴于文档早已经被从缓存加载到浏览器中，所以更新后的文档不会重新渲染，直到页面重新加载(可以手动或通过程序).

**二次更新的问题**

我们知道每次使用离线应用时，在有网络连接的情况下，浏览器都会逐字节的检查menifest文件是否有更新，而当menifest文件有更新时，就会重新下载menifest文件中列出的所有资源，资源下载成功后会触发updateready事件。这时离线应用本身并不会立即更新，而会在下次访问时才更新，这就是我们所说的二次更新。我们在开发web程序时，一般都是前端页面和后端接口同步更新，但是二次更新问题会导致页面更新不受控制，无法和后端接口同步更新，因此要做好后端接口的向前兼容，这迫使我们抛弃传统的web开发思路而采取native开发思路来管理离线应用。我们可以通过检测updateready事件，在新的缓存可用时通知用户更新。

```js
window.addEventListener('load', function(e) {  
    window.applicationCache.addEventListener('updateready', function(e) { 
        //缓存更新完毕 
        if (window.applicationCache.status == window.applicationCache.UPDATEREADY) {  
            //切换为最新缓存
            window.applicationCache.swapCache();  
            if (confirm('新版本已经更新完成，是否重新加载?')) {  
                window.location.reload();  
            }  
        }  
    }, false);  
}, false);  
```

**webview中的问题**

在标准的html5浏览器中，我们可以放心的使用Application Cache，并且不需要任何设置。但是在webview中，则可能需要显示的设置，比如android系统的webview默认是不支持Application Cache的，因此需要显示设置：

```java
webView.getSettings().setAppCacheEnabled(true);        //默认是关闭的
webView.getSettings().setAppCacheMaxSize(1024*1024*5); //缓存大小
webView.getSettings().setAppCachePath("路径");          //缓存路径
```

#### 5.可以使用`Canvas API`、`SVG`等绘制高性能的动画

### CSS

#### 1.`CSS`盒模型，在不同浏览器的差异

box-sizing:

content-box

border-box

盒模型是由： 内容(content)、内边距(padding)、边框(border)、外边距(margin) 组成的。

标准模型的宽高是指的content区宽高；
IE盒模型的宽高是指的content+padding+border的宽高。

不要给元素添加具有指定宽度的内边距，而是尝试将内边距或外边距添加到元素的父元素和子元素。

#### 2.`CSS`所有选择器及其优先级、使用场景，哪些可以继承，如何运用`at`规则

**选择器**

 - 标签选择 
 - id选择器
 - class选择器
 - 后代选择 （div a）
 - 子代选择 （div > p）
 - 相邻选择 （div + p）
 - 通配符选择 （*）
 - 否定选择器 :not(.link){}
 - 属性选择器
 - 伪类选择器
 - 伪元素选择器 ::before{}

**继承属性**

- 关于文字排版的属性如：
  - font
  - word-break
  - letter-spacing
  - text-align
  - text-rendering
  - word-spacing
  - white-space
  - text-indent
  - text-transform
  - text-shadow
- line-height
- color
- visibility
- cursor

AT规则

- [常规规则](https://www.kancloud.cn/surahe/front-end-notebook/1061167#_1)

- - [@charset](https://www.kancloud.cn/surahe/front-end-notebook/1061167#charset_8)
  - [@import](https://www.kancloud.cn/surahe/front-end-notebook/1061167#import_17)
  - [@namespace](https://www.kancloud.cn/surahe/front-end-notebook/1061167#namespace_28)

- [嵌套规则](https://www.kancloud.cn/surahe/front-end-notebook/1061167#_40)

- - [@document](https://www.kancloud.cn/surahe/front-end-notebook/1061167#document_49)
  - [@font-face ](https://www.kancloud.cn/surahe/front-end-notebook/1061167#fontface_77)自定义字体
  - [@keyframes](https://www.kancloud.cn/surahe/front-end-notebook/1061167#keyframes_89) animation动画关键帧
  - [@media](https://www.kancloud.cn/surahe/front-end-notebook/1061167#media_105) 媒体查询
  - [@page](https://www.kancloud.cn/surahe/front-end-notebook/1061167#page_142) 打印文档时修改一些CSS属性
  - [@supports](https://www.kancloud.cn/surahe/front-end-notebook/1061167#supports_155)

#### 3.`CSS`伪类和伪元素有哪些，它们的区别和实际应用

伪元素:在内容元素的前后插入额外的元素或样式，但是这些元素实际上并不在文档中生成。它们只在外部显示可见，但不会在文档的源代码中找到它们，因此，称为“伪”元素。例如：

```css
p::before {content:"第一章：";}
p::after {content:"Hot!";}
p::first-line {background:red;}
p::first-letter {font-size:30px;}、
```

伪类: 将特殊的效果添加到特定选择器上。它是已有元素上添加类别的，不会产生新的元素。例如：

```css
a:hover {color: #FF00FF}
a:link
a:active
a:visited
input:focus
p:first-child {color: red}
p:last-child
p:nth-child
p:nth-last-child
p:nth-of-ty
```

**实际应用**：清楚浮动，画分割线，计数器，增大点击热区

#### 4.`HTML`文档流的排版规则，`CSS`几种定位的规则、定位参照物、对文档流的影响，如何选择最好的定位方式，雪碧图实现原理

**定位规则**

- position:static;可以取消元素的定位设置,使之恢复为原先在常规流中的显示方式.static是默认值.

- position:relative;可以使元素相对于常规流的位置偏移一定距离.

- position:absolute;可以使元素相对于常规流的位置或最近定位祖先元素的位置偏移一定的距离.

- position:fixed;可以使元素相对于窗口偏移一定的距离.

- z-index可以设置元素的堆叠顺序,数值越大,元素越靠上.

  **定位参照**

- 如果设定位置的元素没有定位祖先元素,那么``就成为定位祖先元素,换言之,``是默认设定位置元素.

- 最近定位元素必须是有效的祖先元素(relative|absolute|fixed),css不支持相对于文档中任意元素进行定位的方法.

- position:relative;是一个非常好的创建定位祖先元素的方法,因为它不会离开常规流.使用这种方法,能够创建出既保持常规流又实现绝对定位的布局.

  **雪碧图**

  将页面小图标整合在一张大图上，利用background-positon ,减轻服务器对图片的请求数量，减少图片命名困扰

#### 5.水平垂直居中的方案、可以实现`6`种以上并对比它们的优缺点

方法①：（父）text-align + table-cell + vertical-align，（子）inline-block（兼容性方案）

方法②：（子）absolute + transform（不影响父元素方案，不兼容）

方法③：（父）flex + justify - content + align - items（不影响子元素方案，不兼容）

方法④：（父）flex + （子）margin：auto（不影响子元素方案，子元素宽高可不固定，不兼容）

方法⑤：浏览器窗口margin: 50vh auto;transform: translateY(-50%);（子元素宽高可不固定，）

方法⑥：（子）absolute,left,top,margin-left:-50%width,tmargin-op:-50%height（子元素宽高固定，）

#### 6.`BFC`实现原理，可以解决的问题，如何创建`BFC`

W3C对BFC定义：

> 浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的BFC（块级格式上下文）。

BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

BFC作用：

1. 利用BFC避免外边距折叠
2. 清除内部浮动 （撑开高度）
   1. 原理: 触发父div的BFC属性，使下面的子div都处在父div的同一个BFC区域之内
3. 避免文字环绕
4. 分属于不同的BFC时，可以阻止margin重叠
5. 多列布局中使用BFC

如何生成BFC：（脱离文档流，满足下列的任意一个或多个条件即可）

1. 根元素，即HTML元素（最大的一个BFC）
2. float的值不为none
3. position的值为absolute或fixed
4. overflow的值不为visible（默认值。内容不会被修剪，会呈现在元素框之外）
5. display的值为inline-block、table-cell、table-caption

#### 7.可使用`CSS`函数复用代码，实现特殊效果

@mixin定义代码块

#### 8.`PostCSS`、`Sass`、`Less`的异同，以及使用配置，至少掌握一种

PostCSS是一个平台，允许强大的插件在它上面跑，简化编程。PostCSS实际上改变了一种编程模式,例如：autoprefixer 插件会帮我们做好兼容处理

Sass 是一种 CSS 的预编译语言。它提供了 变量（variables）、嵌套（nested rules）、 混合（mixins）、 函数（functions）等功能.

http://sass.bootcss.com/docs/sass-reference/

Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。Less 可以运行在 Node 或浏览器端。

http://lesscss.cn/

**sass和less主要区别**:在于实现方式 less是基于JavaScript的在客户端处理 所以安装的时候用npm，sass是基于ruby所以在服务器处理。

因为less需要JavaScript引擎花额外的时间来处理代码，然后输出修改过的CSS到浏览器，为了避免这个问题，只在开发环节使用LESS。一旦完成了开发，就复制然后粘贴LESS输出的到一个压缩器，然后到一个单独的CSS文件来替代LESS文件。另一个选择是使用LESS.app来编译和压缩你的LESS文件

#### 9.`CSS`模块化方案、如何配置按需加载、如何防止`CSS`阻塞渲染

防止CSS阻塞渲染：

一些 CSS 样式只在特定条件下（例如显示网页或将网页投影到大型显示器上时）使用，可以通过 CSS“媒体类型”和“媒体查询”来解决这类用例.

（1）Inline CSS

　　我们可以将那些页面首屏渲染需要用到的CSS代码加入Inline CSS。

（2）推迟加载CSS

　　对于那些首屏渲染不需要用到的CSS，我们可以依旧使用文件形式并在页面内容渲染完成后再加载。

#### 10.熟练使用`CSS`实现常见动画，如渐变、移动、旋转、缩放等等

见animation.html 学习测试

#### 11.`CSS`浏览器兼容性写法，了解不同`API`在不同浏览器下的兼容性情况

https://www.runoob.com/cssref/css3-browsersupport.html

#### 12.掌握一套完整的响应式布局方案

### 手写

- 1.手写图片瀑布流效果
- 2.使用`CSS`绘制几何图形（圆形、三角形、扇形、菱形等）
- 3.使用纯`CSS`实现曲线运动（贝塞尔曲线）
- 4.实现常用布局（三栏、圣杯、双飞翼、吸顶），可是说出多种方式并理解其优缺点