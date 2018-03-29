# Vue lesson 2

> 简单的列表渲染和条件渲染

## 引言

上一节，简单的接触了如何使用 `vue.js` 和他简单的响应式渲染。这一节我们去接触如何使用列表渲染和条件渲染。

有了列表渲染能很大的减少我们的重复的代码，而条件渲染带来了很多的便利。

## 列表渲染

列表渲染最基本的使用环境就是做一个简单的list，当我们以前去写一个简单的列表时候可能会这样子

```html
<body>
  <ul>
    <li>1. Javascript基础</li>
    <li>2. CSS基础</li>
    <li>3. Vue.js入门</li>
  </ul>
</body>
```

嗯，看起来也没有太麻烦，不过等等，如果我们要为每一个项添加一个点击事件呢？

然后如果我们要为每一个不同的项，有一个自己的独立的不同于其他项的点击事件，那是不是太可怕了。

接下来我们来看看 vue.js 是怎么处理列表循环的

```html
<body>
  <div id="app">
    <ul>
      <li v-for="item in list">{{ item.text }}</li>
    </ul>
  </div>
  <script>
    let app = new Vue({
      el: '#app',
      data: {
        list: [
          { text: '1. Javascript基础' },
          { text: '2. CSS基础' },
          { text: '3. Vue.js入门' }
        ]
      }
    })
  </script>
</body>
```

[在线例子](https://jsfiddle.net/JefferyLiang/wg4fjod1/)

对应html文件在`/demo/lesson2-1.html`。

上面我们完成了一个列表的渲染，那么现在给它添加一个点击事件。

### 详细说明

我们通过 `v-for` 指令完成了对根据 `data` 里面的 `list` 数组对列表进行渲染。

指令的内容如下 `item in list`，这里做的事就是，根据 `list` 数组遍历出每一个 `item`，能够通过 `item` 去访问每一个数组值里的详细信息，如：`item.text`。

我们可以通过 Javascript 的一个 for 的例子去理解

```javascript

let list = [
  { text: '1. Javascript基础' },
  { text: '2. CSS基础' },
  { text: '3. Vue.js入门' }
]

for (let item of list) {
  console.log(item.text)
}

// => 1. Javascript基础
// => 2. CSS基础
// => 3. Vue.js入门

```

当然我们能够在 `v-for` 里面获取数组的 `key`，指令如下: `(item, index) in list`。

老规矩通过 Javascript 的代码去理解

```javascript
let list = [
  { text: '1. Javascript基础' },
  { text: '2. CSS基础' },
  { text: '3. Vue.js入门' }
]

list.forEach((item, index) => {
  console.log(`${index}: ${item.text}`)
})

// => 1: 1. Javascript基础
// => 2: 2. CSS基础
// => 3: 3. Vue.js入门
```

#### Tips

一般情况下 `v-for` 需要绑定 `key`，我的个人使用习惯时 `<li v-for="(item, index) in list" :key="index"> ... </li>`

---

```html
<body>
  <div id="app">
    <ul>
      <li v-for="item in list" @click="log(item.text)">{{ item.text }}</li>
    </ul>
  </div>
  <script>
    let app = new Vue({
      el: '#app',
      data: {
        list: [
          { text: '1. Javascript基础' },
          { text: '2. CSS基础' },
          { text: '3. Vue.js入门' }
        ]
      },
      methods: {
        log (msg) {
          alert(msg)
        }
      }
    })
  </script>
</body>
```

[在线例子](https://jsfiddle.net/JefferyLiang/h2d62km6/3/)

该 html 文件位置 `/demo/lesson2-2.html`

#### 详细说明

跟上一个例子的区别就在于我们在 Vue 里的 methods 里面定义了一个 `log` 的方法

并且在 `HTML` 上添加了 `@click="log(item.text)"` 完成了。给每一个列表项添加点击方法了。

通过 vue.js 的列表渲染我们能够更方便的去完成我们列表的，列表循环除了这种简单的循环还能循环出组件列表。

## 条件渲染

上面简单的介绍了列表渲染，使用 `v-for` 去渲染列表，现在我们看一下条件渲染。

条件渲染能根据我们定义的条件去决定该 DOM 是否进行渲染。主要使用到的是 `v-if` 指令。

那我们接着上面的那个例子来做

```html
<body>
  <div id="app">
    <ul>
      <li v-for="(item, index) in list" @click="log(item.text)" v-if="index % 2 === 0">{{ item.text }}</li>
    </ul>
  </div>
  <script>
    let app = new Vue({
      el: '#app',
      data: {
        list: [
          { text: '1. Javascript基础' },
          { text: '2. CSS基础' },
          { text: '3. Vue.js入门' },
          { text: '4. HTML基础' },
          { text: '5. SASS入门' }
        ]
      },
      methods: {
        log (msg) {
          alert(msg)
        }
      }
    })
  </script>
</body>
``` 

[在线例子](https://jsfiddle.net/JefferyLiang/nfc19tfh/)

文件本地位置: `/demo/lesson2-3.html`

### 详细说明

在上面的例子我们看到在 `v-for` 指令里面 `(item, index) in list` 。我们遍历 `list` 每一个对应一个 `item`(内容项) 和 `index`(数组对应的键值)。

然后在 `v-if` 指令里面进行了筛选 `index % 2 === 0`，当符合这个条件的时候，该 DOM 才会被渲染出来，这个就是条件渲染。

当 `v-for` 配合 `v-if` 使用的理解方法

我们继续通过 Javascript 去理解一下:

```javascript
let list = [
  { text: '1. Javascript基础' },
  { text: '2. CSS基础' },
  { text: '3. Vue.js入门' },
  { text: '4. HTML基础' },
  { text: '5. SASS入门' }
]

for (let key in list) {
  if (key % 2 === 0) console.log(list[key].text)
}

// 注意数组第一个 key 是 0

// => 1. Javascript基础
// => 3. Vue.js入门
// => 5. SASS入门

```

## 结语

我们这一节初步的去了解了 `列表渲染` 和 `条件渲染` 怎么使用，这些都是我们去继续学习 `vue.js` 的基础，通过 `Javascript` 的思路去理解这些指令，我觉得更适合一个程序员。

后面将开始我们的数据绑定，事件绑定和计算属性的东西了。

时间: 2018-03-29

作者: Jeffery Liang

邮箱: lzh.jeffery@gmail.com

如果要转载请通过邮箱联系我。并声明原文地址。