#Hbuider学习
##一.页面跳转处理
>在App开发中，页面间传值是很常见的开发需求，mui框架根据业务场景不同，提供了两种传值模式。

###1、页面初始化时，通过扩展参数传值；
mui在初始化页面时，提供了extras配置参数，通过该参数可以设置页面参数，从而实现页面间传值；
mui框架在如下几种场景下，会执行页面初始化操作：
- 通过mui.openWindow()打开新页面（若目标页面为已预加载成功的页面，则在openWindow方法中传递的extras参数无效）；
- 通过mui.init()方法创建子页面；
- 通过mui.init()方法预加载页面；
- 通过mui.preload()方法预加载页面；

示例，假设我们有如下需求：
在首页中打开关于页面时，传递当前产品名称及版本号，然后在关于页面中读取这两个参数并显示出来；

首页实现代码：

```
mui.openWindow({
    url:'info.html',
    id:'info.html',
    extras:{
        name:'mui',
        version:'0.5.8'
    }
});
```
关于页面实现代码：

```
var self = plus.webview.currentWebview();
var name = self.name;
var version = self.version;
```
###2、页面已创建，通过自定义事件传值
>在App开发中，经常会遇到页面间传值的需求，比如从新闻列表页进入详情页，需要将新闻id传递过去； Html5Plus规范设计了evalJS方法来解决该问题； 但evalJS方法仅接收字符串参数，涉及多个参数时，需要开发人员手动拼字符串； 为简化开发，mui框架在evalJS方法的基础上，封装了自定义事件，通过自定义事件，用户可以轻松实现多webview间数据传递。

仅能在5+ App及流应用中使用
因为是多webview之间传值，故无法在手机浏览器、微信中使用；
**监听自定义事件**
添加自定义事件监听操作和标准js事件监听类似，可直接通过window对象添加，如下：

```
window.addEventListener('customEvent',function(event){
  //通过event.detail可获得传递过来的参数内容
  ....
});
```
**触发自定义事件**
通过```mui.fire()```方法可触发目标窗口的自定义事件：
- **.file(target,event,data)**
>**target**
Type:*WebviewObject*
需传值的目标webview
**event**
Type:*String*
自定义事件名称
**data**
Type:*JSON*
json格式的数据




- **目标webview必须触发loaded事件后才能使用自定义事件**
>若新创建一个webview，不等该webview的loaded事件发生，就立即使用webview.evalJS()或mui.fire(webview,'eventName',{})，则可能无效；

**示例**
假设如下场景：从新闻列表页面进入新闻详情页面，新闻详情页面为共用页面，通过传递新闻ID通知详情页面需要显示具体哪个新闻，详情页面再动态向服务器请求数据，mui要实现类似需求可通过如下步骤实现：

- 在列表页面中预加载详情页面**（假设为detail.html）**
- 列表页面在点击新闻标题时，首先，获得该新闻id，触发详情页面的newsId事件，并将新闻id作为事件参数传递过去；然后再打开详情页面；
- 详情页面监听newsId自定义事件
列表页面代码如下：

```
//初始化预加载详情页面
mui.init({
  preloadPages:[{
    id:'detail.html',
    url:'detail.html'           
  }
  ]
});

var detailPage = null;
//添加列表项的点击事件
mui('.mui-content').on('tap', 'a', function(e) {
  var id = this.getAttribute('id');
  //获得详情页面
  if(!detailPage){
    detailPage = plus.webview.getWebviewById('detail.html');
  }
  //触发详情页面的newsId事件
  mui.fire(detailPage,'newsId',{
    id:id
  });
//打开详情页面          
  mui.openWindow({
    id:'detail.html'
  });
});
```
详情页面代码如下：

```
//添加newId自定义事件监听
window.addEventListener('newsId',function(event){
  //获得事件参数
  var id = event.detail.id;
  //根据id向服务器请求新闻详情
  .....
});
```

**扩展阅读**
代码块激活字符:   ``` mfire```
