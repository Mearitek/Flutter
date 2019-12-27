# 响应式编程与函数式编程
## 1. 响应式编程
### 1.1 响应式编程的定义
>在计算机领域，响应式编程是一个专注于数据流和变化传递的异步编程范式。这意味着可以使用编程语言很容易地表示静态（例如数组）或动态（例如事件发射器）数据流，并且在关联的执行模型中，存在着可推断的依赖关系，这个关系的存在有利于自动传播与数据流有关的更改。

### 1.2 响应式的由来
**先看一段代码片段**
```

    int a = 1;
    int b = a + 1;
    print(b); //  b=2
    a = 10;
    print("b=$b"); //  b=2
```

上面是一段很常见的代码，简单的赋值打印语句，但是这种代码有一个缺陷，那就是如果我们想表达的并不是一个赋值动作，而是b和a之间的关系,即无论a如何变化，b永远比a大1。那么可以预见，我们就需要花额外的精力去构建和维护一个b和a的关系。

而响应式编程的想法正是企图用某种操作符帮助你构建这种关系。
它的思想完全可以用下面的代码片段来表达：
```
    int a = 1;
    int b <= a + 1; // <= 符号只是表示a和b之间关系的操作符
    print(b); //  b=2
    a = 10;
    print("b=$b"); //  b=11
```

这就是是响应式的思想，它希望有某种方式能够构建关系，而不是执行某种赋值命令。

我们为什么需要构建关系的代码而不是命令式的代码呢？因为app每一个交互的页面其实内部都包含了一系列的业务逻辑。而产品的每个需求，其实也对应了一系列的业务逻辑相互作用。总之，我们的开发就是在构建一系列的业务逻辑之间的关系。

说回响应式，前期由于真实的编程环境中并没有构建关系的操作符，主流的编程语言并不支持这种构建关系的方式，所以一开始响应式主要停留在想的层面，直到出现了Rx和一些其他支持这种思想的框架,才真正把响应式编程引入到了实际的代码开发中。

