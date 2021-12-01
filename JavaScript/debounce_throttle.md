# 防抖

```js
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}
```

# 节流

```js
function throttle(fn, delay) {
  let last = 0; // 上次触发时间
  return (...args) => {
    const now = Date.now();
    if (now - last > delay) {
      last = now;
      fn.apply(this, args);
    }
  };
}
```
