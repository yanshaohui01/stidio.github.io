---
layout: post
title: 微信小程序开发总结
date: 2018-12-02
tags: [微信小程序, JS, Promise, async/await]
---
### 小程序语法

1. app.js 声明全局变量，getApp获取到小程序全局唯一的 App 实例  

    ```
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

2. app.json，小程序根目录下的 app.json 文件用来对微信小程序进行全局配置  
详情参考[腾讯官网接口app.json部分](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html){:target="_blank"}

    ```
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

3. page生命周期; getCurrentPages() 获取当前页面栈,返回一个数组

    ```
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

    ```
    输出：
    module.exports = config;
    引用：
    var config = require('../../config');
    ```

5. project.config.json
可以在项目根目录使用 project.config.json 文件对项目进行配置;  
.js处理逻辑和.wxml展示分离

6. 微信小程序中 同步请求（get/post）使用promise封装，避免回到地狱  
   [微信小程序使用promise封装异步请求](https://www.cnblogs.com/weihuan/p/9113944.html){:target="_blank"}
    
    ```
    const requestPromisePost = (myUrl,data) => {
      // 返回一个Promise实例对象
      return new Promise((resolve, reject) => {
        wx.request({
          url: myUrl,
          data: data,
          method: 'POST',
          header: {
            'content-type': 'application/json',
          },
          success: function (res) {//服务器返回数据
            if (res.statusCode == 200) {
              resolve(res);
            } else {//返回错误提示信息
              reject(res.data);
            }
          },
          error: function (e) {
            reject('网络出错');
          }
        })
      })
    }
 
     // 封装get请求
     const requestPromiseGet = (url, data) => {
       var promise = new Promise((resolve, reject) => {
         //网络请求
         wx.request({
           url: url,
           data: data,
           header: {
             'content-type': 'application/json',
             'token': wx.getStorageSync('token')
           },
           success: function (res) {//服务器返回数据
             if (res.statusCode == 200) {
               resolve(res);
             } else {//返回错误提示信息
               reject(res.data);
             }
           },
           error: function (e) {
             reject('网络出错');
           }
         })
       });
       return promise;
     }
 
    module.exports = {
       requestPromiseGet,
       requestPromisePost
    }
    ```
7. 小程序存储全局变量的两种方式：
>    * 存储到app对象上的方式
>
        ```
        var app = getApp()
        app.globalData.mydata = {a:1, b:2};  //存储数据到app对象上
        ```
>    * 存储到数据缓存的方式
>
        ```
        wx.setStorage({
          key: "mydata",
          data: {a:1, b:2},
          success: function () {
            wx.navigateBack();   //返回上一个页面
          }
        })
        ```

### js 部分

1. this 的指向问题  两种解决方案如下 1>  2> 

    ```
    Page({
      data: {
        text: 'init data',
        array: [{text: 'init data'}],
        object: {
          text: 'init data'
        }
      },
      changeText: function() {
        this.data.text = 'changed data'  //这是错误的，因为在JavaScript中this代表当前对象，会随着程序的执行而上下改变--解决办法看下面 1>  2>
       1> var that=this; //复制一份this到临时变量that中
    　　　 this.setData({ //此时OK
          text: 'changed data'
        })
    　　2> 函数名:res=> {
    　　　　this.setData({ //此时OK
          text: 'changed data'
        })
    　　}
      }
    });
    
    ```
2. js中 变量声明const,var,let区别  

    > + const定义的变量不可以修改，而且必须初始化。
    > + var定义的变量可以修改，如果不初始化会输出undefined，不会报错。
    > + let是块级作用域，函数内部使用let定义后，对函数外部无影响。
    
3. 函数声明三种方式

    ```
    1) const myFunction = function(){};
    2) function myFunction(参数可传可不传){};
    3) 箭头函数相当于 匿名函数(可变参数)： (x, y, ...rest) =>{ var i,sum=x+y;for(i=0;i<rest.length;i++){sum += rest[i];} return sum}; 
    ```
4. JS 其他语法总结
>
>   * 5种基本数据类型（undefined / null /  boolean / number / string）（3种也做了引用类型封装，分别是Boolean、Number、String）
>    * 1种复杂的数据类型（Array：数组类型   Date：日期类型  RegExp：正则表达式类型）  
>    * 基本类型用typeof操作符；引用类型用instanceof操作符
>    * 作用域：子函数可以访问父函数的作用域=>祖先函数的作用域=>直到全局作用域
>    * 基本操作符：
    常用的操作符包括：算术操作符、关系操作符、布尔操作符、赋值操作符。  
    == ===  &&   ! !! !!! (undefined null 0)  var a  if(!a){}
>    * 基本控制语句：
    if语句、switch语句、for语句、while-do语句、do-while、for-in语句
    对象.属性 
>    * 分清楚是JSON对象还是json字符串;