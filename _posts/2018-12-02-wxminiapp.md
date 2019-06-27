---
layout: post
title: 微信小程序开发总结
date: 2018-12-02
tags: [微信小程序, JS, Promise, async/await]
---
### 小程序语法
1. app.js 声明全局变量  
getApp 获取到小程序全局唯一的 App 实例  
```apple js
App({
  globalData: {
    userInfo: null,
    hasLogin: false,
    backgroundAudioManager: backgroundAudioManager
  },
  onLaunch: function () {
    //调用API从本地缓存中获取数据
    var logs = wx.getStorageSync('logs') || []
    logs.unshift(Date.now())
    wx.setStorageSync('logs', logs)
    if(userinfo.unionId) {
      wx.request({
        url: '',
        data: {
          unionid: userinfo.unionId
        },
        method: 'POST', // OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT
        // header: {}, // 设置请求的 header
        success: function(res){
          //console.log(res);
          if(res.data.content.focus) {
            
            });
          }
        },
        fail: function(e) {
          console.log('请求关注情况失败')
          console.log(e);
        }
      })
    }
   
    wx.cloud.init({
      env:'qmt-test-869cb2',
      traceUser: true,
    })
  },
  
  onPageNotFound(res) {
    console.log('页面不存在:' + res.path);
  },
  onError(error){
    console.log('qmt小程序初始化错误'+error);
  }
})
```
2. app.json
小程序根目录下的 app.json 文件用来对微信小程序进行全局配置  
详情参考[腾讯官网接口app.json部分](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html){:target="_blank"}
```apple js
{
  "pages": [
    "components/cell/index",
    "components/cell-group/index",
    "components/rate/index"
  ],
  "requiredBackgroundModes": [
    "audio"
  ],
  "window": {
    "backgroundColor": "#F6F6F6",
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#000000",
  },
  "tabBar": {
    "color": "#a9b7b7",
    "selectedColor": "#09BB07",
    "list": [
      {
        "selectedIconPath": "image/ic_discovery_show.png",
        "iconPath": "image/ic_discovery.png",
        "pagePath": "pages/discovery/discovery",
        "text": "发现"
      },
      {
        "selectedIconPath": "image/ic_my_show.png",
        "iconPath": "image/ic_my.png",
        "pagePath": "pages/my/my",
        "text": "我的"
      }
    ]
  },
  "networkTimeout": {
    "request": 60000,
    "connectSocket": 60000,
    "uploadFile": 60000,
    "downloadFile": 60000
  },
  "permission": {
    "scope.userLocation": {
      "desc": "用于小程序提示您所在的位置"
    }
  },
  "functionalPages": true,
  "plugins": {
    "WechatSI": {
      "version": "0.2.3",
      "provider": "wx069ba97219f66d99"
    },
    "TTMapPlugin": {
      "version": "1.0.6",
      "provider": "wx5bc2ac602a747594"
    },
    "tencentvideo": {
      "version": "1.3.0",
      "provider": "wxa75efa648b60994b"
    },
    "tucao": {
      "provider": "wx8abaf00ee8c3202e",
      "version": "1.1.4"
    }
  },
  "cloud": true,
  "sitemapLocation": "sitemap.json",
  "navigateToMiniProgramAppIdList": [
    "wx9aec1067417febfd"
]
}
```

3. page生命周期;  
getCurrentPages() 获取当前页面栈,返回一个数组
```apple js
Page({
  data: { // 参与页面渲染的数据
    logs: [],
	abc: '1234'
  },
  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
  },
  /**
   * 生命周期函数--监听页面初次渲染完成
   */
  onReady: function () {
  },
  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {
  },
  /**
   * 生命周期函数--监听页面隐藏
   */
  onHide: function () {
  },
  /**
   * 生命周期函数--监听页面卸载
   */
  onUnload: function () {
  },
  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {
  },
  /**
   * 页面上拉触底事件的处理函数
   */
  onReachBottom: function () {
  },
  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {
  }
})
```

4. require  module  exports 模块
```apple js
输出：
module.exports = config;
引用：
var config = require('../../config');
```
5. project.config.json
可以在项目根目录使用 project.config.json 文件对项目进行配置;  
.js处理逻辑和.wxml展示分离

### js 部分
