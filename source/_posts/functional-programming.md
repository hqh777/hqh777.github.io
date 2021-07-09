---
title: 函数式编程上
date: 2021-06-04 13:49:16
tags: 
---
## 什么是函数式编程？
函数式编程，缩写FP，是一种编程范式，也是一种编程风格，和面向对象是并列的关系。函数式编程我们可以认为是一种思维模式，加上实现方法。其思维方式就是把现实世界事物和事物之间的联系抽象到程序世界（是对运算过程进行抽象）

常听说的编程范式还有面向过程编程（按照步骤来实现）、面向对象编程（把现实中的事物抽象成类和对象，通过封装、继承和多态来演示不同事物之间的联系）。

### 函数式编程和面向对象编程的不同
- 从思维方式上来说
面向对象编程是对事物的抽象，而函数式编程是对运算过程的抽象

### 对于函数式编程思维方式的理解：
- 程序的本质：根据输入通过某种运算获得相应的输出，程序开发过程中会涉及很多输入和输出的函数。
- 函数式编程中的函数指的不是程序中的函数Function，而是数学中的函数即映射关系，例如：y=sin(x)，是这种x和y的关系
- 相同的输入时总要得到相同的输出（纯函数）
- 函数式编程用描述数据（函数）之间的映射

```js
// 非函数式
let num1 = 2
let num2 = 3
let sum = num1 + num2
console.log(sum)

// 函数式
function add(n1, n2) {
    return n1 + n2
}
let sum = add(2, 3)
console.log(sum)
```
## 函数式编程的前置知识
在JS中函数就是一个普通的对象，我们可以把函数存储到变量/数组中，它还可以作为另一个函数的参数和返回值，甚至我们可以在程序运行的时候通过`new Function('alert(1)')`来构造一个新的函数。
- 函数可以存储在变量中
```js
// 把函数赋值给变量
let fn = function () {
    console.log("hi")
}

fn()

// 一个示例
const BlogController = {
    index (posts) { return Views.index(posts) },
    show (post) { return Views.show(post) },
    create (attrs) { return Db.create(attrs) },
    update (post, attrs) { return Db.update(post, attrs) },
    destroy (post) { return Db.destroy(post) }
}

// 优化 赋值的是Views的index方法，不是方法的调用
const BlogController = {
    index: Views.index,
    show: Views.show,
    create: Db.create,
    update: Db.update,
    destroy: Db.destroy
}    
```
下面两个特性在高阶函数中会有详细说明
- 函数可以作为参数
- 函数可以作为返回值

### 高阶函数
高阶函数（Higher-order function）
- 函数可以作为参数

```js
// forEach
// 定义一个遍历数组的并对每一项做处理的函数，第一个函数是一个数组，第二个参数是一个函数。
function forEach (array, fn) {
    for (let i = 0; i < array.length; i++) {
        fn(array[i]) 
    } 
}

// test
let arr = [1, 2, 3]
forEach(arr, item => {
    item = item * 2
    console.log(item) // 2 4 6
})
```
```js
// filter
// 遍历数组，并把满足条件的元素存储成数组，再进行返回
function filter(array, fn) {
    let results = []
    for (let i = 0; i < array.length; i++) { 
        //如果满足条件  
        if (fn(array[i])) { 
            results.push(array[i]) 
        }    
    }
    return results
}

// test
let arr = [1, 3, 4, 7, 8]
let result = filter(arr, item => item % 2 === 0)
console.log(result) // [4, 8]
```
- 函数作为返回值

```js
// 一个函数返回另一个函数
function makeFn () {
    let msg = 'Hello function' 
    return function () { 
        console.log(msg) 
    } 
}

// test
// 第一种调用方式
const fn = makeFn() 
fn() //Hello function

// 第二种调用方式
makeFn()()///Hello function
```
```js
// once
// 让函数只执行一次

function once(fn) {
    let done = false
    return function() {
        // 判断值有没有被执行，如果是false表示没有执行，如果是true表示已经执行过了，不必再执行
        if(!done) {
            done = true
            // 调用fn，当前this直接传递过来，第二个参数是把fn的参数传递给return的函数
            return fn.apply(this, arguments)
        }
    }
}

// test
let pay = once(function (money) {
    console.log(`支付：${money} RMB`)
})

pay(5) //支付：5 RMB
pay(5)
pay(5)
pay(5)
pay(5)
```
#### 使用高阶函数的意义
- 抽象可以帮我们屏蔽细节，我们只需要知道我们的目标和解决这类问题的函数，我们不需要关心实现的细节
- 高阶函数是用来抽象通用的问题

