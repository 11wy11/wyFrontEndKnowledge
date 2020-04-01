

## JSON 对象

1、js对象(数组) --> json对象(数组)：

```javascript
	JSON.stringify(obj/arr)
```

2、json对象(数组) --> js对象(数组)：


```javascript
	JSON.parse(json)
```


上面这两个方法是ES5中提供的。

我们要记住，我们通常说的“json字符串”，只有两种：**json对象、json数组**。

`typeof json字符串`的返回结果是string。

## Object的扩展

ES5给Object扩展了一些静态方法，常用的有2个，我们接下来讲解。


### 方法一

```javascript
	Object.create(prototype, [descriptors])
```

作用: 以指定对象为原型，创建新的对象。同时，第二个参数可以为为新的对象添加新的属性，并对此属性进行描述。

**举例1**：（没有第二个参数时）

```javascript
    var obj1 = {username: 'smyhvae', age: 26};
    var obj2 = {address:'shenzhen'};

    obj2 = Object.create(obj1);
    console.log(obj2);
```

打印结果：

![](http://img.smyhvae.com/20180401_2150.png)

我们发现，obj1成为了obj2的原型。

**举例2**：（有第二个参数时）

第二个参数可以给新的对象添加新的属性。我们修改上面的代码，尝试给obj2添加新属性`sex`：

```javascript
    var obj1 = {username: 'smyhvae', age: 26};
    var obj2 = {address: 'shenzhen'};

    obj2 = Object.create(obj1, {
        sex: {//给obj2添加新的属性`sex`。注意，这一行的冒号不要漏掉
            value: '男',  //通过value关键字设置sex的属性值
            writable: false,
            configurable: true,
            enumerable: true
        }
    });

    console.log(obj2);

```

上方代码中，我们通过第5行的sex给obj2设置了一个新的属性`sex`，但是要通过`value`来设置属性值（第6行）。

设置完属性值后，这个属性值默认是不可修改的，要通过`writable`来设置。总而言之，这几个关键字的解释如下：

- `value`：设置属性值。

- `writable`：标识当前属性值是否可修改。如果不写的话，默认为false，不可修改。

- `configurable`：标识当前属性是否可以被删除。默认为false，不可删除。

- `enumerable`：标识当前属性是否能用 for in 枚举。 默认为false，不可。

### 方法二

> 这个方法有点难理解。


```javascript
	Object.defineProperties(object, descriptors)
```

**作用**：为指定对象定义扩展多个属性。

代码举例：


```javascript
    var obj2 = {
        firstName : 'smyh',
        lastName : 'vae'
    };
    Object.defineProperties(obj2, {
        fullName : {
            get : function () {
                return this.firstName + '-' + this.lastName
            },
            set : function (data) {  //监听扩展属性，当扩展属性发生变化的时候自动调用，自动调用后将变化的值作为实参注入到set函数
                var names = data.split('-');
                this.firstName = names[0];
                this.lastName = names[1];
            }
        }
    });
    console.log(obj2.fullName);
    obj2.firstName = 'tim';
    obj2.lastName = 'duncan';
    console.log(obj2.fullName);
    obj2.fullName = 'kobe-bryant';
    console.log(obj2.fullName);
```

- get ：用来获取当前属性值的回调函数

- set ：修改当前属性值得触发的回调函数，并且实参即为修改后的值

存取器属性：setter,getter一个用来存值，一个用来取值。

## Object的扩展（二）

obj对象本身就自带了两个方法。格式如下：


```javascript
get 属性名(){} 用来得到当前属性值的回调函数

set 属性名(){} 用来监视当前属性值变化的回调函数

```

举例如下：



```javascript
    var obj = {
        firstName : 'kobe',
        lastName : 'bryant',
        get fullName(){
            return this.firstName + ' ' + this.lastName
        },
        set fullName(data){
            var names = data.split(' ');
            this.firstName = names[0];
            this.lastName = names[1];
        }
    };
    console.log(obj.fullName);
    obj.fullName = 'curry stephen';
    console.log(obj.fullName);
```


## 数组的扩展

> 下面讲的这几个方法，都是给数组的实例用的。

> 下面提到的数组的这五个方法，更详细的内容，可以看《03-JavaScript基础/15-数组的常见方法.md》

**方法1**：


```javascript
	Array.prototype.indexOf(value)
```

作用：获取 value 在数组中的第一个下标。

**方法2**：


```javascript
	Array.prototype.lastIndexOf(value)
```

作用：获取 value 在数组中的最后一个下标。

**方法3**：遍历数组


```javascript
	Array.prototype.forEach(function(item, index){})
```


**方法4**：

```javascript
	Array.prototype.map(function(item, index){})
```

作用：遍历数组返回一个新的数组，返回的是**加工之后**的新数组。


**方法5**：

```javascript
	Array.prototype.filter(function(item, index){})
```

作用：遍历过滤出一个新的子数组，返回条件为true的值。

## 函数function的扩展：bind()

> ES5中新增了`bind()`函数来改变this的指向。


```javascript
	Function.prototype.bind(obj)
```

作用：将函数内的this绑定为obj, 并将函数返回。

**面试题**: call()、apply()和bind()的区别：

- 都能改变this的指向

- call()/apply()是**立即调用函数**

- bind()：绑定完this后，不会立即调用当前函数，而是**将函数返回**，因此后面还需要再加`()`才能调用。

PS：bind()传参的方式和call()一样。

**分析**：

为什么ES5中要加入bind()方法来改变this的指向呢？因为bind()不会立即调用当前函数。

bind()通常使用在回调函数中，因为回调函数并不会立即调用。如果你希望在回调函数中改变this，不妨使用bind()。

## ES6 的变量声明

ES6 中新增了 let 和 const 来定义变量：

- `var`：ES5 和 ES6中，定义**全局变量**（是variable的简写）。

- `let`：定义**局部变量**，替代 var。

- `const`：定义**常量**（定义后，不可修改）。

## 变量的解构赋值

ES6允许我们，通过数组或者对象的方式，对一组变量进行赋值，这被称为解构。

解构赋值在实际开发中可以大量减少我们的代码量，并且让程序结构更清晰。

### 数组的解构赋值

**举例：**

通常情况下，我们在为一组变量赋值时，一般是这样写：


```javascript
	let a = 0;
	let b = 1;
	let c = 2;

```

现在我们可以通过数组解构的方式进行赋值：

```javascript
	let [a, b, c] = [1, 2, 3];
```

二者的效果是一样的

### 对象的解构赋值

通常情况下，我们从接口拿到json数据后，一般这么赋值：

```javascript
var a = json.a;

var b = json.b;

bar c = json.c;
```

上面这样写，过于麻烦了。

现在，我们同样可以针对对象，进行结构赋值。

**举例如下：**

```
	let { foo, bar } = { bar: '我是 bar 的值', foo: '我是 foo 的值' };
	console.log(foo + ',' + bar); //输出结果：我是键 foo 的值,我是键 bar 的值

```

上方代码可以看出，对象的解构与数组的结构，有一个重要的区别：**数组**的元素是按次序排列的，变量的取值由它的**位置**决定；而**对象的属性没有次序**，是**根据键来取值**的。

for…of 的循环可以避免我们开拓内存空间，增加代码运行效率，所以建议大家在以后的工作中使用for…of循环。

注意，上面的数组中，`for ... of`获取的是数组里的值；`for ... in`获取的是index索引值。

### Map对象的遍历

`for ... of`既可以遍历数组，也可以遍历Map对象。

## 字符串的扩展

ES6中的字符串扩展，用得少，而且逻辑相对简单。如下：

- `includes(str)`：判断是否包含指定的字符串

- `startsWith(str)`：判断是否以指定字符串开头

- `endsWith(str)`：判断是否以指定字符串结尾

- `repeat(count)`：重复指定次数

## 模板字符串

我们以前让字符串进行拼接的时候，是这样做的：（传统写法的字符串拼接）

```javascript
    var name = 'smyhvae';
    var age = '26';
    console.log('name:'+name+',age:'+age);   //传统写法
```


这种写法，比较繁琐，而且容易出错。

现在有了 ES6 语法，字符串拼接可以这样写：

```javascript
    var name = 'smyhvae';
    var age = '26';

    console.log('name:'+name+',age:'+age);   //传统写法

    console.log(`name:${name},age:${age}`);  //ES6 写法

```

**注意**，上方代码中，倒数第二行用的符号是单引号，最后一行用的符号是反引号（在tab键的上方）。

## 对象的扩展

### 扩展1


```javascript
	Object.is(v1, v2)
```

**作用：**判断两个数据是否完全相等。底层是通过**字符串**来判断的。

我们先来看下面这两行代码的打印结果：


```javascript
        console.log(0 == -0);
        console.log(NaN == NaN);
```

打印结果：

```
	true
	false
```

上方代码中，第一行代码的打印结果为true，这个很好理解。第二行代码的打印结果为false，因为NaN和任何值都不相等。

但是，如果换成下面这种方式来比较：

```javascript
        console.log(Object.is(0, -0));
        console.log(Object.is(NaN, NaN));
```

打印结果却是：

```
	false
	true
```

代码解释：还是刚刚说的那样，`Object.is(v1, v2)`比较的是字符串是否相等。

### 扩展2（重要）

```javascript
	Object.assign(目标对象, 源对象1, 源对象2...)
```

**作用：** 将源对象的属性追加到目标对象上。如果对象里属性名相同，会被覆盖。

其实可以理解成：将多个对象**合并**为一个新的对象。



举例：

```javascript
        let obj1 = { name: 'smyhvae', age: 26 };
        let obj2 = { city: 'shenzhen' };
        let obj3 = {};

        Object.assign(obj3, obj1, obj2);
        console.log(obj3);
```

打印结果：

![](http://img.smyhvae.com/20180404_2240.png)

上图显示，成功将obj1和obj2的属性复制给了obj3。



### 扩展3：`__proto__`属性

举例：

```javascript
       let obj1 = {name:'smyhvae'};
       let obj2 = {};

       obj2.__proto__ = obj1;

       console.log(obj1);
       console.log(obj2);
       console.log(obj2.name);
```

打印结果：

![](http://img.smyhvae.com/20180404_2251.png)

上方代码中，obj2本身是没有属性的，但是通过`__proto__`属性和obj1产生关联，于是就可以获得obj1里的属性。



#### Number扩展

- Number.isFinite(i)`：判断是否为有限大的数。比如`Infinity`这种无穷大的数，返回的就是false。
- `Number.isNaN(i)`：判断是否为NaN。
- `Number.isInteger(i)`：判断是否为整数。
- `Number.parseInt(str)`：将字符串转换为对应的数值。
- `Math.trunc(i)`：去除小数部分。

#### 函数扩展

ES6在**函数扩展**方面，新增了很多特性。例如：

- 箭头函数

- 参数默认值

- 参数结构赋值

- 扩展运算符

- rest参数

- this绑定

- 尾调用