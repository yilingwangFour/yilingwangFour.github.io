# JS高级 day1

### day1

![image-20220808194255776](JS%E9%AB%98%E7%BA%A7.assets/image-20220808194255776.png)

![](JS%E9%AB%98%E7%BA%A7.assets/image-20220808194628096.png)

#### 一：作用域

##### 1.1 作用域：

```js
         <script>
        // 作用域: Scope  
        // 变量和函数的可访问范围. 即 作用域 控制着变量和函数的可见性 和 生命周期. 

        // 2. 全局作用域 
        // 直接写在script 或者js文件中的 代码, 属于全局作用域.
        // 特点: 
        // 可见性 : 全局做用域中声明的变量, 在代码任何地方都可以访问.
        // 生命周期: 关闭页面的时候结束.

        // Eg.举例
        // const num = 10  // 全局作用域中的变量, 
        // function fn () {
        //     // 函数内部可以访问
        //     console.log(num)
        // }
        // fn()

        // 不用任何关键字声明的变量, 会成为全局变量
        // function foo(){
        //     a = 10
        // }
        // foo() // 需要调用一下函数
        // console.log(a)

        // 3. 局部作用域
        // ES5以前, 局部作用域就是函数作用域
        // 在函数内部声明的变量, 只能在函数内部访问, 外部无法 直接 访问!

        // 可见性: 函数外部无法访问函数内部的变量
        // 生命周期: 函数执行完毕就销毁.

        function getSum(){
            // 函数作用域 局部作用域
            const num = 10
        }
        getSum()
        console.log(num)  // Error 函数外部无法访问函数内部的变量
    </script>
```

##### 1.2 块级作用域：

![image-20220808194731010](JS%E9%AB%98%E7%BA%A7.assets/image-20220808194731010.png)

```js
    <script>
        // 为什么需要块级作用域

        var temp = new Date()
        function fn(){
            var temp  // undefined
            console.log(temp)
            if(false) {
             temp = 'hello world'
            }
        }
        fn() // hello world

        // if , for 不是函数, 是语句!

        // var 声明的变量, 会提升到当前作用域的最顶层, 赋值没有提升
        // var 声明的变量, 不赋值, 初始为undefined


        // 3. 块级作用域
        // 定义: 使用 let 或者 const声明的变量, 在 {} 中会产生块级作用域
        var temp = new Date()
        function fn(){
            console.log(temp)

            if(false) {
             let temp = 'hello world'
            }
        }
        fn() 

        // 1. for,if 不是函数, 但是 {}, 能生成块级作用域
        // 2. var 声明的变量, 不会产生块级作用域
        // 3. let , const 不存在变量提升 


        // 相互不影响
        // for (let i = 0; i < 5; i++) {
        //     console.log(i)
        // }

        // for (let i = 0; i < 5; i++) {
        //     console.log(i)
        // }

        // if (true) {
        //     const i = 10
        // }
        // console.log(i)


        // let num = 10
        // window.num

        //==> var 声明的变量 会成为window对象的属性, let, const声明的不会
        // var num1 = 10
        // console.log(window.num1)

        let a1 = 10
        console.log(a1)
        console.log(window.a1)

    </script>
```

##### 1.3 作用域链：

```js
   <script>
        // 全局作用域
        let a = 3 
        let b = 1
        function f(){
            // 局部作用域
            // let a = 1
            function g(){
                // 局部作用域
                a = 2
                console.log(a)
            }
            g() // 调用g
        }
        f() // 调用f

        // 作用域链: 本质就是底层变量的查找机制.
        // 查找规则
        // 1. 优先在当前作用域中查找变量
        // 2. 如果当前作用域找不到, 依次往上层作用域查找, 直到全局作用域
    </script>
```

![image-20220808203801600](JS%E9%AB%98%E7%BA%A7.assets/image-20220808203801600.png)

##### 1.4 垃圾回收机制：

