# 函数对象、实例对象
1、将函数作为对象用的时候，简称为函数对象
2、实例对象，就是new构造函数产生的对象，我们称为实例对象
**每一个函数里面有一个不可变的属性，叫做name，然后输出构造函数中的name，就会输出构造函数的名字**
# 回调函数的分类
回调函数我们定义的，我们没有执行，当异步或者同步事件结束之后就会调用我们定义的函数
1、同步的回调函数：
  立即在主线程上执行，不会放入回调队列中
2、异步的回调函数：
  不会立即执行，会放如回调队列中以后执行
# 错误类型说明
1、错误的类型
- error：所有错误的父类型
- ReferenceError：引用的变量不存在
- TypeError：数据类型不正确
- RangeError：数据值不在允许范围类
- SyntaxError：语法错误
2、错误处理
- 捕捉错误：try{}catch(){}
- 抛出错误：throw error
```javascript
try{
  //放可能出现错误的代码，一旦出现错误就停止try中代码的执行，并执行catch中的内容并携带错误信息
}catch(error){
  console.log(error);
}
```
3、错误对象
- message：错误相关信息
- stack属性：记录信息
# Promise的理解和使用
## Promise是什么
抽象的表达：Promise是JS中进行异步编程的新方案
旧方案：就是使用纯回调的形式
具体表达：从语义上说Promise是一个构造函数
从功能上来说Promise的实例对象可以用来封装一个异步操作，并可以获取其成功或失败的值
## Promise基本编码流程
- 1、创建Promise实例对象(pending状态)，传入executor函数
- 2、在executor中启动异步任务(定时器、ajax请求)
- 3、根据异步任务的结果，做不同处理：
      3.1 如果异步任务成功了：
          我们调用resolve，让Promise实例对象的状态变为(fulfilled)，同时指定成功的value
      3.2 如果异步任务失败了：
          我们调用reject，让Promise实例对象状态变为失败(rejected)，同时指定失败的reason
- 4、通过then方法对Promise指定成功、失败的回调函数，来获取成功的value、失败的reason
      注意：then方法指定的：成功的回调、失败的回调，都是异步的回调
## Promise
状态改变只有两种情况
pending->fulfilled
pending->rejected
状态只能改变一次
通过then指定成功的回调或者失败的回调的时候，会被放入队列，所以在then中的函数是异步的
当失败的回调没有指定时就会报错
## Promise的API
### 实例的API
then()方法

catch()方法：是then()方法的语法糖

### 构造函数的API
Promise.resolve()，快速创建一个状态为fulfilled或rejected的实例对象，然后为什么会出现rejected状态了，如果传入的value的值，是一个失败状态的Promise值，的时候，通过Promise.resolve()创建的Promise实例对象就是一个状态为rejected的实例

Promise.reject()快速创建一个状态必为rejected的Promise实例

Promise.all(PromiseArray)会返回一个新的Promise实例，只有所有的Promise都成功才成功，只要有一个失败了就直接返回失败的那个值

Promise.race(PromiseArray)会返回一个谁先出结果就返回哪一个的状态的value或者reason
## 如何改变Promise实例的状态
Promise实例的状态只能改变一次，执行resolve(value)，会将pending变为fulfilled，执行reject(reason)，会将pending变为rejected，如果executor抛出异常，如果没有走到resolve或者reject的话，就会将状态变为rejected，然后抛出错误的Error

## 改变Promise实例的状态和指定回调函数谁先谁后
1、都有可能，正常情况下是先指定回调改变状态，但也可以先改状态在指定回调
## 如何先改变状态再指定回调
** 一般是先制定回调**
 ```javascript
const p = Pormise((resolve,reject) => {
  setTimeout(() => {
    resolve('a');
  },1000)
})
p.then(
  (value) => {console.log(value);},
  (reason) => {console.log(reason);}
)
 ```


