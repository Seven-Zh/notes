---
typora-root-url: media
---

# 一、Ajax基础

## 1.传统网站中出现的问题

1.网速慢的情况下，页面加载时间长，用户只能等待

2.表单提交后，如果一项内容不合格，需要重新填写所有表单内容

3.页面跳转，重新加载页面，造成资源浪费，增加用户等待时间



## 2.Ajax概述

Ajax(阿贾克斯)：它是浏览器提供的一套方法，可以实现页面无刷新更新数据，提高用用户浏览网站应用的体验

简单的说就是可以在用户浏览网页时，局部刷新网页数据



## 3.Ajax的应用场景

1.页面上拉加载更多数据
2.列表数据无刷新分页
3.表单项离开焦点数据验证
4.搜索框提示文字下拉列表



## 4.Ajax的运行环境

Ajax技术需要**运行在网站环境中才能生效**，即需要通过浏览器访问域名的方式(不能直接双击HTML文件)





# 二、Ajax运行原理及实现

## 1.Ajax运行原理

Ajax相当于浏览器发送请求与接收响应的**代理人**，以实现在不影响用户浏览贞面的情兄下，局部更新页面数据，从而提高用户体验

![image-20200530162315720](/../笔记.assets/image-20200530162315720.png)



## 2.Ajax的实现步骤

1.创建Ajax对象

`var xhr = new XMLHttpRequest();`

2.告诉Ajax请求地址及请求方式

`xhr.open('get' , 'http://www.itheima.com');`

3.发送请求

`xhr.send();`

4.获取服务器端给与客户端响应的数据

`xhr.onload = function() {`

​		`console.log(xhr.responseText); `

`}`

**xhr.responseText就是服务器端响应的数据**

.

## 3.服务器端响应的数据格式

真实项目中，服务器端大多数情况下会以**JSON对象作为响应数据的格式**，客户端需要将JSON对象和HTML字符串进行拼接，然后显示在页面中

在http请求与响应的过程中，无论是请求参数还是响应内容，如果是对象类型，最终都会被转换为象字符串进行传输

**用JSON.parse()方法将json字符串转换为json对象**



## 4.请求参数传递

### 4.1GET请求参数

get请求参数需要自己去拼接

`xhr.open('get','http://www.itheima.com?name=zhangsan&age=18')`

代码示例：

![image-20200530204954473](/../笔记.assets/image-20200530204954473.png)



### 4.2POST请求参数

POST请求方式**必须设置请求参数的类型**，并且**请求参数必须写在send方法中**

`xhr.setRequestHeader('Contend-Type','application/x-www-form-urlencoded');`

`xhr.send('name=zhangsan&age=18')`

代码示例：

![image-20200530211126309](/../笔记.assets/image-20200530211126309.png)



## 5.请求参数的格式

### 5.1application/x-www-form-urlencoded

键值对的形式

name=zhangsan&age=20



### 5.2application/json

JSON对象的格式，请求方式必须是post

{name:'zhangsan',age:'20'}

**请求头要这样写**：

`xhr.setRequestHeader('Contend-Type','application/json');`

**同时，在服务器端，也不能用**

`app.use(bodyParser.urlencoded())`

**而要用**

`app.use(bodyParser.json())`

**来解析json对象**

![image-20200530214610077](/../笔记.assets/image-20200530214610077.png)



## 6.另外一种获取服务器端响应的方式(了解)

![image-20200530215344017](/../笔记.assets/image-20200530215344017.png)

在xhr.open之后要用**onreadystatechange**方法监听

![image-20200530215941568](/../笔记.assets/image-20200530215941568.png)

**注意：这个方法兼容IE低版本浏览器**



## 7.Ajax错误处理

1.网络畅通，服务器端能接收到请求，服务器端返回的结果不是预期结果。
**可以判断服务器端返回的状态码，分别进行处理。 xhr.status获取http狀态码**

![image-20200530221706544](/../笔记.assets/image-20200530221706544.png)



2.网络畅通，服务器端没有接收到请求，返回404状态码。
**一般是请求地址写错了，检查请求地址是否错误。**



3.网络畅通，服务器端能接收到请求，服务器端返回500状态码。
**服务器端错误，找后端程序员进行沟通**



4.网络中断，请求无法发送到务器端。
**会触发xh对象下面的 onerror事件，在 onerror事件处理函数中对错误进行处理。**

![image-20200530223306094](/../笔记.assets/image-20200530223306094.png)





## 8.低版本IE浏览器的缓存问题

