# 原生AJAX
AJAX就是异步JS和XML，通过AJAX在浏览器中向浏览器中向服务器发送异步请求：实现页面无刷新请求
# XML简介
XML可扩展标记语言
XML被设计用来传输和存储数据
XML和HTML类似，不同的是HTML中都是预定义标签，而XML中没有预定义标签，全都是自定义标签，用来标识一些数据
现在已经被JSON取代了
# AJAX的特点
## AJAX的优点
1、可以无需刷星页面而与服务器端进行通信
2、允许你根据用户事件来更新部分页面
## AJAX的缺点
1、没有浏览历史，不能回退
2、存在跨域问题
3、SEO不友好
# AJAX的使用
## 核心对象
XMLHttpRequest  AJAX的所有操作都是通过该对象进行的
## 使用步骤
在js中创建对象，通过XMLHttpRequest()构造函数
这个实例对象中有readyState这个状态属性，然后通过状态属性判断是否完成
  然后将完成后的xml对象中的response属性的值写入dom中
绑定发送的method和url，通过实例对象的open()方法
然后发送请求
## XMLHttpRequest构造函数
- 创建实例化对象
```javascript
const xhr = new XMLHttpRequest();
```
## 实例化对象中的属性和方法
- 属性
  - **readyState属性**，获取实例化对象的状态
  - **response属性**，获取实例化对象完成后返回的响应
  - **status**,获取响应的状态码
  - **responseType**用于指定返回数据的格式
- 方法
  - **open(**)方法绑定要发送的method和url
  - **onreadystatechange()**方法，当实例对象的装袋发生变化的时候，就会执行一次这个方法
  - **setRequestHeader()**设置请求头的内容，一般在1状态设置
  - **getResponseHeader()**获取请求头的数据，一般在3或者4状态的时候获取
  - **send()**方法发送请求，请求体的参数放在send()中
## 五种状态
- 0就是实例化出来那一颗就是0，初始状态
- 1open已经调用了，但是send()没有调用，此时可以修改请求头的内容
- 2send()已经调用了，无法修改请求头
- 3已经回来一部分了，小的数据会在此阶段一次性接受完毕，较大的数据要进一步接收，响应头已经回来了
- 4数据完美回来了
一般会在4状态的时候做操作

# 关于参数
1、形如：key=value就是query参数
2、形如：/xxx/xxx就是params参数


## 手写ajax请求
```javascript
const btn = document.getElementById('btn');
const content = document.getElementById('content');
//给按钮绑定监听
btn.onclick = () => {
  //创建xhr实例对象
  const xhr = new XMLHttpRequest();
  //xhr内部有五种状态分别是：0、1、2、3、4
  //xhr实例对象在实例出来的那一刻状态就是0
  //绑定监听
  xhr.onreadystatechange = () => {
  };
  //指定发送的：url、method
  xhr.open('GET','http://localhost:8080/test_get?name=laoliu&age=18');
  //发送请求
  xhr.send();
    };
```
post请求，一般post请求的参数和查询字符串，都会放在请求体中，然后传给服务器，get请求一般将参数和查询字符串放在请求头的url中传给服务器，同时post请求也可以在请求头的url中传输

```javascript
const btn = document.getElementById('btn');
const content = document.getElementById('content');
//给按钮绑定监听
btn.onclick = () => {
//创建xhr实例对象
const xhr = new XMLHttpRequest();
//xhr内部有五种状态分别是：0、1、2、3、4
//xhr实例对象在实例出来的那一刻状态就是0
//绑定监听
xhr.onreadystatechange = () => {
  if(xhr.readyState === 4){
    if(xhr.status >= 200 && xhr.status < 300){
      content.innerHTML = xhr.responseText;
    }
  }
};
//指定发送的：url、method
xhr.open('POST','http://localhost:8080/test_post?name=laoliu&age=18');
//发送请求
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
// xhr.setRequestHeader('Content-Type','application/json');
xhr.send('[{name:zhangsan,age:18}]');
// xhr.send('name=zhangsan&age=18');
};
```

# 连续解构赋值
- a:{}表示a值对象


# 回调地狱
就是在AJAX请求中，添加请求，一直添加请求，就会出现回调地狱


# 跨域与同源策略
原因就是浏览器为了安全，而采用的同源策略
## 什么是同源策略
就是网景公司提出的一个著名的安全策略，现在所有javascript的浏览器都会使
用这个策略，web是1构建在同源策略基础之上的，浏览器知识针对同源策略的一
种实现，所谓的同源是指，协议、域名、端口必须完全相同，即：协议、域名、
端口都相同，才能算是在同一个域里

## 非同源受到那些限制
- DOM无法获得
- cookie不能获取
- AjAX请求不能获取数据
## 如何在开发中解决跨域问题
1、JSONP解决发送请求跨域问题
//开发中使用，但是面试的时候会问
是一个非观法的跨域解决方案，只支持get请求的跨域问题，带有外部资源的请求都是get请求
```javascript
 const submit = document.getElementById("submit");
  submit.onclick = () => {
    //同源策略就是可以请求到服务器，但是浏览器会把响应拦截了
    //JSONP解决跨域问题
    //同源策略没有限制标签，通过script标签引入js代码，通过后端传函数中的参数，前端执行代码中的参数，将页面中的js代码写在引入代码的上面
    //动态创建script标签，动态使用地址
    

    /*
    创建script标签
    给节点指点指定src属性
    将节点放在body中
    就是不使用前端中的AJAX发送并请求，利用script标签，在前端调用js函数，在后端发送js函数
    */
   let scriptNode = document.createElement('script');
   scriptNode.src = 'http://localhost:8080/test_jsonp';
   document.body.appendChild(scriptNode);
   window.demo = function(a){
     console.log(a);
   }
  }
```
2、官方解决跨域问题的解决方法
跨域问题：浏览器中有个AJAX引擎，所有XMLHttpRequest发出去的请求都要金国AJAX引擎，可以发请求，但是AJAX引擎会拦住请求，CORS解决同源策略
在返回数据的前方加一个特殊的响应头，当AJAX引擎要拦截的时候，就会响应返回
CORS就靠一组特殊的响应头来解决跨域问题，前端人员不用做什么处理，后端会解决跨域问题，要设置一组响应头，get和post请求都是简单请求，没有预请求，给后端加预请求才能够将复杂请求，才能让响应正常返回
```
Access-Control-Allow-Origin
Access-Control-Expose-Headers
Access-Control-Allow-Methods
```