** 先改状态，后指定回调**
```javascript
const p = Promise((resolve,reject) => {
  resolve('a');
})
setTimeout(() => {
  p.then(
    (value) => {console.log(value);},
    (reason) => {console.log(reason);}
  )
},1000)
```
必须满足，状态已经改变，已经指定回调了，才能拿到数据
先指定回调，就会将回调存下来，先指定状态就会将状态和value存下来
## then的链式调用
Promise实例.then()返回的一个新的Promise实例，它的值和状态由什么决定
1、简单的表达：由then()所指定的回调函数指定的结果
2、详细的表达：
  (1)如果then所指定的回调返回的非Promise值a：
    就是成功了的回调的返回值，就会影响下一个then的value
    那么新的Promise实例状态为：成功(fulfilled)，成功的value为a
  (2)如果then所指的回调返回的是一个Promise实例p：
    那么新的Promise实例的状态、值都与p一致
  (3)如果then所指定的回调抛出异常：
    则then新指定的回调为rejected

```javascript
const p = Promise((resolve,reject) => {
  setTimeout(() => {
    resolve(100);
  },1000)
})
const x = p.then(
  value => {console.log(value);},
  reason => {console.log(reason);}
)
x.then(
  value => {console.log(value);},
  reason => {console.log(reason);}
)
```
```javascript
//创建发送异步请求的ajax函数，指定一个url和data，返回一个Promise实例对象
    //Promise是一个内置构造函数，用来处理异步回调函数的新方案，是一个约定，当事件循环结束后，返回异步回调的值和状态
    //即使异步的结果已经完成，通过then还可以执行，异步的回调的值和状态
    //可以解决回调地狱的问题
    //通过then中回调返回的值，会作为Promise的value或者reason，如果返回的是非Promise则会一直走成功的回调，返回的值作为下一次then的value，如果是
    function sendAjax(url,data){
      return new Promise((resolve,reject) => {
        //创建Ajax中的get请求
        const xhr = new XMLHttpRequest();
        //当状态改变的时候的事件
        xhr.onreadystatechange = () => {
          if(xhr.readyState === 4){
            if(xhr.status >= 200 && xhr.status < 300){
              resolve(xhr.response);
            }else{
              reject('错误',xhr.status);
            }
          }
        };
        //整理参数
        let str = '';
        for(let i in data){
          str += `${i}=${data[i]}&`;
        }
        str = str.slice(0,-1);
        //绑定method和url
        xhr.open('GET',url+'?'+str);
        //读取返回的json
        xhr.responseType = 'json';
        //发送请求
        xhr.send();
      })
    }
    let x = sendAjax('https://api.apiopen.top/getJoke',{page:1,count:2,type:'video'});
    //链式调用
    x
    .then(
      value => {
        console.log('第一次调用成功',value);
        //返回
        return sendAjax('https://api.apiopen.top/getJoke',{page:1,count:2,type:'video'});
      },
      reason => {
        console.log('错误',reason);
      }
    )
    .then(
      value => {
        console.log('第二次调用成功',value);
        return sendAjax('https://api.apiopen.top/getJoke',{page:1,count:2,type:'video'});
      },
      reason => {
        console.log('错误',reason);
      }
    )
    .then(
      value => {
        console.log('第三次调用成功',value);
      },
      reason => {
        console.log('错误',reason);
      }
    )
```
```javascript
//中断Promise链：
    //在Promise的then链式调用时，在中间中断，不在调用后面的回调函数
    //方法：在失败的回调函数中返回一个状态为pending的Promise实例
    function sendAjax(url,data){
      return new Promise((resolve,reject) => {
        //创建Ajax中的get请求
        const xhr = new XMLHttpRequest();
        //当状态改变的时候的事件
        xhr.onreadystatechange = () => {
          if(xhr.readyState === 4){
            if(xhr.status >= 200 && xhr.status < 300){
              resolve(xhr.response);
            }else{
              reject('错误',xhr.status);
            }
          }
        };
        //整理参数
        let str = '';
        for(let i in data){
          str += `${i}=${data[i]}&`;
        }
        str = str.slice(0,-1);
        //绑定method和url
        xhr.open('GET',url+'?'+str);
        //读取返回的json
        xhr.responseType = 'json';
        //发送请求
        xhr.send();
      })
    }
    let x = sendAjax('https://api.apiopen.top/getJoke',{page:1,count:2,type:'video'});
    //链式调用
    x
    .then(
      value => {
        console.log('第一次调用成功',value);
        //返回
        return sendAjax('https://api.apiopen.top/getJoke',{page:1,count:2,type:'video'});
      },
      reason => {
        console.log('错误',reason);
        return new Promise(() => {});
      }
    )
    .then(
      value => {
        console.log('第二次调用成功',value);
        return sendAjax('https://api.apiopen.top/getJoke',{page:1,count:2,type:'video'});
      },
      reason => {
        console.log('错误',reason);
        return new Promise(() => {});
      }
    )
    .then(
      value => {
        console.log('第三次调用成功',value);
      },
      reason => {
        console.log('错误',reason);
        return new Promise(() => {});
      }
    )
```

