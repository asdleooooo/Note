## Vue

vue是渐进式的前端框架，所谓的渐进式就是类似于迭代开发，就是vue.js中只有一些核心的而代码，可以让你搭建基本的页面，如果你的页面的功能比较丰富，那么需要相关的一些插件去完成，这就是渐进式

插件：就是一些功能代码模块，它是为了已经完成的功能代码，额外去添加功能用的

vue是数据为尊，动态显示页面

### 初探vue

wm(数据代理)和配置对象不是同一个，vm代理了data当中的数据，找vm获取数据最终还是拿的data当中的属性值，修改vm的数据其实本质是修改data中的数据，借助Object.definePropery(对象,属性,{

get(){},

set(value){}

})来实现vm对于配置对象的代理

#### 配置对象

配置对象就是属性名固定属性值可变的对象，vue的配置对象中的属性值

#### 挂载点

配置对象中的el属性名以及其对应的属性值，属性值一般是表达式或者选择器，一旦挂载之后这个元素内部就会成为一个模板

（模板就是不是单纯的html，是html和js，模板的语法，就是插值和指令）

在body中必须写一个人挂载点

```js
const vm = new Vue({
  el:"#app",
  data:{
    msg:'Hello world'
  }
});

```

```js
const vm = new Vue({
  el:"#app",
  data(){
    return {
      msg:'Hello world'
    }
  }
});
```

#### data属性

data属性的使用有两种形式，值有两种形式，一种是作为对象的形式，还有一种是作为函数的形式，将数据作为返回值

#### methods属性



#### computed属性

一般存放data中没有的属性，但是要使用data中的数据进行生成的时候就用到了computed，但是computed中只能使用同步修改数据

#### watch属性

watch用于监视一个属性，监视的属性一定是data中存在的，可以异步修改数据，只会在属性修改的时候才会执行，如果要在页面一开始就要执行一次handler函数中的代码的时候就加上immediate:true



### 指令

#### 单向数据绑定

v-bind

#### 双向数据绑定

v-model默认收集的是input中的value值

#### 事件绑定

v-on

#### 循环指令

v-for

#### 条件指令

v-if——v-else

v-show

```html
<div id="app">
    <p v-if="isOk">表白成功</p>
    <p v-else>表白失败</p>
    <p v-show="isOk">表白成功</p>
    <p v-show="!isOk">表白失败</p>
  </div>
  <script src="../vue.js"></script>
  <script>
    const vm = new Vue({
      el:'#app',
      data(){
        return {
          isOk:true,
        }
      }
    });
  </script>
```

v-if和v-show都可以达到条件渲染的效果，但是他们是由很大区别的，后期两个用多个都比较多

v-if条件渲染的时候条件为真的被渲染，条件为假的不渲染，其实条件为假的元素根本存在DOM上(内存中没有)

v-show条件为真的被渲染，条件为假的不渲染，但是它是真实在DOM上存在的，只有采用样式display:none去做的隐藏(内存中存在)

以后如果切换的频率很高就用v-show，切换频率低的话就用v-if

![image-20211221140241616](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211221140241616.png)

#### v-text

用来绑定标签中的内容

#### v-html

用来绑定标签中的元素内容

#### ref

用来操作DOM的时候，标识数据

在vm中会有一个this.$refs.标识符传递的字符串，用的比较多

### 插值

插值一般是用来改变标签的内容的，一般是{{}}

### 响应式数据对于对象和数组的区别

当修改数据的时候，页面会响应式跟着变化

vue当中处理响应式数据，对于数组和对象是不一样的

1、如果修改的是对象的属性随便改，都是响应式的，因为vue一开始就为data中所有的属性通过Object.definePropery，修改为响应式属性

2、数组修改的时候必须使用特定的几个方法才能是响应式，如果直接通过下标操作数组的数组，不是响应式

为什么数组的方法可以是响应式？

这个splice非原生splice，vue当中给数组部分方法添加了修改页面的功能（重写数组的方法）

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

vue在对待数组和对象的时候处理响应式是不一样的，对象是添加，数组是用响应式的方法，（一开始data中有的属性）虽然通过下标修改数组，数组的数据修改了但是页面没有响应式的改变，如果是一个新的数据的时候也会在页面响应式更新

### 动态绑定样式

几乎用的都是类的方式来绑定样式的，需要传递一个对象的形式，来修改样式

#### 绑定类

```html
<div id="app">
    <!-- 现在这个元素用哪个类是后台数据告诉我的 -->
    <p :class="myClass">Hello world!</p>
  </div>
  <script src="../vue.js"></script>
  <script>
    const vm = new Vue({
      el:'#app',
      data(){
        return {
          myClass:'class-a',
        }
      }
    });
  </script>
```

**元素绑定的是哪个类那些类确定了，生不生效由数据决定**后期用的最多的一种方式

静态数据绑定，就是直接写，动态就是用v-bind的方式绑定