**问题**：在低版本的浏览器中，Ajax请求有严重的存问题，即在请求地址不发生变化的情兄下，**只有第一次请求会真正发送到服务器端**，**后续的请求都会从浏览器的缓存中获取结果**。即使服务器端的数据更新了，客户端依然拿到的是缓存中的旧数据



**解决方案**：在请求地址的后面加请求参数，保证每一次请求中的请求参数的值不相同。**但是这个参数不能和已经有的参数相同**

![image-20200530230052617](/../笔记.assets/image-20200530230052617.png)





# 三、Ajax异步编程

## 1.Ajax异步编程

整个Ajax请求是一个异步过程



## 2.Ajax封装

`Object.assign(obj1 , obj2);`

浅拷贝，后面对象的属性会覆盖前边对象的属性







# 四、模板引擎

## 1.模板引擎概述

通常情况下服务器端是将数据和模板拼接好之后再传到客户端，但是在使用了Ajax技术之后，**服务器端通常是发送的JSON格式数据给Ajax**，这样就需要在客户端进行模板拼接。



## 2.使用方法

1.官网下载art-template的js文件，并在html文件中引入

![image-20200602204110829](/../笔记.assets/image-20200602204110829.png)



2.准备art-template模板

注意：这个模板是html文件中的一个片段，用script标签包裹，有id属性，type类型设置为text/html，这样浏览器才会把这段代码当html文件解析

![image-20200602204311721](/../笔记.assets/image-20200602204311721.png)



3.告诉模板引擎将哪个模板和哪个数据拼接

![image-20200602204443477](/../笔记.assets/image-20200602204443477.png)

通过**id属性**告诉模板引擎哪个模板



4.将拼接好的html字符串添加到页面中

![image-20200602204646785](/../笔记.assets/image-20200602204646785.png)



5.通过模板语法告诉模板引擎，数据和html字符串要如何拼接，**语法与node.js中一样**

![image-20200602204740537](/../笔记.assets/image-20200602204740537.png)





# 五、FormData对象

## 1.FormData对象的作用

1.模拟HTML表单，相当于将HTML表单映射成表单对象，**自动将表单对象中的数据拼接成请求参数的格式**

但是请求方式必须是POST



2.异步上传二进制文件



## 2.FormData对象的使用

1.准备HTML表单

![image-20200603181510144](/../笔记.assets/image-20200603181510144.png)

不需要action和method属性，但是表单控件必须有name属性



2.将HTML表单转化为FormData对象

![image-20200603181628628](/../笔记.assets/image-20200603181628628.png)



3.提交表单对象

![image-20200603181647159](/../笔记.assets/image-20200603181647159.png)



## 注意：

POST方式提交的FormData参数不能用bodyparser来解析，需要用**formidable**

![image-20200603182103113](/../笔记.assets/image-20200603182103113.png)



## 3.FormData对象的实例方法

1.获取表单对象属性中的值

`formData.get('key')`



2.设置表单对象属性中的值

`formData.set('key ' , 'value');`

如果原来有该属性，则重新设置

如果没有，新创建一个属性

可以创建一个新的表单对象，再往里面插入值



3.删除表单对象属性中的值

`formData.delete('key');`

多用在设置两次密码时，删除第二次密码



4.向表单对象中追加属性

`formData.append('key' , 'value');`



set方法和append方法的区别：在属性名已存在的情况下，**set方法会直接覆盖原有属性值，append方法会保留两个值**



## 4.FormData 二进制文件上传

![image-20200603215849768](/../笔记.assets/image-20200603215849768.png)

file上传的文件都存在files属性中，该属性的值是一个集合

代码示例：

![image-20200603222107076](/../笔记.assets/image-20200603222107076.png)



### 文件上传进度条

在xhr对象中有upload中有个onprogress事件，在文件上传时，会持续触发

`xhr.upload.onprogress = function() {}`

![image-20200603223816388](/../笔记.assets/image-20200603223816388.png)



### 图片即时预览

文件上传完成后，返回文件在服务器上的路径

![image-20200603225957491](/../笔记.assets/image-20200603225957491.png)

动态创建img，待加载完成，将图片显示在页面中

![image-20200603225937850](/../笔记.assets/image-20200603225937850.png)





# 六、同源政策

## 1.Ajax请求限制

Ajax只能向自己的服务器发送请求，就是请求限制。如A网站的html文件不能像B网站发送Ajax请求。



## 2.同源

如果两个页面有相同的协议、域名和端口，那么这两个页面属于同源。只要有一个不同，就不同源。



## 3.目的

同源政策的目的是为了保证用户信息的安全，防止恶意网站窃取用户信息



## 4.JSONP解决同源限制

JSONP是json with padding 的缩写，不属于Ajax请求，但可以模拟Ajax请求

