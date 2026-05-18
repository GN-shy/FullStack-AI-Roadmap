# DOM操作元素内容和属性

## 操作元素内容

目标：能够修改元素的文本更换内容

- DOM对象都是根据标签生成的，所以操作标签，本质上就是操作DOM对象
- 就是操作对象使用的点语法
- 如果想要修改标签元素的里面的内容，则可以使用以下方式：

### 对象.innerText属性

- 将文本内容添加/更新到任意标签位置
- 显示纯文本，不解析标签

举例说明

```javascript
const info = document.querySelector('.info')
// 获取标签内部的文字
// console.log(info.innerText)
// 添加/修改标签内部文字内容
info.innerText = '嗨喽,俺是刘德华~ '
```

### 对象.innerHTML属性

- 将文本内容添加/更新到任意标签位置
- 会解析标签，多标签建议使用模板字符

```javascript
const info = document.querySelector('.info')
// 获取标签内部的文字
// console.log(info.innerHTML)
// 添加/修改标签内部文字内容
info.innerHTML = `嗨喽,俺是<strong>刘德华</strong>~ `

```



## 操作元素属性

### 操作元素常用属性

还可以通过JS设置/修改标签元素属性，比如通过src更换图片

最常见的属性比如：href,title,src等

语法：

```
对象.属性=值
```

```
// 1. 获取元素
const pic = document.querySelector('img')

// 2. 操作元素
pic.src = './images/b02.jpg'
pic.title = '刘德华黑马演唱会'
```

# 页面刷新，图片随机更换
需求

当我们刷新页面，页面中的图片随机显示不同的图片

分析

① 随机显示，则需要用到随机函数
② 更换图片需要用到图片的src属性，进行修改
③ 核心思路
1. 获取图片元素
2. 随机得到图片序号
3. 图片.src = 图片随机路径

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <img src="./images/1.webp" alt="">
    <script>
        function getRandom(N, M) {
            return Math.floor(Math.random() * (M - N + 1)) + N;
        }
        // 1.获取图片对象
        const img = document.querySelector('img');
        // 2.随机得到序号
        const random = getRandom(1, 6);
        // 3.更换图片路径
        img.src = `./images/${random}.webp`;
    </script>
</body>
</html>
```

**2.2 操作元素样式属性**

通过JS设置/修改标签元素的样式属性，比如通过轮播图小圆点自动更换颜色样式，点击按钮可以滚动图片（移动图片的位置left)

 **2.2.1 通过style属性操作css**

语法：

```
对象.style.样式属性=值
```

例：

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>JS 修改元素样式</title>
  <style>
    .box {
      width: 200px;
      height: 200px;
      background-color: pink;
    }
  </style>
</head>
<body>
  <div class="box"></div>

  <script>
    // 1. 获取元素
    const box = document.querySelector('.box');

    // 2. 修改样式属性 对象.style.样式属性 = '值' 别忘了跟单位
    box.style.width = '300px';

    // 多组单词的采取 小驼峰命名法
    box.style.backgroundColor = 'hotpink';
  </script>
</body>
</html>
```

注意：

1. 修改样式通过style属性引出

2. 如果属性有-连接符，需要转换为小驼峰命名法

3. 赋值的时候，需要的时候不要忘记加css单位

   ​

 **2.2.2 通过类名操作css**

如果修改的样式比较多，直接通过style属性修改比较繁琐，我们可以通过借助于css类名的形式。

语法：

```
// active 是一个css类名
元素.className = 'active'
```

注意：

1. 由于 class 是关键字，所以使用 className 去代替
2. className 是使用新值换旧值，如果需要添加一个类，需要保留之前的类名

例：

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>JS 修改类名样式</title>
  <style>
    div {
      width: 200px;
      height: 200px;
      background-color: pink;
    }

    .box {
      width: 300px;
      height: 300px;
      background-color: skyblue;
      margin: 100px auto;
      padding: 10px;
      border: 1px solid #000;
    }
  </style>
</head>
<body>
  <div class="nav"></div>

  <script>
    // 1. 获取元素
    const div = document.querySelector('div');
    // 2. 添加类名  class 是个关键字 我们用 className
    div.className = 'box';
  </script>
