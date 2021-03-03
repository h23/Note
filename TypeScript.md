# TypeScript

TS解决JS类型系统的问题，大大的提高代码的可靠性。

## 一、JavaScript

类型安全区分编程语言：

- 强类型：语言层面限制函数的实参类型必须与形参类型相同。不允许任意的隐式类型转换。编译时就报错。
- 弱类型：语言层面不会限制实参的类型。

类型检查区分编程语言：

- 静态类型：变量在声明时他的类型就是明确的。声明过后，他的类型就不允许再修改。
- 动态类型：运行阶段才能够明确变量类型。变量类型可以随时变化。变量没有类型，变量值有类型。

### 1. JS类型系统特征

 JS为弱类型 & 动态语言，缺失了类型系统的可靠性。

原因：

1. 早前的JS应用简单
2. JS是脚本语言，没有编译环节

### 2. 弱类型的问题

- 程序中的异常在运行时才能发现
- 类型不明确函数功能会发生改变（eg: 100+‘1000’）
- 对对象索引器（key会自动转成字符串）的错误用法

### 3. 强类型的优势

- 错误更早暴露
- 代码更智能，编码更准确
- 重构更牢靠
- 减少不必要的类型判断

## 二、Flow

### 1. Flow是JavaScript类型检查器

```js
   // : number 叫做类型注解，通过babel编译
   function sum (a: number, b: number) {
     return a + b
   }
   console.log(sum(1, 2))
```

### 2. 如何安装并使用flow

- 先执行`yarn init -y`

- 执行`yarn add flow-bin`

- 在代码中第一行添加flow注释：` // @flow`

- 在函数中形参后面加上冒号和类型：`function sum (a: number, b: number)`

- 执行`yarn flow init`创建.flowconfig

- 执行`yarn flow`

  ```js
  // @flow
  // : number 叫做类型注解
  function sum (a: number, b: number) {
    return a + b
  }
  console.log(sum(1, 2))
  
  console.log(sum('100', '100'))
  ```

### 3. 如何移除flow注解

flow注解不是JS的标准语法，运行时会报错，需要移除。

flow官方提供的操作:

- `yarn add flow-remove-types --dev`
- `yarn flow-remove-types src -d dist`

使用babel配合flow转换的插件:

- `yarn add @babel/core @babel/cli @babel/preset-flow --dev`

- `.babelr`文件:

  ```js
  {
    "presets": ["@babel/preset-flow"]
  }
  ```

- `yarn babel src -d dist`

### 4. 开发工具插件

VsCode中的插件：Flow Language Support

### 5. Flow支持的类型

Flow支持类型推断。

#### 5.1 原始类型

```js
const a: string = 'foo'
const b: number = Infinity // NaN // 100
const c: boolean = false // true
const d: null = null
const e: void = undefined
const f: symbol = Symbol()
const arr: Array<number> = [1, 2, 3]
const arr2: number[] = [1, 2, 3]
```

#### 5.2 数组类型

```js
const arr1:Array<number> = [1,2,3];
const arr2:number[] = [1,2,3];

const foo: [string, number] = ['foo', 100]  // 元祖：固定长度数组
const obj1: {foo: string, bar: number} = {foo: 'string', bar: 100}
```

#### 5.3 对象类型

```js
const obj1: {foo: string, bar: number} = {foo: 'string', bar: 100}

// 问号表示可有可与的属性
const obj2: {foo?: string, bar: number} = {bar: 100}

// 表示当前对象可以添加任意个数的键，不过键值的类型都必须是字符串
const obj3: {[string]: string} = {}
obj3.key1 = 'value1'
// obj3.key2 = 100
```

#### 5.4 函数类型

```js
function fn (callback: (string, number) => void) {
  callback('string', 100)
}

fn(function (str, n) {

})


```

#### 5.5 特殊类型

```js
// 字面量类型
const fo: 'foo' = 'foo'

// 联合类型，变量的值只能是其中之一
const type: 'success' | 'warning' | 'danger' = 'success'

// 变量类型只能是其中的一种类型
const g: string | number = 100

type StringOrNumber = string | number
const h: StringOrNumber = 'stri' // 100

// maybe类型 加一个问号，变量除了可以接受number类型以外，还可以接受null或undefined
const gender: ?number = null
// 相当于
// const gender: number | null | void = undefined
```

#### 5.6 Mixed与Any

mixed是强类型，any是弱类型。

any是为了兼容老代码，是不安全的，尽量不用。

```js
// string | number | boolean |...
function passMixed (value: mixed) {

}
passMixed('string')
passMixed(100)

function passAny (value: any) {

}
passAny('string')
passAny(100)

const element: HTMLElement | null = document.getElementById('root')
```

### 6. 运行环境API

 Flow内置支持运行环境API的类型定义。