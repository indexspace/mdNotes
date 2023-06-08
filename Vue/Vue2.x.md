# ==官方文档==

> 百度 `vue.js`即可

[介绍 — Vue2.x.js (vuejs.org)](https://v2.cn.vuejs.org/v2/guide/)



# 第一个Vue2.x程序

1. 创建一个空文件夹

1. 用 IDEA 打开该文件夹

1. 安装插件 vue.js

 ## 引入vue的CDN

+ ```html
        <!-- 开发环境版本，包含了有帮助的命令行警告 -->
        <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    ```
    
+ ```html
        <!-- 生产环境版本，优化了尺寸和速度 -->
        <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
    ```

## 使用vue (绑定数据)

> MVVM模式 *(model view viewModel)* 
>
> 其中, viewModel的实现者是 vue

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <p id="obj1">                   <!--V: 视图层-->
        {{message}}
    </p>

    <script src="http://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
    <script>
        var vue = new Vue({         <!--内部绑定V元素和M数据-->
            el: "#obj1",
            data: {                 <!--M: 模型层-->
                message: "Hello vue!"
            }
        });
    </script>
</body>
</html>
```



# vue基础

## 简单介绍

### v-bind

+ 文件头`<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">`
+ 用法demo![image-20230522150536315](./image-20230522150536315.png)
+ 效果<img src="./image-20230522150549254.png" alt="image-20230522150549254" style="zoom:150%;" />



### v-if  & v-for

+ 用法demo
    + <img src="./image-20230522152750382.png" alt="image-20230522152750382" style="zoom:67%;" />
    + <img src="./image-20230522152806115.png" alt="image-20230522152806115" style="zoom:67%;" />
+ 效果
    + ![image-20230522152922593](./image-20230522152922593.png)



### 绑定方法  *(v-on:click)*

> 语法demo

```js
var app5 = new Vue({
  el: '#app-5',                           // 指定视图
  data: {                                 // 指定model(数据)
    message: 'Hello Vue.js!'                
  },
  methods: {                              // 指定model(方法)  
    reverseMessage: function () {  
      this.message = this.message.split('').reverse().join('')   
    }
  }
})
```

> 使用demo

```html
<div id="app-5">
  	<p>{{ message }}</p>
  	<button v-on:click="reverseMessage">反转消息</button>
</div>
```



### v-model

> 表单输入与应用状态的双向绑定

```html
<div id="app-6">
  	<p>{{ message }}</p>
  	<input v-model="message"> <!--输入框的默认值为message的值, 输入框内容和message同步更改-->
</div>
```

![vue001](./vue001.gif)



### 组件注册

![image-20230522203705268](./image-20230522203705268.png)



## Vue实例

### 数据与方法

+ <img src="./image-20230522205710743.png" alt="image-20230522205710743" style="zoom:67%;" />
+ <img src="./image-20230522205828551.png" alt="image-20230522205828551" style="zoom:67%;" />



### 生命周期钩子

