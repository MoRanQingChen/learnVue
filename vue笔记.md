vue学习笔记
------
npm install vue就可以下载vue了，像普通js一样引用
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
`{{message}}`和angular里的一样，双向数据绑定
```JavaScript
var data = { a: 1 }
var vm = new Vue({
  el:'#example'
  data: data
})
```
可以用data.a直接操作vm.a，这有点像
```
var a={x:xx}
A.prototype=a
```
`vm.xx`可以直接访问到`vm`里的`data里的xx属性`，可是如果想直接访问`vm`里的`el`或者`data`之类的**非data内部的属性**，需要加$，如：`vm.$el`