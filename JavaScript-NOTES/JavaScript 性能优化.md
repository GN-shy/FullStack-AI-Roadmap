# JavaScript 性能优化
## 一、代码层面优化
### 1. 循环优化
- 避免在循环条件中重复计算属性，提前缓存变量
```js
// 不推荐
for (let i = 0; i < arr.length; i++) {}

// 推荐
const len = arr.length
for (let i = 0; i < len; i++) {}
```
- 优先使用 `for`、`while` 循环，高频场景减少高阶函数嵌套
- 不在循环内部创建函数、对象，避免频繁开辟内存

### 2. 减少重复运算
- 缓存频繁访问的对象属性、数组元素
- 公共计算逻辑抽离为独立函数，避免重复执行

### 3. 变量使用规范
- 优先使用局部变量，局部变量访问速度优于全局变量
- 多用字面量创建对象、数组，替代 `new Object()`、`new Array()`

## 二、DOM 操作优化
### 1. 减少 DOM 重绘与重排
- 避免频繁增删改 DOM 节点，使用 `DocumentFragment` 在内存中拼接结构，再一次性插入页面
```js
const ul = document.querySelector('ul')
const frag = document.createDocumentFragment()
for (let i = 0; i < 50; i++) {
  const li = document.createElement('li')
  frag.appendChild(li)
}
ul.appendChild(frag)
```
- 统一修改类名 `class` 替代逐行修改行内样式
- 动画优先使用 `transform`、`opacity`，仅触发合成层，不产生重排

### 2. 合理获取 DOM
- 重复使用的 DOM 节点提前缓存，不重复调用选择器
- 避免循环内连续读取 `offset`、`scroll` 等布局属性，防止强制同步布局

## 三、事件优化
### 1. 事件委托（事件代理）
利用事件冒泡，将子元素事件统一绑定到父元素，减少事件绑定数量，提升性能与可维护性
```js
// 父元素统一绑定事件
document.querySelector('ul').addEventListener('click', function(e) {
  if (e.target.tagName === 'LI') {
    console.log('点击了列表项')
  }
})
```

### 2. 防抖与节流（重点）
用于限制高频触发事件（输入、滚动、点击、窗口缩放）的执行频率，是前端核心性能优化手段。

---
### （一）防抖 Debounce
#### 1. 核心原理
事件持续触发时**重置计时**，只有当触发停止，且等待指定时长后，函数才执行**一次**。
通俗理解：多次触发，只认最后一次。

#### 2. 适用场景
- 搜索框输入联想、输入框实时校验
- 窗口 `resize` 窗口大小监听
- 防止按钮连续重复点击、表单重复提交
- 编辑器自动保存

#### 3. 基础版防抖（非立即执行）
```js
/**
 * 防抖函数
 * @param {Function} fn 目标执行函数
 * @param {number} delay 延迟毫秒数
 * @returns {Function} 防抖处理后的函数
 */
function debounce(fn, delay = 500) {
  let timer = null
  // 返回新函数承接事件触发
  return function(...args) {
    // 每次触发都清除上一次定时器，重置计时
    if (timer) clearTimeout(timer)
    // 重新开启定时器
    timer = setTimeout(() => {
      // 保留原函数this和参数
      fn.apply(this, args)
      timer = null
    }, delay)
  }
}
```

#### 4. 立即执行版防抖
首次触发立即执行函数，后续连续触发期间不再执行，等待停止触发并延时后，才允许再次执行。适合防重复点击。
```js
/**
 * 完整版防抖：支持立即执行
 * @param {Function} fn 目标函数
 * @param {number} delay 延迟时间
 * @param {boolean} immediate true=立即执行，false=延迟执行
 * @returns {Function} 防抖函数
 */
function debounce(fn, delay = 500, immediate = false) {
  let timer = null
  return function(...args) {
    const that = this
    if (timer) clearTimeout(timer)

    if (immediate) {
      // 无定时器时代表可以执行
      const canRun = !timer
      timer = setTimeout(() => {
        timer = null
      }, delay)
      if (canRun) fn.apply(that, args)
    } else {
      timer = setTimeout(() => {
        fn.apply(that, args)
        timer = null
      }, delay)
    }
  }
}
```

#### 5. 实际使用示例
```js
// 1. 搜索框输入防抖（延迟执行）
const input = document.querySelector('#search')
const searchFn = debounce((val) => {
  console.log('请求搜索接口：', val)
}, 300)
input.addEventListener('input', e => searchFn(e.target.value))

// 2. 提交按钮防抖（立即执行，防连点）
const btn = document.querySelector('#submit')
const submitFn = debounce(() => {
  console.log('提交表单')
}, 1000, true)
btn.addEventListener('click', submitFn)
```

