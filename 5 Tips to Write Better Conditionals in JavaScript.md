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

让我们把前面的例子进行扩展，添加以下两个条件：

- 如果没有水果，就抛出错误
- 如果水果数量超过10就输出`big quantity`

```javascript
function test(fruit, quantity) {
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

  // condition 1: fruit must has value
  if (fruit) {
    // condition 2: must be red
    if (redFruits.includes(fruit)) {
      console.log('red');

      // condition 3: must be big quantity
      if (quantity > 10) {
        console.log('big quantity');
      }
    }
  } else {
    throw new Error('No fruit!');
  }
}

// test results
test(null); // error: No fruits
test('apple'); // print: red
test('apple', 20); // print: red, big quantity
```

在上面的例子中，我们有 `if/else` 语句过滤不符合的条件，3个if嵌套（如代码中condition 1,2,3）。

我们可以通过一条很普通的规则 **不符合的条件尽早返回** 来优化代码。

```javascript
// return early when invalid conditions found

function test(fruit, quantity) {
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

  // condition 1: throw error early
  if (!fruit) throw new Error('No fruit!');

  // condition 2: must be red
  if (redFruits.includes(fruit)) {
    console.log('red');

    // condition 3: must be big quantity
    if (quantity > 10) {
      console.log('big quantity');
    }
  }
}
```

通过上面一步操作，if嵌套就少了一个。当我们有一个长的if语句体的时候，上面的代码就显得非常的简洁。

通过反转条件和提前返回，我们可以进一步减少嵌套。

```javascript
// return early when invalid conditions found

function test(fruit, quantity) {
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

  if (!fruit) throw new Error('No fruit!'); // condition 1: throw error early
  if (!redFruits.includes(fruit)) return; // condition 2: stop when fruit is not red

  console.log('red');

  // condition 3: must be big quantity
  if (quantity > 10) {
    console.log('big quantity');
  }
}
```

通过反转条件2，我们的代码就已经没有 `if/else` 嵌套了。当我们有很长的逻辑要执行，并且想在条件不满足的时候退出执行，这个小技巧就非常的有用。然而，这不是硬性的规则，有人觉得条件2使用嵌套要比没有使用嵌套的代码清晰（上上面那段代码比上面那段代码清晰），有人则不然。

于我而言，我更喜欢条件2使用嵌套的效果，因为代码简洁直接，使用嵌套if更清晰；反转条件会引发更多的思考过程（添加认知负担）。

因此，**总以减少嵌套和提前返回为目标，但是不能过分使用**。如果你有兴趣，你可以看一下下面关于这个目标的进一步探讨的文章和 StackOverflow 的讨论。

- [Avoid Else, Return Early](http://blog.timoxley.com/post/47041269194/avoid-else-return-early) by Tim Oxley
- [StackOverflow discussion](https://softwareengineering.stackexchange.com/questions/18454/should-i-return-from-a-function-early-or-use-an-if-statement) on if/else coding style

## 3. 使用默认的函数参数和解构赋值

## 4. 优先选择 **Map/对象遍历** 而不是 **switch** 语句

## 5. 对全部/部分判断时，使用 **Array.every** 和 **Array.some**
