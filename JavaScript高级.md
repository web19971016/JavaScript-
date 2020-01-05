# 构造函数和原型对象和实例对象之间的关系

构造函数中的prototype即为原型对象，通过构造函数.protptype创建的属性或方法都存在于构造函数的原型对象中

使用原型对象可以解决数据共享、节省内存空间等问题。

原型对象中的constructor构造器指向的是自己的构造函数，

构造函数创建了实例对象，实例对象的\__proto__指向了它所在的构造函数的原型对象，所以实例对象可以直接调用构造函数的原型对象prototype上的属性或方法

![1577770969525](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1577770969525.png)



# 什么样子的数据是需要写在原型中？

需要共享的数据就可以写在原型中（例如方法等），不需要共享的数据写在构造函数中

原型的作用之一：数据共享

```javascript
    function Student(name,age,sex){
        this.name = name;
        this.age = age;
        this.sex = sex; 
    }
    Student.prototype.school = "清华大学"
    Student.prototype.print = function (){
        console.log(`名字:${this.name}，年龄：${this.age}，性别：${this.sex}，就读于${Student.prototype.school}`)
}
	let stu = new Student("张三",20,"男")
	let stu2 = new Student("尼古拉斯赵四",44,"男")
	stu.print()
	stu2.print()
```




打印结果：

![1577773515564](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1577773515564.png)





# 简单的原型写法

```javascript
    function Student (name,age,sex){
        this.name = name;
        this.age = age;
        this.sex = sex;
        //因为prototype本身就是一个对象
        Student.prototype = {
            //手动修改构造器指向,因为默认的prototype对象已经被当前{}覆盖了
            constructor:Student,
            school:"北京大学",
            print(){
                console.log(`名字:${this.name}，年龄：${this.age}，性别：${this.sex}，就读于${Student.prototype.school}`)
            }
        }
    }
```





# 实例对象使用的属性和方法层层的搜索

实例对象使用的属性或者方法，先从实例对象中查找，找到了则直接使用，找不到则去实例对象\__proto__所指向的原型对象,找到了则使用，找不到则报错。

![1577777561660](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1577777561660.png)



```javascript
    function Student(name,age){
        this.name  = name;
        this.age = age;
    }
    Student.prototype.name = "张三"
    let stu = new Stufent("李四",20)
	console.log(stu.name)
```







# 原型链

原型链：是一种关系，实例对象和原型对象之间的关系，关系是通过（\__proto__）来联系的

![1578032415977](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1578032415977.png)



1.实例对象的\__proto__和和构造函数的prototype指向是相同的

2.实例对象中的\__proto__原型指向的是构造函数中的原型prototype

3.实例对象中\__proto__是原型，供浏览器使用

4.构造函数中prototype是原型，供程序员使用





# 原型指向可以改变

![1578033991595](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1578033991595.png)

1.实例对象的原型\__proto__指向的是该对象所在的构造函数的原型对象。

2.构造函数的原型对象（prototype）改变时，实例对象的原型(\__proto__)也会跟着改变。

3.实例对象和原型对象之间的关系是通过(\__proto__)联系起来的，这个关系就叫原型链





# 原型指向改变如何添加原型方法

1.如果原型指向改变了，那么就应该在原型指向改变之后添加原型方法

```javascript
    function Person(age){
        this.age = age;
    }
    //指向改变了，指向了一个新的对象
    Person.prototype = {
        eat(){
            console.log("吃了吗")
        }
    }
    //然后再添加原型方法
    Person.prototype.sayHi = function(){
        console.log("你好")
    }
    let per = new Person(15)
	per.sayHi()
```



# 借用构造函数继承

借用构造函数:构造函数名字.call(当前对象(this),属性1，属性2)

解决了属性继承，并且值不重复的问题

缺陷：父级类别中的方法不能继承

```javascript
    function Person(name,age,sex){
        this.name = name;
        this.age = age;
        this.sex = sex;
    }
    Person.prototype.sayHi = function (){
        console.log('你好啊')
    }
    function Student(name,age,sex,score){
        //借用构造函数
        Person.call(this,name,age,sex)
        this.scroe = scroe
    }
    let stu1 = new Student('张三',39,'男',89)
	let stu2 = new Student('李四',29,'男',73)
```





# 组合继承：原型继承+借用构造函数继承

解决了父级类别中的方法不能继承的问题

<script>
    function Person(name,age,sex){
        this.name = name;
        this.age = age;
        this.sex = sex;
    }
    Person.prototype.sayHi = function(){
        console.log('你好呀')
    }
    function Student(name,age,sex,scroe){
        //借用构造函数继承
        Person.call(this,name,age,sex)
        this.scroe = scroe
    }
    //改变原型指向----->继承
    Student.prototype = new Person()
    Student.prototype.study = function (){
        console.log('我爱学习')
    }
    let stu1 = new Student('张三',15,'男',99)
	let stu2 = new Student('李四',16,'女',69)
	console.log(stu1.name,stu1.age,stu1.sex,stu1.scroe)
    stu1.sayHi()
    stu1.study()
	console.log(stu2.name,stu2.age,stu2.sex,stu2.scroe)
    stu2.sayHi()
    stu2.study()
