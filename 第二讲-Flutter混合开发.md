<h1><center> 目录 </center></h1>

[TOC]


##语言特性
+ 在Dart中，一切都是对象，一切对象都是class的实例，哪怕是数字类型、方法甚至null都是对象，所有的对象都是继承自Object
+ 尽管 Dart 是强类型语言，但是在声明变量时指定类型是可选的，因为 Dart 可以进行类型推断。
+ Dart 支持泛型，比如 List<int>（表示一组由 int 对象组成的列表）或 List<dynamic>（表示一组由任何类型对象组成的列表）。
```
// 声明数组,里面包含< DeviceHealthModel >
List<DeviceHealthModel> dataSource = [];
```
+ 跟Java不同的是，Dart没有public protected private等关键字，如果某个变量以下划线（_）开头，代表这个变量在库中是私有的
+ Dart中变量可以以字母或下划线开头，后面跟着任意组合的字符或数字

---
## 变量和常量
+ **创建变量**
+ 
```
   var age = 19; // 类型推断为int
   dynamic name1 = '小菊'; // 未知类型
   String name2 = '小菊'; // 指定类型为String
   List<int> ages = [1, 2, 3, 4]; // 声明int数组
   int myage; // 未初始化的变量, 默认值为null
```

+ **创建常量 final和const**

+ 如果一个变量的值不再修改，则使用`final`或者`const`代替`var`。
一个`final`变量只能赋值一次；一个`const`变量是编译时常量，`const`变量默认为`final`变量。
被`final`修饰的顶级变量或类变量在第一次声明的时候就需要初始化。

```
void main(List<String> args) {
 final String name = "小菊";
  const age = 18; // 声明时必须初始化, 否则报错 The constant 'age' must be initialized.
  //  The name 'deviceName' is already defined.
 }
```

+ 顶层`final`变量与类变量在首次访问时执行初始化。需要注意的是，实例变量可以为`final`,但是不能是`const`
+ 
```
class DeviceHealth extends StatefulWidget {
  @override
  final a ; // final修饰的实例变量可以在下面初始化的时候赋值

  static const b = ""; //  常量如果是类里的，请使用 static const

  DeviceHealth({
    this.a = "",
    // this.a = "a", // 无法二次修改
    GlobalKey<_DeviceHealthState> key,
  }) : super(key: key);

  State<StatefulWidget> createState() {
    // TODO: implement createState
    return null;
  }
}
class _DeviceHealthState extends State<DeviceHealth> {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    throw UnimplementedError();
  }
}
```


+ `const` 关键字不仅仅可以用来定义常量，还可以用来创建常量值，该常量值可以赋予给任何变量


```
void main(List<String> args) {
  var a = const [1, 2]; // const创建 常量列表
  a = [2]; // 可以被重新赋值

  final b = const [1, 2]; 
  b = [2]; // final 和 const定义 还是不可以被重新赋值
 }
 
```
+ `const` 只能被赋值`const`类型, 无法被final 或其他类型赋值.


## 数据类型
+ **numbers**
Dart支持两种类型的数字

```
void main(List<String> args) {
  var a = 1; // int
  var b = 1.0; // double
  int c = int.parse("1"); // string 转 int 
  double d = double.parse("1.1"); // string 转 double 

  String str1 = 1.toString(); // 数字转字符串
  String str2 = 3.1415926.toStringAsFixed(2); // 输出3.14
 }
 
```
+ **String** 
+ 
```
void main(List<String> args) {
  String str1 = "sdhsadkjhas"; // 创建字符串
  String str2 = "fnjdshuic\""; //  \用于转义
  print("字符串1为 $str1"); // $ 格式化
  print("字符串2为 ${str1 + str2}"); // ${} 插入表达式 , + 拼接字符串
  String str3 = '''
     创建多行字符串 
   ''';
  print(str1.length); // 字符串长度
  print(str1.isEmpty); // 字符串是否为空
  str1.contains("sss"); // 字符串是否包含另外一个字符串
  String str4 = str1.substring(1,4); // 字符串截取
  bool a = str4.startsWith("s"); // 字符串是否以xxx开头
  int b = str4.indexOf("sss"); // 第一次某个字符串的下标
  int c = str4.lastIndexOf("sss"); // 最后一次出现某个字符串的下标
  str4.toLowerCase(); // 字符串转小写
  str4.toUpperCase(); // 字符串转大写
  str4.trim(); // 字符串清除空格
  str4.trimLeft(); // 清除最左边的空格
  str4.trimRight(); // 清除最右边的空格
  List strList = str4.split("."); // 字符串分割后数组

  str4.replaceAll(new RegExp(r'e'), 'é'); // 字符串替换
  str4.replaceAll("a", "s");
 }
```

