> 1、实现一个LazyMan，可以按照以下方式调用:
         1) LazyMan(“Hank”)输出:
         Hi! This is Hank!
        2) LazyMan(“Hank”).sleep(10).eat(“dinner”)输出
         Hi! This is Hank!
         //等待10秒..
         Wake up after 10
         Eat dinner~
        3) LazyMan(“Hank”).eat(“dinner”).eat(“supper”)输出
         Hi This is Hank!
         Eat dinner~
         Eat supper~
        4) LazyMan(“Hank”).eat(“dinner”).sleepFirst(5).eat(“supper”)输出
         //等待5秒
         Hi This is Hank!
         Wake up after 5
         Eat supper

```
class Man {
            constructor(name) {
                //constructor代表Man的函数体
                this.ary = [];//存放要执行的事件，再设立一个专门的函数去执行数组中的事件
                // this.next();不好使，因为此时的ary是空，需要等eat，sleep执行完之后再去执行next函数
                let f = () => {
                    console.log(`Hi This is ${name}!`);
                    this.next();
                }
                this.ary.push(f);
                setTimeout(() => {
                    //异步函数会在同步执行完成之后执行
                    this.next();
                })
            }
            next() {
                let f = this.ary.shift();
                f && f();
            }
            eat(meal) {
                let f = () => {
                    //定义一个事件
                    console.log(`Eat ${meal}~`);
                    this.next();
                }
                //把事件放到this.ary里
                this.ary.push(f);
                return this;
            }
            sleep(t) {
                let f = () => {
                    setTimeout(() => {
                        console.log(`Wake up after ${t}`);
                        this.next();
                    }, t * 1000);
                }
                this.ary.push(f);
                return this;
            }
            sleepFirst(t) {
                let f = () => {
                    setTimeout(() => {
                        console.log(`Wake up after ${t}`);
                        this.next();//继续调用事件池中的函数
                    }, t * 1000);
                }
                this.ary.unshift(f);
                return this;
            }
        }
        function LazyMan(name) {
            return new Man(name);
        }
        LazyMan('Hank').eat('dinner').sleep(3).eat('supper').sleepFirst(5)
```