```js
   <script>

        // function fn(){
        //     // 当函数执行完毕, 这个变量所占的内存空间会被 自动回收
        //     const str = 'hello'
        //     console.log(str)
        // }

        // fn() 
        

        // 垃圾回收机制  简称 GC
        // 内存在不使用的时候会被垃圾回收程序自动回收。（JS中内存的分配和闲置资源的回收都是自动完成的）

        // 内存泄漏: 不再用到的内存，没有及时释放

        // 内存的生命:
        // 1. 内存分配
        // 2. 内存使用
        // 3. 内存回收

        // 1. 引用计数
        let person = {
            age: 18,
            name: 'pink老师'
        }
        let p = person
        person = 1
        p = null

        // 引用计数缺陷：循环引用，无法回收; 造成内存泄漏

        // function problem () {
        //     // {}  ==》 new Object()
        //     let oA = {}
        //     let oB = {}
        //     oA.a = oB  //  oB被oA引用
        //     oB.a = oA  //  oA又被oB引用
        // }
        // problem()
    </script>
```

![image-20220808203831904](JS%E9%AB%98%E7%BA%A7.assets/image-20220808203831904.png)

##### 1.5 闭包：

```js
    <script>

        // 面试80 90%考 

        
        // 定义 ： 内部函数引用外部函数的变量的集合
        // ==> 闭包 = 内层函数 + 外层函数的变量
        // 注意： 内层函数一定要引用外层函数的变量

        // 1. 首先要有内层函数
        // 2. 内层函数使用了外层函数的变量 

        // 简单的写法
        // closure 闭包
        function outer(){
            // 外层函数outer 把内存函数和这个被引用的变量 , 包起来.
            let a = 10
            function fn(){
                console.log(a)
            }
            fn()
        }
        outer()

        // scope 作用域  local 局部作用域 global 全局作用与 closure 闭包


    </script>
```

##### 1.5.1 闭包的应用与防抖节流

```js
    <script>
        // 闭包的应用
        // 可以统计函数调用的次数

        // 普通的方式
        // let i = 0
        // function fn(){
        //     i++
        //     console.log(`函数调用了${i}多少次`)
        // }
        // fn()

        // 闭包的方式来写
        // 记录函数调用的次数, 外部不能修改这个i变量,但是可以访问.
        // 实现了数据的私有化 
        function count(){
            let i = 0 // 局部变量
            return function(){
                i++
                console.log(`函数调用了${i}多少次`)
            }
        }
        const fun = count()
        fun()
        // fun()

        // 引用计数
        // 标记清除

        // 闭包会产生的问题  : 内存泄漏
        // 1.本来 i 是函数内部的局部变量, 调用count()函数, 按理说, 应该释放这个i变量(内存)
        // 2.但是这里, 我们每次调用 fun() 都能访问到这个 i变量, i本来应该被释放, 而没被释放.
        // 所以会造成内存泄漏.
        //  (标记清除: 从全局作用域出发, 能访问到的变量都做上标记, 不回收) 

        // function bar(){
        //     const a = 1
        // }
        // bar()

        // 闭包应用 ：  节流， 防抖 (性能优化 ) 
        // 节流：throttle 减少事件的执行频率。 50ms 执行一次； 
        // 防抖：debounce 比如 点击按钮， 打印一句话。 500ms内，只让它触发一次。
        window.addEventListener('resize', function(){
            console.log(1)
        })
    </script>
```

![image-20220808203907951](JS%E9%AB%98%E7%BA%A7.assets/image-20220808203907951.png)

##### 1.6 变量的提升

```js
    <script>

        // 1. var 声明的变量， 会提升到当前作用域的最顶层
        // 2. 只提升声明， 不提升赋值。
        var num
        console.log(num + '件')
        num = 10
        // var num
        // console.log(num + '件')
        // num = 10
        // console.log(num)

        function fn() {
            var num
            // 只提升到当前作用域的最前
            console.log(num)
            num = 10
        }
        fn()

    </script>
```

##### 1.6.1 函数声明的提升

```js
<script>
        // 定义函数有两种方式 
        // 1. function声明的函数， 会提升到当前作用域最顶层。

        // 声明式 
        // fn()
        // function fn(){
        //     console.log(123)
        // }

        // 表达式
        // 2. 用var声明的函数，会提升声明， 赋值不提升。
        var fn; // undefined
        fn()
        fn = function(){
            console.log(111)
        }
    </script>
```

#### 二：函数进阶

##### 2.1  函数声明的提升

<script>
        // 定义函数有两种方式 
        // 1. function声明的函数， 会提升到当前作用域最顶层。

