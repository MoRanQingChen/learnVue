vue学习笔记
------
`npm install vue`就可以下载vue了，像普通js一样引用
```html
<div id="demo">
  {{ message }}
</div>
```
```javascript
new Vue({
  el: '#demo',
  data: {
    message: 'Hello World!'
  }
})
```
`el`规定了这个`new Vue`的作用域(scope)，`data`规定了作用域里面的数值

 1. `{{message}}`和angular里的一样，双向数据绑定
 2. 花括号内部支持JavaScript的全功能表达式
 3. 如果想让mssage变量只更新一次不双向绑定的话，改写为`{{*message}}`就OK了
 4. 假如message是一个html元素的话，要三个花括弧才会将message的内容解析为html标签`{{{message}}}`

---
花括号的写法不止可以用于标签内部，也可以是标签`id`、`class`、`style`以及`其他属性`,如：
```javascript
<div id="{{ id }}"></div>
```
----------

```JavaScript
var data = { a: 1 }
var vm = new Vue({
  el:'#example'
  data: data
})
```
可以用`data.a`直接操作`vm.a`，这有点像
```
var a={x:xx}
A.prototype=a
```
然后直接操作`a`就改变了`A`的`prototype`

---

`vm.xx`可以直接访问到`vm`里的`data里的xx属性`，可是如果想直接访问`vm`里的`el`或者`data`之类的**非data内部的属性**，需要加$，如：`vm.$el`

---
因为并不是所有的属性都是直接确定的，有的属性是要计算的，这样的属性写在`computed`里
`computed`里通常是存放的一些方法，使用方法如：
```javascript
var vm = new Vue({
  el: '#example',
  data: {
    a: 1
  },
  computed: {
    b: function () {
      return this.a + 1
    }
  }
})
```


---
**vue指令**


---
**vue过滤器**

---
**v-bind绑定class和style**
```html
<div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB }"></div>
```
```javascript
var vm = new Vue({
  el: 'div',
  data: {
  isA: true,
  isB: false
}})
```
`{'类名':data属性}`，data属性为真时，类名被渲染出来。
渲染结果为:
```html
<div class="static class-a"></div>
```
---
`v-bind:style`就非常直观了，很像css，不过注意的是
```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">Style 对象语法</div>
```
这里bind的style用花括号包起来的，其实是js的对象

---

`bind:class="[data的key，data的key]"`
`bind:style="[data的key，data的key]"`
这两种比较简单了，方括号的形式是直接吧`data`的`value`拿出来拼合在一起

---

`v-if`、`v-else`
```html
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```
这里的`ok`是`data`里的属性，可是要注意了，**并没有花括号！**
而且`v-else`一定在`v-if`后出现，单独出现无效
如果想让`v-if`控制多个连续的元素，用`template`包裹起来，如
```
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```
`v-show`和`v-if`非常像，不同的是`v-show`只是`css`的`display:none`，所以`v-show`切换比`v-if`更快，但是初次加载时也相应的更慢，**适合用于频繁切换的元素**
`v-for`的用法：
```html
<ul id="example">
  <li v-for="item in items">
    {{ $index }}-{{ item.message }}
  </li>
</ul>
```
自带一个` $index`表示索引，同样的`v-for`也可以用于`template`
 

---
***表单相关***

---