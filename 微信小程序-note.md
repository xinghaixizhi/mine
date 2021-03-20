# 微信小程序开发

### 什么是小程序

后面我们会专门整理解释

### 官网

https://mp.weixin.qq.com/

### 账号

目前已对个人开放，注册过程不再赘述

### IDE下载安装

下载安装包，下一步下一步

https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html

### 文档

https://developers.weixin.qq.com/miniprogram/dev/framework/



### 起步

#### 项目配置

`project.config.json`，小程序的项目配置，一般可通过页面交互进行配置修改

#### sitemap配置

`sitemap.json`，配置小程序及其页面能否被微信搜索引擎索引

```
{
  "rules":[{
    "action": "allow",
    "page": "*"
  }]
}
```

上面的配置表示所有页面都会被索引（一般默认不用改）

#### 全局配置

`app.json`, 对小程序进行全局配置

```
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "Weixin",
    "navigationBarTextStyle":"black"
  },
  "style": "v2",
  "sitemapLocation": "sitemap.json"
}
```

- pages,  页面路径列表， 默认第一个元素是首页，示例中的"pages/index/index"
- navigationBarBackgroundColor，导航栏背景颜色， 仅支持HexColor类型的颜色，如`#fff`

- navigationBarTextStyle，导航栏文字样式（颜色） 仅支持 black | white

- 局部页面的配置不用写window，直接写就好，如 logs 页面的配置

  ```
  // logs.json
  {
    "usingComponents": {},
    "navigationBarTitleText": "日志"
  }
  ```

  

#### 文件说明

`*.wxml`,  模板文件

`*.wxss`,  样式文件

`*.js`,  js文件

`*.json`,  配置文件

#### 小程序样式

- 适配，小程序底层支持`rpx`单位，免去在不同设备之间换算像素单位的烦恼

- 设置页面高度， 需要在全局`app.wxss`中为`page`指定高度为`100%`

  ```
  // app.wxss
  page {
    height: 100%;
  }
  ```

- 不能使用`id选择器`



#### 数据绑定

https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/data.html

1. `data`中初始化数据
2. `this.setDate()`修改数据, **修改数据的行为始终是同步的**，无论是在自身的钩子中，还是其他异步的API中，如定时器
3. 单向数据流， model ---> view， vue也是单项数据流，只是实现了数据的双向绑定

#### 事件

https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html

##### 事件分类与绑定

1. 冒泡事件：当一个组件上的事件被触发后，该事件会向父节点传递。

   使用`bind`来绑定，如`bindTouch()`

2. 非冒泡事件：当一个组件上的事件被触发后，该事件不会向父节点传递。

   使用`cath`来绑定，如`catchTouch()`

##### 事件处理

​	事件处理函数与小程序自身的钩子函数平级

#### 路由

https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.switchTab.html

url 前面需要加上`/`，否则会当成相对路径 , 正确的应该像这样,  如` /pages/index/index`

```
toLogs() {
    wx.navigateTo({
      url: '/pages/logs/logs',
    })
},
```

- navigateTo 保留当前页面，跳转到某个页面
- redirectTo 关闭当前页面，跳转到某个页面

- reLaunch 关闭所有页面，跳转到某个页面

#### 生命周期

https://res.wx.qq.com/wxdoc/dist/assets/img/page-lifecycle.2e646c86.png

模拟器有点问题，和官网描述不太一致，debugger不会阻止视图层加载和渲染

#### 组件

##### button

```
<button open-type="getUserInfo" bindgetuserinfo="handleGetUserInfo">获取用户信息</button>
```

- open-type, 微信开发能力， getUserInfo获取用户信息， 点击按钮会弹出授权窗口

- 可以在getuserinfo的事件回调中处理用户信息，如果用户点拒绝， response 的 detail属性会有一个errMsg的值，告诉开发者获取用户信息被拒绝

  ```
  {
  	...,
  	detail:
  		{ errMsg: "getUserInfo:fail auth deny" }
  }
  ```

#### WXML语法

##### 条件渲染

```
  <image wx:if="{{userInfo.avatarUrl}}" src="{{userInfo.avatarUrl}}" class="avatar"></image>
  <button wx:else open-type="getUserInfo" bindgetuserinfo="handleGetUserInfo" class="tapButton">获取用户信息</button>
```

#### 开放接口

##### wx.getUserInfo 

用于授权情况下获取用户信息





### 拓展

#### 阿里矢量图标库

##### 官网

https://www.iconfont.cn/home/index

##### 使用字体图标

1. 将自己需要的图标加入项目（已有或新建）

2. 将生成的css文件保存为`.wxss`

3. 在全局样式文件中引入

   ```
   // app.wxss
   
   @import "/static/iconfont/iconfont.wxss";
   ```

#### utools内网穿透工具

https://www.u.tools/

应用截图

![image-20201115162851530](C:\Users\kean\AppData\Roaming\Typora\typora-user-images\image-20201115162851530.png)



### Tips

#### 事件流的三个阶段

祖先元素捕获到了相应事件向内传递至目标元素，目标元素执行事件对应的操作(自己的回调)，并把事件向外传递，直到被阻止或到达顶层父元素

1. 捕获：从外向内
2. 执行目标阶段
3. 冒泡：从内向外

