# Promise对象
## Promise的特点

 - 对象的状态不受外界影响。Promise对象代表一个异步操作，有3种状态：Pending、Resolved(Fullfilled)、Rejected
 - 一旦状态改变就不会再变，任何时候都可以得到这个结果。Pending => Resolved 和Pending => Rejected
## Promise的优缺点
优点：
 - 可以将异步操作以同步操作的流程表达出来，避免层层嵌套的回调函数
 - 提供了统一API，使控制异步操作更加容易
缺点：
 - 无法取消Promise，一旦新建它就会立即执行，无法中途取消
 - 如果不设置回调函数，Promise内部抛出的错误不会反应到外部
 - 当处于Pending状态时，无法得知目前进展到哪个阶段（刚刚开始还是即将完成）

## Promise基本用法
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject，它们是两个函数。其中resolve函数的作用是把Promise对象状态从Pending => Resoved，reject函数的作用是把Promise对象状态从Pending => Rejected。

Promise实例生成后，可以用then犯法分别指定Resolved状态和Rejected状态的回调函数。then方法接受两个回调函数作为参数，第一个回调函数是Promise对象的状态 Pending => Resolved，第二个回调函数是Promise对象状态Pending => Rejected。第二个是可选的，不一定要提供，这两个函数都接受Promise对象传出的值作为参数

## Promise.prototype.then()

 - then方法是定义在原型对象Promise.prototype上，它的作用是为Promise实例添加改变状态时的回调函数
 - then方法返回的是一个新的Promise实例（不是原来的那个Promise实例），因此可以采用链式写法，即then方法后再调用另一个then方法

## Promise.prototype.catch()

 - Promise.prototype.catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数
 - Promise在resolve里再抛错误，并不会被捕获，等于没抛出
 - Promise对象的错误具有『冒泡』性质，会一直往后传递，直到被捕获为止，即错误总是会被下一个catch语句捕获
 - 推荐promise.then().catch()写法，不推荐promise().then((data) => {xx}, (eror) => {xx})
 - catch方法返回的还是一个Promise实例，还可以接着调用then方法
 - catch方法还能再抛出错误，若后面没有别的catch方法，导致这个错误不会被捕获，也不会传递到外层
## Promise.all()
 - Promise.all()方法用于将多个Promise实例包装成新的Promise实例
 - p1，p2，p3都是Promise实例，若不是Promise实例，会调用Promise.resolve方法，将参数转为Promise实例，再进一步处理
 - **p的状态由p1、p2、p3决定。只有三个的状态都变为Fullfilled，P的状态才会变为Fullfilled**，此时p1、p2、p3的返回值会传递给p的回调函数。**若其中一个状态变为Rejected，p的状态则变为Rejected，第一个被Rejected实例的返回值会传递给p的回调函数**
  ```
 var p = Promise.all([p1,p2,p3]);
 
 ```
  ## Promise.race()
 
 - Promise.race()方法也是将多个Promise实例包装成新的Promise实例
 - **只要p1、p2、p3中有一个实例率先改变状态**，p的状态就会跟着改变，那个率先改变的Promise实例的返回值，就传给p的回调函数
 - Promise.race()方法和Promise.all()方法一样，如果不是Promise实例，就会调用Promise.resolve()方法，将参数转为Promise实例，再做进一步的处理
 
 ## Promise.resolve()
 
 - 将现有对象转为Promise对象
 - 如果Promise.resolve方法的参数不是具有then方法的对象，则返回一个新的Promise对象，且状态为Resolved
 - Promise.resolve方法允许调用时不带参数，故希望得到一个Promise对象，比较方便的方法就是直接调用Promise.resoleve方法

## Promise.reject()

 - Promise.reject(reason)方法会返回一个新的Promise实例，状态为Rejected。Promise.reject方法的参数reason会被传递给实例的回调函数
 
 ## Promise.done()
 

