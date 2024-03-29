# 解构赋值踩坑

## 数组解构赋值

普通情况下：

```
let [a, b, c] = [1, 2, 3]  // 1 2 3
let [a, b, c] = [1, 2]  // 1 2 undefind
let [a, ...b] = [1, 2, 3, 4]  // 1 [2, 3, 4]
let [a, b] = [1, null] // 1 null
```

上面代码中，只要解构时值严格等于（===）`undefind`，赋值时的值是`undefind`，其他情况下都为解构的值。

在有初始值的情况下：

```
let [a = 1, b = 2] = [3 ,4] // 3 4
let [a = 1, b = 2] = [3] // 3 2
let [a = 1, b = 2] = [] // 1 2
let [a = 1, b = 2] = [null] // null 2
```

规律是与上面相同，在严格等于（===）`undefind`时，若有初始值，就为初始值，否则为`undefind`。例如第二条，相当于以下代码：

```
let a = 1;
let b = 2;
if ([3][0] === undefind) {
  if (a === undefind) { // 代码层面是无意义的，只是为了更好理解
    a = undefind;
  }
} else {
  a = [3][0];
}
if ([3][1] === undefind) {
  if (b === undefind) {
    b = undefind;
  }
} else {
  b = [3][1];
}
```

相互赋值情况下：

```
let [a = 1 ,b = a] = [2, 3] // 2 3
```

上面代码的赋值逻辑是：

```
let a = 1;
let b = a;
if (2, 3][0] === undefind) {
  if (a === undefind) { // 代码层面是无意义的，只是为了更好理解
    a = undefind;
  }
} else {
  a = [3][0];
}
if ([2, 3][1] === undefind) {
  if (b === undefind) {
    b = undefind;
  }
} else {
  b = [2, 3][1];
}
```

## 对象解构赋值

栗子：

```
let {a, b, c} = {a: 1, b: 2, c: 3}  // a 1   b 2   c 3
let {b, c, a} = {a: 1, b: 2, c: 3}  // a 1   b 2   c 3
let {x: a, y: b} = {a: 1, b: 2, c: 3}  // x 1   y 2   a 报错   b 报错
```

上面代码中，第一条和第二条可以看出，对象的解构赋值和数组不一样的是，对象是无序的，是通过属性名来找到值。而数组是有序的，必须按照顺序赋值。

第三条中，可以看出对象解构的时候，是通过值来找到解构对象的属性的，第一条第二条其实是简写：

```
let {a: a, b: b, c: c} = {a: 1, b: 2, c: 3}
```

因为：

```
{a: a, b: b, c: c} === {a, b, c} // 逻辑层面
```

当对象复杂并且层级多的时候：

```
let obj = {
  a: 1,
  b: [2, 3],
  c: {
    d: 'sss'
  }
}

let {
  a,
  b: [x, y],
  c: {
    d
  }
} = obj;
// a 1
// x 2
// y 3
// d 'sss'
// b 报错
// c 报错
```

上面代码中，a,x,y,d 很好理解，但是 b,c 为什么会报错？

因为对于 a,x,y,d 来说，是一个确定的值，而 b,c 只是一个模式或者说是结构。要给 b,c 赋值，需要单独再赋值：

```
let {
  a,
  b: [x, y],
  c: {
    d
  },
  b, // 单独赋值
  c // 单独赋值
} = obj;
```

对象解构也可以设置默认值，与数组大同小异。

给对象进行解构的时候，不能写成下面的格式：

```
let a;
{ a } = {a: 1};
```

在 js 执行中，会把`{a}`识别成代码块，要把它识别成对象，就必须包上圆括号：

```
let a;
({ a }) = {a: 1};
```

那什么时候可以用圆括号，什么时候不能？（书写代码其实并不会遇到这种情况，但是笔试就不一定了）

三个规律：

1. 值可以包，模式不能包
2. 声明语句不能包
3. 代码块必须包

```
let [(a)] = [1]; // 声明语句不能包 报错
({ a }) = {a: 1}; // 代码块
({ (a) }) = {a: 1}; // 值 + 代码块
([a]) = [1]; // 模式不能包 报错
{(a: b)} = {b: 1};  // 模式不能包 报错
```
