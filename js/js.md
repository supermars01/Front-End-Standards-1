## 语言规范

JavaScript 是一种客户端脚本语言，这里列出了编写 JavaScript 时需要遵守的规则。

### 类型

- 基本类型
  - 字符串
  - 数值
  - 布尔类型
  - null
  - undefined

  ```js
  const foo = 1
  let bar = foo

  bar = 9

  console.log(foo, bar) // 1, 9
  ```

- 复杂类型
  - object
  - array
  - function

  ```js
  const foo = [1, 2, 3]
  const bar = foo

  bar[0] = 9

  console.log(foo[0], bar[0]) // 9, 9
  ```

### 引用

`const` 和 `let` 都是块级作用域，`var` 是函数级作用域

- 对所有引用都使用 `const`，不要使用 `var`

  ```js
  // bad
  var a = 1
  var b = 2

  // good
  const a = 1
  const b = 2
  ```

- 如果引用是可变动的，则使用 `let`

  ```js
  // bad
  var count = 1
  if (count < 10) {
    count += 1
  }

  // good
  let count = 1
  if (count < 10) {
    count += 1
  }
  ```

### 对象

- 请使用字面量值创建对象

  ```js
  // bad
  const a = new Object{}

  // good
  const a = {}
  ```

- 别使用保留字作为对象的键值，这样在 IE8 下不会运行

  ```js
  // bad
  const a = {
    default: {},  // default 是保留字
    common: {}
  }

  // good
  const a = {
    defaults: {},
    common: {}
  }
  ```

- 请使用对象方法的简写方式

  ```js
  // bad
  const item = {
    value: 1,

    addValue: function (val) {
      return item.value + val
    }
  }

  // good
  const item = {
    value: 1,

    addValue(val) {
      return item.value + val
    }
  }
  ```

- 请使用对象属性值的简写方式

  ```js
  const job = 'FrontEnd'

  // bad
  const item = {
    job: job
  }

  // good
  const item = {
    job
  }
  ```

- 对象属性值的简写方式要和声明式的方式分组

  ```js
  const job = 'FrontEnd'
  const department = 'JDC'

  // bad
  const item = {
    sex: 'male',
    job,
    age: 25,
    department
  }

  // good
  const item = {
    job,
    department,
    sex: 'male',
    age: 25
  }
  ```

### 数组

- 请使用字面量值创建数组

  ```js
  // bad
  const items = new Array()

  // good
  const items = []
  ```

- 向数组中添加元素时，请使用 `push` 方法

  ```js
  const items = []

  // bad
  items[items.length] = 'test'

  // good
  items.push('test')
  ```

- 使用拓展运算符 `...` 复制数组

  ```js
  // bad
  const items = []
  const itemsCopy = []
  const len = items.length
  let i

  // bad
  for (i = 0; i < len; i++) {
    itemsCopy[i] = items[i]
  }

  // good
  itemsCopy = [...items]
  ```

- 使用数组的 `map` 等方法时，请使用 `return` 声明，如果是单一声明语句的情况，可省略 `return`

  ```js
  // good
  [1, 2, 3].map(x => {
    const y = x + 1
    return x * y
  })

  // good
  [1, 2, 3].map(x => x + 1)

  // bad
  const flat = {}
  [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
    const flatten = memo.concat(item)
    flat[index] = flatten
  })

  // good
  const flat = {}
  [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
    const flatten = memo.concat(item)
    flat[index] = flatten
    return flatten
  })

  // bad
  inbox.filter((msg) => {
    const { subject, author } = msg
    if (subject === 'Mockingbird') {
      return author === 'Harper Lee'
    } else {
      return false
    }
  })

  // good
  inbox.filter((msg) => {
    const { subject, author } = msg
    if (subject === 'Mockingbird') {
      return author === 'Harper Lee'
    }

    return false
  })
  ```

### 解构赋值

