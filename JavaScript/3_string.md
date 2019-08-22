# String 和实例的方法

## String

### String.fromCharCode

用于把 unicode 码转换成字符

```
console.log(String.fromCharCode('21834'))
// 啊
```

但是如果码率大于 0XFFFF 的字符，最高两位会发生溢出，造成转换错误。

ES6 中，`String.formCodePoint`可以支持大于 0XFFFF 的字符。

### String.formCodePoint

可以支持大于 0XFFFF 的字符。

### String.raw

`String.raw`普通用法是用标签模板的方式执行的，作用是对字符串进行特殊字符转义：

```
String.raw`a\nb`
// "a\nb"
```

若在普通情况下，`\n`会被识别为换行：

```
`a\nb`
// "a
// b"
```

## 实例

```
let s = 'abcd'
```

### indexOf, includes, startsWith, endsWith

在 es5 中，查找字符串中的字符用`indexOf`,并返回匹配的位数，不匹配返回-1。

```
s.indexOf('bc') // 1
s.indexOf('nnn') // -1
```

es6 新增了三个方法，匹配会直接返回一个布尔值。

```
s.includes('a') // true
s.startsWith('a') // true
s.endsWith('d') // true
s.includes('a', 2) // false
s.startsWith('a', 1) // false
s.startsWith('d', 1) // true
s.endsWith('c', 2) // true
s.endsWith('d', 2) // false
```

includes：查找字符是否在其中

startsWith: 查找字符是否在头部

endsWith: 查找字符是否在尾部

三个方法都能接受第二个参数，includes 和 startWith 表示从第几个位数开始查找，endsWith 表示前几个位数的字符。

### charCodeAt, codePointAt

获取位数的 unicode 字符,默认为 0。

```
s.charCodeAt(1) // 98
```

无法转换码数超过 0XFFFF 位数的字符。需要用到 codePointAt。

### repeat

重复输出指定字符串，数字向下取整，若为 0 或`NAN`，输出空字符串，若参数能转为数字，则能输出，其他情况下报错。

```
'a'.repeat(3) // "aaa"

```

### trim, trimStart, trimEnd

去除字符串前后空格，`trimStart`去除头部空格，`trimEnd`去除尾部空格。

### padStart, padEnd

补全字符串至长度为某个值。

```
'a'.padStart(6,'bcd') // bcdbca
'a'.padEnd(6,'bcd') // abcdbc
```
