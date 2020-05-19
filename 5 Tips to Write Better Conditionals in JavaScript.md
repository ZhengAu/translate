# 写出更好的JavaScript条件语句的5个小技巧

> - 原文地址： [5 Tips to Write Better Conditionals in JavaScript](https://scotch.io/bar-talk/5-tips-to-write-better-conditionals-in-javascript#toc-3-use-default-function-parameters-and-destructuring)
>
> - 原文作者：[ecelyn Yeen](https://scotch.io/@jecelyn)[(@jecelynyeen)](https://twitter.com/jecelynyeen)
>
> - 译者：[ZhengAu](https://github.com/ZhengAu)

---

用 `JavaScript` 工作时，我们会经常处理很多条件语句，接下来推荐5个小技巧让你写出更好更干净的条件语句。

## 1. 多重条件时使用 **Array.includes**

首先，看一下下面这段代码。

```javascript
// condition
function test(fruit) {
  if (fruit == 'apple' || fruit == 'strawberry') {
    console.log('red');
  }
}
```

咋一看，上面例子的代码看起来非常的棒。然而，如果我们还有很多红色的水果，比如 `cherry` 和 `cranberries` ? 难道我们还要继续扩展 **||** 运算符吗？

下面，我们使用 **Array.includes** 重写上面那段代码。

```javascript
function test(fruit) {
  // extract conditions to array
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

  if (redFruits.includes(fruit)) {
    console.log('red');
  }
}
```

通过把 **red fruits** 的条件改成数组形式，上面的代码看起来更加简洁了。

## 2. 尽早 **return**，少嵌套

## 3. 使用默认的函数参数和解构赋值

## 4. 优先选择 **Map/对象遍历** 而不是 **switch** 语句

## 5. 对全部/部分判断时，使用 **Array.every** 和 **Array.some**