#### 常用的高阶函数
有一个通用的特点，就是需要一个函数作为参数。
- forEach
- map
对数组中的每个元素进行遍历，并处理，处理的结果放在一个新数组中返回
```js
const map = (array, fn) => { 
    let results = [] 
    for (const value of array) { 
        results.push(fn(value)) 
    }
    return results 
}

// test
let arr = [1, 2, 3, 4]
arr = map(arr, v => v * v)
console.log(arr)
//
```
- filter
- every
数组中的每一个元素是否都匹配我们指定的一个条件，如果都满足返回true，如果不满足返回false
```js
const every = (array, fn) => { 
    let result = true 
    for (const value of array) {
        result = fn(value) 
        // 如果有一个元素不满足就直接跳出循环
        if (!result) { 
            break 
        }
    }
    return result
}

// test
let arr = [11, 12, 14]
let r = every(arr, v => v > 10)
console.log(r) // false

r = every(arr, v => v > 12)
console.log(r) // false
```
- some
判断数组中是否有一个元素满足我们指定的条件，满足是true，都不满足为false
```js
const some = (array, fn) => { 
    let result = false 
    for (const value of array) {
        result = fn(value) 
        // 如果有一个元素不满足就直接跳出循环
        if (result) { 
            break 
        }
    }
    return result
}

// test
let arr = [1, 3, 4, 9]
let arr1 = [1, 3, 5, 9]
let r = some(arr, v => v % 2 === 0)
console.log(r) // true
r = some(arr1, v => v % 2 === 0)
console.log(r) // false

```
- find/findIndex
- reduce
- sort

### 闭包
#### 闭包的概念
闭包：函数和其周围的状态（词法环境）的引用捆绑在一起形成闭包
- 可以在另一个作用域中调用一个函数的内部函数并访问到该函数作用域中的成员

在上面函数作为返回值的过程中，其实我们就用到了闭包，下面进行语法演示：
```js
function makeFn () {
    let msg = 'Hello function'
}
// 正常情况下，执行完makeFn，里面的变量msg会释放掉
// 但是下面的情况

function makeFn () {
    let msg = 'Hello function'
    return function () { 
        console.log(msg)
    } 
}
// 在上面函数中，返回了一个函数，而且在函数中还访问了原来函数内部的成员，就可以称为闭包

const fn = makeFn()
fn()
// fn为外部函数，当外部函数对内部成员有引用的时候，那么内部的成员msg就不能被释放。当我们调用fn的时候，我们就会访问到msg。

//注意的点：
//1、我们可以在另一个作用域调用makeFn的内部函数
//2、当我们调用内部函数的时候我们可以访问到内部成员

```
#### 闭包的核心作用
把函数内部成员的作用范围延长
#### 闭包的本质
函数在执行的时候会放到一个执行栈上，当函数执行完毕之后会从执行栈上移除。但是堆上的作用域成员因为被外部引用不能释放，因此内部函数依然可以访问外部函数的成员。

/*解读：函数执行的时候在执行栈上，执行完毕之后从执行栈上移除，内部成员的内存被释放。但是在函数执行完毕移除之后，释放内存的时候，如果外部有引用，则内部成员的内存不能被释放。*/

#### 闭包的案例
##### 案例一
计算一个数平方和立方的运算
```js
Math.pow(4, 2)
Math.pow(5, 2)
// 后面的二次方三次方很多次重复，下面要写一个二次方三次方的函数
function makePower (power) {
  return function (number) {
    return Math.pow(number, power)
  }
}

// 求平方
let power2 = makePower(2)
let power3 = makePower(3)

console.log(power2(4)) // 16
console.log(power2(5)) // 25
console.log(power3(4)) // 64
```
##### 案例二
计算不同级别的员工工资
```js
// 假设计算员工工资的函数第一个函数传基本工资，第二个参数传绩效工资
// getSalary(12000, 2000)
// getSalary(15000, 3000)
// getSalary(15000, 4000)

// 不同级别的员工基本工资是一样的，所以我们将基本工资提取出来，之后只需要加上绩效工资
function makeSalary (base) { 
    return function (performance) { 
        return base + performance 
    }
}
let salaryLevel1 = makeSalary(12000)
let salaryLevel2 = makeSalary(15000)

console.log(salaryLevel1(2000)) //14000
console.log(salaryLevel2(3000)) //18000
console.log(salaryLevel2(4000)) //19000
```

## 纯函数
### 纯函数的概念
**相同的输入永远会得到相同的输出**，而且没有任何可观察的副作用。

