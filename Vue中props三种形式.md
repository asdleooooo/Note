## Vue中props三种形式

### 数组的方式

```js
props:['属性声明']
```

### 对象的方式

```js
props:{
  属性:属性的类型
}
```

### 对象的方式第二种

props的第三种写法，比第二种写法更加严格

可以限制属性值的更多，是一个配置对象,required和default只写其中一个

```js
addToDo:{
  type:Number,//值得类型
  required:true,//代表必须传
  defalut:10//设置默认值
}
```





