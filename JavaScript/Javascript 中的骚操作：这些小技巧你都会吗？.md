### JavaScript 中的骚操作：这些小技巧你都会吗？

JavaScript 是一种让人又爱又恨的语言。它灵活、多变，有时甚至显得有点“皮”，允许你写出让自己五分钟后都看不懂的代码。但是，正是因为这份灵活，JS 也有许多骚操作，这些技巧不仅让代码更简洁，还让你有种在玩魔法的感觉。今天，我们就来看看几种不常见但非常有趣的 JavaScript 高级技巧，让你的代码看起来更优雅、更炫酷。

#### 1. `+new Date()`：时间戳的骚操作

你可能知道 `new Date().getTime()` 可以获取当前时间的时间戳，但我们可以更简单：

```javascript
console.log(+new Date()); // 类似：1696871204353
```

只需要一个 `+`，就能把 `Date` 对象转换为数字，也就是时间戳。这个技巧利用了 `+` 号的隐式转换功能，比 `new Date().getTime()` 简洁不少。看似很骚气，实际上非常实用！

#### 2. `~` 操作符：查找数组元素的新姿势

当你用 `indexOf()` 查找数组中某个元素时，通常要这么写：

```javascript
if (arr.indexOf(2) !== -1) {
  // 找到了
}
```

有点冗长是不是？试试这个骚操作：

```javascript
if (~arr.indexOf(2)) {
  // 找到了
}
```

这行代码看着有点让人摸不着头脑。其实，`~` 是按位非操作符，它会把一个数的每一位取反。因为 `indexOf` 找不到元素时返回 `-1`，`~-1` 变成了 `0`，这是 falsy 值；如果找到了，返回的索引经过 `~` 后则是 truthy 值。这样你就可以省去多余的 `!== -1`，一行搞定查找。

#### 3. `!!` 双重否定：真假判断的捷径

有时候你想快速判断一个值的真假，可以用这个小技巧：

```javascript
console.log(!!1); // true
console.log(!!0); // false
console.log(!!"hello"); // true
console.log(!!""); // false
```

`!!` 是双重否定，先将值转换成布尔值，再否定两次。这个技巧让你可以快速判断值的 truthiness，无论是数字、字符串、对象等，都可以简洁地转换为布尔类型。

#### 4. 取整的小技巧

要去掉小数部分，你可能会习惯用 `Math.floor()` 之类的函数。骚操作来了，以下方法能让你不依赖取整函数，快速完成任务：

- **`~~` 双取反**：

  ```javascript
  console.log(~~4.9); // 4
  console.log(~~-4.9); // -4
  ```

  这个操作符会把数字转换为 32 位整数，类似于 `Math.floor()` 的效果，但更简洁。

- **按位或 `| 0`**：

  ```javascript
  console.log(4.9 | 0); // 4
  console.log(-4.9 | 0); // -4
  ```

  这个技巧同样去掉小数部分，但比 `~~` 更常见。

- **`Math.trunc()`**：

  如果你不想搞复杂，可以直接用 `Math.trunc()`，它是专门为截断小数部分设计的：

  ```javascript
  console.log(Math.trunc(4.9)); // 4
  console.log(Math.trunc(-4.9)); // -4
  ```

  这些方法不仅能让代码简洁，而且在处理大多数情况下都非常高效。

#### 5. 短路运算符：简化条件判断

JavaScript 的 `||` 和 `&&` 不仅可以用来做逻辑判断，还能帮你写出更简洁的代码。

比如，你想给函数参数设置默认值：

```javascript
function greet(name) {
  name = name || "匿名";
  console.log("你好，" + name);
}
```

如果 `name` 是 falsy 值（如 `undefined`），`||` 会让 `name` 取默认值 `'匿名'`。同样地，`&&` 也能让逻辑判断更短更直观：

```javascript
const isLoggedIn = true;
isLoggedIn && console.log("欢迎回来！"); // 条件成立，打印消息
```

#### 6. 解构赋值：优雅地提取对象属性

解构赋值让你可以优雅地从对象中提取属性。假设你有一个嵌套的用户信息对象：

```javascript
const user = {
  name: "Alice",
  address: {
    city: "Wonderland",
    zip: "12345",
  },
};
```

你可以这样直接提取属性：

```javascript
const {
  name,
  address: { city },
} = user;
console.log(name); // Alice
console.log(city); // Wonderland
```

解构赋值避免了反复使用点运算符，让代码简洁易读，尤其在处理复杂嵌套对象时非常有用。

#### 7. 标签模板字符串：灵活处理字符串

ES6 的模板字符串你可能用过，写起来很方便：

```javascript
const name = "Alice";
console.log(`Hello, ${name}!`);
```

但骚气一点的玩法是标签模板字符串，它允许你自定义模板字符串的处理逻辑。来看个例子：

```javascript
function myTag(strings, ...values) {
  return strings
    .reduce((acc, str, i) => acc + str + (values[i] || ""), "")
    .toUpperCase();
}

const name = "Alice";
const place = "Wonderland";
console.log(myTag`Hello ${name} from ${place}!`); // "HELLO ALICE FROM WONDERLAND!"
```

这个标签模板函数会把字符串和插值内容拼接起来并全部转换成大写。标签模板可以处理复杂的模板场景，如格式化字符串、国际化等，非常灵活。

#### 8. 数组展开运算符 `...`：数组处理的万能钥匙

`...` 操作符不仅可以用来合并数组，还能快速展开数组元素：

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]
```

展开运算符还能将数组传递为函数参数：

```javascript
function sum(a, b, c) {
  return a + b + c;
}

const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6
```

不管是合并数组还是传参，`...` 都能让代码更清晰简洁。

---

### 结语

以上这些骚操作和小技巧，能让你用 JavaScript 写出更简洁、更高效的代码。无论是处理时间、数组，还是快速判断真假值，这些技巧都能在日常开发中派上用场。

不过，骚归骚，实用才是最重要的。在炫技的时候，也要注意代码的可读性，毕竟能让自己和团队都看懂的代码才是好代码。
