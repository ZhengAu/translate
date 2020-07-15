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

下面这段代码对我们来说都比较熟悉，我们在使用 `JavaScript` 时需要经常判断 **null/undefined** 和赋默认值。

```javascript
function test(fruit, quantity) {
  if (!fruit) return;
  const q = quantity || 1; // if quantity not provided, default to one

  console.log(`We have ${q} ${fruit}!`);
}

//test results
test('banana'); // We have 1 banana!
test('apple', 2); // We have 2 apple!
```

事实上，我们可以通过分配默认的函数参数来消除变量 `q` 。

```javascript
function test(fruit, quantity = 1) { // if quantity not provided, default to one
  if (!fruit) return;
  console.log(`We have ${quantity} ${fruit}!`);
}

//test results
test('banana'); // We have 1 banana!
test('apple', 2); // We have 2 apple!
```

上面的代码看起来更加简洁和直观了。这里注意一下，每个参数都可以有自己默认的函数参数。例如，我们也可以给 `fruit` 赋默认值，`function test(fruit = 'unknown', quantity = 1)` 。

假设参数 `fruit` 是一个对象，我们还能够赋默认值吗？

```javascript
function test(fruit) {
  // printing fruit name if value provided
  if (fruit && fruit.name)  {
    console.log (fruit.name);
  } else {
    console.log('unknown');
  }
}

//test results
test(undefined); // unknown
test({ }); // unknown
test({ name: 'apple', color: 'red' }); // apple
```

看上面的例子，如果水果 `name` 存在就输出水果名字，否则输出 `unknown` 。我们可以忽略 `fruit && fruit.name` 的条件，检查默认的函数参数和解构赋值。

```javascript
// destructing - get name property only
// assign default empty object {}
function test({name} = {}) {
  console.log (name || 'unknown');
}

//test results
test(undefined); // unknown
test({ }); // unknown
test({ name: 'apple', color: 'red' }); // apple
```

如果我们只需要水果的 `name` ，可以使用 `{name}` 从函数参数中解构出来，接下来在代码中就可以使用变量 `name` 代替 `fruit.name` 。

我们可以赋值空对象 `{}` 为默认的参数。如果我们没有赋值默认参数 `{}` ，当我们执行 `test(undefined)` 就会得到 `Cannot destructure property name of 'undefined' or 'null'` ，因为在 `undefined` 中没有 `name` 这个属性。

如果你不介意使用第三方库，这里有几种方法减少空检查：

- 使用 [Lodash get](https://lodash.com/docs/4.17.15#get)
- 使用facebook的开源库 [idx](https://github.com/facebookincubator/idx)

下面是一个使用 `lodash` 的例子：

```javascript
// Include lodash library, you will get _
function test(fruit) {
  // get property name, if not available, assign default value 'unknown'
  console.log(_.get(fruit, 'name', 'unknown');
}

//test results
test(undefined); // unknown
test({ }); // unknown
test({ name: 'apple', color: 'red' }); // apple
```

## 4. 优先选择 **Map/对象遍历** 而不是 **switch** 语句

下面的代码，我们将按颜色输出水果。

```javascript
function test(color) {
  // use switch case to find fruits in color
  switch (color) {
    case 'red':
      return ['apple', 'strawberry'];
    case 'yellow':
      return ['banana', 'pineapple'];
    case 'purple':
      return ['grape', 'plum'];
    default:
      return [];
  }
}

//test results
test(null); // []
test('yellow'); // ['banana', 'pineapple']
```

上面的代码看起来没有什么问题，但我觉得它相当的啰嗦。同样的结果，我们可以使用更加干净的语法--对象字面量来实现。

```javascript
// use object literal to find fruits in color
  const fruitColor = {
    red: ['apple', 'strawberry'],
    yellow: ['banana', 'pineapple'],
    purple: ['grape', 'plum']
  };

function test(color) {
  return fruitColor[color] || [];
}
```

或许，我们可以使用 **[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)** 来实现同样的结果。

```javascript
// use Map to find fruits in color
  const fruitColor = new Map()
    .set('red', ['apple', 'strawberry'])
    .set('yellow', ['banana', 'pineapple'])
    .set('purple', ['grape', 'plum']);

  function test(color) {
    return fruitColor.get(color) || [];
  }
```

**[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)** 是自 ES2015 起可用的对象类型，允许存储键值对。

我们应该禁止使用 *switch* 语句吗？不需要限制自己使用它。就我个人而言，我会尽可能的使用对象字面量，但是我不会设置严格的规则来禁止使用它，你可以在你的场景中合理使用它。

Todd Motto 有一篇深入讨论了使用 switch 语句和对象字面量的 [文章](https://ultimatecourses.com/blog/deprecating-the-switch-statement-for-object-literals)。

对于上面的例子，我们可以使用 **Array.filter** 来重构上面的代码来得到同样的结果。

```javascript
 const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'strawberry', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'pineapple', color: 'yellow' },
    { name: 'grape', color: 'purple' },
    { name: 'plum', color: 'purple' }
];

function test(color) {
  // use Array filter to find fruits in color

  return fruits.filter(f => f.color == color);
}
```

总有不止一种方法来得到同样的结果。我们在上面展示了4种方法来得到同一个结果。所以说，编程总是有趣的！

## 5. 对全部/部分判断时，使用 **Array.every** 和 **Array.some**

最后一个技巧是利用新的（但不是很新的）Javascript 数组方法去减少代码行数。下面的代码，我们想要检查所有的水果是否是红色的。

```javascript
const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
  ];

function test() {
  let isAllRed = true;

  // condition: all fruits must be red
  for (let f of fruits) {
    if (!isAllRed) break;
    isAllRed = (f.color == 'red');
  }

  console.log(isAllRed); // false
}
```

上面的代码实在太长了，我们可以使用 **Array.every** 来减少代码的行数。

```javascript
const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
  ];

function test() {
  // condition: short way, all fruits must be red
  const isAllRed = fruits.every(f => f.color == 'red');

  console.log(isAllRed); // false
}
```

现在代码是否更加简介了？同样的方式，如果我们想检查这些水果中是否有红色的，我们可以使用 **Array.some** 一行代码实现这效果。

```javascript
const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
];

function test() {
  // condition: if any fruit is red
  const isAnyRed = fruits.some(f => f.color == 'red');

  console.log(isAnyRed); // true
}
```

## 总结

让我们一起生产更多高可读性的代码吧。希望你可以在这篇文章中学到实用的技巧。

初次翻译文章，大部分都是一句一句的翻译，尽己所能努力做到意译，加油！希望这篇翻译能对你有用。