```js
   <script>
    // 定义函数有两种方式 
    // 1. function声明的函数， 会提升到当前作用域最顶层。
	// 声明式 
    // fn()
    // function fn(){
    //     console.log(123)
    // }

    // 表达式
    // 2. 用var声明的函数，会提升声明， 赋值不提升。
    var fn; // undefined
    fn()
    fn = function(){
        console.log(111)
    }
</script>
```

##### 2.2 函数参数

###### 2.2.1 动态参数

```js
   <script>

        // arguments 函数的动态参数

        // 1. 所有的函数都内置了（默认就存在，自带的）一个arguments对象
        // 2. arguments是一个伪数组， 存储了传递来的所有实参。
        // 3. 它只存在于函数中


        // function getSum(){
        //     console.log(arguments)
        //     let sum = 0 
        //     for (let i = 0; i < arguments.length; i++) {
        //         sum += arguments[i]
        //     }
        //     console.log(sum)
        // }
        // // getSum(1, 2, 3)
        // getSum(1, 2, 3, 4, 5)

        // 伪数组： 有length， 有索引号， 没有数组的pop， push一些方式， 可以遍历

    </script>
```

![image-20220808204503854](JS%E9%AB%98%E7%BA%A7.assets/image-20220808204503854.png)

###### 2.2.2 剩余参数

```js
   <script>
        // rest参数， 剩余参数。 ES6新增。
        // 语法： ...变量
        // 1. rest剩余参数 是一个真实的数组， 存了剩余的实参
        // 2. 只能放到参数的最后一位。

        // function getSum(...arr) {
        //     console.log(arr)
        //     let sum = 0; 
        //     for (let i = 0; i < arr.length; i++) {
        //         sum += arr[i]
        //     }
        //     console.log(sum)
        // }
        // 不能写到参数的中间， 只能是最后一位
        // function getSum(a, b, ...arr ) {
        //     console.log(arr)
        // }
        
        // getSum(1, 2, 3, 4, 5)

        // 箭头函数的写法
        // 1. 箭头函数没有arguments动态参数， 但是有rest剩余参数。
        const getSum = (...arr) => {
            console.log(arr)
            let sum = 0; 
            for (let i = 0; i < arr.length; i++) {
                sum += arr[i]
            }
            console.log(sum)
        }

        getSum(1, 2, 3, 4, 5)
    </script>
```

![image-20220808204631390](JS%E9%AB%98%E7%BA%A7.assets/image-20220808204631390.png)

###### 2.2.3 扩展运算符

```js
<script>
        // 剩余参数， 剩余的参数列表放到数组里

        // spread 
        // 扩展运算符(...arr) 将一个数组进行展开, 变为参数列表
        // const arr = [1, 5, 3, 8, 2]
        // console.log(...arr)
        // 注意: 不会修改原数组

        // 主要应用 
        // 1. 求数组最大最小值
        const arr = [1, 3, 5, 9, 8]
        console.log(Math.max(...arr)) // Math.max() 接收的是参数列表
        console.log(Math.min(...arr))

        // 2. 合并数组
        const arr1 = [1, 2, 3]
        const arr2 = [5, 6, 7]

        const arr3 = [...arr1, ...arr2]
        console.log(arr3)
    </script>
```

![image-20220808204814439](JS%E9%AB%98%E7%BA%A7.assets/image-20220808204814439.png)

![image-20220808204828798](JS%E9%AB%98%E7%BA%A7.assets/image-20220808204828798.png)

##### 2.3 箭头函数

```js
    <script>
        // 表达式的方式
        const fn = function(){
            console.log(1)
        }

        // 箭头函数 =>  来定义一个函数
        const fn1 = () => {
            console.log(1)
        }
        fn1()

        // 1. 传参 
        const fn2 = (x) => {
            console.log(x)
        }
        fn2(8)

        // 2. 只有一个形参的时候，可以省略小括号 ()
        const fn3 = x => {
            console.log(x)
        }
        fn3(9)

        // 3. 只有一行代码的时候， 可以省略花括号 {} 大括号   ()小   []中（方）  {} 大（花） 
        const fn4 = x => console.log(x)
        fn4(6)

        // 4. 只有一行代码的时候， 可以省略return
        const fn5_ = x => {
            return x + x 
        }
        const fn5 = x => x + x 
        console.log(fn5(6))

        // 5. 箭头函数可以直接返回一个对象， 但必须在对象外面加上小括号
        const fn6_ = (name) => {
            return {user_name : name}
        }
        const fn6 = name => ({user_name : name})

        console.log(fn6('周杰伦'))

    </script>
```

