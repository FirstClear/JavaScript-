# JS面向对象
## 一、JS中定义对象
### 1、js中的数据类型
1.    基本类型:number,string,boolean,null,undefine,symbol,
2.    引用类型：Object，Function，Array

### 2、instance of

instanceof 操作符的内部实现机制和隐式原型、显式原型有直接的关系。instanceof的左值一般是一个对象，右值一般是一个构造函数，用来判断左值是否是右值的实例。它的内部实现原理是这样的：

```
//设 L instanceof R 
//通过判断
 L.__proto__.__proto__ ..... === R.prototype ？
//最终返回true or false

console.log(Array instanceof Object);//true
console.log(Function instanceof Object);//true
console.log(Array instanceof Array);//false
```
### 3、js中的类
1. es5及之前js中类的实现
```
function Animal(name){
    this.name = name;
}
Animal.prototype.say = function(sound){
    console.log('i am '+this.name+' ,'+sound);
}
var cat = new Animal('cat');
console.log(cat.name);
cat.say('miao');
//cat
//i am cat ,miao
```
new关键字：new关键字相当于将普通函数转变为一个构造函数，如果不用new关键字，就类似于一个普通函数，当然，返回值为空，cat应该为undefined。
我们想知道的是，new关键字做了些什么。

```
function t(args) {
    var obj = {};
    Animal.call(obj,args);
    obj.__proto__ = Animal.prototype;
    return obj;
}
var dog = t('dog');
console.log(dog.name);
dog.say('wang');
//dog
//i am dog ,wang
```
2.原型和原型链（\_\_proto\_\_和prototype）

   每一个构造函数（construtor）都有一个prototype属性，通过构造器派生（new）的对象都有一个\_\_proto\_\_属性，对象的\_\_proto\_\_指向其构造器的prototype。我们可以验证一下
   
```
function Animal(name){
    this.name = name;
}
Animal.prototype.say = function(sound){
    console.log(sound);
}

var cat = new Animal('cat');

console.log(cat.__proto__==Animal.prototype);
//true
```
可能现在会有同学问：这有什么用呢？
事实上，在调用一个实例对象的属性或者方法前，浏览器会在实例中查找，没有的话就会去实例的\_\_proto\_\_中查找，直到查找到对应属性，或者一直到Object.\_\_proto\_\_,如果还没有则返回undefined。

这种数据结构很好理解，就是数据结构中的链表，如果用数据结构以及C++的结构体描述的话。


```
//这部分内容是一个伪代码
//这就是一个简单的原型链的基本实现
struct Person{
    int age;
    string name;
    struct prototype{
        constructor; //构造函数，指向本身
        *method; //自定义方法
        __proto__; //__proto__属性
    }* __proto__;
}*constructor

```
3、构造器

构造器（constructor）在js中也是个很有意思的东西。一般来说一个对象的构造器指向创造该对象的构造函数。

```
function Animal(){
    
}
console.log(Animal.prototype.constructor==Animal)
//true;
var cat = new Animal();
console.log(cat.constructor==Animal)
//true;
console.log(cat.constructor.name)
//Animal
```
4、原型式继承
只介绍几种常用方式
1、拷贝继承

```
function Father(name){
    this.name = name;
}
Father.prototype.say = function(){
    console.log("i am father");
}
function Son(name){
    Father.call(this,name);
}
for(var each in Father.prototype){
    Son.prototype[each] = Father.prototype[each];
}
//缺点，循环 很慢，而且会有无法枚举的类型不能继承

```
2、组合继承

```
function Father(name){
    this.name = name;
}
Father.prototype.say = function(){
    console.log("i am father");
}
function Son(name){
    Father.call(this,name);
}
Son.prototype = new Father();
//还原Son的构造函数
Son.prototype.constructor = Son;
```
3、道格拉斯推荐的一种，寄生组合继承

```
function Father(name){
    this.name = name;
}
Father.prototype.say = function(){
    console.log("i am father");
}
function Son(name){
    Father.call(this,name);
}
(function(){
    function f(){};
    f.prototype = Father.prototype;
    Son.prototype = new f();
})


```
问题，如何判断一个变量是数组对象
instanceof和constructor方法判断存在局限性，只能判断从Array派生出的数组，如果一个构造器继承Array后，再进行派生，无法判断。
ES之后新增方法
Array.isArray(obj);
ES5之前
Object.prototype.toString.call(obj) === "[Object Array]";

ES6之后的面向对象和继承，通过class，super和constructor来实现。

```
class Father{
    constructor(fullName){
        this.fullName = fullName;
    }
    say(){
        console.log("hello");
    }
}
class Son extends Father{
    constructor(fullName){
        super(fullName);
    }
    say(){
        console.log("fine");
    }
}
```
至此，js面向对象部分基本结束



