# promise的完整实现

```
    const PENDING='pending';
    const FULFILLED='fulfilled';
    const REJECTED='rejected';
    class MyPromise{
        constructor(callback){
            this.onFulfilled=[];  //成功的回调
            this.onRejected=[];   //失败的回调
            this._status=PENDING;

            const resolve=(val)=>{
                if(this._status===PENDING){
                    this._status=FULFILLED;
                    this._value=val;
                    this.onFulfilled.forEach(func=>func());
                }
            }
            const reject=(reason)=>{
                if(this._status===PENDING){
                    this._status=REJECTED;
                    this._reason=reason;
                    this.onRejected.forEach(func=>func());
                }
            }
            try{
                callback(resolve, reject);
            }catch(e){
                reject(e)
            }
 
        }
        then(onFullfilled, onRejected){
            onFullfilled = typeof onFullfilled === 'function' ? onFullfilled : value=>value;
            onRejected = typeof onRejected === 'function' ? onRejected : reason=>{throw reason};
            let anotherPromise = new MyPromise((resolve, reject)=>{
                if(this._status===FULFILLED){
                    setTimeout(()=>{
                        try{
                            let result=onFullfilled(this._value);
                            resolvePromise(anotherPromise, result, resolve, reject)
                        }catch(e){
                            reject(e);
                        }
                    }
                )
                }else if(this._status===REJECTED){
                    setTimeout(()=>{
                        try{
                            let result=onRejected(this._reason);
                            resolvePromise(anotherPromise, result, resolve, reject)
                        }catch(e){
                            reject(e);
                        }
                    })
                } else if(this._status===PENDING){
                    this.onFulfilled.push(()=>{
                        setTimeout(()=>{
                            try{
                                let result= onFullfilled(this._value)
                                resolvePromise(anotherPromise, result, resolve, reject)
                            }catch(e){
                                reject(e);
                            }
                        })
                    })
                    this.onRejected.push(()=>{
                        setTimeout(()=>{
                            try{
                                let result= onRejected(this._reason)
                                resolvePromise(anotherPromise, result, resolve, reject)
                            }catch(e){
                                reject(e);
                            }
                        })
                    })
                } 
            })
            function resolvePromise(promise, result, resolve, reject){
                if(promise===result){
                    reject(new TypeError('cycle error'));
                }
                if(result && typeof result==='object' || typeof result==='function'){
                    let used;
                    try{
                        let then=result.then;
                        if(typeof then === 'function'){
                            then.call(result, (y)=>{
                                if(used) return;
                                used=true;
                                resolvePromise(promise, y, resolve, reject);
                            }, (r)=>{
                                if(used) return;
                                used=true;
                                reject(r);
                            })
                        } else {
                            if(used) return;
                            used=true;
                            resolve(result);
                        }
                    } catch(e) {
                        if(used) return;
                        used=true;
                        reject(e)
                    }
                } else {
                    resolve(result);
                }
            }
            return anotherPromise;
        }
        catch(onRejected){
            return this.then(null, onRejected)
        }
        finally(callback){
            return this.then((value)=>{
                return MyPromise.resolve(callback()).then(()=>{
                    return value
                })
            }, (reason)=>{
                return MyPromise.resolve(callback()).then(()=>{
                    throw err;
                })
            })
        }
        static resolve(param){
            if(param instanceof MyPromise) return param;
            return new MyPromise((resolve, reject)=>{
                if(param&&param.then&&typeof param.then === 'function'){
                    setTimeout(()=>{
                        param.then(resolve, reject)
                    })
                } else{
                    resolve(param)
                }
            })
        }
        static reject(reason){
            return new MyPromise((resolve, reject)=>{
                reject(reason);
            })
        }
        static all(promises){
            return new MyPromise((resolve, reject)=>{
                let index=0;
                let result=[];
                if(promises.length===0) {
                    resolve(result);
                } else {
                    function processValue(i, data){
                        result[i]=data;
                        if(++index===promises.length){
                            resolve(result);
                        }
                    }
                    for(let i=0;i<promises.length;i++){
                        //promises[i]可能为普通值
                        MyPromise.resolve(promises[i]).then(data=>{
                            processValue(i, data);
                        }, err=>{
                            reject(err);
                            return;
                        })
                    }
                }
            })
        }
        static race(promises){
            return new MyPromise((resolve, reject)=>{
                if(promises.length===0){
                    return;
                }else{
                    for(let i=0;i<promises.length;i++){
                        MyPromise.resolve(promises[i]).then(data=>{
                            resolve(data);
                            return;
                        }, err=>{
                            reject(err);
                            return;
                        })
                    }
                }
            })
        }
        static any(promises){
            return new MyPromise((resolve, reject)=>{
                let index=0;
                let errResult=[];
                if(promises.length===0){
                    return;
                }else{
                    function processReject(i, data){
                        errResult[i]=data;
                        if(++index===promises.length){
                            reject(errResult);
                        }
                    }
                    for(let i=0;i<promises.length;i++){
                        MyPromise.resolve(promises[i]).then(data=>{
                            resolve(data);
                            return;
                        }, err=>{
                            processReject(i, err);
                        })
                    }
                }
            })
        }
        static allSettled(promises){
            return new MyPromise((resolve, reject)=>{
                let index=0;
                let result=[];
                if(promises.length===0){
                    return result;
                }else{
                    function process(i, data){
                        result[i]=data;
                        if(++index===promises.length){
                            resolve(result);
                        }
                    }
                    for(let i=0;i<promises.length;i++){
                        MyPromise.resolve(promises[i]).then(data=>{
                            process(i, {status: 'fulfilled', value: data});
                        }, err=>{
                            process(i, {status: 'rejected', reason: err});
                        })
                    }
                }
            })
        }
        static try(callback){
            return new MyPromise((resolve, reject)=>{
                try{
                    resolve(callback())
                }catch(e){
                    reject(e);
                }
            })
        }
        //测试专用
        static defer(){
            let dfd={}
            dfd.promise=new MyPromise((resolve, reject)=>{
                dfd.resolve=resolve;
                dfd.reject=reject;
            })
            return dfd;
        }
         //测试专用
        static deferred(){
            let dfd={}
            dfd.promise=new MyPromise((resolve, reject)=>{
                dfd.resolve=resolve;
                dfd.reject=reject;
            })
            return dfd;
        }
    }
    module.exports = MyPromise;
```

