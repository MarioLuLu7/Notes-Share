# function 值传递

## argument

在 function 的参数中，总体是以数组传递的，换种方式来说，不管定义的参数有多少，都可以自由传递参数。

```
function aaa(a, b, c) {
  console.log(a, b, c)
}
aaa(1) // 1, undefind, undefind
aaa(1, 2, 3) // 1, 2, 3
```

在这种机制下，函数也无法进行重载了。当定义多个同名函数时，即使参数个数不同，名字不同，最终也只定义最后一个函数。

argument 可以认为是一个参数集合。可以通过 argument[0]等等获取参数值。

```
function aaa() {
  console.log(argument)
}
aaa(1, 2, 3)
// [1, 2, 3]
```

当对 argument 的某个值改变时，参数值也会变化，但两者并不是同一个值，只是两者同步在数据。

```
function aaa(a) {
  argument[0] = 2
  console.log(a)
}
aaa(1)
// 2
```