</script>

```javascript
    function Person(name,age,sex){
        this.name = name;
        this.age = age;
        this.sex = sex;
    }
    Person.prototype.sayHi = function(){
        console.log('你好呀')
    }
    function Student(name,age,sex,scroe){
        //借用构造函数继承
        Person.call(this,name,age,sex)
        this.scroe = scroe
    }
    //改变原型指向----->继承
    Student.prototype = new Person()
    Student.prototype.study = function (){
        console.log('我爱学习')
    }
    let stu1 = new Student('张三',15,'男',99)
	let stu2 = new Student('李四',16,'女',69)
	console.log(stu1.name,stu1.age,stu1.sex,stu1.scroe)
    stu1.sayHi()
    stu1.study()
	console.log(stu2.name,stu2.age,stu2.sex,stu2.scroe)
    stu2.sayHi()
    stu2.study()
```





# 拷贝继承

把一个对象当中的属性和方法通过循环遍历的方式放到另外一个对象中，就叫拷贝继承

方式一：

```javascript
   let obj1 = {
       name:'张三',
       age:20,
       sex:'男'
   }
   let obj2={}
   for(let key in obj1){
       obj2[key] = obj1[key]
   }   

	console.log(boj2.name)
```





方式二：

```javascript
    function Person (){
        
    }
	Person.prototype.name = "李四"
	Person.prototype.age = 20
	Person.prototype.sex = "男"
	Person.prototype.play = function (){
    
	}

	let obj = {}
	//Person构造函数中由原型对象prototype，prototype就是一个对象，里面的name、age、sex、play()都是	该对象中的属性或方法
	for(let key in Person.prototype){
    	obj[key] = Person.prototype[key]
	}
	console.dir(obj)
```





# 浅拷贝

拷贝就是复制，就相当于把一个对象中的所有内容，复制一份给另外一个对象。或者说，就是把一个对象的地址给了另外一个对象，它们的指向相同，两个对象之间有相同的属性或者方法。

![1578207543709](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\1578207543709.png)

```javascript
    let obj1 = {
        age:10,
        name:'张三',
        eat(){
            console.log('吃了吗')
        }
    }
    let obj2 = {}
    
    //写一个函数，作用：把一个对象的属性复制到另外一个对象中，浅拷贝
    //把objOne对象中的所有属性复制到objTwo中
    function extend(objOne,objTwo){
        for(let key in objOne){
            objTwo[key] = objOne[key]
        }
    }
    
    console.dir(obj1)
    
    extend(obj1,obj2)
    console.dir(obj2)
```





# 深拷贝

拷贝还是复制，深：把一个对象中的所有属性或者方法，一个一个的找到，并且在另一个对象中开辟相应的空间，一个一个的存储到另一个对象中

```javascript
    let obj1 = {
        name:'张三',
        age:20,
        car:['奔驰','宝马'],
        dog:{
            name:'小黄',
            age:2
        }
    }
    let obj2 = {}
    
    //通过函数实现，把一个对象的所有属性深拷贝到另一个对象中
    function extend(a,b){
        for(let key in a){
            let item = a[key];
            //判断这个属性的值是否为数组
            if(item instanceof Array){
                //如果是数组，那么在b对象中添加一个新的属性，并且这个属性也是数组
               b[key] = [];
                //通过递归，把a对象中这个数组的属性值一个一个的复制到b对象的这个数组中
               extend(item,b[key]);
             }
            //同上
            else if(item instanceof Object){
               b[key] = {};
               extend(item,b[key])
             }
            else{
                //如果是简单值，那么直接复制
               b[key] = item
            }
        }
    }
```





# 函数中this的指向

1.普通函数中的this的指向 ---> window

```javascript
    function f1(){
        console.log(this)
    }
    f1()//window
```



2.构造函数中的this ---> 实例对象

```javascript
     function Person() {
        console.log(this)
    }
    let per = new Person()//Person{}
```



3.定时器中的this ---> window

```javascript
    setTiemout(function (){
        console.log(this)
    },100) //window
```



4. 对象.方法中的this ---> 当前实例对象

```javascript
    function Person(){
        this.eat = function(){
            console.log(this)
        }
    }
    let per = new Person()
    per.eat()//当前实例对象
```



5.原型对象方法中的this ---> 当前实例对象

```javascript
    function Person(){
        
    }
    Person.prototype.eat = function(){
        console.log(this)
    }
    
    let per = new Person()
    per.eat()//当前实例对象
```