### 1.3 如何使用响应式编程
**Sample**
[![](https://upload-images.jianshu.io/upload_images/689802-246858228f5a8fac.png?imageMogr2/auto-orient/strip|imageView2/2/w/930/format/webp)](https://upload-images.jianshu.io/upload_images/689802-246858228f5a8fac.png?imageMogr2/auto-orient/strip|imageView2/2/w/930/format/webp "markdown")
这是一个比较理想化的APP初始化逻辑，完成SDK初始化，数据库初始化，登陆，之后跳转主界面。

非响应式编程怎么实现？

**方法一：同一线程链式调用**
```
new Future((){
    initSDK();
	initDB();
	login();
	goMain();
});
```
缺点：如果每个接口都比较耗时，登录会比较慢。

**方法二：异步并发执行**
```
boolean isInitSDK = false;
boolean isInitDB = false;
boolean isLogin = false;

void init() async{
	new Future((){
	   await isInitSDK= initSDK();
	   if(isInitSDK){
	   	 if(isInitDB && isLogin){
		 	goMain();
			}
	   }else{
	   	print("initSDK Error");
	   }
	});
	new Future((){
	   await isInitDB= initDB();
	   if(isInitDB){
	   	 if(isInitSDK && isLogin){
		 	goMain();
			}
	   }else{
	   	print("initDB Error");
	   }
	});
	new Future((){
	   await isLogin= login();
	   if(isLogin){
	   	 if(isInitDB && isInitSDK){
		 	goMain();
			}
	   }else{
	   	print("login Error");
	   }
	});
}
```

缺点：业务逻辑耦合度高，代码阅读性与可维护性差。

**方法三：使用响应式编程**

```
Observable obserInitSDK=Observable.create{initSDK(context)}.subscribeOn(Schedulers.newThread()

Observable obserInitDB=Observable.create{initDatabase(context)}.subscribeOn(Schedulers.newThread())

Observable obserLogin=Observable.create{login()}.subscribeOn(Schedulers.newThread())
                            
Observable observable = Observable.merge(obserInitSDK,obserInitDB,obserLogin)

observable.subscribe(()->{goMain()})

```

### 1.4 重新理解响应式编程的定义
**1、数据流**

响应编程能够简化编程，它依赖于事件，代码运行的顺序不是代码行的顺序，而是和一个以上的事件有关，这些事件发生是以随着时间的推移的序列。我们把这一系列事件称为“流”。

在某种程度上，这并不是什么新东西。用户输入、单击事件、变量值等都可以看做一个流，你可以观察这个流，并基于这个流做一些操作。一个流就是一个将要发生的以时间为序的事件序列。它能发射出三种不同的东西：一个数据值(某种类型的)，一个错误（error）或者一个“完成（completed）”的信号。

**2、变化传递**

顾名思义，a与b建立依赖关系，a变化后会传播数据流，自动修改b的值。

**3、异步**

响应式编程是“响应”这些不知何时发生的事件而得以命名。同步调用有时无法反映业务本来的关系，而且会让你的程序“反应”更慢。

**4、编程范式**

所谓编程范式（programming paradigm），指的是计算机编程的基本风格或典范模式。
编程是为了解决问题，而解决问题可以有多种视角和思路，其中普适且行之有效的模式被归结为范式。
面向对象，面向过程都是一种编程范式。

#### 一些常见的范式
+ 过程式编程
+ 命令式编程
+ 声明式编程
+ 面向对象编程
+ 面向协议编程
+ 响应式编程
+ 函数式编程
+ 防御式编程
+ 契约式编程

**5、重新理解响应式编程的定义**
>在计算机领域，响应式编程是一个专注于数据流和变化传递的异步编程范式。这意味着可以使用编程语言很容易地表示静态（例如数组）或动态（例如事件发射器）数据流，并且在关联的执行模型中，存在着可推断的依赖关系，这个关系的存在有利于自动传播与数据流有关的更改。

### 1.5 为什么要使用响应式编程？
+ 1、高聚合，低耦合


目前Android中常用的MVVM模式就是采用响应式编程的思想，以我们的app首页下拉刷新获取设备列表举例，

如果采用响应式编程：
```
//业务层
LiveData<List<Device>> devices = new LiveData();

//获取列表
model.getDeviceList("parameter",new callBack(){

	onSuccess(List list){
		//只需要设置新数据，不需要手动去调用刷新ui，不需要持有视图层的实例
		devices.setValue(list);
	}
	onError(){
	
	}
}

//View层： 
viewModel.devices.observe(this){ adapter.setNewData(it)};

```
上述代码表示观察(响应)一个数据的变化，在View层开发者只需要将数据与UI建立关联即可。

不采用响应式：
```
//业务层 获取列表
model.getDeviceList("parameter",new callBack(){

	onSuccess(List list){
		view.refreshDeviceList(list);
	}
	onError(){
	
	}
}
//视图层 
void refreshDeviceList(List list){
	adapter.setNewData(list);
}

```

以上MVP模式下，业务逻辑层需要持有视图层的实例，仍有一定的耦合度，一旦业务层发生需求变更，可能影响view层跟model层。
	
+ 2、数据流可与Ui建立双向依赖关系
	
不止数据发生变更时ui可以自动变更，当ui发生变更时，数据也可自动变更。
	
再举个栗子，例如我们的app有个修改昵称界面。一个输入框EditText(Android),TextFiled(IOS)，输入用户的昵称，点击保存会将昵称保存到服务器。
采用非响应式的情况下，用户点击保存的时候需要先获取文本框的内容，然后再保存到对象的字段上，最后将对象上传服务器。
而如果采用响应式，则是将输入框与对象的字段绑定即可，然后就可以不用管了，点击保存的时候直接将对象上传即可，因为文本框已与对象绑定，一旦文本框内容改变，对象的字段自动改变。

** 这有啥用？好像也没方便多少？ **

如果要输入用户的昵称，年龄，性别，地址，国家，身高，体重，生日,xxxx呢？
	
非响应式
```
void save(){
	String nickName=nickText.getText();
	String ageName=ageText.getText();
	String sexName=sexText.getText();
	String addName=addressText.getText();
	String countryName=countryText.getText();
	String heightName=heightText.getText();
	String weightName=weightText.getText();
	String birthdayName=birthdayText.getText();
	...
	user.setNickName(nickName);
	user.setAge(ageName);
	user.setSex(sexName);
	user.setAddr(addName);
	user.setCountry(countryName);
	user.setHeight(heightName);
	user.setWeight(weightName);
	user.setBirthday(birthdayName);
	...
	//上传服务器
	saveUserInfo(user);
}

	
```
响应式:
```
void save(){
	//上传服务器
	saveUserInfo(user);
}
```
是的，就是这么easy。
在android中，有DataBinding技术，直接将XML和ViewModel绑定起来。iOS里，也可以通过ReactiveCocoa来实现数据的双向绑定。而在Flutter中，我们可以借助Stram&Sink来实现数据变更的通知，StreamBuilder来做View层的绑定。
## 2. 函数式编程
### 2.1 函数式编程的定义
>函数式编程是种编程方式，它将电脑运算视为函数的计算。函数编程语言最重要的基础是λ演算（lambda calculus），而且λ演算的函数可以接受函数当作输入（参数）和输出（返回值）。

### 2.2 函数式编程的特性
#### 1、函数是"第一等公民"
在函数式语言中，函数作为一等公民，可以在任何地方定义，在函数内或函数外，可以作为函数的参数和返回值，可以对函数进行组合。
#### 虚假的万物皆对象JAVA 
```
public void test(int number){
	System.out.println("hello world"+ number);
}
//无法定义一个函数为对象
object myFun = test;
```
#### 真实的万物皆对象 Dart
```
//函数可以在其他函数内被定义为一个对象
var myFun = (int number) => "hello world $number";
var list = [2, 3, 4];
//函数可以作为其他函数的参数
list.forEach(myFun);

//函数作为返回值
Function onTest(){
    return (int number) => "hello world $number";
}

```
不过Dart中的函数作为返回值支持的不是很好，只能定义为Function类型，不能具体的约束函数的参数与返回类型。
```
//kotlin定义函数作为返回值的同时可以约束参数类型
fun kotlinTest(): (Int) -> (String) {
    return {number:Int -> "hello world $number"}//正确
	//return {s:String -> 3} //编译器提示错误 
}
```
#### 2、变量的值是不可变的（immutable）
函数式编程中的函数这个术语不是指计算机中的函数（实际上是Subroutine），而是指数学中的函数，即自变量的映射。也就是说一个函数的值仅决定于函数参数的值，不依赖其他状态。比如sqrt(x)函数计算x的平方根，只要x不变，不论什么时候调用，调用几次，值都是不变的。

```
int a=1;
//每次运行结果都不同
void test1(){
  print(a++);
}
	
List<Person> list;
//每次运行结果都不同
void test2(Person person){
	list.add(list);
	//哪怕每次传递的参数相同，结果也会不同
	print(list.length);
}

```
#### 3、只用"表达式"，不用"语句"
"表达式"（expression）是一个单纯的运算过程，总是有返回值；"语句"（statement）是执行某种操作，没有返回值。函数式编程要求，只使用表达式，不使用语句。也就是说，每一步都是单纯的运算，而且都有返回值。
原因是函数式编程的开发动机，一开始就是为了处理运算（computation），不考虑系统的读写（I/O）。"语句"属于对系统的读写操作，所以就被排斥在外。

看以下三个例子
```
//sample1
int a, b, r;
void add_abs() {
	r = abs(a) + abs(b)
	print(r);
}

//sample2
int add_abs(int a, int b) {
	int r = 0;
	r += abs(a);
	r += abs(b);
	return r;
}

//sample3
int add_abs(int a, int b) {
	return abs(a) + abs(b);
}
```
	

	
程序1 是用命令来表示程序, 用命令的顺序执行来表示程序的组合, 不算函数式

程序2 是用函数来表示程序, 但在内部是用命令的顺序执行来实现, 不太函数式

程序3 是用函数来表示程序, 用函数的组合来表达程序的组合, 是完全的函数式编程

函数式编程思维, 就是用计算(函数)来表示程序, 用计算(函数)的组合来表达程序的组合的思维方式。
#### 4、没有副作用
所谓"副作用"（side effect），指的是函数内部与外部互动（最典型的情况，就是修改全局变量的值），产生运算以外的其他结果。

函数式编程强调没有"副作用"，意味着函数要保持独立，所有功能就是返回一个新的值，没有其他行为，尤其是不得修改外部变量的值

总之，函数式编程是面向数学的抽象，将计算描述为一种表达式求值，一句话，函数式程序就是一个表达式。

### 2.3 为什么要使用函数式编程？
#### 1、更方便的代码管理
函数式编程不依赖、也不会改变外界的状态，只要给定输入参数，返回的结果必定相同。因此，每一个函数都可以被看做独立单元，很有利于进行单元测试（unit testing）和除错（debugging），以及模块化组合。
#### 2、易于"并发编程"
函数式编程不需要考虑"死锁"（deadlock），因为它不修改变量，所以根本不存在"锁"线程的问题。不必担心一个线程的数据，被另一个线程修改，所以可以很放心地把工作分摊到多个线程，部署"并发编程"（concurrency）。
#### 3、高阶函数
高阶函数就是参数为函数或返回值为函数的函数。有了高阶函数，就可以将复用的粒度降低到函数级别，相对于面向对象语言，复用的粒度更低。
## 终极形态
>函数式编程在使用的时候的特点就是，你已经再也不知道数据是从哪里来了，每一个函数都是用小函数组织成更大的函数，函数的参数也是函数，函数返回的也是函数，最后得到一个超级牛逼的函数，就等着别人用他来写一个main函数把数据灌进去了。
	
# 题外
## 为什么讲响应式编程与函数式编程

首先目前前端几乎所有框架的设计都是响应式设计，如果要深入理解前端语言(js，dart)，响应式编程必须要了解。
	
学习函数式编程，主要是移动端出现的函数响应式框架将两者结合的非常出彩，使用Rx系列的框架对原生app可以带来众多的好处（比如解放android那乱七八糟的耦合代码）。

**函数响应式框架数据**

Github Star
+ RxJava  41.4K 
+ ReactiveCocoa 19.7K
+ RxSwift 17.7K
+ RxJs 19.7K
+ RxDart 2.1K

对一些偏后端使用的语言，其实也有支持，比如RxGo,RxPhp,RxScala，但是后端使用的提升并没有偏前端的语言带来的提升大，因为响应式更适合与ui的交互。
	
更老的语言(Java)因为语言特性没有现代化语言那么先进，使用Rx库能做到以前非常麻烦才能做到的事，甚至做不到的事(因为很多人不懂响应式与函数式)。因此star数遥遥领先其他库应该是它太老了(我猜的)。

**Bloc**

BLoC是一种利用reactive programming方式构建应用的方法，这是一个由流构成的完全异步的世界。

是不是很眼熟？reactive programming翻译就是响应式编程。MVVM也好，Bloc也好，都是基于响应式编程，关于Bloc，期待下次的分享。