+ **booleans**
+ 
```
 true 和 false
```

+ **list**
+ 
```
void main(List<String> args) {
  List list = [10, 7, 23]; //创建一个int类型的list
  var animal = new List();
  animal.add('dog'); // 添加
  animal.addAll(['cat', 'cat']); // 从另外一个数组添加多个
  print(animal.length); // 获取List的长度
  print(animal.first); // 第一个
  print(animal.last); // 最后一个
  print(animal[0]); // 索引获取
  print(animal.indexOf('cat')); // 查找某个元素的索引
  print(animal.removeAt(0)); // 删除指定位置，返回删除的元素
  animal.remove('cat'); // 删除指定元素,成功返回true，失败返回false, 如果集合里面有多个, 只会删除集合中第一个改元素
  animal.removeLast(); // 删除最后一个，返回删除的元素
  animal.removeRange(0, 2); // 删除 [0, 2) 内元素
  animal.removeWhere((item) => item.length > 3); // 删除指定条件的元素(这里是元素长度大于3)
  animal.clear(); // 删除所有
  animal.sort(); // 按照ASCII码排序
  animal.sublist(1,3); // 获取子数组[) 左开右闭
} 
```

+ **map**
+ 
```
void main(List<String> args) {
  Map<String, String> animal1 = {}; // 创建空map
  var animal2 = new Map<String, String>(); // 创建空map

  // 如果Key 不存在于 Map 中 取值则会返回一个 null

  animal1["a"] = "1"; // 赋值
  animal1.remove("a"); // 移除
  animal1.containsKey("a"); // 是否存在某个key
  animal1.containsValue("b"); // 是否存在某个value
  print(animal1.keys); // 打印所有的key
  print(animal1.values); // 打印所有的value
  animal1.clear(); // 清空map
  animal1.isEmpty; // 检测是否为空
  animal1.addAll({"c": "2", "d" : "3"}); // 从另外一个map 添加
} 
```

## 运算符 operators



|描述	|操作符|
| :---: |:---: |
|一元后缀|	expr++  expr--  ()  []  .  ?.|
|一元前缀	|expr !expr ~expr ++expr --expr|
|乘除法	|*  / %  ~/|
|加减法	|+ -|
|位运算	|<<  >>|
|按位与	|&|
|按位或	|   \|	|
|按位异或	|^|
|逻辑与	|&&|
|逻辑或	|	\|\|	|
|关系和类型测试	|>=  >  <=  <  as  is  is!|
|相等判断|	==  !=|
|空判断	|??|
|条件表达式	|expr1 ? expr2 : expr3|
|赋值	|= *= /= ~/= %= += -= <<= >>= &= ^= = ??=|
|级联	|..|

+ **挑一些区别性的**

| 操作符 | 作用 | 
|:---: | :---: | 
| is | 判断一个变量是不是某个类型的数据 |
| is! | 判断变量不是某个类型的数据|
| as | 强制转换 |
| / |  除法, 可能带小数点 |
| ~/ | 是取整运算符 |
| ??=|  如果 ??= 运算符前面的变量为null，则赋值，否则不赋值 |
| .. | 同一个对象上连续调用多个对象的变量或方法 |

+ **is,  is!**
(判断一个变量是不是某个类型的数据)

```
  String a = "1";
  print(a is String); // true
  print(a is! String); // false 
```

+ **as**
(强制转换)

