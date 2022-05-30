# 造飞机

## 目录

- [javascript](#js)
- [框架](#frameworks)
- [html](#html)
- [css](#css)
- [网络](#network)
- [浏览器](#browser)
- [设计模式](#design)
- [工程化](#package)
- [需求实现](#implement)


### <span id="js"> javascript</span>
---
<details>
<summary class="question">什么是防抖和节流？有什么区别？如何实现？</summary>
<div class="answer">
防抖：动作绑定事件，动作发生后一定时间后触发事件，在这段时间内，如果该动作又发生，则重新等待一定时间再触发事件。

```js
  function debounce(func, time) {
    let timer = null;
    return () => {
      clearTimeout(timer);
      timer = setTimeout(()=> {
        func.apply(this, arguments)
      }, time);
    }
  }
```

节流： 动作绑定事件，动作发生后一段时间后触发事件，在这段时间内，如果动作又发生，则无视该动作，直到事件执行完后，才能重新触发。

```js
const myThrottle2 = function (func, wait = 50) {
  var canRun = true
  return function (...args) {
    if (!canRun) {
      return
    } else {
      canRun = false
      func.apply(this, args) // 将方法放在外面, 这样即便该函数是异步的，也可以保证在下一句之前执行
      setTimeout(function () {canRun = true}, wait)
    }
  }
}
```
</div>
</details>

<!--  -->

<details>
<summary class="question">
Map 和 Object区别
</summary>
<div class="answer">
Object和Map非常相似，两者都可以完成键-值对的设置，获取和删除

|             | Map         | Object |
| ----------- | ----------- |----------- |
| key命名 | 任意类型 | 1.对象的键名只能是String和 Symbol 类型 2.其他类型的键名会被转换成字符串类型 3.对象转字符串默认会调用 toString 方法。 |
| 附加的Key    | Map没有默认的key值       |Object具有原型对象，所以它包含默认的key值，并且使用不当时会和自定义的key值产生冲突（在ES5中可以通过Object.create(null)来设置去掉默认的key值，但这种解决方法并不常用）|
|     Key的顺序        | Map中的key值排序简单直接，一个Map对象迭代键值对、Key、Value的顺序和插入时的顺序相同         | 一般对象的键值是有顺序的，但这并不绝对，有时对象的键值排序会变得很复杂，所以最好不要依赖于插入的顺序。 |
|       大小      | Map的大小可以轻松通过size属性来获得         | Object的大小必须通过自行获取 |
|       迭代      | Map是可迭代对象，可以轻松完成迭代         | Object没有实现迭代协议，所以无法被for...of直接迭代（但可以自行实现迭代协议，或者使用Object.keys()或Object.entries()来迭代对象的键值和实体，for...in也可以迭代Object的可枚举属性） |
|    性能         | 频繁增减键值对时表现会更好         | 频繁增减键值对时表现较差 |
</div>
</details>

<!--  -->

<details>
<summary class="question">
模块化发展？（cmd，amd，commonjs，esmodule）
</summary>
https://www.processon.com/view/link/5c8409bbe4b02b2ce492286a#map


|  | IIFE | COMMONJS | AMD | CMD | ES MODULE | 
| --- | --- | --- | --- | --- |  --- | 
| 区别 | 变量私有化，解决全局污染问题 | 同步化的模块解决方案 | 通过异步加载，提前声明依赖 | 异步化按需加载 |  兼容浏览器端和服务器端，并且统一了使用的语法 | 
</details>

<!--  -->

<details>
<summary class="question">
箭头函数与普通函数（function）的区别是什么？构造函数（function）可以使用 new 生成实例，那么箭头函数可以吗？为什么？
</summary>
箭头函数是普通函数的简写，可以更优雅的定义一个函数，和普通函数相比，有以下几点差异：

1、函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。

2、不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

3、不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

4、不可以使用 new 命令，因为：

没有自己的 this，无法调用 call，apply。
没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 `__proto__`
箭头函数并没有[[Construct]]方法, 所以不能被用作构造函数:
JavaScript函数两个内部方法: [[Call]]和[[Construct]]
直接调用时执行[[Call]]方法, 直接执行函数体
new调用时执行[[Construct]]方法, 创建一个实例对象

</details>

<!--  -->

<details>
<summary class="question">
var、let 和 const 区别和实现原理是什么
</summary>

区别：
|  | var | let | const | 
| --- | --- | --- | --- | 
| 作用域 | 全局作用域，可以由顶层（window）对象访问 | 块级作用域 | 块级作用域 | 
| 变量声明 | 可以重复声明 | 声明一次 | 声明一次 | 
| 变量赋值 | 可以赋值 | 可以赋值 | 静态常量不可以赋值 | 
| 变量提升 | 有 | 暂时性死区  | 暂时性死区 | 

实现原理：

var的话会直接在栈内存里预分配内存空间，然后等到实际语句执行的时候，再存储对应的变量，如果传的是引用类型，那么会在堆内存里开辟一个内存空间存储实际内容，栈内存会存储一个指向堆内存的指针

let的话，是不会在栈内存里预分配内存空间，而且在栈内存分配变量时，做一个检查，如果已经有相同变量名存在就会报错

const的话，也不会预分配内存空间，在栈内存分配变量时也会做同样的检查。不过const存储的变量是不可修改的，对于基本类型来说你无法修改定义的值，对于引用类型来说你无法修改栈内存里分配的指针，但是你可以修改指针指向的对象里面的属性
</details>

<!--  -->

<details>
<summary class="question">
Async/Await 如何通过同步的方式实现异步
</summary>
同步代码：按照代码执行顺序执行
异步代码: 是非现在运行的代码，或者说在将来某个时刻会执行的代码

async/await 是参照生成器（Generator）封装的一套异步处理方案，可以理解为 Generator 的语法糖，

generator 文档：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator

```js
function* gen(){  // 这里的*可以看成 async
  var url = 'https://api.github.com/users/github';
  var result = yield fetch(url);  // 这里的yield可以看成 await
  console.log(result.bio);
}
```
</details>

<!--  -->

<details>
<summary class="question">
实现Promise.all, Promise.race, Promise.finally
</summary>

Promise.all
```js
function promiseAll(promises = []) {
  let result = [];
  return new Promise((resolve,reject) => {
    for (const promise of promises) {
      promise.then(res => {
        result.push(res);
        if (result.length === promises.length) {
          resolve(result);
        }
      })
      .catch(error=>reject(error))
    }
  })
}
```

Promise.race
```js
function race(promises=[]){
  return new Promise((resolve,reject)=>{
    promises.forEach(promise=>{
      promise.then(resolve,reject)
    })
  })
}
```

Promise.finally
```js
Promise.prototype.myFinally = function (cb) {
  cb = typeof cb === 'function' ? cb : function () { };
  return this.then(
    value => {
      cb();
      return value;
    },
    reason => {
      cb();
      throw reason
    }
  );
};
```
</details>

<!--  -->

<details>
<summary class="question">
实现promise
</summary>

```js
function MyPromise(executor){
  // 设置状态
  this.status = 'pending'
  // 保存then中onfulfilled,onrejected默认值（实现异步）
  this.onFulfilledFunc = Function.prototype
  this.onRejectedFunc = Function.prototype

  let resolver = (value) =>{
    if(this.status === 'pending'){
      this.value = value
      this.status = 'fulfilled'
      // 执行then方法中onfulfilled回调
      this.onFulfilledFunc(this.value)
    }
  }

  let rejector = (reason) => {
    if(this.status === 'pending'){
      this.reason = reason
      this.status = 'rejected'

      this.onRejectedFunc(this.reason)
    }
  }
  executor(resolver,rejector)
}

MyPromise.prototype.then = function (onfulfilled,onrejected) {
  if(this.status === 'fulfilled') {
    onfulfilled(this.value)
  }
  if(this.status === 'rejected') {
    onrejected(this.reason)
  }
  if (this.status === 'pending') {
    this.onFulfilledFunc = onfulfilled
    this.onRejectedFunc = onrejected
  }
}
```

</details>

<!--  -->

<details>
<summary class="question">
JS 异步解决方案的发展历程以及优缺点。
</summary>

|  | 回调 | promise | async/await |
|---|---|---|---|
|特点|解决异步问题；缺乏顺序性： 回调地狱导致的调试困难，和大脑的思维方式不符；嵌套函数存在耦合性，一旦有所改动，就会牵一发而动全身，即（控制反转）；嵌套函数过多的多话，很难处理错误| 解决了回调地狱的问题；无法取消 Promise | 代码清晰，处理了回调地狱的问题；await 将异步代码改造成同步代码，如果多个异步操作没有依赖性而使用 await 会导致性能上的降低。 |

</details>

### <span id="frameworks"> 框架</span>
---
<details>
<summary class="question">
框架diff对比策略
</summary>

react vs vue
共同点：三级比较
- tree diff: 同层节点比较
- component diff：同类型脏检查替换节点及其子节点
- element diff：增加，移动，删除虚拟节点

区别：

1. Vue进行diff时，调用patch打补丁函数，一边比较一边给真实的DOM打补丁

2. Vue对比节点，当节点元素类型相同，但是className不同时，认为是不同类型的元素，删除重新创建，而react则认为是同类型节点，进行修改操作

3. vue列表对比的时候，采用从两端到中间的方式，旧集合和新集合两端各存在两个指针，两两进行比较，每次对比结束后，指针向队列中间移动；react则是从左往右一次对比，利用元素的index和lastindex进行比较

angular脏检查：

1. 变更检测从上到下检查组件树中的每个组件，以查看相应的模型是否已更改

2. 如果有新值，它将更新组件的视图（DOM）

</details>

<!--  -->

<details>
<summary class="question">
框架变化检测
</summary>

vue: 数据劫持监听变化，观察者模式发送变化通知

react: setState初始化状态变化，执行render函数重新构建组件和子组件的虚拟节点，对比虚拟节点更新视图

angular：采用脏检查机制，由浏览器或异步操作触发检测机制，然后通过default和onpush两种策略进行检测

- default: 每次变更检测都会引起组件的变更检测，包括其他组件的状态变化，以及本组件引用型变量内部属性值变化

- Onpush: 每次变更检测会跳过本组件的变更检查，除非满足一些条件

</details>

<details>
<summary class="question">
vue router vs react router
</summary>
https://www.cnblogs.com/marui01/p/13215468.html
</details>

<!--  -->

<details>
<summary class="question">
 vue 3.0新增
</summary>
https://blog.csdn.net/u014212540/article/details/124303432

- Performance：性能比VUE2.x快1.2-2倍
- Tree shaking support按需编译，体积比Vue2.x更小
- Composition API：组合API（类似React Hooks）
- Better TypeScript support：更好的Ts支持
- Custom Renderer API：暴露了自定义渲染API
- Fragment，Teleport（Protal）,Suspense：更先进的组件

tree shaking:
```
通过包引入的方式而不是直接在实例化时就注入
```

性能：
```
1. diff 算法优化

Vue3 新增静态标记

Vue2 中无论元素是否参与更新，每次都会重新创建，然后在渲染
Vue3 中对于不参与更新的元素，会做静态提升，只被创建一次，在渲染时直接复用即可
cacheHandlers 事件侦听器缓存

```

</details>



### <span id="html"> html</span>
---

### <span id="css"> css</span>
---

### <span id="network"> 网络</span>
---

<details>
<summary class="question">
谈谈你对TCP三次握手和四次挥手的理解
</summary>

<img class="image--md" src="https://user-images.githubusercontent.com/38830527/169838298-9d9c2727-120f-4e80-a932-469ff290d055.jpeg" alt="三次握手">

<img class="image--md" src="https://user-images.githubusercontent.com/38830527/169847636-cf18668a-cb61-4e5b-a034-281c47de49d1.jpeg" alt="四次挥手">


</details>

<!--  -->

<details>
<summary class="question">
https 与 http
</summary>

http： 超文本传输协议，以明文方式发送内容

https：超文本传输安全协议，在HTTP的基础上加入了SSL/TLS协议，SSL/TLS依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密。

https传输过程：
```
1、【浏览器】向服务器发送 https 请求
2、【服务器】向 CA 机构获取证书
3、【服务器】向浏览器发送数字证书(包含 public key)
4、【浏览器】用预置的 CA 列表验证证书，生成随机对称秘钥【key】，并使用公钥加密，如有问题会提示风险，
5、【浏览器】加密后的【key】，发送给【服务器】，作为接下来请求的秘钥
6、【服务器】用自己的 private key 解密得到对称秘钥 key
7、【浏览器】使用随机码 key 进行解密数据
8、【浏览器】【服务器】使用该秘钥进行通信
```

ca证书验证过程：
```
首先从证书的内容中获取证书的颁发机构，然后从浏览器系统中去寻找此颁发机构是否为浏览器的信任机构。

从证书中得知证书的颁发机构，然后从浏览器系统中去寻找此颁发机构的根证书

从根证书中取得那个根公钥，用根公钥去解密此证书的数字签名（根私钥加密的），成功解密的话就得到证书的指纹（记为 h1，代表根证书的原始内容）和指纹算法；然后用指纹算法对当前接收到的证书内容再进行一次 hash 计算得到另一个值 h2，代表当前证书的内容，如果此时 h2 和 h1 是相等的，就代表证书没有被修改过。

此时再检查证书的持有者对应的URL是否为我们请求的URL，如果是，那么就可以证明浏览器当前连接的是正确的网址
```

</details>

<!--  -->

<details>
<summary class="question">
介绍下 http1.0、1.1、2.0 协议的区别？
</summary>

http 1.0：1996年

- 每个请求都附加了 HTTP 版本
- 在响应开始时发送状态代码
- 请求和响应都包含 HTTP 报文头
- 内容类型能够传输 HTML 文件以外的文档

http 1.1：1997年

- 长连接：新增Connection字段，可以设置keep-alive值保持连接不断开
- 管道化：基于上面长连接的基础，管道化可以不等第一个请求响应继续发送后面的请求，但响应的顺序还是按照请求的顺序返回
- 缓存处理：新增字段cache-control
- 断点传输
  
http 2：2015年

- 二进制分帧：将所有传输的信息分割为更小的消息和帧,并对它们采用二进制格式的编码
- 多路复用： 在共享TCP链接的基础上同时发送请求和响应；基于二进制分帧，在同一域名下所有访问都是从同一个tcp连接中走，http消息被分解为独立的帧，乱序发送，服务端根据标识符和首部将消息重新组装起来
- 头部压缩
- 服务器推送：服务器可以额外的向客户端推送资源，而无需客户端明确的请求

二进制分帧：
```
HTTP/2 会将所有传输的信息分割为更小的消息和帧（frame）,并对它们采用二进制格式的编码。

帧：最小的通信单位，承载特定类型的数据，比如HTTP首部、负荷等等

消息：逻辑上的HTTP消息，比如请求、响应，由一或多个帧组成

流：虚拟信道，可以承载双向消息

HTTP2.0通信都在一个连接上完成，这个连接可以承载任意数据量的双向数据流。相应地，每个数据流以消息的形式发送，而消息由一或多个帧组成，这些帧可以乱序发送，然后再根据每个帧首部的流标识符重新组装。HTTP2.0的所有帧都采用二进制编码，所有首部数据都会被压缩。

```
<img class="image--md" src="https://user-images.githubusercontent.com/38830527/169827308-83ad87fb-22b9-4567-ac68-d67f46f77a1b.png" />


</details>

<!--  -->

<details>
<summary class="question">
301 与 302 区别
</summary>

301 Moved Permanently：永久重定向 说明请求的资源已经被移动到了由 Location 头部指定的url上，是固定的不会再改变。搜索引擎会根据该响应修正

302 Found：重定向状态码表明请求的资源被暂时的移动到了由该HTTP响应的响应头Location 指定的 URL 上。浏览器会重定向到这个URL， 但是搜索引擎不会对该资源的链接进行更新 

1. 应用场景

- 301应用场景: 域名到期不想继续用这个,换了地址
- 302应用场景: 做活动时候,从首页跳到活动页面

2. 浏览器缓存

- 默认情况下会被缓存，只有在第一次的时候，才会去真正的发起第一个请求，后面的都会被缓存起来，直接跳转到 redirect 的请求
- 默认情况下不会缓存

</details>

### <span id="browser"> 浏览器</span>
---

<details>
<summary class="question">
同源和跨域解决方案
</summary>
[链接](https://blog.csdn.net/ch834301/article/details/119582822)

同源策略：协议、域名、端口号都相同
跨域：跨域资源共享，会遇到浏览器安全限制

跨域解决方案：jsonp，cors，websocket，window.postMessage，document.domain + Iframe，

- jsonp：
1. 客户端：ajax方法放入回调函数名
2. 服务端：获取回调函数名返回jsonp格式，函数名+json数据放入
- cors：
1. 客户端：可以正常请求（浏览器会自动附带`Access-Control-Request-Method`等信息）
2. 服务端：返回`Access-Control-Allow-Origin`等http头不信息
- websocket：没有使用http，因此也没有跨域的限制，通过建立客户端和服务器之间存在持久的连接
- window.postMessage：可以安全地实现跨源通信
```js
window.postMessage("hello there!", "http://example.com");

window.addEventListener("message", (event) => {
  if (event.origin !== "http://example.com")
    return;
}, false);
```
- document.domain + Iframe：二级域名相同情况下设置`document.domain="test.com"`可以跨域通信

</details>

<!--  -->

<details>
<summary class="question">
浏览器存储方式都有哪些？cookie, localstorage, sessionStorage, indexedDB
</summary>

[链接](https://zhuanlan.zhihu.com/p/159268611)

|  | cookie | localstorage | sessionStorage | indexedDB |
| --- | --- | --- | --- | --- |
| 大小 | 4k | 5M | 5M | 不限制（硬盘容量） |
| 时间 | 可以设置过期时间 | 永久存储 | 会话（页面）关闭 |永久存储 |
| 传输 | 每次携带在header中 | 不跟随 | 不跟随 | 不跟随 |

cookie：
- 作用：解决http无状态的缺点，在客户端存储会话信息，记录用户的状态
- 构成：　　
1. 名称：一个唯一确定cookie的名称
2. 值：存储在cookie中的字符串值，值必须被URL编码
3. 域：cookie对于哪个域是有效的，所有向该域发送的请求都会包含这个cookie信息
4. 路径：对于指定域中的路径，应该向服务器发送cookie
5. 失效时间：表示cookie何时应该被删除的时间戳
6. 安全标志：指定后，cookie只有在使用SSL连接的时候才发送到服务器
- 使用：通过document.cookie获取对象
- 缺点：安全性，存储大小限制（20x4KB），频繁传送浪费资源

sessionStorage：
- 特点：
1. 同源策略限制
2. 单标签页限制
- 使用：
```js
sessionStorage.setItem(key,value)
sessionStorage.getItem(key)
```

localStorage:
- 特点：
1. IE8以上高版本支持
2. 同源限制
3. 频繁大量使用影响性能
4. 永久存储
- 使用：
```js
localStorage.setItem(key,value)
localStorage.getItem(key)
```

IndexedDB:
- 作用：本地数据库，允许建立索引，更接近NoSQL
- 特点：
1. 键值对储存
2. 同源限制
3. 异步操作防止锁死
4. 支持事务（transaction）回滚：一系列操作步骤之中，只要有一步失败，整个事务就都取消，数据库回滚到事务发生之前的状态
5. 支持二进制储存对象（Blob等）
- 使用：
```js
indexedDB.open("name",version)
// create store
db.createObjectStore("customers", { keyPath: "ssn" });
// add value
db.transaction(["customers"], "readwrite")
                .objectStore("customers")
                .add(value)
// delete value
db.transaction(["customers"], "readwrite")
                .objectStore("customers")
                .delete("444-44-4444");
// get value
db.transaction(["customers"])
                .objectStore("customers")
                .get("444-44-4444")
// put value
db.transaction(["customers"], "readwrite")
                .objectStore("customers")
                .put(data)
```
</details>

<!--  -->

<details>
<summary class="question">
浏览器缓存
</summary>

缓存策略：
[链接](https://blog.csdn.net/ljyahaha/article/details/118280568)

浏览器的缓存过程如下：
1. 开始加载，域名解析，DNS缓存
2. 本地缓存（memory缓存）
3. Http缓存（强缓存和协商缓存）
4. 服务端缓存（cdn缓存）


<img class="image--lg" src="https://user-images.githubusercontent.com/38830527/170191918-d46d3d83-9bf7-4bd4-92ad-056aa4365551.png" />

---
缓存位置：

优先级由上至下
1. Service Worker
2. Memory Cache：内存缓存
3. Disk Cache：硬盘缓存

内存缓存：作为短期缓存，几乎所有的请求资源 都能进入 memory cache,保证了一个页面中如果有两个相同的请求都实际只会被请求最多一次，避免浪费。
- 读取效率高，但是持续时间短，会随着进程的释放而释放

硬盘缓存：作为持久存储，严格根据 HTTP 头信息中的各类字段来判定哪些资源可以缓存
- 持续时间长，是实际存在于文件系统中的缓存
- 比内存缓存慢，但是优势在于存储容量大

service worker：页面与网络之间增加拦截器，用来缓存和拦截请求，它也作为PWA来试着解决离线存储和消息推送的问题。使得用户在离线环境中，也可以使用网络应用。
- 可以自由的控制缓存哪些文件、如何匹配读取缓存
- 缓存是持续性的
- 传输协议必须是 HTTPS

</details>

<!--  -->

<details>
<summary class="question">
介绍下重绘和回流（Repaint & Reflow），以及如何进行优化
</summary>

[链接](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/24)

浏览器渲染机制:

- 浏览器采用流式布局模型（Flow Based Layout）
- 浏览器会把HTML解析成DOM，把CSS解析成CSSOM，DOM和CSSOM合并就产生了渲染树（Render Tree）。
- 有了RenderTree，我们就知道了所有节点的样式，然后计算他们在页面上的大小和位置，最后把节点绘制到页面上。
- 由于浏览器使用流式布局，对Render Tree的计算通常只需要遍历一次就可以完成

重绘(repaint)：

- 由于节点的几何属性发生改变或者由于样式发生改变而不会影响布局的，称为重绘，例如outline, visibility, color、background-color等，重绘的代价是高昂的，因为浏览器必须验证DOM树上其他节点元素的可见性。

回流(reflow)：

- 回流是<span class="underline">布局</span>或者几何属性需要改变就称为回流。回流是影响浏览器性能的关键因素，因为其变化涉及到部分页面（或是整个页面）的布局更新。一个元素的回流可能会导致了其所有子元素以及DOM中紧随其后的节点、祖先节点元素的随后的回流。

<span class="underline">回流必定会发生重绘，重绘不一定会引发回流。 </span>

减少重绘与回流:

1. 减少使用改变布局的css属性，visibility 替换 display: none，transform 替代 top
2. 避免过于具体的 CSS 选择器，减少渲染成本
3. 将动画效果应用到position属性为absolute或fixed的元素上,脱离文档流
4. 避免频繁操作样式：样式列表定义为class并一次性更改class属性
5. 避免频繁操作DOM：创建一个`documentFragment`，在它上面应用所有DOM操作，最后再把它添加到文档中


</details>

### <span id="design"> 设计模式</span>
---
<details>
<summary class="question">
实现发布订阅模式
</summary>

[链接](https://blog.csdn.net/weixin_44761091/article/details/123636899)

- 定义map`{eventName:[fn1,fn2]}`存储事件和回调函数
- `on`方法存放事件和对应回调
- `emit`从map中找到对应事件全部触发


</details>

### <span id="package"> 工程化</span>
---

### <span id="implement"> 需求实现</span>
---

<details>
<summary class="question">
实现瀑布流
</summary>

[链接](https://blog.csdn.net/weixin_44116302/article/details/119561343)

[demo](https://codesandbox.io/s/pu-bu-liu-k9u2ii)

1. css： 分割数据到左右两个数组，垂直渲染（display：flex）
2. css：容器设置`column-count: 3;`,元素设置固定宽度
3. js: 
- 首次渲染可以获取元素高度
- 循环元素列表
- 设置数组记录第一行中元素高度，从第二行起在高度数组中找到最小高度，在该元素下方设置元素left,top属性放置图片，更新数组中高度信息
</details>

<!--  -->
<details>
<summary class="question">
实现虚拟列表
</summary>

[链接](https://blog.csdn.net/weixin_42232325/article/details/122560039)

[demo](https://codesandbox.io/s/virtuallist-1-rp8pi?file=/src/components/VirtualList.vue)

- 计算当前可视区域起始数据索引(`startIndex=Math.floor(scrollTop / this.itemSize)`)
- 计算当前可视区域结束数据索引(`endIndex=this.start + this.visibleCount`)
- 计算当前可视区域的数据，并渲染到页面中（`visibleData=data.slice(startIndex,endIndex)`）
- 计算startIndex对应的数据在整个列表中的偏移位置startOffset并设置到列表上(`this.startOffset = scrollTop - (scrollTop % this.itemSize);`)
</details>

<style>
  .question {
    box-shadow: inset 0 0 10px grey;
    color: #42c5f5;
    font-size:20px
  }
  .answer{
    box-shadow: inset 0 0 10px grey;
  }
  .image--sm {
    max-width: 100px;
  }
  .image--md {
    max-width: 500px;
  }
  .image--lg {
    max-width: 800px;
  }
  .underline {
    border-bottom: 1px solid red;
  }
  .bold{
    font-weight: bold;
  }
</style>