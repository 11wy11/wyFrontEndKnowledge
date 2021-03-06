## 总规范：

1. 忽略（Omit）协议：如 background: url(http://www.google.com/images/example); 应该写background: url(//www.google.com/images/example);以方便http与https协议切换，除非必须使用某个协议
2. 缩进使用两个空格，不要使用tab
3. 标签属性使用小写
4. 尾部不要留有空格，以防diff
5. 使用 UTF-8 (no BOM)，文档中也加入 <meta charset=”utf-8″>
6. TODO(contact) 以及TODO: action item，不要使用@@

## HTML与CSS

### HTML规范：

1. 使用html5的规范<!DOCTYPE html>

2. 注意html标签行为与语义，<li onclick=”goToRecommendations();”>All recommendations</li>应该写为<a href=”recommendations/”>All recommendations</a>

3. 多媒体标签向后兼容，记得加上alt属性

4. 由于utf-8编码的使用，某些记号无需转码，如&mdash;, &rdquo;, or &#x263a;当然<或是&除外

5. 注意[HTML5 specification](http://www.whatwg.org/specs/web-apps/current-work/multipage/syntax.html#syntax-tag-omission)?中规定的可省略标签

6. type属性不要使用！HTML5中已经规定为默认且无必要

7. 为block标签另起一行，li出现例外可以放在一行例：

   ```
   <blockquote>
     <p><em>Space</em>, the final frontier.</p>
   </blockquote>
   ```

   ```
   <ul>
     <li>Moe
     <li>Larry
     <li>Curly
   </ul>
   ```

   ```
   <table>
     <thead>
       <tr>
         <th scope="col">Income
         <th scope="col">Taxes
     <tbody>
       <tr>
         <td>$ 5.00
         <td>$ 4.50
   </table>
   ```

### CSS规范：

1. 正确使用缩写，例如navigation就可以缩写为nav，而author就不要缩写

2. id与类名前不必加上标签类型

3. 属性尽量使用简写形式，如font或background等

4. 0后面不要加上单位

5. 小数前不要加上0

6. url()中不要加入引号，例如@import url(//www.google.com/css/go.css);

7. 16进制尽量使用3位表示

8. 可以为项目加入前缀，例如.adw-help {} /* AdWords */

9. 分词使用“-”，如前例

10. 属性采用字典序申明，包括前缀如moz安排在webkit之前

    ```
    background: fuchsia;
    border: 1px solid;
    -moz-border-radius: 4px;
    -webkit-border-radius: 4px;
    border-radius: 4px;
    color: black;
    text-align: center;
    text-indent: 2em;
    ```

11. {}里面都应该使用缩进，包括属性申明或另一个{}

12. 属性：与值之间用一个空格分开，例如font-weight: bold;

13. 为每个选择符及每个属性申明单独使用一行

    ```
    h1,
    h2,
    h3 {
      font-weight: normal;
      line-height: 1.2;
    }
    ```

14. 规则之间用一个空行分开

## JAVASCRIPT

1. 变量声明必须使用var

2. Constants使用大写名称，并适当加上@const注释（如果名称已经体现就不用添加了），不要使用const关键字，例：

3. 记得使用分号结尾，错误例子：

   ```
   //1.
   MyClass.prototype.myMethod = function() { return 42; } // No semicolon here.
   (function() {
   // Some initialization code wrapped in a function to create a scope for locals.
   })();
   var x = { 'i': 1, 'j': 2 } // No semicolon here.
   // 2. Trying to do one thing on Internet Explorer and another on Firefox.
   // I know you'd never write code like this, but throw me a bone.
   [normalVersion, ffVersion][isIE]();
   var THINGS_TO_EAT = [apples, oysters, sprayOnCheese] // No semicolon here.
   // 3. conditional execution a la bash
   -1 == resultOfOperation() || die();
   ```

   错误1. ；错误2. 解释为x={}[][]();即尝试执行对象的某个方法；错误3. 解释为THINGS_TO_EAT = […]-1 == resultOfOperation() || die();即赋值为die()；

4. 随意使用嵌套函数

5. 块级域中不用使用函数声明形式（不符合标准），应该这样处理：

   ```
   if (x) {
     var foo = function() {}
   }
   ```

6. 随意使用自定义exception

7. 原生类型不要使用包装对象的构造函数来进行声明，但是可以用来进行类型转换如：

   ```
   var x = Boolean(0);
   if (x) {
     alert('hi');  // This will never be alerted.
   }
   typeof Boolean(0) == 'boolean';
   typeof new Boolean(0) == 'object';
   ```

8. 不推荐多重继承，可以使用[the Closure Library](http://code.google.com/closure/library/)

9. 闭包，注意dom元素的引用计数问题，例：function有对element的引用，而element也有对function的引用

   ```
   function foo(element, a, b) {
     element.onclick = function() { /* uses a and b */ };
   }
   ```

10. eval只用于rpc解码json，注意

11. 不要使用with

12. for in 不要用在遍历array上，只能用在object上

13. 关联数组如hash，map应该使用object实现而不是array

14. 多行的字符串字面量，应该使用+进行字符串拼接，而不是进行换行（不符合标准）

15. array与object创建使用字面量而不是包装构造函数

16. 不要改变内置对象的prototype

17. 不要使用ie的条件注释

    ```
    var f = function () {
        /*@cc_on if (@_jscript) { return 2* @*/  3; /*@ } @*/
    };
    ```

18. 命名：`functionNamesLikeThis`,`variableNamesLikeThis`,`ClassNamesLikeThis`,`EnumNamesLikeThis`,?`methodNamesLikeThis`, and`SYMBOLIC_CONSTANTS_LIKE_THIS`.

19. 私有成员使用结尾下划线(trailing undercore)，保护成员不使用结尾下划线；可选参数以opt_开头，如果函数有可变参数，则最后一个参数命名为var_args，但不要使用他而要使用arguments数组进行访问；不要使用get和set关键字，如果要写则使用getFoo();setFoo(val);isFoo()的形式；

20. 永远要为项目添加一个命名空间，不要为外部引入的代码再引入自定义的成员，如果必要则应该使用外部代码暴露的api

21. 为过长的类型名使用别名引用，例如

    ```
    /**
     * @constructor
     */
    some.long.namespace.MyClass = function() {
    };
    
    /**
     * @param {some.long.namespace.MyClass} a
     */
    some.long.namespace.MyClass.staticHelper = function(a) {
      ...
    };
    
    myapp.main = function() {
      var MyClass = some.long.namespace.MyClass;
      var staticHelper = some.long.namespace.MyClass.staticHelper;
      staticHelper(new MyClass());
    };
    ```

22. 但是不要为命名空间使用别名；同时避免访问别名类型的属性，除非其是enum类型；也不要在全局域中使用别名，只能在函数内部使用

23. 文件命名统一使用小写”.js”，同时推荐”-“而不是”_”

24. 自定义的toString方法必须保证1.正确性；2.没有副作用；否则很容易在assert时出错

25. 可以推迟初始化

26. 总是使用显式的域，例如不要依赖window一定存在于域中

27. 代码规范：基本是C++规范；”{“总是开始在一行上；可以单行初始化数组或对象；多行的数组或对象初始化应该要缩进2空格，属性与值都不用对齐；每行80单词，如果超过则注意函数参数的排放；函数中调用函数缩进方式例子如下：

    ```
    if (veryLongFunctionNameA(
            veryLongArgumentName) ||
        veryLongFunctionNameB(
        veryLongArgumentName)) {
      veryLongFunctionNameC(veryLongFunctionNameD(
          veryLongFunctioNameE(
              veryLongFunctionNameF)));
    }
    ```

    匿名函数内缩进为两个空格；事实上，除了数组或对象初始化，匿名函数内，其他情况要么与上行左边对齐，要么缩进4个空格！；针对操作符缩进：

    ```
    var x = a ? b : c;  // All on one line if it will fit.
    
    // Indentation +4 is OK.
    var y = a ?
        longButSimpleOperandB : longButSimpleOperandC;
    
    // Indenting to the line position of the first operand is also OK.
    var z = a ?
            moreComplicatedB :
            moreComplicatedC;
    ```

28. 不要在如delete，typeof，void或是return，throw后面添加使用括号（Parentheses）

29. 字符串相对于双引号，推荐使用单引号

30. 可见性规范：推荐使用jsdoc的@private以及@protected；@private与c语言中static类似，同时子类不能访问；@protected中子类可以访问

31. js类型规范：

    js类型规范：支持[JS2](http://wiki.ecmascript.org/doku.php?id=spec:spec)风格类型与JS1.x类型：

    JS2规范的类型语言：

    | 操作名称                                                     | 语法                                                         | 描述                                                         | 过期语法                                                     |
    | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | Type Name                                                    | `{boolean}`, `{Window}`, `{goog.ui.Menu}`                    | 类型名称                                                     |                                                              |
    | Type Application                                             | `{Array.}` An array of strings.`{Object.}` An object in which the keys are strings and the values are numbers. | 为类型增加参数类型，类似于java                               |                                                              |
    | Type Union                                                   | `{(number|boolean)}` A number or a boolean.                  | 说明值为类型A或类型B                                         | `{(number,boolean)}`,`{number|boolean}`,`{(number||boolean)}` |
    | Record Type                                                  | `{{myNum: number, myObject}}` An anonymous type with the given type members. | 说明值的成员类型固定`注意大括号也是成员的一部分，例如：由对象构成的``Array` 拥有`length` 属性, 你应该写为 `Array.<{length}>`. |                                                              |
    | Nullable type                                                | `{?number}` A number or NULL.                                | 说明值为类型A or `null`. 默认所有对象类型均为nullable 注意 Function Type不是nullable. | `{number?}`                                                  |
    | Non-nullable type                                            | `{!Object}` An Object, but never the `null` value.           | 说明值要么是类型A要么not null. 默认所有的值类型(boolean, number, string, and undefined) 都不是nullable. | `{Object!}`                                                  |
    | Function Type                                                | `{function(string, boolean)}` A function that takes two arguments (a string and a boolean), and has an unknown return value. | 标明函数                                                     |                                                              |
    | Function Return Type                                         | `{function(): number}` A function that takes no arguments and returns a number. | 标明函数返回类型                                             |                                                              |
    | Function `this` Type                                         | `{function(this:goog.ui.Menu, string)}` A function that takes one argument (a string), and executes in the context of a goog.ui.Menu. | 标明函数上下文的类型                                         |                                                              |
    | Function `new` Type                                          | `{function(new:goog.ui.Menu, string)}` A constructor that takes one argument (a string), and creates a new instance of goog.ui.Menu when called with the ‘new’ keyword. | 标明函数构造器的类型                                         |                                                              |
    | Variable arguments                                           | `{function(string, ...[number]): number}` A function that takes one argument (a string), and then a variable number of arguments that must be numbers. | 标明函数参数                                                 |                                                              |
    | Variable arguments (in`@param`annotations)                   | `@param {...number} var_args` A variable number of arguments to an annotated function. | 标明函数参数可变                                             |                                                              |
    | Function [optional arguments](http://google-styleguide.googlecode.com/svn/trunk/optional) | `{function(?string=, number=)}` A function that takes one optional, nullable string and one optional number as arguments. The `=` syntax is only for `function` type declarations. | 标明可选的函数参数                                           |                                                              |
    | Function [optional arguments](http://google-styleguide.googlecode.com/svn/trunk/optional) (in`@param`annotations) | `@param {number=} opt_argument` An optional parameter of type `number`. | 标明函数接收可变参数                                         |                                                              |
    | The ALL type                                                 | `{*}`                                                        | 说明变量接受任何类型                                         |                                                              |
    | The UNKNOWN type                                             | `{?}`                                                        | 说明变量可使用任何类型且编译器不应对类型进行检查             |                                                              |

    Types in JavaScript

    | TYPE EXAMPLE                     | VALUE EXAMPLES                                               | DESCRIPTION                                                  |
    | -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | number                           | `1 1.0 -5 1e5 Math.PI`                                       |                                                              |
    | Number                           | `new Number(true)`                                           | [Number object](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml#Wrapper_objects_for_primitive_types) |
    | string                           | `'Hello' "World" String(42)`                                 | String value                                                 |
    | String                           | `new String('Hello') new String(42)`                         | [String object](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml#Wrapper_objects_for_primitive_types) |
    | boolean                          | `true false Boolean(0)`                                      | Boolean value                                                |
    | Boolean                          | `new Boolean(true)`                                          | [Boolean object](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml#Wrapper_objects_for_primitive_types) |
    | RegExp                           | `new RegExp('hello') /world/g`                               |                                                              |
    | Date                             | `new Date new Date()`                                        |                                                              |
    | null                             | `null`                                                       |                                                              |
    | undefined                        | `undefined`                                                  |                                                              |
    | void                             | `function f() {  return; }`                                  | No return value                                              |
    | Array                            | `['foo', 0.3, null] []`                                      | Untyped Array                                                |
    | Array.<number>                   | `[11, 22, 33]`                                               | An Array of numbers                                          |
    | Array.<Array.<string>>           | `[['one', 'two', 'three'], ['foo', 'bar']]`                  | Array of Arrays of strings                                   |
    | Object                           | `{} {foo: 'abc', bar: 123, baz: null}`                       |                                                              |
    | Object.<string>                  | `{'foo': 'bar'}`                                             | An Object in which the values are strings.                   |
    | Object.<number, string>          | `var obj = {}; obj[1] = 'bar';`                              | An Object in which the keys are numbers and the values are strings.Note that in JavaScript, the keys are always implicitly coverted to strings, so `obj['1'] == obj[1]`. So the key wil always be a string in for…in loops. But the compiler will verify the type if the key when indexing into the object. |
    | Function                         | `function(x, y) {  return x * y; }`                          | [Function object](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml#Wrapper_objects_for_primitive_types) |
    | function(number, number): number | `function(x, y) {  return x * y; }`                          | function value                                               |
    | SomeClass                        | `/** @constructor */ function SomeClass() {} new SomeClass();` |                                                              |
    | SomeInterface                    | `/** @interface */ function SomeInterface() {} SomeInterface.prototype.draw = function() {};` |                                                              |
    | project.MyClass                  | `/** @constructor */ project.MyClass = function () {} new project.MyClass()` |                                                              |
    | project.MyEnum                   | `/** @enum {string} */ project.MyEnum = {  BLUE: '#0000dd',  RED: '#dd0000' };` | [Enumeration](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml#enums) |
    | Element                          | `document.createElement('div')`                              | Elements in the DOM.                                         |
    | Node                             | `document.body.firstChild`                                   | Nodes in the DOM.                                            |
    | HTMLInputElement                 | `htmlDocument.getElementsByTagName('input')[0]`              | A specific type of DOM element.                              |

    如果不能使用类型检查来表明，则可以用注释来表明类型并可以将注释写在括号中。

    ```
    /** @type {number} */ (x)
    (/** @type {number} */ x)
    ```

    js中必须区别可选（optional），可空（nullable），及未定义（undefined）的函数参数和类属性。

    对象类型（是js中的引用类型）默认是可空，函数类型默认就不是可空

    ```
    /**
     * Some class, initialized with a value.
     * @param {Object} value Some value.
     * @constructor
     */
    function MyClass(value) {
      /**
       * Some value.
       * @type {Object}
       * @private
       */
      this.myValue_ = value;
    }
    ```

    这告诉编译器myValue_属性赋值为对象类型（即可空），如果myValue_属性不可为空，则应该申明：

    ```
    /**
     * Some class, initialized with a non-null value.
     * @param {!Object} value Some value.
     * @constructor
     */
    function MyClass(value) {
      /**
       * Some value.
       * @type {!Object}
       * @private
       */
      this.myValue_ = value;
    }
    ```

    可选函数参数可能在运行时未定义，故而如果将它们赋值给类属性，这些参数必须申明

    ```
    /**
     * Some class, initialized with an optional value.
     * @param {Object=} opt_value Some value (optional).
     * @constructor
     */
    function MyClass(opt_value) {
      /**
       * Some value.
       * @type {Object|undefined}
       * @private
       */
      this.myValue_ = opt_value;
    }
    ```

    这告诉编译器myValue_属性赋值为对象，可空或保持为未定义，注意这里opt_value被申明为{Object=}，而不是{Object|undefined}，这是因为这样申明不容易阅读。注意可空与未定义是正交属性，下面的四个申明并不相同：

    ```
    /** * Takes four arguments, two of which are nullable, and two of which are * optional. * @param {!Object} nonNull Mandatory (must not be undefined), must not be null. * @param {Object} mayBeNull Mandatory (must not be undefined), may be null. * @param {!Object=} opt_nonNull Optional (may be undefined), but if present, * must not be null! * @param {Object=} opt_mayBeNull Optional (may be undefined), may be null. */ function strangeButTrue(nonNull, mayBeNull, opt_nonNull, opt_mayBeNull) { // ... };
    ```

    可以使用typedef来定义复杂的类型：

    ```
    /** @typedef {(string|Element|Text|Array.<Element>|Array.<Text>)} */
    goog.ElementContent;
    
    /**
     * @param {string} tagName
     * @param {goog.ElementContent} contents
     * @return {!Element}
     */
    goog.createElement = function(tagName, contents) {
    ...
    };
    ```

    模板类型被编译器受限，只能通过匿名函数推测this的类型，以及this参数是否缺少。

    ```
    /**
     * @param {function(this:T, ...)} fn
     * @param {T} thisObj
     * @param {...*} var_args
     * @template T
     */
    goog.bind = function(fn, thisObj, var_args) {
    ...
    };
    // Possibly generates a missing property warning.
    goog.bind(function() { this.someProperty; }, new SomeClass());
    // Generates an undefined this warning.
    goog.bind(function() { this.someProperty; });
    ```

    注意在适当地方加入类型注释，

32. 注释规范：

    使用JSDoc，语法基本遵守 [JavaDoc](http://www.oracle.com/technetwork/java/javase/documentation/index-137868.html)；

    如果需要换行则必须缩进4个空格，但不用缩进@fileoverview命令；

    ```
    /**
     * Illustrates line wrapping for long param/return descriptions.
     * @param {string} foo This is a param with a description too long to fit in
     *     one line.
     * @return {number} This returns something that has a description too long to
     *     fit in one line.
     */
    project.MyClass.prototype.method = function(foo) {
      return 5;
    };
    ```

    JSdoc也支持html的标签；

    ```
    /**
     * Computes weight based on three factors:
     * <ul>
     * <li>items sent
     * <li>items received
     * <li>last timestamp
     * </ul>
     */
    ```

    文件最开头的注释；

    ```
    // Copyright 2009 Google Inc. All Rights Reserved.
    
    /**
     * @fileoverview Description of file, its uses and information
     * about its dependencies.
     * @author user@google.com (Firstname Lastname)
     */
    ```

    类必须有相应的注释并使用相应的类型tag；

    ```
    /**
     * Class making something fun and easy.
     * @param {string} arg1 An argument that makes this more interesting.
     * @param {Array.<number>} arg2 List of numbers to be processed.
     * @constructor
     * @extends {goog.Disposable}
     */
    project.MyClass = function(arg1, arg2) {
      // ...
    };
    goog.inherits(project.MyClass, goog.Disposable);
    ```

    方法与函数必须提供功能描述与参数，方法的描述必须使用第三人称；对于简单的getter方法可以省略注释；

    ```
    /**
     * Operates on an instance of MyClass and returns something.
     * @param {project.MyClass} obj Instance of MyClass which leads to a long
     *     comment that needs to be wrapped to two lines.
     * @return {boolean} Whether something occured.
     */
    function PR_someMethod(obj) {
      // ...
    }
    ```

    属性注释；

    ```
    /**
     * Maximum number of things per pane.
     * @type {number}
     */
    project.MyClass.prototype.someProperty = 4;
    ```

33. 技巧：

    关于boolean，下面这些为false

    - `null`
    - `undefined`
    - `''空字符串`
    - `数字0`

    但是注意这些为true

    - `'0'` 字符
    - `[]` 空数组
    - `{}` 空对象

    这意味着你可以这样写

    ```
    while (x) {
    ```

    以取代（因为你不期待x是空字符串或是为0）

    ```
    while (x != null) {
    ```

    如果你希望查看字符串是否为空或null，则可以写作

    ```
    if (y) {
    ```

    而不是

    ```
    if (y != null && y != '') {
    ```

    注意：

    - `Boolean('0') == true '0' != true`
    - `0 != null 0 == [] 0 == false`
    - `Boolean(null) == false null != true null != false`
    - `Boolean(undefined) == false undefined != true undefined != false`
    - `Boolean([]) == true [] != true [] == false`
    - `Boolean({}) == true {} != true {} != false`

    使用三目运算符(?:)取代if判断语句，充分利用&&与||运算符来缩短代码

    使用join()来连接字符串，即通过数组的join方法来连接字符串（主要是因为IE中字符串的+=操作过慢），同时注意数组赋值比push方法更快

    注意访问node list的length属性本身就是O(n)，即如果遍历使用length属性来进行判断会变为O(n^2)

    ```
    var paragraphs = document.getElementsByTagName('p');
    for (var i = 0; i < paragraphs.length; i++) {
      doSomething(paragraphs[i]);
    }
    ```

    应该写为

    ```
    var paragraphs = document.getElementsByTagName('p');
    for (var i = 0, paragraph; paragraph = paragraphs[i]; i++) {
      doSomething(paragraph);
    }
    ```

    这同样适用于数组的遍历（只要数组元素不会为false），对于子节点的遍历可以写作

    ```
    var parentNode = document.getElementById('foo');
    for (var child = parentNode.firstChild; child; child = child.nextSibling) {
      doSomething(child);
    }
    ```