- 当需要使用对象的多个属性时，请使用解构赋值

  ```js
  // bad
  function getFullName (user) {
    const firstName = user.firstName
    const lastName = user.lastName

    return `${firstName} ${lastName}`
  }

  // good
  function getFullName (user) {
    const { firstName, lastName } = user

    return `${firstName} ${lastName}`
  }

  // better
  function getFullName ({ firstName, lastName }) {
    return `${firstName} ${lastName}`
  }
  ```

- 当需要使用数组的多个值时，请同样使用解构赋值

  ```js
  const arr = [1, 2, 3, 4]

  // bad
  const first = arr[0]
  const second = arr[1]

  // good
  const [first, second] = arr
  ```

- 函数需要回传多个值时，请使用对象的解构，而不是数组的解构

  ```js
  // bad
  function doSomething () {
    return [top, right, bottom, left]
  }

  // 如果是数组解构，那么在调用时就需要考虑数据的顺序
  const [top, xx, xxx, left] = doSomething()

  // good
  function doSomething () {
    return { top, right, bottom, left }
  }

  // 此时不需要考虑数据的顺序
  const { top, left } = doSomething()
  ```

### 字符串

- 字符串统一使用单引号的形式 `''`

  ```js
  // bad
  const department = "JDC"

  // good
  const department = 'JDC'
  ```