![image-20220808205125919](JS%E9%AB%98%E7%BA%A7.assets/image-20220808205125919.png)

###### 2.3.1 箭头函数的this

```js
    <button class="btn"></button>
    <script>
        // this指向， 谁调用， 指向谁。
        // this 在定义的时候，不能确定this的指向，执行的时候才能确定。 

        // 普通函数，定时器
        // function fn(){
        //     console.log(this)  // window
        // }
        // fn()  // window.fn()

        // setTimeout(function(){
        //     console.log(this)  // window
        // }, 1000);
        // window.setTimeout()

        // 对象调用， 谁调用， 指向谁
        const obj = {
            name:'xd',
            age:18,
            sayHi:function(){
                // console.log(this)  // obj
                console.log(this.name)
            }
        }
        obj.sayHi() 
        
        const btn = document.querySelector('.btn')
        btn.addEventListener('click', function(e){
            console.log(this) // 绑定事件的元素
            console.log(e.target) // 触发事件的元素 点的谁指向谁
            console.log(e.currentTarget)  // 绑定事件的元素， 同this
        })


        // 1. 箭头函数的this是上层作用域中的this  
        // const fn = () => {
        //     // this
        //     console.log(this)
        // }
        // fn()

        // 2. 对象里面不建议写箭头函数！ 
        const obj2 = {
            user:'xx',
            sayHi: () => {
                console.log(this)  // window
                // 箭头函数里没有this, 会向上层作用域中找this
            }
        } 
        obj2.sayHi()
    </script>
```

![image-20220808205332776](JS%E9%AB%98%E7%BA%A7.assets/image-20220808205332776.png)

#### 三：解构赋值

##### 3.1 数组解构

```js
    <script>
        // ES5 为变量赋值，只能直接指定值
        // let a = 1
        // let b = 1
        // let c = 1

        // ES6 解构赋值
        let [a, b, c] = [1, 2, 3]
        console.log(a, b, c)
        // 从数组中提取值，按照对应的位置，对变量赋值

        const arr = [100, 60, 80]
        const [max, min, avg] = arr
        console.log(max, min, avg)

        // 交换两个变量
        let e = 4
        let f = 8
        // ES6 解构赋值 交换两个变量
        ;[f, e] = [e, f]
        console.log(e, f);
        // 注意 [] 前面需要加分号;

        // 需要加分号的情况
        // 1.() 立即执行函数
        // 2.[] 数组

        // 得到一个新数组，新数组的元素的平方
        const arr1 = [1, 2, 3]
        const res = arr1.map( x => x * x)
        console.log(res)

        // const res = arr1.map(function(x) {
        //     return x * x 
        // })
        // console.log(res)

        // 遍历数组
        ;[1, 2, 3].forEach(item => {
            console.log(item)
        })

    </script>
```

![image-20220808205743967](JS%E9%AB%98%E7%BA%A7.assets/image-20220808205743967.png)

###### 3.1.1 数组解构练习

```js
    <script>
        const pc = ['惠普', '联想', '苹果', '小米']
        const [hp, lx, ap, mi] = pc
        console.log(hp, lx, ap, mi)

        function getValue() {
            return [100, 60]
        }
        const [max, min] = getValue()
        console.log(max, min)
    </script>
```

###### 3.1.2 解构赋值的细节