## 错误穿透
Promise错误穿透：
不用指定失败的回调，在最后添加catch(),指定失败的回调，每次失败的时候都会执行最后的catch(),底层会补一个抛出错误的reason，抛出了之后，就会将Promise的实例状态改为rejected，然后执行下面
  (1)当使用Promise链式调用的时候，不写错误的回调，在最后补一个.catch()的失败的回调


## Promise的优势
- 更加灵活，可以在任何一个时间点，调用回调得到回调的值和状态
- 支持链式调用可以解决回调地狱，就是可以解决当第一个请求完成之后调用第二个请求，在第二个请求完成之后，解决第三个请求
- 不是一个很优秀的解决方案:then的链式调用
- 终极解决方案：async/await(底层实际上依然使用then的链式调用)


## async/await的使用
如果只想拿到成功的值，就直接使用await,写立即执行函数的时候前面的语句要带分号
```javascript
const p = new Promise((resolve,reject) => {
  setTimeout(() => {
    resolve('a');
  },1000);
});

//要将await放在一个被async修饰的函数中
async function demo(){
  try{
    const result = await p;
    console.log(result);
  }catch(error){
    console.log(error);
  }
}
demo();
```

```javascript
const p = new Promise((resolve,reject) => {
  setTimeout(() => {resolve('a');},1000)
});
const p1 = new Promise((resolve,reject) => {
  setTimeout(
    () => {resolve('b');},
    2000
  )
});
const p2 = new Promise((resolve,reject) => {
  setTimeout(
    () => {resolve('c');},
    3000
  )
});
//调用成功的结果
;(async() => {
  try {
    let result = await p;
    console.log(result);
    result = await p1;
    console.log(result);
    result = await p2;
    console.log(result);
  } catch (error) {
    console.log(err);
  }
})()
```
async是一个修饰符，如果你的函数被async修饰了，可以返回一个Promise实例对象，promise的结果由函数return的值决定
await右侧的值一般是一个Promise实例，如果是一个非Promise，就输出这个非Promise值，用await就一定要将await写在async修饰的函数中
## await的原理
如果我们使用async和await配合的这种写法：表面上补出现任何回调函数，但实际上底层把我们写的代码进行了加工，把回调函数还原回来了，最终运行的代码依然有回调，会将浏览器会将async/await加工成，把回调函数给还原回来了，还是then的操作，会将async函数中的代码做一个then中的resolve函数的封装
```javascript
const p = new Promise((resolve,reject) => {
  setTimeout(() => {
    resolve('a');
  },1000);
});
async function demo(){
  try{
    let result = await p;
  }catch(err){
    throw err;
  }
}
```


## 宏队列和微队列
宏队列中放的都是宏任务，定时器所指定的回调，是一个宏任务要放到宏队列中
微队列，Promise的回调是微任务，要放到微队列中
微队列中的优先级比宏队列优先级要高，每次要执行宏队列前，首先要查看微队列中是否还有任务，要微任务有就执行微任务