# axios
## HTTP相关
### HTTP请求基本过程
请求分为：请求行、请求头、请求体
响应分为：响应行、响应头、响应体
- 1、浏览器向服务器发送HTTP请求(请求报文)
- 2、后台服务器接收到请求后，处理请求，像浏览器端返回HTTP响应(响应报文)
- 浏览器端接收到响应，解析显示响应体或回调函数
### HTTP请求报文
**请求行**
请求的方式、请求的url

**请求头**
Host、cookie、control

**请求体**
请求携带的数据

### HTTP响应报文
**响应行**
状态码、状态描述
**响应头**
Content-type:text/html;charset=utf-8
Set-cookie:BD_CK_SAM=1;path=/
**响应体**
html/json/js/css/图片...
## 请求方式和请求参数
### 请求的方式
1、GET(索取)：从服务端读取数据  查
2、POST(交差)：向服务器添加新数据  增
3、PUT：更新服务器端已存在的数据  改
4、DELETE：删除服务器端数据  删

### 请求参数
1、query参数
数据包含在请求地址中
2、params参数
数据包含在请求地址中


3、请求体参数
参数包含在请求体中
常用的两种格式
urlencoded

json



## API相关
### API的分类
#### REST API
- 发送请求进行增删改查进行哪一种操作通过请求的方式决定
- 同一个请求路径可以进行多个操作
- 请求方式会用到GET/POST/PUT/DELETE
#### 非REST API
- 请求方式不决定请求的增删改查操作
- 一个请求路径只对应一个操作
- 一般只有GET/POST
## 使用json-server搭建REST API
### json-server是什么
用来快速搭建REST API的工具包
### 使用json-server
1、在线文档
2、下载
3、目标目录下创建数据库json文件：db.json
```
json-server 文件名.json
```
json-server的id属性，都是放在Params中传递参数

## 一般http请求与ajax请求
1、ajax请求是一种特别的http请求
2、对服务器端来说，没有任何泣别，区别在浏览器端
3、浏览器端发送请求：只有XHR或fetch发出的才是ajax请求，其它所有的嗾使非ajax请求
4、浏览器端接收到请求
(1)一般请求：浏览器一般会直接显示响应体数据，也就是我们常说的自动刷新/页面跳转
(2)ajax请求：浏览器不会对外界面进行任何更新操作，只是调用监视的回调函数并传入响应相关数据
# axios的理解和使用
## axios是什么
1、前端最流行的ajax请求库
2、react/vue官方都推荐使用axios发ajax请求
3、文档：http://github.com/axios/axios
## axios特点
1、基本Promise的异步ajax请求库
2、浏览器端/node端都可以使用
3、支持请求/响应拦截器
4、请求/响应数据转换
5、批量发送多个请求
## axios的基本使用
1、axios调用的返回值是Promise实例
2、成功的值叫response，失败的值叫error
3、服务器真正返回的内容在response的data属性中
response对象中的属性
- config请求配置对象，请求相关的东西
- data服务器端返回的数据  
- headers响应头信息
- request就是原始的XMLHttpRequest
- status状态码
- statusText
## axios的基本使用
1、axios调用的返回值是一个Promise实例
2、成功的值叫response，失败的值叫error
3、axios成功的值是一个axios封装的response对象，服务器返回的真正数据在response.data中
4、携带的query参数时，编写配置项叫做params
5、当需要携带Params参数的时候，直接在url处添加
6、可以进行全局默认配置，axios.defaults.baseURL/axios.defaults.headers/axios.defaults.timeout
7、可以创建axios实例设置let axios1 = axios.create(),参数是一个对象
## 拦截器
**请求拦截器**
1、是什么
在真正发送请求前执行的额一个回调函数
2、作用
对所有的请求做统一的处理：追加请求头、追加参数、界面loading提示信息等
**响应拦截器**
1、是什么
得到响应之后执行的一组回调函数，成功和失败的响应拦截器
2、作用
若请求成功，对成功的数据进行处理
若请求失败，对失败进行进一步操作
失败的返回值，返回一个pending状态的Promise实例
### 基本的取消请求
创建CancelToken，能为一次请求打标识，需要写一个特殊的配置项
需要关闭要传一个标识，
```javascript
const CancelToken = axios.CancelToken;//CancelToken为一次请求打标识
let cancel;
axios.get('',{
  cancelToken: new CancelToken(function executor(c){
    cancel = c;
  })
})
//调用取消
cancel();
```
怎么判断是否是错误还是真的取消请求
isCancel(error)判断错误是否为用户取消请求造成的，输出error.message
只能点击一次，或者发送一次请求
在请求函数最前方加上if(cancel){
  cancel();
}
### 拦截器和取消请求一起使用
在请求拦截器中使用
判断请求是否是重复的，在请求中设置
```javascript
let cancel;
axios.interceptors.request.use((config) => {
  if(cancel){
    cancel();
  }
  config.cancelToken = new CancelToken((c) => {
    cancel = c;
  })
  return config
})
```

使用一下响应拦截器
```javascript
axios.interceptors.response.use(
  response => {
    console.log(response);
    return response
  },
  error => {
    if(isCancel(error)){
      console.log(error.message);
    }esle(
      console.log(error);
    )
    return new Promise(() => {})
  }
)
```
## 批量发送求
axios.all([]),三次同时发送