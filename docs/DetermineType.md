# 类型判断之typeof、instanceof、constructor

### 使用写法
> `typeof`和`constructor`都有返回值，根据返回值去判断相应的数据类型

> `instanceof` 算是操作符，左右两边写值进行判断，返回值是布尔类型
>
> 注：右边的值一定要是`object`,否则报错`Right-hand side of 'instanceof' is not an object`

写法实例
```
const unkownData // 不知道属性的值
typeof unkownData // 操作符写法
typeof(unkownData) // 函数写法（推荐）

unkownData.constructor

unkownData instanceof String
```

### 相同点
- 三者都可以用来判断数据类型

### 不同点
`typeof`（基本数据类型判断推荐使用）
1. `typeof`的返回值有`number`,`boolean`,`string`,`function`,`object`,`undefined`,`symbol`(ES2015新增),`bigint`(ES2020新增)
2. `Null`、`Object`、`Array`等特殊对象，`typeof`一律返回`object`

`constructor`（不推荐使用）
1. `constructor`它的实质是构造函数中`prototype`的属性值，可以被修改，不可信赖
> 当在原型对象中设置`constructor`属性时，只是屏蔽了原型的constructor属性值，而没有覆盖

> “JavaScript中的所有内容都是对象”。所以就算是基本数据类型，都会有对应的`constructor`属性值

> JS中基本数据类型的构造函数有`Boolean`、`Number`、`String`、`Funciton`、`Symbol`、`BigInt`、`Object`，还有一些JS内置构造函数`Date`、`Marh`等，以及通过`new`自定义声明的构造函数等


`instanceof` (引用数据类型判断推荐使用)
1. 用于判断一个数据是否出现在右边的实例对象的原型链上
2. 不能用来判断字符串和数字等
> 详细解释参考[MDN-instanceof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof)

>> 数组的判断推荐使用`Array.isArray()`