```js
    <script>
        // 1.变量多，数组元素很少
        // const [a, b, c, d] = [1, 2, 3]  // d undefined
        // console.log(a, b, c, d)

        // 2.变量少，数组元素多
        // const [a, b] = [1, 2, 3, 4, 5]  
        // console.log(a, b)

        // 3.变量少， 数组元素多，可以利用剩余参数接收多余的元素
        // const [a, b, ...c] = [1, 2, 3, 4, 5]  
        // console.log(a, b, c)

        // 4.按照对应的位置， 依次对变量赋值
        // const [a, b, , d] = [1, 2, 3, 4]
        // console.log(a, b, d)

        // 5.解构赋值允许指定默认值
        // const [a = 0, b = 0] = [1, 2]
        // console.log(a, b)

        // 当数组对应位置不存在 或者 严格等于undefined时，默认值才生效
        // const [a = 0, b = 0] = []

        // 引号包起来的都是字符串 ==> '1'  'null'
        const [a = 0, b = 1] = ['undefined', undefined]
        console.log(a)  // undefined
        console.log(b)  // 1

        // 6.如果等号右边不是数组（不可遍历数组的结构），那么会报错
        // let [foo] = 1
        // let [foo] = false
        // let [foo] = NaN
        // let [foo] = null
        // let [foo] = undefined
        // let [foo] = {}
    </script>
```

![image-20220808205916823](JS%E9%AB%98%E7%BA%A7.assets/image-20220808205916823.png)

###### 3.1.3 多维数组

```js
   <script>
        // 多维数组： 数组嵌套数组
        const arr = [1, 2, [3, 4]]  // 二维数组

        console.log([arr[2]])
        console.log(arr[2][0])
        console.log(arr[2][1])

        // 多维数组的解析
        const arr1 = [1, 2, [3, 4]]
        // const [a, b, c] = arr1
        // console.log(a, b, c)

        const [a, b, [c, d]] = [1, 2, [3, 4]]
        console.log(a, b, c, d)
    </script>
```

###### 3.1.4 数组解构总结

```js
   <script>
         // 本质上, 解构赋值的写法属于模式匹配, 只要等号两边的模式相同
        // 左边的变量就会被赋予对应的值
        let [foo,[[bar], baz]] = [1, [[2], 3]]
        console.log(foo, bar, baz)

        let [, , third] = ['foo', 'bar', 'baz']
        console.log(third)

        // let [x, , y] = [1, 2, 3]
        // console.log(x, y)

        let [head, ...tail] = [1, 2, 3, 4]
        console.log(head, tail)

        let [x, y, ...z] = ['a']
        console.log(x) // 'a'
        console.log(y) // undefined
        console.log(z) // []

        let [fn] = [] // 
        console.log(fn) // undefined
        let [fn1, fn2] = [1]
        console.log(fn1, fn2)  // 1, undefined
    </script>
```

##### 3.2对象解构

![image-20220808210344510](JS%E9%AB%98%E7%BA%A7.assets/image-20220808210344510.png)

```js
 <script>
        const obj = {
            uname:'周杰伦',
            age: 18
        }
        // obj.uname
        // obj.age

        // >>> 数组的解构  按照位置依次赋值  []
        // 对象的解构，是按照属性名解构，可以交换位置。

        // ==> 要求变量名必须和属性名一致才可以解构
        const {age, uname} = {uname:'周杰伦', age: 18}
        console.log(uname, age)
    </script>
```

###### 3.2.1 对象解构重命名

```js
 <script>
        // 1. 对象的解构赋值， 可以重新命名
        // 旧变量名：新变量名
        const {uname:username, age1} = {uname:'周杰伦', age1: 18}

        console.log(username, age1)

        const obj = {first:'Hello', last:'world'}
        const {first: f, last: l} = obj

        console.log(f, l)

        // 结论
        // 对象解构的内部机制，是先找到同名属性
        // 然后在赋值给对应的变量，真正被赋值的是冒号后面那个

        // 2.解构数组对象
        const pigs = [
            {
                name:'佩奇',
                 age: 10
            }
        ]
        const [{name, age}] = pigs
        console.log(name, age)




        // 练习
        // 1.
        const pig = {name:'佩奇', age: 6}
        const {name:mz, age:ages} = pig
        console.log(mz, ages)


        // 3.
        const goods = [{
            goodsName:'小米',
            price: 1999
        }]
        const [{goodsName, price}] = goods
        console.log(goods);
    </script>
```

![image-20220808210448109](JS%E9%AB%98%E7%BA%A7.assets/image-20220808210448109.png)

###### 3.2.2 多级对象的解构

