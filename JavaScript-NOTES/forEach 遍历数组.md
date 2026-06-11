# forEach 遍历数组

## 一、用法讲解
`forEach` 是数组自带的方法，用于遍历数组的每一个元素，执行指定的回调函数。
- 语法：`数组.forEach((当前元素, 索引, 原数组) => { 要执行的操作 })`
- 特点：
  1.  会自动遍历数组的每一个元素，为每个元素执行一次回调函数
  2.  没有返回值，不会改变原数组（除非手动修改）
  3.  不支持 `break`/`continue`，无法中途停止遍历

---

## 二、基础示例
```javascript
// 定义数组
const people = [
  {
    name: '佩奇',
    family: {
      mother: '猪妈妈',
      father: '猪爸爸',
      sister: '乔治'
    },
    age: 6
  },
  {
    name: '乔治',
    family: {
      mother: '猪妈妈',
      father: '猪爸爸',
      sister: '佩奇'
    },
    age: 4
  }
];

// 1. 基础遍历：打印每个元素
people.forEach((item, index) => {
  console.log(`索引 ${index}：`, item);
});

// 2. 结合解构，直接取出需要的属性
people.forEach(({ name, age, family: { mother } }) => {
  console.log(`${name} 今年 ${age} 岁，妈妈是 ${mother}`);
});
```

---

## 三、常用场景示例
```javascript
// 场景1：遍历接口返回的 data 数组，渲染列表
const msg = {
  code: 200,
  msg: "获取新闻列表成功",
  data: [
    { id: 1, title: "5G商用新闻", count: 58 },
    { id: 2, title: "国际头条速览", count: 56 }
  ]
};

// 取出 data 数组并遍历渲染
const { data } = msg;
data.forEach(({ id, title, count }) => {
  console.log(`新闻ID: ${id}，标题: ${title}，阅读量: ${count}`);
  // 实际开发中，这里可以写 DOM 渲染代码，比如：
  // document.body.innerHTML += `<div>${title}</div>`;
});
```
