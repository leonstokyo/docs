# 实现Promise

> 根据promise/A+规范实现一个Promise

## 基础版Promise
```javascript
class Promise {
    constructor(executorCallBack) {
        this.status = 'pending';
        this.value = undefined;
        this.fulfilledArr = [];
        this.rejectedArr = [];

        let resolveFn = (result) => {
            if (this.status !== 'pending') {
                return
            }
            let timer = setTimeout(() => {
                clearTimeout(timer);
                this.status = 'fulfilled';
                this.value = result;
                this.fulfilledArr.forEach(item => item(this.value))
            })
        };

        let rejectFn = (reason) => {
            if (this.status !== 'pending') {
                return
            }
            let timer = setTimeout(() => {
                clearTimeout(timer);
                this.status = 'rejected';
                this.value = reason;
                this.rejectedArr.forEach(item => item(this.value))
            })
        };
      executorCallBack(resolveFn, rejectFn)
    }

    then(fulfilledCallBack, rejectedCallBack) {
        this.fulfilledArr.push(fulfilledCallBack);
        this.rejectedArr.push(rejectedCallBack)
    }
}

module.exports = Promise;
```
