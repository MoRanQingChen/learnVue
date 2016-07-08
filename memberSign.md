**访问时项目整体流程：**

* 1、后端处理
先是后端的各种处理、然后到**后端路由**，访问**`membersign.html`**时将会转跳到`web`下的`views`下的`newMemberSign`下的**`Index`**，然后`index`**引用了`dist`下的编译后的**`app.js`**，进入签到项目的单页入口**

* 2、单页入口app.js
`import`加载各种模块引用，然后注册组件
```JavaScript
Vue.component("load", load);   //注册小鱼loading
Vue.component("tab-component", tabComponent);//底部切换组件
var app = new Vue({el:'#app',data:…,methods:…,beforeCompile:…})//全局级的主容器组件
```
其中`app`里的`data`储存着主容器的各种变量，`methods`封装了各种方法
组件即将被初始化时自动调用`beforeCompile`方法
其中的方法主要是用于初始化手机页面的设置，以及检验是否登录，在客户端还是浏览器登录
之后会执行 `(autoLogin)`->`ajaxList`->`setRouter`->`routerConfig(this)`//这里的`this`是`app`
其中`routerConfig` 来自上面的`import`
```
import routerConfig from "router";
```
* 3、进入路由
所以执行到`js/router.js`文件，既然是import来的，所以先看这个JS的入口
```JavaScript
export default function (app) \\接收的app来自上面的this，也就是大容器组件app
{
    var router = new Router(); \\入口内注册一个new Router()
    router.on("/index", config.index); \\并用on进行监听hash变化
    router.on . . . . . 
```
当`url`接收到`#/index`时，触发`router.js`上面定义的`config`对象里面封装的方法
```
index:function()
    {
        app.currentView = "load";//加载小鱼loading
        require.ensure([], function ()
        {
            Vue.component("index", require('index'));//注册一个index组件，内容来自view/index.vue
            app.currentView = "index";//注册此组件的名字，用于日后切换
            app.setTitle("游币玩乐惠", "分享", app.tcShare);//设置app顶部titel
        }, "index");//编译后的名字为index放到dist文件夹下
    },
```
* 4、渲染vue
上面`require('index')`，所以进入`index.vue`


---
**新建时开发流程：**


来做一个test试试看
首先进入工作目录`D:\Work\TCW\8.Web\TCWireless.Touch.Deal.Web\Script\MemberSign`
然后打开配置好的webpack的.bat文件`npm_run_build.bat`
(如果不能运行，请注意`node model`)


* 1、先配置好webpack，打开MemberSign下的webpack.config.js
设置好新建文件的别名，如：
```JavaScript
'test':'view/test' //webpack在require的时候可以直接写test就访问到view下的test.vue文件了
```
然后运行.bat文件`npm_run_build.bat`
* 2、配置路由：src/router.js添加一个router.on，如：
```JavaScript
router.on("/test",config.test);
```
需要页面间穿参数时可以用url，后面加`/:`，如:
```javascript
router.on("/test/:tabIndex",config.test);
```
意思是当别人访问/test时，触发上面config对象里面的test函数
然后我们在config对象里面添加一个test函数
```
test: function(){
        app.currentView = "load";
        require.ensure([], function ()//写给webpack用的，新的页面
        {
            Vue.component("test", require('test'));//设置一个test组件，加载来自view下的test.vue(webpack设置的)
            app.currentView = "test";//根组件下的属性，用于切换组件
            app.setTitle("历届红人榜");//顶部的名字(封装在app里的方法)
        }, "test");//最后编译为test.js放在dist下
    }
```
` require('test')`要调用`test.vue`文件了嘛，所以我们开始写`test.vue`
* 3、写vue文件
所有的vue文件都有相似的结构
```html
<style lang="sass">
    @import "../css/pxtorem.scss";
      /*sass样式*/
</style>
<template>
      <!-- html结构 -->
</template>
<script>
    module.exports={
       //当前组件内容，结构同vue组件
    }
<script>
```