```html
<div id="app">
    <!-- 现在这个元素用哪个类是后台数据告诉我的 -->
    <p :class="myClass">Hello world!</p>
    <!-- 第二种方式 -->
    <p :class="{ classa: isA, classb: isB }">Hello world!</p>
  </div>
  <script src="../vue.js"></script>
  <script>
    const vm = new Vue({
      el:'#app',
      data(){
        return {
          myClass:'classa',
          isA:false,
          isB:true
        }
      }
    });
  </script>
```



#### 绑定内嵌的形式

```html
<div id="app">
    <p :style="{color:myColor,fontSize:mySize}">Hello world!</p>
  </div>
  <script src="../vue.js"></script>
  <script>
    const vm = new Vue({
      el:'#app',
      data(){
        return {
          myColor:'red',
          mySize:'50px'
        }
      }
    });
  </script>
```

### 绑定事件

#### 事件对象是什么

系统调用回调函数的时候默认传递的一个实参，这个实参就是我们说的事件对象，事件对象是这个一次事件触发后相关的所有一切信息被封装成一个对象

#### 为什么浏览器要传递事件对象

是为了防止用户在函数内部用到这次事件的相关信息

#### 怎么做

vue中在添加自己的定义的参数的时候可以用$event来传递事件对象，然后传递额外的参数

```html
<div id="app">
    <button @click="test1">test1</button>
    <!-- 在vue模板当中，事件会带哦函数在调用的时候可以传递$event，名字不能随意改，这个东西就是事件对象 -->
    <button @click="test1($event)">test1</button>
    <!-- 事件在添加的时候除了事件对象以外还要传递自己的参数的时候必须写$event -->
    <button @click="test2('Tom',$event)">test2</button>
  </div>
  <script src="../vue.js"></script>
  <script>
    const vm = new Vue({
      el:'#app',
      data(){
        return {

        }
      },
      methods:{
        test1(event){
          console.log(event.target.innerHTML);
        },
        test2(type,event){
          console.log(type);
          console.log(event.target.innerHTML);
        }
      }
    });
  </script>
```

#### 事件中阻止冒泡

冒泡

div——>app——>body——>html——>初始包含框(浏览器创建的的然后不是元素)——>document

document没有高度，然后html定义宽高百分比的时候，就没有依据的元素了，然后浏览器厂商就加了一个初始包含框

```
//  阻止冒泡
event.stopPropagation();
```

在vue中阻止冒泡，vue中有一个事件描述符来阻止冒泡

```
@click.stop = "函数"
```

### 生命周期

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

下面的阶段是在数据代理和响应式

第一阶段：beforeCreate、created

下面的阶段是创建真实的DOM

第二阶段：beforeMount、mounted

前两个阶段完成，代表初始化页面完成了



数据更新，虚拟DOM的re-rendser,页面数据更新前和页面数据更新后

vm数据已经更新了，但是页面的数据还没有更新过来

都更新了之后

第三阶段beforeUpdate、updated



第四阶段beforeDestory、destoryed



其中mounted用的最多，一般哟关于发送ajax请求获取数据，还有开启定时器，添加一些事件

beforeDestory用的也比较多，在销毁之前解绑事件监听，取消定时器等操作，清除定时器id的时候将这个空间也释放掉

### 自定义指令

分为全局自定义指令和局部自定义指令

功能					参数				返回值

定义全局指令

参数：指令名称(不包含v-、只能是全小写)、回调函数(第一个参数时使用这个指令的哪个节点，第二个参数代表这个节点包含数据(表达式)的集合体)

Vue.directive('upper',function(node,bindings){})

每一个Vue的实例都可以使用

```html
 <div id="app">
    <p v-text="msg"></p>
    <p v-upper="msg"></p>
    <p v-lower="msg"></p>
  </div>
  <script src="../vue.js"></script>
  <script>
    Vue.directive('upper',function(node,binding){
      console.log(node,binding);
      node.innerHTML =binding.value.toUpperCase();
    });
    const vm = new Vue({
      el:'#app',
      data(){
        return {
          msg:'Hello world!',
        }
      },
      directives:{
        lower(node,bindings){
          node.innerHTML = bindings.value.toLowerCase();
        }
      }
    });
  </script>
```

### 过滤器

定义全局过滤器和局部过滤器

Vue.filter(过滤器的名字,回调函数)

```html
  <div id="app">
    <p>{{timeNow}}</p>
    <p>{{timeNow | timeFormat}}</p>
  </div>
  <script src="../vue.js"></script>
  <script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.29.1/moment.min.js"></script>
  <script>
    Vue.filter('timeFormat',function(value){
      return moment(value).format('YYYY-MM-DD HH:mm:ss');
    });
    const vm = new Vue({
      el:'#app',
      data:{
        timeNow:Date.now()
      },
    });
  </script>
```

### 自定义插件

https://cn.vuejs.org/v2/guide/plugins.html

### 组件

#### 非单文件组件

组件和组件化