---
### （二）节流 Throttle
#### 1. 核心原理
在**固定时间间隔内**，无论事件触发多少次，函数**仅执行一次**。
通俗理解：持续触发，按固定频率执行，到点才执行。

#### 2. 适用场景
- 页面滚动 `scroll` 监听
- 鼠标移动 `mousemove` 事件
- 拖拽、页面滚动加载、游戏帧率控制

#### 3. 时间戳版节流（触发立即执行，间隔内不再执行）
特点：首次触发马上执行，停止触发后，不会再补执行最后一次。
```js
/**
 * 节流 - 时间戳版
 * @param {Function} fn 目标函数
 * @param {number} interval 时间间隔(ms)
 * @returns {Function} 节流函数
 */
function throttle(fn, interval = 500) {
  let lastTime = 0
  return function(...args) {
    const nowTime = Date.now()
    // 当前时间 - 上一次执行时间 >= 间隔时间，才执行
    if (nowTime - lastTime >= interval) {
      fn.apply(this, args)
      lastTime = nowTime
    }
  }
}
```

#### 4. 定时器版节流（延迟执行）
特点：首次触发延迟执行，停止触发后，会再执行最后一次。
```js
/**
 * 节流 - 定时器版
 * @param {Function} fn 目标函数
 * @param {number} interval 时间间隔(ms)
 * @returns {Function} 节流函数
 */
function throttle(fn, interval = 500) {
  let timer = null
  return function(...args) {
    if (!timer) {
      timer = setTimeout(() => {
        fn.apply(this, args)
        timer = null
      }, interval)
    }
  }
}
```

#### 5. 综合版节流（结合时间戳+定时器，兼顾首尾执行）
兼顾首次立即执行、停止触发后收尾执行，项目中最常用。
```js
function throttle(fn, interval = 500) {
  let lastTime = 0
  let timer = null
  return function(...args) {
    const nowTime = Date.now()
    // 剩余时间不足，设置定时器收尾
    const remain = interval - (nowTime - lastTime)
    if (remain <= 0) {
      // 到达间隔，立即执行
      if (timer) {
        clearTimeout(timer)
        timer = null
      }
      fn.apply(this, args)
      lastTime = nowTime
    } else if (!timer) {
      // 未到间隔，延迟执行最后一次
      timer = setTimeout(() => {
        fn.apply(this, args)
        lastTime = Date.now()
        timer = null
      }, remain)
    }
  }
}
```

#### 6. 节流使用示例
```js
// 页面滚动节流
const scrollFn = throttle(() => {
  console.log('监听页面滚动位置')
}, 200)
window.addEventListener('scroll', scrollFn)
```

---
### （三）防抖与节流对比
1. **防抖**：多次触发，只执行最后一次（或第一次），适合输入、按钮点击
2. **节流**：固定周期内只执行一次，控制执行频率，适合滚动、鼠标移动

## 四、内存优化
### 1. 避免内存泄漏
- 定时器、事件监听使用完毕后及时清除、解绑
- 不再使用的全局变量、大对象手动置空，切断引用
- 合理使用闭包，避免闭包长期持有无用DOM/数据

### 2. 数据缓存
对重复请求、重复计算的结果进行缓存，减少重复开销，可使用 `Object`、`Map` 实现简易缓存。

## 五、脚本加载与懒加载
### 1. 脚本加载优化
- 非核心脚本放在页面底部，避免阻塞页面渲染
- 使用 `defer` / `async` 异步加载JS
```html
<!-- 顺序执行，HTML解析完后运行 -->
<script src="a.js" defer></script>
<!-- 加载完成立即执行，不保证顺序 -->
<script src="b.js" async></script>
```

### 2. 懒加载
- 图片使用原生 `loading="lazy"` 实现懒加载
- 路由、组件使用动态 `import()` 实现代码分割与懒加载

## 六、数据结构与算法优化
- 去重、快速查找场景优先使用 `Set`、`Map`
- 尽量减少嵌套循环，降低代码时间复杂度
- 大数据遍历选择高效遍历方式

## 七、性能调试工具
1. `console.time()` / `console.timeEnd()`：手动统计代码执行耗时
```js
console.time('test')
// 测试代码
for(let i = 0; i < 100000; i++){}
console.timeEnd('test') // 输出耗时
```
2. 浏览器 DevTools
- **Performance**：分析运行时性能、卡顿
- **Memory**：查看内存占用、排查内存泄漏

