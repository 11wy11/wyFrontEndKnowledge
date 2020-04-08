###  async、Promise、Generator的对比（async的特点）

1、不需要像Generator去调用next方法，遇到await等待，当前的异步操作完成就往下执行。

2、async返回的总是Promise对象，可以用then方法进行下一步操作。

3、async取代Generator函数的星号*，await取代Generator的yield。

4、语意上更为明确，使用简单。

**Generator**

generator 是更为优雅的流程控制方式，可以让函数可中断执行:

```text
function* generatorA() {
    console.log('a')
    yield
    console.log('b')
}
const genA = generatorA()
genA.next() // a
genA.next() // b
```

yield 关键字后面可以包含表达式，表达式会传给 next().value。

next() 可以传递参数，参数作为 yield 的返回值。

下面是这个特性的例子：

```text
function* generatorB(count) {
    console.log(count)
    const result = yield 5
    console.log(result * count)
}
const genB = generatorB(2)
genB.next() // 2
const genBValue = genB.next(7).value // 14
// genBValue undefined
```

第一个 next 是没有参数的，因为在执行 generator 函数时，初始值已经传入，第一个 next 的参数没有任何意义，传入也会被丢弃。

```text
const result = yield 5
```

这一句，返回值不是想当然的 5。其的作用是将 5 传递给 genB.next()，其值，由下一个 next genB.next(7) 传给了它，所以语句等于 const result = 7。

最后一个 genBValue，是最后一个 next 的返回值，这个值，就是函数的 return 值，显然为 undefined。

我们回到这个语句：

```text
const result = yield 5
```

如果返回值是 5，是不是就清晰了许多？是的，这种语法就是 await。所以 Async Await与 generator 有着莫大的关联，桥梁就是 生成器，我们稍后介绍 生成器。