```js
 <script>
        //   const pig = {
        //     name:'佩奇',
        //     family: {
        //         mother: '猪妈妈',
        //         father: '猪爸爸',
        //         sister: '乔治'
        //     },
        //     age: 6
        // }
        // 多级对象的解构
        // const {name, family:{mother, father, sister}, age} = pig
        // // console.log( family)
        // console.log(name, mother, father, sister, age)

        // 思考 2
        const pig = [
            {
                name:'佩奇',
                family: {
                    mother: '猪妈妈',
                    father: '猪爸爸',
                    sister: '乔治'
                },
                age: 6
            }
        ]

        // 冒号左边的family不是解构， 是模式匹配
        const [{name, family:{mother, father, sister}, age}] = pig
        // console.log( family)
        console.log(name, mother, father, sister, age)
    </script>
```

###### 3.2.3 函数参数的解构

```js
  <script>
               // 函数参数的解构
        // const pos = {x:10, y: 20}
        // function move(pos){
        //     // console.log()
        //     const {x, y} = pos
        //     console.log(x,y)
        // }
        // move(pos)

        // 2. 在形参的位置解构， 里面直接用
        const pos = {x:10, y: 20}
        function move({x, y}){
            console.log(x,y)
        }
        move(pos)

        // 3. 直接解构，同时重命名
        // const pos = {x:10, y: 20}
        // function move({x:aa, y}){
        //     console.log(aa,y)
        // }
        // move(pos)

        // 4. 函数的参数是数组
        // 可以在形参的位置直接解构
        function add([x, y]) {
            return x + y
        }
        const ass = add([3, 6])
        console.log(ass)
    </script>
```

#### 四：渲染商品案例

```js
<body>
    <div class="list">
        <!-- <div class="item">
          <img src="" alt="">
          <p class="name"></p>
          <p class="price"></p>
        </div> -->
      </div>
    <script>
      const goodsList = [
      {
        id: '4001172',
        name: '称心如意手摇咖啡磨豆机咖啡豆研磨机',
        price: '289.00',
        picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg',
      },
      {
        id: '4001594',
        name: '日式黑陶功夫茶组双侧把茶具礼盒装',
        price: '288.00',
        picture: 'https://yanxuan-item.nosdn.127.net/3346b7b92f9563c7a7e24c7ead883f18.jpg',
      },
      {
        id: '4001009',
        name: '竹制干泡茶盘正方形沥水茶台品茶盘',
        price: '109.00',
        picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png',
      },
      {
        id: '4001874',
        name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器',
        price: '488.00',
        picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png',
      },
      {
        id: '4001649',
        name: '大师监制龙泉青瓷茶叶罐',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png',
      },
      {
        id: '3997185',
        name: '与众不同的口感汝瓷白酒杯套组1壶4杯',
        price: '108.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8e21c794dfd3a4e8573273ddae50bce2.jpg',
      },
      {
        id: '3997403',
        name: '手工吹制更厚实白酒杯壶套装6壶6杯',
        price: '99.00',
        picture: 'https://yanxuan-item.nosdn.127.net/af2371a65f60bce152a61fc22745ff3f.jpg',
      },
      {
        id: '3998274',
        name: '德国百年工艺高端水晶玻璃红酒杯2支装',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8896b897b3ec6639bbd1134d66b9715c.jpg',
      },
    ]
        // 1. 声明一个字符串变量
        // let sun = ''
        // goodsList.forEach(item => {
        //     const { name, price, picture } = item
        //     sun += `
        // <div class="item">
        //    <img src="${picture}" alt="">
        //    <p class="name">${name}</p>
        // <p class="price">${price}</p>
        // </div>`
        // }) 
        // //3. 渲染页面
        // document.querySelector('.list').innerHTML = sun
         
        // 用 map 的方法渲染
        const lis = document.querySelector('.list')
        const res = goodsList.map(({name, price, picture}) => {
            return `<div class="item">
          <img src="${picture}" alt="">
          <p class="name">${name}</p>
          <p class="price">${price}</p>
        </div>`
        })
        // 数组转换成字符串  arr.join（''）
      lis.innerHTML = res.join('')
        
    </script>
