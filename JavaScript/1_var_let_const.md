# var let const

在 ES5 中，声明变量只有两种方式：`var` 和 `function`。在 ES6，新增了 `let`，`const`，`class` 和 `import`。对于 `var` `let` `const` 的区别，主要是分析 ES5 的变量声明所存在的缺陷和 ES6 所做的一个补救。

## 变量提升

在 ES5 之前，声明变量时经常会造成许多问题，原因是 var 的变量提升机制。

```
console.log(a);
var a = 1;

// undefined
```

在上面的例子中，在声明变量之前就出输出 a，a 是 `undefind` 而不是报错，是因为 `var` 声明把 a 提升到了最前面，实际上的代码运行会变成：

```
var a;
console.log(a);
a = 1;
```

这种机制的存在，很容易让在变量声明之前的代码发生紊乱。所以在 ES6 中新加入了两种声明方式，`let` 和 `const`。这两种声明方式都不会造成变量提升。

```
console.log(a);
let a = 1;

// 报错
```

用 let 声明变量之前输出变量是不允许的，`const` 同理。对于 `let` 或者 `const` 声明变量之前的区域，我们称为“暂时性死区”。

## 作用域

在 ES5 中，声明作用域只有全局和函数作用域，没有块级作用域。

```
if (false) {
  var a = 1;
}
console.log(a);

// undefined
```

在上面代码中，`if`代码块中用`var`声明会把`a`提升到块级作用域外。

而用`let`或者`const`声明时，就不会发生这种情况：

```
if (false) {
  let a = 1;
}
console.log(a);

// 报错
```

在 ES5 中，使用`for`循环后，i 在外部也会存在：

```
for (var i = 0; i<10;i++) {
  // do something
}
console.log(i)

// 10
```

有了`let`之后，外部不会产生变量：

```
for (let i = 0; i<10;i++) {
  // do something
}
console.log(i)

// 报错
```

## `let`和`const`的区别

const 的作用是声明常量。被`const`声明的量在一般情况下不能被改变。

```
const a = 1;
a = 2;
console.log(a);

// 报错 或者 输出1
```

在一般模式下对 a 赋值不会改变 a 的值，在严格模式下会直接报错。

当声明一个普通类型的常量（string,number...）,保存的是值，当声明特殊常量（array,object），保存的是一个指针，const 不能保证指针所对应的值是否改变。

```
const o = {
  a: 1;
};
o.a = 2;
console.log(o);

// {a: 2}
```

js 就提供了一个方法，`Object.freeze()`，可用来锁定第一层的属性也无法被改变。

```
const o = Object.freeze({a:1});
o.a = 2;
console.log(o);

// 报错 或者 输出{a: 1}
```

## 其它

在 ES5 中，我们经常会使用：

```
(function() {
  var a = 1;
  // do something
})();
```

上面代码会生成匿名的自执行函数，避免产生全局污染的函数名并利用垃圾回收机制。

但在`let` `const`出现后，我们可以直接这样写：

```
{
  let a = 1;
  // do something
}
```