- 字符串太长的时候，请不要使用字符串连接符换行 `\`，而是使用 `+`

  ```js
  const str = '凹凸实验室 凹凸实验室 凹凸实验室' +
    '凹凸实验室 凹凸实验室 凹凸实验室' +
    '凹凸实验室 凹凸实验室'
  ```

- 程序化生成字符串时，请使用模板字符串

  ```js
  const test = 'test'

  // bad
  const str = ['a', 'b', test].join()

  // bad
  const str = 'a' + 'b' + test

  // good
  const str = `ab${test}`
  ```

### 函数

- 请使用函数声明，而不是函数表达式

  ```js
  // bad
  const foo = function () {
    // do something
  }

  // good
  function foo () {
    // do something
  }
  ```

- 不要在非函数代码块中声明函数

  ```js
  // bad
  if (isUse) {
    function test () {
      // do something
    }
  }

  // good
  let test
  if (isUse) {
    test = () => {
      // do something
    }
  }
  ```

- 不要使用 `arguments`，可以选择使用 `...`

  > `arguments` 只是一个类数组，而 `...` 是一个真正的数组

  ```js
  // bad
  function test () {
    const args = Array.prototype.slice.call(arguments)
    return args.join('')
  }

  // good
  function test (...args) {
    return args.join('')
  }
  ```

- 不要更改函数参数的值

  ```js
  // bad
  function test (opts) {
    opts = opts || {}
  }

  // good
  function test (opts = {}) {
    // ...
  }
  ```

### 原型

- 使用 `class`，避免直接操作 `prototype`

  ```js
  // bad
  function Queue (contents = []) {
    this._queue = [..contents]
  }
  Queue.prototype.pop = function () {
    const value = this._queue[0]
    this._queue.splice(0, 1)
    return value
  }

  // good
  class Queue {
    constructor (contents = []) {
      this._queue = [...contents]
    }

    pop () {
      const value = this._queue[0]
      this._queue.splice(0, 1)
      return value
    }
  }
  ```

### 模块

- 使用标准的 ES6 模块语法 `import` 和 `export`

  ```js
  // bad
  const util = require('./util')
  module.exports = util

  // good
  import Util from './util'
  export default Util

  // better
  import { Util } from './util'
  export default Util
  ```

- 不要使用 `import` 的通配符 `*`，这样可以确保你只有一个默认的 export

  ```js
  // bad
  import * as Util from './util'

  // good
  import Util from './util'
  ```

### 迭代器

- 不要使用 `iterators`

  ```js
  const numbers = [1, 2, 3, 4, 5]

  // bad
  let sum = 0
  for (let num of numbers) {
    sum += num
  }

  // good
  let sum = 0
  numbers.forEach(num => sum += num)

  // better
  const sum = numbers.reduce((total, num) => total + num, 0)
  ```

### 对象属性

- 使用 `.` 来访问对象属性

  ```js
  const joke = {
    name: 'haha',
    age: 28
  }

  // bad
  const name = joke['name']

  // good
  const name = joke.name
  ```

### 变量声明

- 声明变量时，请使用 `const`、`let` 关键字，如果没有写关键字，变量就会暴露在全局上下文中，这样很可能会和现有变量冲突，另外，也很难明确该变量的作用域是什么。这里推荐使用 `const` 来声明变量，我们需要避免全局命名空间的污染。

  ```js
  // bad
  demo = new Demo()

  // good
  const demo = new Demo()
  ```

- 将所有的 `const` 和 `let` 分组

  ```js
  // bad
  let a
  const b
  let c
  const d
  let e

  // good
  const b
  const d
  let a
  let c
  let e
  ```

### Hoisting

- `var` 存在变量提升的情况，即 `var` 声明会被提升至该作用域的顶部，但是他们的赋值并不会。而 `const` 和 `let` 并不存在这种情况，他们被赋予了 [Temporal Dead Zones, TDZ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)

  ```js
  function example () {
    console.log(notDefined)   // => throws a ReferenceError
  }

  function example () {
    console.log(declareButNotAssigned)  // => undefined
    var declaredButNotAssigned = true
  }

  function example () {
    let declaredButNotAssigned
    console.log(declaredButNotAssigned)   // => undefined
    declaredButNotAssigned = true
  }

  function example () {
    console.log(declaredButNotAssigned)   // => throws a ReferenceError
    console.log(typeof declaredButNotAssigned)  // => throws a ReferenceError
    const declaredButNotAssigned = true
  }
  ```

- 匿名函数的变量名会提升，但函数内容不会

  ```js
  function example () {
    console.log(anonymous)  // => undefined

    anonymous()

    var anonymous = function () {
      console.log('test')
    }
  }
  ```

- 命名的函数表达式的变量名会被提升，但函数名和函数函数内容并不会

  ```js
  function example() {
    console.log(named)  // => undefined

    named()   // => TypeError named is not a function

    superPower()  // => ReferenceError superPower is not defined

    var named = function superPower () {
      console.log('Flying')
    }
  }

  function example() {
    console.log(named)  // => undefined

    named()   // => TypeError named is not a function

    var named = function named () {
      console.log('named')
    }
  }
  ```

### 分号

- 我们遵循 `Standard` 的规范，不使用分号。

  > 关于应不应该使用分号的讨论有很多，本规范认为非必要的时候，应该不使用分号，好的 `JS` 程序员应该清楚场景下是一定要加分号的，相信你也是名好的开发者。

  ```js
  // bad
  const test = 'good';
  (function () {
    const str = 'hahaha';
  })()

  // good
  const test = 'good'
  ;(() => {
    const str = 'hahaha'
  })();
  ```

### 标准特性

为了代码的可移植性和兼容性，我们应该最大化的使用标准方法，例如优先使用 `string.charAt(3)` 而不是 `string[3]`

### eval()

由于 `eval` 方法比较 `evil`，所以我们约定禁止使用该方法

### with() {}

由于 `with` 方法会产生神奇的作用域，所以我们也是禁止使用该方法的

### for-in 循环

推荐使用 `for in` 语法，但是在对对象进行操作时，容易忘了检测 `hasOwnProperty(key)`，所以我们启用了 `ESLint` 的 `guard-for-in` 选项

> 对数组进行 `for in` 的时候，顺序是不固定的

### 修改内置对象的原型

不要修改内置对象，如 `Object` 和 `Array`

## 编码规范

统一团队的编码规范，有助于代码的维护。本章是传统意义上的 `Style Guideline`，目的是统一一些相对主观化的代码风格。

### 单行代码块

在单行代码块中使用空格

*不推荐*

```js
function foo () {return true}
if (foo) {bar = 0}
```

*推荐*

```js
function foo () { return true }
if (foo) { bar = 0 }
```

### 大括号风格

在编程过程中，大括号风格与缩进风格紧密联系，用来描述大括号相对代码块位置的方法有很多。在 JavaScript 中，主要有三种风格，如下：

- **One True Brace Style**

  ```js
  if (foo) {
    bar()
  } else {
    baz()
  }
  ```

- **Stroustrup**

  ```js
  if (foo) {
    bar()
  }
  else {
    baz()
  }
  ```

- **Allman**

  ```js
  if (foo)
  {
    bar()
  }
  else
  {
    baz()
  }
  ```

我们团队约定使用 `One True Brace Style` 风格

### 变量命名

当命名变量时，主流分为驼峰式命名（variableName）和下划线命名（variable_name）两大阵营。

> 团队约定使用驼峰式命名

### 拖尾逗号

在 ECMAScript5 里面，对象字面量中的拖尾逗号是合法的，但在 IE8（非 IE8 文档模式）下，当出现拖尾逗号，则会抛出错误。

拖尾逗号的例子：

```js
var foo = {
  name: 'foo',
  age: '22',
}
```

拖尾逗号的好处是，简化了对象和数组添加或删除元素，我们只需要修改新增的行即可，并不会增加差异化的代码行数。

> 因为拖尾逗号有好也有不好，所以团队约定允许在最后一个元素或属性与闭括号 `]` 或 `}` 在不同行时，可以（但不要求）使用拖尾逗号。当在同一行时，禁止使用拖尾逗号。

### 逗号空格

逗号前后的空格可以提高代码的可读性，团队约定在逗号后面使用空格，逗号前面不加空格。

*不推荐*

```js
var foo = 1,bar = 2
var foo = 1 , bar = 2
var foo = 1 ,bar = 2
```

*推荐*

```js
var foo = 1, bar = 2
```

### 逗号风格

逗号分隔列表时，在 JavaScript 中主要有两种逗号风格：
- 标准风格，逗号放置在当前行的末尾
- 逗号前置风格，逗号放置在下一行的开始位置

> 团队约定使用标准风格

*不推荐*

```js
var foo = 1
,
bar = 2

