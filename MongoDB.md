# MongoDB
## 简介
MongoDB是为了快速开发互联网web应用而设计的数据库系统，数据库是按照数据结构来组织、存储和管理数据的仓库，是一个应用程序
## 三个重要的概念
- 一个服务可以从创建喝多数据库
- 数据库是一个仓库，在仓库中刻意存放集合
- 集合类似于js中的数组，在集合中可以存放文档
- 文档，就相当于对象


## 一个项目一般只是用一个数据库
_id是mongodb为我们文档创建的唯一标识
### 修改端口号
```
mongod --port=27018
```
## 常用命令
### 数据库集合命令
1、显示所有的数据库
```
//只会显示有数据的内容
show dbs
show databases
```
2、切换到指定的数据库，也可以通过use创建一个不存在的数据库
```
use 数据库名
```
3、显示当前所在的数据库
```
db
```
4、创建一个集合
```
db.createCollection('集合的名字');
show collections
```
5、删除当前集合
```
db.collection.drop();
```
6、重命名集合
```
db.collection.renameCollection('以前的名字','现在的命名')
```
### 文档命令
1、插入文档
```
db.collection.insert('文档对象');
```
2、查找文档
```
db.collection.find();
db.collection.findOne({key:value});
```
3、更新文档
```
db.collection.update({条件对象},{新文档},{配置对象});
//更改一部分内容
db.collection.update({条件对象},{$set:{修改目标对象的属性}});

//配置对象
{
  //可选默认为false，表示如果不存在查询对象是否插入新对象
  upsert:boolean,
//可选默认为false，只更新找到一条记录，如果为true，就把按照条件查出来多条记录全部更新
  multi:boolean
}
```
4、删除集合中的文档
```
db.collection.remove(查询条件);
```
实际场景中极少数删除数据，不会真的去删除，而是去加一些特殊的属性标识来替换
### 条件控制
在mongodb不能> < >= <= = 等一些运算符
##### 运算符

在 mongodb 不能 > < >=  <= !== 等运算符，需要使用替代符号

* `>`   使用 `$gt`
* `<`   使用 `$lt`
* `>=`   使用 `$gte`
* `<=`   使用 `$lte`
* `!==`   使用 `$ne`

##### 逻辑或

`$in` 满足其中一个即可

```
db.students.find({age:{$in:[18,24,26]}})
```

`$or` 逻辑或的情况

```js
db.students.find({$or:[{age:18},{age:24}]});
```

`$and` 逻辑与的情况

```
db.students.find({$and: [{age: {$lt:20}}, {age: {$gt: 15}}]});
```

##### 正则匹配

条件中可以直接使用 JS 的正则语法

```js
db.students.find({name:/imissyou/});
```
# Mongoose
## 介绍
Mongoose是一个对象文档模型(ODM)库。它对Node原生的MongoDB模块进行了进一步的优化封装，并提供了更多的功能。
## 作用
使用代码操作mongodb数据库
## 使用流程
一、安装Mongoose
在命令行使用npm或者其他包管理工具安装(cnpm yarn)
```
npm install mongoose -S
```
二、引入包
在运行文件中引入mongoose
```
var mongoose = require('mongoose');
```
三、链接数据库
```
mongoose.connect('mongodb://127.0.0.1/data');
//如果出现警告提醒，就按照提醒增加选项就可以了
```
### mongoose初体验
```javascript
//引入
const mongoose = require('mongoose');
//链接数据库,协议、url、端口号、数据库名称
mongoose.connect('mongodb://127.0.0.1:27017/project');


//设置连接成功的回调
mongoose.connection.on('open',() => {
  console.log('连接成功...');
  //创建文档结构对象，对文档结构对象做一个说明
  const UserSchema = new mongoose.Schema({
    //对于文当的参数做说明
    username:String,
    password:String,
    age:Number
  });
  //创建一个模型对象
  const UserModel = mongoose.model('user',UserSchema);
  //数据操作
  UserModel.create({
    username:'zhangsan',
    password:'12315646546',
    age:21
  },(err,data) => {
    if(err) throw err;
    console.log(data);
    //关闭数据库连接
    mongoose.connection.close();
  })
})


// _id: new ObjectId("61949bd84b61b3ad96f3ecf6")
//是mongodb中产生的唯一的标识
// __v: 0
//是mongoose产生的一个属性
```
### 批量插入数据
```javascript
const mongoose = require('mongoose');

//连接数据库
mongoose.connect('mongodb://127.0.0.1:27017/project');

//连接成功事件
mongoose.connection.on('open',() => {
  //创建文档的结构
  const StarSchema = new mongoose.Schema({
    name:String,
    age:Number,
    tags:Array
  });
  //创建文档模型
  const StarModel = mongoose.model('star',StarSchema);
  //批量插入
  StarModel.insertMany([{
    name:'蔡徐坤',
    age:26,
    tags:['唱','跳']
  },
  {
    name:'马老师',
    age:30,
    tags:['主播']
  }
],(err,data) => {
  if(err) throw err;
  console.log(data);
});
})
```

## 注意
数据库中会将你定义的集合的名称变为复数
```
> show collections
stars
users
>
```
## 更新
```javascript
//引入模块
const mongoose = require('mongoose');
//连接数据库服务
mongoose.connect('mongodb://127.0.0.1:27017/project');

//连接成功事件
mongoose.connection.on('open',() => {
  //创建文档结构对象
  const StarSchema = mongoose.Schema({
    name:String,
    age:Number,
    tags:String
  });
  //创建集合模型对象
  const StarModel = mongoose.model('stars',StarSchema);
  // StarModel.updateOne({name:'蔡徐坤'},{name:'KUN'},(err,data) => {
  //   if(err) throw err;
  //   console.log(data);
  // });


  StarModel.updateMany({name:'蔡徐坤'},{name:'KUN'},(err,data) => {
    if(err) throw err;
    console.log(data);
  })
})
```
### 读取数据
读取所有的数据，获得的是一个数组
```javascript
//引用
const mongoose = require('mongoose');

//连接服务
mongoose.connect('mongodb://127.0.0.1:27017/project');

//连接成功的回调
mongoose.connection.on('open',() => {
  const StarSchema = new mongoose.Schema({
    name:String,
    age:Number,
    tags:Array
  })
  const StarModel = mongoose.model('stars',StarSchema);
  StarModel.find({name:'马老师'},(err,data) => {
    if(err) throw err;
    console.log(data);
  })
})
```
读取单条数据，获取的是一个对象findOne()

通过ID来读取数据findById()


## 数据的个性化读取
