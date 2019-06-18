Promise 初探
===

### Promise认识
- Promise 对象是由关键字 new 及其构造函数来创建的,有三种状态的，等待态pending / 成功态resolved / 失败态rejected  
- Promise的状态是可以转换的，可以从pending -> resolved 或 rejected，但是不可逆，状态只会改变一次  
- 当异步任务顺利完成且返回结果值时，会调用 resolve 函数；
- 而当异步任务失败且返回失败原因（通常是一个错误对象）时，会调用reject 函数

        let myPromise = new Promise(((resolve, reject) => {
            //做一些异步操作
            setTimeout(() => {
                let num = Math.random().toFixed(1);
                num >= 0.5 ? resolve(num) : reject(num);
            }, 1000);
        }));
    
        myPromise.then(value => {
            console.log('success-->', value);
        }).catch(error => {
            console.log('fail-->', error);
        });

### Promise.all
- 可以将多个Promise实例包装成一个新的Promise实例（异步请求并行操作）  
- 所有promise结果成功返回时按照请求顺序返回resolve，返回结果数组
- 当其中有一个失败方法时，则进入reject

        let promise1 = new Promise((resolve => resolve("promise 1  success")));
        let promise2 = new Promise((resolve => resolve("promise 2  success")));
        let promise3 = new Promise(((resolve, reject) => reject("promise 3  fail")));
    
        Promise.all([promise1, promise2]).then(res => {
            console.log(res);   //['promise 1  success','promise 2  success' ]
        }).catch(error => {
            console.log(error);
        });
    
        Promise.all([promise1, promise2, promise3]).then(res => {
            console.log(res);
        }).catch(error => {
            console.log(error); // promise 3  fail
        });
        
### Promise.race
- 返回一个 Promise, 这个 Promise 是一个已经被 resolved或者rejected  
- 类似于赛跑，返回的结果为最快被resolved或者rejected的值  

        //正常情况
        let racePromise1 = new Promise((resolve, reject) => {
            setTimeout(() => resolve('racePromise1'), 1000);
        });
        let racePromise2 = new Promise((resolve, reject) => {
            setTimeout(() => resolve('racePromise2'), 500);
        });
    
        //正常情况，哪个promise先resolve或