纯函数就类似数学中的函数(用来描述输入和输出之间的关系)，y = f(x)


```js
let numbers = [1, 2, 3, 4, 5] 
// 纯函数 
// 对于相同的函数，输出是一样的

// slice方法，截取的时候返回截取的函数，不影响原数组
numbers.slice(0, 3) // => [1, 2, 3] 
numbers.slice(0, 3) // => [1, 2, 3] 
numbers.slice(0, 3) // => [1, 2, 3] 

// 不纯的函数 
// 对于相同的输入，输出是不一样的

// splice方法，返回原数组，改变原数组
numbers.splice(0, 3) // => [1, 2, 3] 
numbers.splice(0, 3) // => [4, 5] 
numbers.splice(0, 3) // => []

// 下面函数也是纯函数 
function getSum (n1, n2) {
    return n1 + n2
}
console.log(getSum(1, 2)) // 3
console.log(getSum(1, 2)) // 3
console.log(getSum(1, 2)) // 3
```

- 函数式编程不会保留计算中间的结果，所以变量是不可变的(无状态的)
- 我们也可以把一个函数的执行结果交给另一个函数处理

### Lodash——纯函数的代表
- lodash 是一个纯函数的功能库，提供了模块化、高性能以及一些附加功能。提供了对数组、数字、对象、字符串、函数等操作的一些方法

#### 体验Lodash
- 安装

新建文件夹 -> npm init -y -> npm i lodash

- 体验

```js
const _ = require('lodash')

const array = ['jack', 'tom', 'lucy', 'kate']

// head的别名是first  _.head(array)也可以
console.log(_.first(array)) //jack
console.log(_.last(array)) //kate

console.log(_.toUpper(_.first(array))) //JACK

console.log(_.reverse(array))  //[ 'kate', 'lucy', 'tom', 'jack' ]
// 数组的翻转不是纯函数，因为会改变原数组。这里的reserve是使用了数组的reverse，所以也不是纯函数

const r = _.each(array, (item, index) => {
  console.log(item, index)
  // kate 0
  // lucy 1
  // tom 2
  // jack 3
})
console.log(r) // [ 'kate', 'lucy', 'tom', 'jack' ]
```
### 纯函数的好处
#### 可缓存
因为对于相同的输入始终有相同的结果，那么可以把纯函数的结果缓存起来，可以提高性能。
```js
const _ = require('lodash')

function getArea(r) {
  console.log(r)
  return Math.PI * r * r
}

let getAreaWithMemory = _.memoize(getArea)
console.log(getAreaWithMemory(4))
console.log(getAreaWithMemory(4))
console.log(getAreaWithMemory(4))
// 4
// 50.26548245743669
// 50.26548245743669
// 50.26548245743669

// 看到输出的4只执行了一次，因为其结果被缓存下来了
```
那我们可以模拟一个记忆函数
```js
function memoize (f) {
  let cache = {}
  return function () {
    // arguments是一个伪数组，所以要进行字符串的转化
    let key = JSON.stringify(arguments)
    // 如果缓存中有值就把值赋值，没有值就调用f函数并且把参数传递给它
    cache[key] = cache[key] || f.apply(f,arguments)
    return cache[key]
  }
}

let getAreaWithMemory1 = memoize(getArea)
console.log(getAreaWithMemory1(4))
console.log(getAreaWithMemory1(4))
console.log(getAreaWithMemory1(4))
// 4
// 50.26548245743669
// 50.26548245743669
// 50.26548245743669
```
#### 可测试
纯函数让测试更加的方便
#### 并行处理
- 多线程环境下并行操作共享的内存数据很可能会出现意外情况。纯函数不需要访问共享的内存数据，所以在并行环境下可以任意运行纯函数
- 虽然JS是单线程，但是ES6以后有一个Web Worker，可以开启一个新线程

### 副作用
副作用就是让一个函数变得不纯，纯函数的根据市相同的输入返回相同的输出，如果函数依赖于外部的状态就无法保证输出相同，就会带来副作用，如下面的例子：
```js
// 不纯的函数，因为它依赖于外部的变量
let mini = 18 
function checkAge (age) { 
    return age >= mini 
}
```
副作用来源：
- 配置文件
- 数据库
- 获取用户的输入
- ......

所有的外部交互都有可能带来副作用，副作用也使得方法通用性下降不适合扩展和可重用性，同时副作用会给程序中带来安全隐患给程序带来不确定性，但是副作用不可能完全禁止，我们不能禁止用户输入用户名和密码，只能尽可能控制它们在可控范围内发生。