var foo = 1
, bar = 2

var foo = ['name'
          , 'age']
```

*推荐*

```js
var foo = 1,
    bar = 2

var foo = ['name',
            'age']
```

### 计算属性的空格

团队约定在对象的计算属性内，禁止使用空格

*不推荐*

```js
obj['foo' ]
obj[ 'foo']
obj[ 'foo' ]
```

*推荐*

```js
obj['foo']
```

### 拖尾换行

在非空文件中，存在拖尾换行是一个常见的 `UNIX` 风格，它的好处是可以方便在串联和追加文件时不会打断 `Shell` 的提示。在日常的项目中，保留拖尾换行的好处是，可以减少版本控制时的代码冲突。

*不推荐*

```js
function func () {
  // do something
}
```

*推荐*

```js
function func () {
  // do something
}
  // 此处是新的一行
```

> 可以通过 .editorconfig 添加 EOL

### 函数调用

为了避免语法错误，团队约定在函数调用时，禁止使用空格

*不推荐*

```js
fn ()
fn
()
```

*推荐*

```js
fn()
```

### 缩进

代码保持一致的缩进，是作为工程师的职业素养。但缩进用两个空格，还是四个空格，是用 `Tab` 还是空格呢？这样的争论太多了，也得不出答案。本规范结合了市面上优秀的开源项目，姑且约定使用 `空格` 来缩进，而且缩进使用两个空格。

那是不是不能使用 `Tab` 进行缩进了？我们可以通过配置 `.editorconfig` ，将 `Tab` 自动转换为空格。

### 对象字面量的键值缩进

团队约定对象字面量的键和值之间不能存在空格，且要求对象字面量的冒号和值之间存在一个空格

*不推荐*

```js
var obj = { 'foo' : 'haha' }
```

*推荐*

```js
var obj = { 'foo': 'haha' }
```

### 构造函数首字母大写

在 JavaScript 中 `new` 操作符用来创建某个特定类型的对象的一个实例，该类型的对象是由一个构造函数表示的。由于构造函数只是常规函数，唯一区别是使用 `new` 来调用。所以我们团队约定构造函数的首字母要大小，以此来区分构造函数和普通函数。

*不推荐*

```js
var fooItem = new foo()
```

*推荐*

```js
var fooItem = new Foo()
```

### 构造函数的参数

在 JavaScript 中，通过 `new` 调用构造函数时，如果不带参数，可以省略后面的圆括号。但这样会造成与整体的代码风格不一致，所以团队约定使用圆括号

*不推荐*

```js
var person = new Person
```

*推荐*

```js
var person = new Person()
```

### 链式调用

链式调用如果放在同一行，往往会造成代码的可读性差，但有些时候，短的链式调用并不会影响美观。所以本规范约定一行最多只能有四个链式调用，超过就要求换行。

### 空行

空白行对于分离代码逻辑有帮助，但过多的空行会占据屏幕的空间，影响可读性。团队约定最大连续空行数为 2

*不推荐*

```js
var a = 1