</body>
</html>
```

**2.2.3 通过 classList 操作类控制CSS**

- 为了解决className 容易覆盖以前的类名，我们可以通过classList方式追加和删除类名

语法：

```js
// 追加一个类
元素.classList.add('类名')

// 删除一个类
元素.classList.remove('类名')

// 切换一个类
元素.classList.toggle('类名')
```

*使用 className 和 classList 的区别？*

- *修改大量样式的更方便*
- *修改不多样式的时候方便*
- *classList 是追加和删除不影响以前类名*

**2.3 操作表单元素属性**

表单很多情况，也需要修改属性，比如点击眼睛，可以看到密码，本质是把表单类型转换为文本框

正常的有属性有取值的 跟其他的标签属性没有任何区别

获取: DOM对象.属性名
设置: DOM对象.属性名 = 新值

```js
表单.value = '用户名'
表单.type = 'password'
```

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>表单属性操作示例</title>
</head>
<body>
  <input type="text" value="电脑">

  <script>
    // 1 获取元素
    const uname = document.querySelector('input')

    // 2. 获取值  获取表单里面的值 用的 表单.value
    // console.log(uname.value) // 电脑
    // console.log(uname.innerHTML)  innerHTML 得不到表单的内容

    // 3. 设置表单的值
    // uname.value = '我要买电脑'
    // console.log(uname.type)
    // uname.type = 'password'
  </script>
</body>
</html>
```

关键知识点说明

- **获取表单值**：表单元素的内容必须用 `.value` 获取，`.innerHTML` 无法读取输入框内的值。
- **修改表单值**：通过 `表单.value = 新值` 可以动态修改输入框内容。
- **修改表单类型**：通过 `表单.type = 'password'` 可以将文本框转为密码框（常用于 “显示 / 隐藏密码” 功能）。



表单属性中添加就有效果,移除就没有效果,一律使用布尔值表示 如果为true 代表添加了该属性 如果是false 代表移除了该属性

比如： disabled、checked、selected

代码说明

1.  **获取元素**：通过 `document.querySelector('input')` 获取页面中的单选/复选框元素。
2.  **查看状态**：`ipt.checked` 是一个布尔值，`true` 表示选中，`false` 表示未选中。
3.  **设置选中**：给 `ipt.checked` 赋值为 `true`，即可实现自动打勾；赋值为 `false` 则取消勾选。

完整可运行示例

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>JS 实现复选框自动勾选</title>
</head>
<body>
  <input type="checkbox" id="check1"> 我同意用户协议

  <script>
    // 获取复选框元素
    const ipt = document.querySelector('input')

    // 查看初始状态（默认未选中，输出 false）
    console.log(ipt.checked)

    // 设置为选中状态（实现打勾）
    ipt.checked = true
  </script>
</body>
</html>
```

>  disabled 禁用
>
> 作用：让按钮、输入框**不能点、不能输入**
>
> ```
> // 获取按钮
> const btn = document.querySelector('button')
>
> // 禁用按钮
> btn.disabled = true
>
> // 启用按钮
> btn.disabled = false
> ```
>
> 2. checked 勾选（复选框 / 单选框）
>
> 作用：让复选框**自动打勾**
>
> ```
> const box = document.querySelector('input[type="checkbox"]')
>
> // 打勾
> box.checked = true
>
> // 取消勾
> box.checked = false
> ```
>
> 3. selected 下拉选中
>
> 作用：让 `<select>` 里的 `<option>` 自动选中
>
> ```
> const option = document.querySelector('option')
>
> // 让这个选项被选中
> option.selected = true
> ```



**2.4 自定义属性**

标准属性：

标签天生自带的属性，比如 class、id、title 等，可以直接使用点语法操作，比如：disabled、checked、selected

自定义属性：

HTML5 推出了专门的 data- 自定义属性
在标签上一律以 data- 开头
在 DOM 对象上一律以 dataset 对象方式获取

示例：

```html
<body>
  <div data-id="1" data-spm="不知道">1</div>
  <div data-id="2">2</div>
  <div data-id="3">3</div>
  <div data-id="4">4</div>
  <div data-id="5">5</div>

  <script>
    const one = document.querySelector('div')
    console.log(one.dataset.id)    // 1
    console.log(one.dataset.spm)   // 不知道
  </script>
</body> 
```