```
  dynamic b = 1; // 未知类型
  print(b as int); // 强制转换int
```
+ **/**
(除法, 可能带小数点)

```
var a = 100.0;
a = a / 2;
print(a); // 50.0
```
+ **~/**
(取整)

```
  var a = 100.0;
  int c = a ~/ 3; // 必须用int 接收
  print(c); // 33
```

+ **??=**
(如果 `??=` 运算符前面的变量为`null`，则赋值，否则不赋值)

```
  var str1 = "小菊";
  String str2;
  str1 ??= "孝举"; // 小菊
  str2 ??= "孝举"; // 孝举
  print(str1);
  print(str2);
```

+ **..**
(同一个对象上连续调用多个对象的变量或方法)

```
 void main() {
  var str1 = "小菊";
  str1.toLowerCase()..replaceAll("s", "ss"); // 连续调用多个对象方法.
}
```

## 控制流程语句
+  **if else**
`Dart` 的 `if `语句中的条件必须是一个布尔值，不能是其它类型. (`iOS`中也可以用整型表示, 这里不行)

```
  if (3 > 1) {
    print("大");
  } else {
    print("小");
  }
```
+ **for 循环**
1) 标准`for`循环(可以使用)

```
for (var i = 0; i < 5; i++) {
  print(" $i");
}
```

2) `for-in` 循环

数组遍历

```
    var numbers = [1,2,3,4,5];
    for (var number in numbers) {
      print(number);
    }
```


3) `for-each`
数组遍历

```
var numbers = [1,2,3,4,5];
numbers.forEach((number) =>  
       print(numbers)
);
```

字典 `key-value`遍历, 请使用`forEach`. 

```
    var numbers = {"a": "1", "b": "2"};
    numbers.forEach((key, value) => {
      print(value),
      print(key),
    });
```

+  **while 和 do-while 循环 (和原生语言一样)**
+  **break 和 continue (和原生语言一样)**
+  **switch 和 case**
+  
```
void main() {
    int a = 1;
    switch (a) {
    mark:
    case 0:
      print("0");
      break;
    case 1:
      print("1");
      continue mark; // Continue使用在被标记为mark的子句中
    case 2:
      print("2");
      break;
  }
}

// 输出1  0
```

## 异常 Exceptions

```
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // 指定异常
  buyMoreLlamas();
} on Exception catch (e) {
  // 其它类型的异常
  print('Unknown exception: $e');
} catch (e) {
  // // 不指定类型，处理其它全部
  print('Something really unknown: $e');
}
```

## 函数 Function

## 类 Class
+ **类的基本构成**
构造方法和实例变量
类定义中所有的变量, Dart语言都会隐式的定义 `setter` 方法，针对非空的变量会额外增加 `getter` 方法。

```
typedef void ReConnectButtonClick();

class ReConnectWifiView extends StatefulWidget {
  String deviceName; // 实例变量
  ReConnectButtonClick reConnectButtonClick; // 实例变量

// 初始化方法1
  ReConnectWifiView({
    this.deviceName = "",
    this.reConnectButtonClick,
    GlobalKey<_ReConnectWifiViewState> key,
  }) : super(key: key);

  // iOS方式的初始化方法2, 不推荐 扩展性差) 等同于下面的初始化3
  // ReConnectWifiView(this.deviceName, this.reConnectButtonClick);

 // iOS方式的初始化方法3, 不推荐 扩展性差)
  // ReConnectWifiView(String deviceName, ReConnectButtonClick reConnectButtonClick) {
  //   this.deviceName = deviceName;
  //   this.reConnectButtonClick = reConnectButtonClick;
  // }

  // 命名式构造函数
  // ReConnectWifiView.method1(String deviceName, ReConnectButtonClick reConnectButtonClick) {
  //   this.deviceName = deviceName;
  //   this.reConnectButtonClick = reConnectButtonClick;
  // }

  @override
  State<StatefulWidget> createState() {
    // TODO: implement createState
    return new _ReConnectWifiViewState();
  }
}
class _ReConnectWifiViewState extends State<ReConnectWifiView> {
   @override
  Widget build(BuildContext context) {
    // TODO: implement build
    throw UnimplementedError();
  }
}
```

+ **获取对象的类型**
可以使用` Object `对象的` runtimeType `属性在运行时获取一个对象的类型

```
void main() {
    int a = 1;
    print("这个类型是 ${a.runtimeType}");
}

输出: 这个类型是 int
```

+ **静态变量&&静态方法**
+ 
```
 class Mine  {
   @override  

   static const money = 17; // 定义静态变量, 在其首次被使用的时候才被初始化。
   static String sayHello() {
     return "hello";
   }
 }

 class You {
   _say() {
     Mine.sayHello(); // Mine类调用方法
   }
 }
```

+ **继承(extends)**
Dart 语言中，子类不会继承父类的命名构造函数。如果不显式提供子类的构造函数，系统就提供默认的构造函数
如果想要子类也拥有一个父类一样名字的构造函数，必须在子类是实现这个构造函数。

```
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

由于Employee类没有默认构造方法，只有一个命名构造方法fromJson，所以在Employee类继承Person类时，
需要调用父类的fromJson方法做初始化，
而且必须使用Employee.fromJson(Map data) : super.fromJson(data)这种写法，

class Employee extends Person {
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}
```

+ **set get 方法**
+ 
```
class ReConnectWifiView extends StatefulWidget {
  String deviceName;
  ReConnectButtonClick reConnectButtonClick;

  ReConnectWifiView({
    this.deviceName = "",
    this.reConnectButtonClick,
    GlobalKey<_ReConnectWifiViewState> key,
  }) : super(key: key);

// set方法
  set xiaojuName(String value) {
    deviceName = value + deviceName;
  }
// get 方法
  String get myName {
    return deviceName + "";
  }

  @override
  State<StatefulWidget> createState() {
    // TODO: implement createState
    return new _ReConnectWifiViewState();
  }
}
```

+ **类-抽象类**
+ 
```
abstract class Person {
  void work(); // 抽象方法
  void sayhello() { // 普通方法

  }
 }

 class Me extends Person {
   @override  // 抽象类的子类必须实现抽象方法
   void work() { //  提供一个实现，所以在这里该方法不再是抽象的
  }
 }
```

+ **noSuchMethod()**
如果调用了对象上不存在的方法或实例变量将会触发 `noSuchMethod` 方法，你可以重写` noSuchMethod` 方法来追踪和记录这一行为

```
void main() {
  TestMethod test = new TestMethod();
  dynamic f = test.foo;
  // Invokes `Object.noSuchMethod`, not `TestMethod.noSuchMethod`, so it throws.
  f(42);
}

class TestMethod {
// 除非你重写noSuchMethod，否则使用不存在的成员会导致NoSuchMethodError
  @override
  void noSuchMethod(Invocation invocation) {
    print('You tried to use a non-existent member: ' +
        '${invocation.memberName}');
  }
dynamic foo();
}
```

+ **类-扩展一个类的方法 Extension** (in Dart 2.7)
当您使用他人的`API`或实现广泛使用的库时，更改`API`通常是不切实际或不可能的。 但是您可能仍想添加一些功能。可以使用这个添加方法.
类似`iOS`中的类别, 扩张一个类的方法, 不需要加入到`class`类的代码中. 

```

 class Mine  {
   @override  // 抽象类的子类必须实现抽象方法

   _extensionTest() {
     this.doneAction();
   }
 }

// 为Mine 这个类扩展方法, MineExtension为扩展类名(随便取), doneAction扩展方法
extension MineExtension on Mine {
  void doneAction() {
    print("Ssss");
  }
}
```

+ **枚举**
1) enum

```
enum Color { red, green, blue }
```

2) 和iOS一样, 枚举值默认从0开始,  上面依次为

```
Color.red.index == 0
Color.green.index == 1
Color.blue.index == 2
```

3) 获取枚举中所有值的列表

```
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```

+ **Mixin**
Mixin 是一种在多重继承中复用某个类中代码的方法模式。

```
class Dog {
  String name; 
  sayWang() {
    print("说汪汪汪");
  }
}
class Cat {
  sayMiao() {
    print("说喵喵喵");
  }
}
// 使用with 关键字, 使得person 也能调用Dog, Cat方法和变量
class Person with Dog, Cat {
  say() {
    Person().sayWang(); // 使用dog方法.
    print(Person().name); // 使用dog变量
    Person().sayMiao();
    print("我也会说汪汪汪, 也会喵喵喵");
  }
}
```


## 库和可见性

## 异步支持
`async `和 `await `关键字用于实现异步编程，并且让你的代码看起来就像是同步的一样。
在` await `表达式中， 表达式 的值通常是一个 `Future` 对象；如果不是，那么这个值会自动转为` Future`。

```
// 做异步等待, 直到await 返回数据
  Future<void> _receiveCloudedgeData() async {
    dynamic result;
    try {
      result = await WYChannel.mineDataChannel
          .invokeMethod(WYChannel.chimeSettingMethod);
    } on PlatformException {
      result = "error";
    }
    if (result is String) {
      // print(result);
      GlobalConfig.languageCode = result;
    }
```


```
import 'dart:async';
import 'package:http/http.dart' as http;
// 返回string
Future<String> getNetData() async{
  http.Response res = await http.get("http://www.baidu.com");
  return res.body;
}

main() {
  getNetData().then((str) {
    print(str);
  });
}
```