var b = 2
```

*推荐*

```js
var a = 1


var b = 2
```

### 链式赋值

链式赋值容易造成代码的可读性差，所以团队约定禁止使用链式赋值

*不推荐*

```js
var a = b = c = 1
```

*推荐*

```js
var a = 1
var b = 1
var c = 1
```

### 变量声明

JavaScript 允许在一个声明中，声明多个变量。团队约定在声明变量时，一个声明只能有一个变量

*不推荐*

```js
var a, b, c
```

*推荐*

```js
var a
var b
var c
```

### 分号

JavaScript 在所有类 C 语言中是比较独特的，它不需要在每个语句的末尾有分号。在很多情况下，JavaScript 引擎可以确定一个分号应该在什么位置然后自动添加它。此特征被称为 自动分号插入 (ASI)，被认为是 JavaScript 中较为有争议的特征。

团队中对于是否应该使用分号，也有许多争论，本规范推荐不使用分号，因为我们认为好的工程师应该知道什么时候该加，什么时候不该加。

相关参考 ：[semi](http://eslint.org/docs/rules/semi)

### 代码块空格

一致性是任何风格指南的重要组成部分。虽然在哪里放置块的开括号纯属个人偏好，但在整个项目中应该保持一致。不一致的风格将会分散读者阅读代码的注意力。

> 团队约定代码块前要添加空格

*不推荐*

```js
if (a){
  b()
}

function a (){}
```

*推荐*

```js
if (a) {
  b()
}

function a () {}
```

### 函数声明的空格

当格式化一个函数，函数名或 function 关键字与左括号之间允许有空白。命名函数要求函数名和 function 关键字之间有空格，但是匿名函数要求不加空格。

> 团队约定函数括号前要加空格

*不推荐*

```js
function func(x) {
  // ...
}
```

*推荐*

```js
function func (x) {
  // ...
}
```

### 操作符的空格

团队约定操作符前后都需要添加空格

*不推荐*

```js
var sum = 1+2
```

*推荐*

```js
var sum = 1 + 2
```

### BOM

Unicode 字节顺序标记 (BOM) 用来指定代码单元是高字节序还是低字节序。也就是说，是高位在前还是低位在前。UTF-8 不需要 BOM 来表明字节顺序，因为单个字节并不影响字节顺序。

相信不少同学遇到过 BOM 的坑，这里不多说了，切记不要使用 windows 的记事本改代码！
