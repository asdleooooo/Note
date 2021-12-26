## Window.localStorage的使用及深度监视存储

在vue中，当原始数据发生改变的时候，然后应该将数据保存下来，当页面刷新的时候用已经改变的数据渲染页面，在前端里面本地存储一些信息，一个小型的数据库，localStorage

当数据改变的时候就存储，现在就用上了vue中的watch

当todos数据发生改变的时候，就将变化后的数据存储带哦localStorage当中

​    localStorage是前端本地存储的方案，是一个小型的数据库，存储到localStorage当中的东西都会自动住哪胡为字符串

​    local有四个API

```js
localStorage.setItem("键",值)//给loacalStorage存储数据
lacalStorage.getItem("键")//获取localStorage当中的某个数据
localStorage.removeItem("键")//删除localStorage当中的某个值
localStorage.clear()//清空localStorage所有的数据
```

只读的`localStorage` 属性允许你访问一个[`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 源（origin）的对象 [`Storage`](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage)；存储的数据将保存在浏览器会话中。`localStorage` 类似 [`sessionStorage`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)，但其区别在于：存储在 `localStorage` 的数据可以长期保留；而当页面会话结束——也就是说，当页面被关闭时，存储在 `sessionStorage` 的数据会被清除 。

应注意，无论数据存储在 `localStorage` 还是 `sessionStorage` ，**它们都特定于页面的协议。**

另外，`localStorage` 中的键值对总是以字符串的形式存储。 (需要注意, 和js对象相比, 键值对总是以字符串的形式存储意味着数值类型会自动转化为字符串类型)

**利用JSON.stringify将对象转换为字符串**

## 深度监视

一般监视只能监视数组的数据，数组内部的数据监视不到

```js
watch:{
  todos:{
    deep:true,
    handler(newVal,oldVal){
      localStorage.setItem("TODOS",newVal)
    }
  }
}
```

## 使用localStorage遇到的问题

在获取localStorage的数据的时候如果数据空的话，用JSON.parse解析之后就会返回一个null

```js
todos:JSON.parse(localStorage.getIem("TODOS")) || []
```





