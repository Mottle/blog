---
title: 'FP异闻录0x01:我被Monad折磨的日常'
date: 2020-01-13 22:45:44
tags: [FP, Scala]
---

~~本文是一篇极其不正经且naive,too young too simple的文章~~

~~Monad说白了不过就是自函子范畴上的一个幺半群而已~~（这不是我说的）

要讲明白什么是Monad，我们不妨从一个广为人知的Monad说起

## 一个例子: Promise
要讲Promise就不得不说起一个老生常谈的问题：回调地狱。来看下面一段js代码

```javascript
//假定有这么一个函数readFile， 它的功能是异步的读取文件的内容并把结果或者错误传给callback函数
const readFile = (path, callback) => { ... }

//我要读取一个文件
readFile('filename', (data, error) => {
    if(error == null) {
        ...
    } else {
        ...
    }
})
```
上边的代码很好理解，到目前为止也没有什么问题

那么如果我们有这样一个需求，读取文件a与b并把两个文件的内容合并在一起输出，看代码

```javascript
readFile('a' (data1, error1) => {
    if(error1 == null) {
        readFile('b', (data2, error2) => {
            if(error2 == null) {
                console.log(data1 + data2)
            } else {
                ...
            }
        })
    } else {
        ...
    }
})
```
可以看到这里的代码一层一层，堆得像山一样（~~屎山~~）

有没有觉得有点麻烦了呢，在想想假如要读取十个文件并把他们的结果合并呢？是不是觉得有一股坏味道？回调函数一层套一层，这就是所谓的回调地狱

在有了Promise后我们就能这样写了
```javascript
const readFileP = (fileName, acc) => new Promise((resolve, reject) => {
    readFile(fileName, (data, error) => {
        if(error == null) {
            resolve(data + acc)
        } else {
            reject(error)
        }
    })
})

const readFileA = () => readFileP('a', '')
const readFileB = x => readFileP('b', x)
const readFileC = ...
const readFileD = ...

readFileA().then(readFileB).then(readFileC).then(readFileD).then(x => console.log(x))
```

可以看到代码确实变得简洁了许多，就好像把原有的回调层级拍平（flatten）了一样

Promise到底有什么神奇的地方可以带领我们避开回调地狱呢？先买个关子

再看看下边这个例子

## 再来一个例子：Option
(scala警告⚠️)

写过scala的童鞋都知道，在scala中，对于可能失败（或者说可能出现结果为空的问题）的计算我们都会选择使用Option，其实Option也是一种Monad，现在不妨就来看看

```scala
//这个函数的作用是在一个列表中找到指定的数value第一次出现的位置并返回，如果未出现则返回None
def findInList(list: List[Int], value: Int): Option[Int] = ???
```

如果我们要执行这样一种操作，在指定的数组中找到一个元素a第一次出现的位置，然后把它和另一个元素b第一次出现的位置相乘再加一（~~得有多无聊才会搞这种事情~~）一般的写法是这样

```scala
val x = findInList(list, a)
val y = findInList(list, b)

val res = x match {
    case Some(v1) => y match {
        case Some(v2) => Some(v1 * v2 + 1)
        case _ => None
    }
    case _ => None
}
```

是不是觉得有点麻烦

>这里要说明一下为啥这个findInList要返回Option类型而不是在失败的时候传个-1之类的东西
>因为对于很多问题搞个魔法值作为失败的情况虽然很爽，但这并不是typesafe的，或者说你拿来作为失败情况的魔法
>值是存在安全隐患的，比如对于此处如果找不到就返回-1，你可能会忘记处理失败的情况而使用了错误的结果得出了
>错误的结论

换一种写法呢？

```scala
val x = findInList(list, a)
val y = findInList(list, b)

val res = x.flatMap(m => y.flatMap(n => Some(m * n))).flatMap(t => Some(t + 1))
```
是不是又把本来的多层级代码拍平（flatten）了

## 重头戏，到底什么是Monad

看了上边两个例子，你是不是发现了些什么？Option和Promise都是把原本产生问题的部分拿某种结构包裹了起来，然后使用一个函数（flatMap，then）来处理

没错，不准确的说，只要一种结构M满足其内部保存一个值，且这个结构支持flatMap操作（在haskell中叫bind写作>>=），就可以说M是一个Monad

可是上边说了这么多，还是么说清楚monad是什么flatMap是什么
>未完待续