```html
  <div id="app">
   <buttonnice></buttonnice>
   <buttonnice></buttonnice>
  </div>
  <script src="../vue.js"></script>
  <script>
    /* 
    定义全局组件
      定义的组件之后都是全局组件,本质上是一个配置对象返回一个函数，后期是当构造函数使用
    注册本质是把一个标签名称和刚才定义出来的函数绑定在一起
      全局注册:Vue.component('VueComponent',VueComponent)

    使用 
      在Vue的模板中使用这个标签
    */
   const VueComponent = Vue.extend({
    // 组件的配置对象和Vue的配置对象很相似，但是没有el
    // 组件的配置对象只能写函数式的data
     data(){
       return {
        count:0
       }
     },
     template:'<div class="container"><h2>Hello world!{{count}}</h2><button @click="count++">点击</button></div>'
   })
   Vue.component('buttonnice',VueComponent)
   const vm = new Vue({
     el:'#app',
     data(){
       return {

       }
     },
   });
  </script>
```

##### 定义带注册

```html
<div id="app">
   <buttonnice></buttonnice>
   <buttonnice></buttonnice>
  </div>
  <script src="../vue.js"></script>
  <script>
    /* 
    定义全局组件
      定义的组件之后都是全局组件,本质上是一个配置对象返回一个函数，后期是当构造函数使用
    注册本质是把一个标签名称和刚才定义出来的函数绑定在一起
      全局注册:Vue.component('VueComponent',VueComponent)

    使用 
      在Vue的模板中使用这个标签
    */
/*    const VueComponent = Vue.extend({
    // 组件的配置对象和Vue的配置对象很相似，但是没有el
    // 组件的配置对象只能写函数式的data
     data(){
       return {
        count:0
       }
     },
     template:'<div class="container"><h2>Hello world!{{count}}</h2><button @click="count++">点击</button></div>'
   })
   Vue.component('buttonnice',VueComponent) */



  //  定义带注册
  Vue.component('buttonnice',{
    data(){
       return {
        count:0
       }
     },
    template:'<div class="container"><h2>Hello world!{{count}}</h2><button @click="count++">点击</button></div>'
  })
   const vm = new Vue({
     el:'#app',
     data(){
       return {

       }
     },
   });
  </script>
```

#### 单文件组件

一个vue文件就是一个组件，但是本质不是，本质是定义组件的配置对象，组件的本质就是函数

组件渲染过多，就是实例对象，过多会影响效率

要使用组件就要先注册

### 脚手架

就是用webpack创建的环境

```js
import Vue from "vue";
import App from "@/App.vue"


/* 
  在脚手架中引入的vue是不带解析器的版本，如果你要用以前的那中方式去new Vue的时候就会报没有解析器的错误
  如果要用就要用vue/dist/vue.esm这个文件，用不带解析器的版本，这个打包出来的文件小
  和效率有关
*/


new Vue({
  el:'#app',
  render:h => h(App),
})
```

### 按照功能点拆

### 1、vue中的属性传递数据(父组件给子组件传递)

在vue中传递数据通过单向数据绑定的形式然后将数据绑定给组件的属性，然后在组件内部通过props这个配置项来声明传递的数据的属性名，并在模板中直接使用这个数据

![image-20211223220935901](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211223220935901.png)

![image-20211223221034236](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211223221034236.png)

![image-20211223221100461](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211223221100461.png)

### props组件间通信

是组件通信最常用最简单的一种方式，适用于父子之间的通信

父可以给子传递函数数据和非函数数据，传递非函数数据就是父亲给儿子传递数据，传递函数数据，本质是付清想要要儿子的数据通过函数传参的形式把数据传递给父亲

#### 为什么在methods中不使用箭头函数

vue底层已经将methods中的函数修改了函数中的this，如果是用箭头函数的话，在@click赋值的时候就会向外找找到undefined上，在vue中是严格模式

![image-20211224120947503](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211224120947503.png)

### @代表src文件夹

### 只要是表单页面必须验证

### 2、自定义事件通信（子向父通信，只能从儿子传递给父亲）

自己定义的事件，事件类型自己定义和回调函数自己定义自己触发，回调函数的默认传参，自己定义，自己传，就有自己不传就没有

系统定义的事件，回调函数默认参数是事件对象

#### 父组件

![image-20211224141828786](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211224141828786.png)

通过ref去标识组件对象之后，就可以给组件添加一个事件，自定义事件，创建回调函数

![image-20211224142032582](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211224142032582.png)

在挂载完成之后，就给组件添加一个事件

![image-20211224141917830](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211224141917830.png)

#### 子组件

在子组件中利用$emit来触发事件

![image-20211224142006229](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211224142006229.png)

#### 简单写法

将获取组件对象和创建自定义对象作为一步

![image-20211224142822916](C:\Users\yang\AppData\Roaming\Typora\typora-user-images\image-20211224142822916.png)

调用的写法没有改变

### vm和组件对象的关系

vm实例化对象的原型是组件对象原型的原型，$on、$emit等方法是在Vue的显示原型上

vm的原型对象，就是组件对象的原型对象的原型对象