方法就是将不同源服务器请求地址卸载scrip标签的src属性中，请求不受同源政策影响



步骤：

1.将不同源服务器端请求地址写在script标签的src属性中

![image-20200604133216928](/../笔记.assets/image-20200604133216928.png)



2.服务器响的数据是字符串，**内容必须是一个函数的调用，函数内的形参必须是用户需要的数据**

![image-20200604133418188](/../笔记.assets/image-20200604133418188.png)

**这里可以直接用res.jsonp()方法返回数据**



3.客户端全局作用域下定义函数fn

![image-20200604133447718](/../笔记.assets/image-20200604133447718.png)



4.fn函数内部对服务器端返回的数据进行处理

![image-20200604133518632](/../笔记.assets/image-20200604133518632.png)



## 5.CORS跨越资源共享

CORS：全称为 **Cross-origin resource sharing**，即跨域资源共享，它允许浏览器向跨域服务器发送Ajax请求，克服了Ajax只能同源使用的限制

![image-20200604183836106](/../笔记.assets/image-20200604183836106.png)



代码示例：

客户端的代码还是原来的Ajax请求代码，没有任何变化

服务器端：

![image-20200604191206568](/../笔记.assets/image-20200604191206568.png)



## 6.访问非同源数据  服务器端解决方案

![image-20200604191308491](/../笔记.assets/image-20200604191308491.png)

即A网页向A服务器端发送请求，A服务器再向B服务器发送请求，B服务器响应给A服务器，A服务器再响应回A客户端网页

**需要用到request模块**



代码示例：

![image-20200604192146167](/../笔记.assets/image-20200604192146167.png)



## 7.cookie

![image-20200604211153548](/../笔记.assets/image-20200604211153548.png)



## 8.withCredentials属性

在使用Ajax技术发送跨域请求时，默认情兄下不会在请求中携带 cookie信息。

**with Credentials：**指定在涉及到跨域请求时，是否携带 cookie信息，默认值为 false 

Access- Control-Allow- Credentials:true允许客户端发送请求时携带 cookie

![image-20200604221341664](/../笔记.assets/image-20200604221341664.png)

![image-20200604221418470](/../笔记.assets/image-20200604221418470.png)





# 七、jQuery使用Ajax

## 1.jQuery中的$.ajax()方法

作用：发送ajax请求

![image-20200604222041712](/../笔记.assets/image-20200604222041712.png)



**域名、协议、端口都相同的情况下，就不用写全地址了，url中可以直接写/base**



### 1.1.ajax()中传递参数

该方法中，不管是对象格式，还是字符串格式，**默认会将将参数转换为'name="zhangsan"&age=20'的键值对的格式**。

如果要传递JSON对象，要改contentType，还要将参数转化为JSON对字符串的格式

![image-20200604223124364](/../笔记.assets/image-20200604223124364.png)



### 1.2serialize方法将表单中数据自动拼接成字符串

![image-20200604223427994](/../笔记.assets/image-20200604223427994.png)



### 1.3serializeArray方法

作用可以将表单中数据转化成如下格式的数组

![image-20200605153411014](/../笔记.assets/image-20200605153411014.png)



## 2.$ajxa()方法发送jsonp请求

![image-20200605155920490](/../笔记.assets/image-20200605155920490.png)

如果指定了函数名称，在全局作用域必须定义一个相应的函数



## 3.$.get()和$.post()方法

两个方法分别用于发送get请求和post请求

![image-20200605160515705](/../笔记.assets/image-20200605160515705.png)





## 4.jQuery中的Ajax全局事件

### 全局事件

**全局事件**：只要页面中有Ajax请求被发送，对应的全局事件就会被触发

![image-20200605170043251](/../笔记.assets/image-20200605170043251.png)

**必须要绑定在document元素上**



### NProgress

一个插件，在浏览器中显示进度条

使用方法：

![image-20200605170822014](/../笔记.assets/image-20200605170822014.png)





# 八、RESTful风格的API

## 1.RESTful API概述

传统请求地址语意不明确

RESTful API是一套关于设计请求的规范

![image-20200605171807632](/../笔记.assets/image-20200605171807632.png)



## 2.RESTful API的实现

![image-20200605172606494](/../笔记.assets/image-20200605172606494.png)

获取用户信息时，用query对象获取不到id，要用params对象

![image-20200605173240612](/../笔记.assets/image-20200605173240612.png)



# 九、XML基础(了解)

![image-20200605175300497](/../笔记.assets/image-20200605175300497.png)



服务器端需要写响应头

`res.header('content-Type' , 'text/xml');`



客户端接收数据

`xhr.responseXML`



一样文档对象，可以用DOM方法操作