```

##### 4.1 数组筛选

Array.filter()  filter 筛选,过滤的意思

```js
    <script>
                // Array.filter()  filter 筛选,过滤的意思
        // 创建一个新的数组, 新数组的元素是 通过筛选后的元素

        // filter() 接收一个函数作为参数, 函数返回true或者false
        // true: 表示通过测试, 保留当前元素, false, 不保留

        // 函数的第一个参数  当前处理的元素  item
        // 函数的第二个参数  当前处理的元素的索引号 index

        // 返回值, 一个新的数组, 由通过测试的元素组成.

        const arr = [10, 20, 30, 40]

        // 箭头函数
        const newArr = arr.filter(mask => mask >= 20)
        console.log(arr)
        console.log(newArr)
    </script>
```

##### 4.2 商品渲染综合案例

```js
<body>
  <div class="filter">
    <a data-index="1" href="javascript:;">0-100元</a>
    <a data-index="2" href="javascript:;">100-300元</a>
    <a data-index="3" href="javascript:;">300元以上</a>
    <a href="javascript:;">全部区间</a>
  </div>
  <div class="list">
    <!-- <div class="item">
      <img src="" alt="">
      <p class="name"></p>
      <p class="price"></p>
    </div> -->
  </div>
  <script>
    // 2. 初始化数据
    const goodsList = [
      {
        id: '4001172',
        name: '称心如意手摇咖啡磨豆机咖啡豆研磨机',
        price: '289.00',
        picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg',
      },
      {
        id: '4001594',
        name: '日式黑陶功夫茶组双侧把茶具礼盒装',
        price: '288.00',
        picture: 'https://yanxuan-item.nosdn.127.net/3346b7b92f9563c7a7e24c7ead883f18.jpg',
      },
      {
        id: '4001009',
        name: '竹制干泡茶盘正方形沥水茶台品茶盘',
        price: '109.00',
        picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png',
      },
      {
        id: '4001874',
        name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器',
        price: '488.00',
        picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png',
      },
      {
        id: '4001649',
        name: '大师监制龙泉青瓷茶叶罐',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png',
      },
      {
        id: '3997185',
        name: '与众不同的口感汝瓷白酒杯套组1壶4杯',
        price: '108.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8e21c794dfd3a4e8573273ddae50bce2.jpg',
      },
      {
        id: '3997403',
        name: '手工吹制更厚实白酒杯壶套装6壶6杯',
        price: '99.00',
        picture: 'https://yanxuan-item.nosdn.127.net/af2371a65f60bce152a61fc22745ff3f.jpg',
      },
      {
        id: '3998274',
        name: '德国百年工艺高端水晶玻璃红酒杯2支装',
        price: '139.00',
        picture: 'https://yanxuan-item.nosdn.127.net/8896b897b3ec6639bbd1134d66b9715c.jpg',
      },
    ]
      // 1. 封装一个render渲染函数
      function render(arr) {
        // 任选一种
        // 1. forEach 字符串拼接
        // 2. map  数组转字符串
       
        const res = arr.map(({name, price, picture})  => {
          return `
          <div class="item">
              <img src="${picture}" alt="">
              <p class="name">${name}</p>
              <p class="price">${price}</p>
        </div>`
        })
        // 渲染 变量.join('') 数组转字符串
        document.querySelector('.list').innerHTML = res.join('')
      }
      render(goodsList) // 页面一打开就需要渲染


      // 绑定点击事件
      const filter = document.querySelector('.filter')
      filter.addEventListener('click', function(e) {
        // e.target.dataset.index // 判断点击的哪个a标签
        console.dir(e.target)
        const {tagName, dataset} = e.target
          // 判断 
        if (tagName === 'A') {
           // arr 返回的新数组
          let arr = []
          if(dataset.index === '1') {
            // 解构
            arr = goodsList.filter(({price}) => price > 0 && price <= 100)
          }else if (dataset.index === '2') {
            arr = goodsList.filter(({price}) => price >= 100 && price <= 300)
          }else if (dataset.index === '3') {
            arr = goodsList.filter(({price}) => price >= 300 )
          }


         // switch() 用dataset.index 去匹配 case
        switch(dataset.index) {
          case '1':
            arr = goodsList.filter(({price}) => price > 0 && price <= 100)
            break;
          case '2':
            arr = goodsList.filter(item => item.price > 100 && item.price <= 300)
            break;
          case '3':
            arr = goodsList.filter(item => item.price > 300)
            break;
          default:
            arr = goodsList
            break;
        }
          // 渲染函数
          render(arr)
        }
      })
  </script>
```



