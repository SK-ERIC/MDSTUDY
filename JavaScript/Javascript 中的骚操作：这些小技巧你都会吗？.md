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

需要特别注意：

1. `~` 会将数字转换为 32 位整数后取反
2. 当元素在索引 0 位置时：`~0 === -1`（truthy 值），此时判断仍然有效
3. 但若数组中包含`-1`这个元素时会有误判风险
4. 现代开发建议使用`includes()`方法更直观

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

现代写法（ES6+）建议使用默认参数，更精准判断 undefined：

```javascript
function greet(name = "匿名") {
  console.log("你好，" + name);
}
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

```javascript
// 对象解构配合剩余运算符
const user = { id: 1, name: "Alice", age: 25 };
const { id, ...userInfo } = user;
console.log(id); // 1
console.log(userInfo); // {name: "Alice", age: 25}
```

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

#### 9. 可选链操作符 `?.`：防止报错的优雅方式

处理嵌套对象时，不再需要写冗长的 `&&` 链式判断：

```javascript
const user = {
  profile: {
    name: "Bob",
    address: null,
  },
};

// 传统写法
const city =
  user && user.profile && user.profile.address && user.profile.address.city;

// 可选链写法
const safeCity = user?.profile?.address?.city || "未知城市";
console.log(safeCity); // "未知城市"
```

#### 10. 空值合并运算符 `??`：更精准的默认值

比 `||` 更严格地判断 null/undefined：

```javascript
const config = {
  timeout: 0,
  title: "",
};

console.log(config.timeout || 100); // 100（但0是有效值！）
console.log(config.timeout ?? 100); // 0

console.log(config.title || "默认标题"); // "默认标题"
console.log(config.title ?? "默认标题"); // ""
```

#### 11. 对象转换黑科技

快速在对象和数组间转换：

```javascript
// 对象转键值对数组
const obj = { a: 1, b: 2 };
const arr = Object.entries(obj); // [['a', 1], ['b', 2]]

// 数组转对象
const newObj = Object.fromEntries(arr); // { a: 1, b: 2 }

// 快速创建数字序列
const numbers = Array.from({ length: 5 }, (_, i) => i); // [0, 1, 2, 3, 4]
```

#### 12. 数组的现代操作

```javascript
// 快速去重
const nums = [1, 2, 2, 3];
const unique = [...new Set(nums)]; // [1,2,3]

// 数组拍平
const nested = [1, [2, [3]]];
nested.flat(2); // [1, 2, 3]
// 参数2表示拍平层级深度，默认1层
console.log([1, [2, [3]]].flat()); // [1, 2, [3]]
console.log([1, [2, [3]]].flat(1)); // 同上
console.log([1, [2, [3]]].flat(Infinity)); // 无限层级拍平 [1,2,3]

// 过滤假值
const mixed = [0, "", "hello", null, undefined];
const truthy = mixed.filter(Boolean); // ["hello"]
```

#### 13. 数字分隔符

增强大数字的可读性：

```javascript
const billion = 1_000_000_000; // 等同于1000000000
const hex = 0xde_ad_be_ef; // 十六进制表示
```

#### 14. ES2023 数组方法：更优雅的数组操作

```javascript
// 从后往前查找元素（需要ES2023+）
const arr = [5, 12, 8, 130, 44];
console.log(arr.findLast((n) => n > 10)); // 44（从后往前第一个符合条件的元素）

// 不可变排序（需要ES2023+）
const sorted = arr.toSorted((a, b) => a - b); // 返回新数组
console.log(arr); // 原数组保持原样 [5, 12, 8, 130, 44]

// 带索引的数组遍历（其实一直都有，但很多人不知道）
arr.forEach((item, index) => {
  console.log(`索引 ${index}: 值 ${item}`); // 用字符串模板更直观
});

// toSorted() 是ES2023新增方法，返回新数组
const original = [3, 1, 2];
const sorted = original.toSorted();
console.log(original); // [3, 1, 2]（原数组不变）
console.log(sorted); // [1, 2, 3]
```

#### 15. Promise 技巧：异步操作更流畅

```javascript
// 并行请求处理（注意错误会丢失其他结果）
const [user, posts] = await Promise.all([
  fetch("/user").catch((e) => null),
  fetch("/posts").catch((e) => null),
]);

// 带超时的请求（竞速模式）
function fetchWithTimeout(url, timeout = 5000) {
  const controller = new AbortController();
  // AbortController 用于真正取消网络请求
  const timeoutId = setTimeout(() => controller.abort(), timeout);

  return Promise.race([
    fetch(url, { signal: controller.signal }),
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error("请求超时")), timeout)
    ),
  ]).finally(() => clearTimeout(timeoutId));
}

// 错误处理更优雅（可省略catch参数）
async function loadData() {
  try {
    return await fetchData();
  } catch {
    // 不接收error参数时自动忽略
    return getCacheData(); // 直接降级到本地缓存
  }
}
```

#### 16. URL 解析黑科技

```javascript
// 解析URL比正则更可靠
const url = new URL("https://example.com:8080/path?name=Alice#section");
console.log(url.password); // 自动解析出空密码字段
console.log(url.searchParams.getAll("name")); // 总是返回数组 ['Alice']

// 快速构建带查询参数的URL
const newUrl = new URL("https://example.com/search");
newUrl.searchParams.set("q", "现代JS技巧"); // 自动编码中文
console.log(newUrl.href); // 'https://example.com/search?q=%E7%8E%B0%E4%BB%A3JS%E6%8A%80%E5%B7%A7'
```

#### 17. 其他实用技巧

```javascript
// 使用Symbol创建唯一标识
// Symbol 是一种原始数据类型，用于创建唯一的属性名，避免与其他属性冲突
const PRIVATE_KEY = Symbol("secret");
const obj = {
  [PRIVATE_KEY]: "s3cr3t",
  public: "open",
};

// 动态导入模块
// 动态导入允许按需加载模块，减少初始加载时间，适用于懒加载场景
const module = await import("/path/to/module.js");

// 顶层 await (ES2022)
// 顶层 await 允许在模块顶层直接使用 await，但仅适用于 ES 模块
const data = await fetchData(); // 在模块顶层直接使用

// 使用 Object.hasOwn 替代 hasOwnProperty
// Object.hasOwn 是 ES2022 引入的，用于检查对象是否拥有指定的自有属性
// 它比 hasOwnProperty 更简洁，且避免了原型链上的冲突
const person = { name: "Alice" };
console.log(Object.hasOwn(person, "name")); // true
```

---

### 结语

以上这些骚操作和小技巧，能让你用 JavaScript 写出更简洁、更高效的代码。无论是处理时间、数组，还是快速判断真假值，这些技巧都能在日常开发中派上用场。

不过，骚归骚，实用才是最重要的。在炫技的时候，也要注意代码的可读性，毕竟能让自己和团队都看懂的代码才是好代码。

在使用这些现代特性时，请注意目标运行环境的支持情况。对于生产环境，建议配合 Babel 等编译工具使用，并始终把代码可维护性放在第一位。
