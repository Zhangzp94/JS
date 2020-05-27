### 1、ES6类语法

1.创建类 语法规范

```js
<script>
    /*
    * 1.每个类中都有constructor()函数
    * */
    class Father {
        constructor(x, y) {
            this.x = x;
            this.y = y;
        }
        say() {
            return "this is Father"
        }
        data() {
            return this.x + this.y
        }
    }
    class Son extends Father{
        constructor(x,y) {
            /*
            * 子类中用父类中的data方法时，data中的this指向的是父类中的constructor函数，
            * super(x,y)方法调用了父类中的constructor函数，那么子类中constructor函数
            * 的参数就可以直接传给super也就是父类的constructor
            * */
            super(x,y);//super(x,y)必须放在子类的this之前！
            this.x = x;
            this.y = y;
        }
        say() {
            //这里super.say()可以调用父类中say方法
            return super.say() + "this is Son"
        }
        subtract(){
            return this.x - this.y
        }
    }
    var son  = new Son(1,2)
    console.log(son.data())
</script>
```



