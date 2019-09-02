# function 值传递

## argument

在 `function` 的参数中，总体是以数组传递的，换种方式来说，不管定义的参数有多少，都可以自由传递参数。

```
function aaa(a, b, c) {
  console.log(a, b, c)
}
aaa(1) // 1, undefind, undefind
aaa(1, 2, 3) // 1, 2, 3
```

在这种机制下，函数也无法进行重载了。当定义多个同名函数时，即使参数个数不同，名字不同，最终也只定义最后一个函数。

argument 可以认为是一个参数集合。可以通过 `argument[0]` 等等获取参数值。

```
function aaa() {
  console.log(argument)
}
aaa(1, 2, 3)
// [1, 2, 3]
```

当对 argument 的某个值改变时，参数值也会变化，但两者并不是同一个值，只是两者数据同步。

```
function aaa(a) {
  argument[0] = 2
  console.log(a)
}
aaa(1)
// 2
```

## 值传递

数据传递给方法的时候，存在的形式是值拷贝。当传递普通类型值的时候：

```
let a = 1;
function change(x) {
  x = 2;
  conslole.log(x);
}
change(a);
console.log(a);

// 2
// 1
```

对于一个普通类型的值，传递进方法并对其操作是不会影响原来的值。

对于引用类型的值，也是进行值传递的，很多人会在这里发生歧义，会认为是应用拷贝，比如以下情况：

```
let a = {b: 1};
function change(x) {
  x.b = 2;
  conslole.log(x);
}
change(a);
console.log(a);

// {b: 2}
// {b: 2}
```

当对一个引用类型的值传递进方法并改变时，外部的值也改变了，就会认为是引用传递。但是在以下情况：

```
let a = {b: 1};
function change(x) {
  x = new Object();
  conslole.log(x);
}
change(a);
console.log(a);

// {}
// {b: 1}
```

这时外部值并不会改变，这样就说明传递的时候只是传递了引用的值，也就是拷贝了一份引用并把它的值传递过去，而不是传递了引用。在其它语言中，会有指针变量的存在，就可以进行引用传递。而 js 在方法中只存在值传递。
