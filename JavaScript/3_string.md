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
