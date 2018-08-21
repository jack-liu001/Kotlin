[TOC]

### Kotlin介绍：

Kotlin是一种在Java虚拟机上运行的静态类型编程语言，可以编译成Java字节码，也可以编译成JavaScript。

### 一、Kotlin的特征

1. 服务器端、Android及任何Java运行的地方
2. 静态类型的编程语言
3. 函数式和面向对象
4. 代码简洁、安全、互操作性

```kotlin
internal object Test {
    @JvmStatic
    fun main(args: Array<String>) {
        print("Hello World !")
    }
}
```

### 二、Kotlin基础语法

Kotlin 文件以 `.kt`为后缀

#### 1.函数

##### a.带有返回值的函数 前边有修饰符，必须写明返回值类型，无修饰符的，自动推断类型；

```kotlin
//有修饰符
public fun sum(a:Int,b:Int) : Int{
  return a + b
}
//无修饰符
fun sum (a:Int,b:Int)= a+b
```

##### b.未带返回值的函数 两种方式： Unit可以省略不写

方式一：

```kotlin
public fun sum(a : Int , b :Int){
  println(a + b)
}
```

方式二：

```kotlin
fun sum(a : Int) :Unit{
  println(a)
}
```

##### c.可变长函数

 函数的变长参数用 `vararg` 关键字标识；

```kotlin
fun varLong(vararg m : Int){
  for(vt in m){
    println(vt)
  }
}
//测试
fun main(args: Array<String>){
    //输出12345
    varLong(1,2,3,4,5)
}
```

##### d.lambda表达式(匿名函数)

```kotlin
val sumLambda : (Int : Int) -> Int = {x,y -> x + y};
println(sumLambda(2,3));
```

#### 2.变量

##### a.定义常量与变量

可变变量定义: `var` 关键字,即使var关键字允许变量改变自己的值，但类型确实改变不了。

```kotlin
var <标识符> ：<类型> =<初始化值>
eg: var name :String  = "测试"
```

 不可变变量定义：`val` 关键字，只能赋值一次（类似Java中的final修饰），默认情况下，尽可能使用 `val` 关键字来声明所有的 `Kotlin` 变量

注：尽管val引用自身是不可变的，但是它指向的对象可能是可变的

```kotlin
val <标识符> :<类型> =<初始化值>
eg: val m : Int  = 0x1001 
val lan = arrayListOf<String>("Java")
lan.add("Kotlin")
```

​	在定义了val变量的代码块执行期间，val变量只能进行唯一一次初始化，但是，如果编译器能确保只有唯一一条初始化语句被执行，可以根据条件使用不同的值来初始化它。

```kotlin
 val msg: String
 if (lan.size>0) {
      msg ="Success"
 }else{
      msg ="Failed"
 }
```

常量与变量都可以没有初始化值,但是在引用前必须初始化；

编译器支持自动类型判断,即声明时可以不指定类型,由编译器判断 eg：

```kotlin
val b = 1;  //自动推断类型为Int
val c : Int  //如果不在声明时初始化则必须提供变量类型
c = 6;  //明确赋值
```

##### b.注释

Kotlin的块注释允许嵌套

#### 3.字符串

##### a.字符串模板

 $ 表示一个变量名或者变量值

 $varName 表示变量值

 ${varName.fun()} 表示变量的方法返回值:

```kotlin
var a = 1
// 模板中的简单名称：
val s1 = "a is $a" 

a = 2
// 模板中的任意表达式：
val s2 = "${s1.replace("is", "was")}, but now is $a"
```

##### b.NULL检查机制

 Kotlin的空安全设计对于声明可为空的参数，在使用时要进行空判断处理，有两种处理方式，字段后加 `!!` 像Java一样抛出空异常，另一种字段后加 `?`可不做处理返回值为 `null` 或配合 `?:` 做空判断处理。

```kotlin
//类型后面加?表示可为空
var age: String? = "23" 
var age: String? = null;
//抛出空指针异常
val ages = age!!.toInt()
//不做处理返回 null
val ages1 = age?.toInt()
//age为空返回-1
val ages2 = age?.toInt() ?: -1
```

##### c.类型检测及自动类型转换

 我们可以使用 `is` 运算符检测一个表达式是否某类型的一个实例(类似于Java中的instanceof关键字)。

```kotlin
fun getStringLength(obj : Any):Int?{
  if(obj is String){
   // 做过类型判断以后，obj会被系统自动转换为String类型
    return obj.length
  }
  return null
}
```

或者

```kotlin
//在这里还有一种方法，与Java中instanceof不同，使用!is
fun getStringLength(obj : Any):Int?{
  if(obj !is String){
   // 做过类型判断以后，obj会被系统自动转换为String类型
   return null
  }
   return obj.length
}
```

或者

```kotlin
fun getStringLength (obj : Any) : Int?{
  if(obj is String && obj.length > 0){
    retrun obj.length
  }
  retrun null
}
```

##### d.区间

区间表达式由具有操作符形式 `..` 的 `rangeTo` 函数辅以 `in` 和 `!in` 形成。

区间是为任何可比较类型定义的，但对于整型原生类型，它有一个优化的实现。以下是使用区间的一些示例:

```kotlin
//循环输出 输出1234
for (i in 1..4) print(i)
     print("\n")
//设置步长 输出13
for (i in 1..4 step 2) print(i)
     print("\n")
//使用downto 输出42
for (i in 4 downTo 1 step 2) print(i)
     print("\n")
//使用until 排除结束元素
for (i in 1 until 4) print(i)
```

#### 4.类和属性 

##### a.类

注：在Kotlin中public是默认的可见性，可以省略。

```kotlin
/**
*普通的Java bean类
* 转为Kotlin后的结构
*/
class Person {
    private String name;
    private int age;
    private String sex;

    public Person(String name, int age, String sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getSex() {
        return sex;
    }
}
```

```kotlin
/**
*Kotlin后的结构
* 只有getter方法
* 含有三个参数的构造器
* 这种类(只有数据没有其他代码)，叫做值对象
*/
internal class Person(val name: String, val age: Int, val sex: String)

/**
*Kotlin后的结构
* 只有setter方法
* 含有参数的构造器
*/
internal class Animal(private var name: String?) {

    fun setName(name: String) {
        this.name = name
    }
}


/**
 * <br></br>
 * bean
 * getter和setter方法
 * 无参构造器
 * 含参构造器
 * @author Lyt
 * @version 4.0
 * @date 2018/5/8 下午4:16
 * lyt-版权所有
 */
internal class Tree {
    var name: String? = null

    constructor() {}

    constructor(name: String) {
        this.name = name
    }
}
```

##### b.属性 在Kotlin中，属性是头等的语言特性，完全代替了字段和访问器方法。在类中声明一个属性和声明一个变量一样：使用val和var关键字，声明val的属性是只读的，var属性是可变的。

```kotlin
/**
 * <br>
 * bean
 * name属性只有getter
 * address属性只有getter和setter方法
 * @author Lyt
 * @version 4.0
 * @date 2018/5/8 下午4:56
 * lyt-版权所有
 *
 */
class School(val name: String, var address: String)
//调用
val school = School("北京大学", "beijing")
        print("hello,${school.name}")
```

#### 5.枚举和 `when`

##### a.枚举

Kotlin用了enum class 两个关键字，enum是一个软关键字，Java只有 一个enum关键字

如果要在枚举类中定义任何方法，就要使用分号把枚举常量和方法定义分开。

```kotlin
/**
 * Created by ${lei} on 2018/5/22.
 * Desc:枚举类
 * Since：
 */
enum class Color(val r: Int, val g: Int, val b: Int) {
    RED(255, 0, 0), ORANGE(255, 165, 0), YELLOW(155, 155, 0), GREEN(0, 255, 0), BLUE(0, 0, 255);
 
    fun rhg() = (r * 256 + g) * 256 + b
}
```

##### b. `when` 处理枚举类

when允许任何对象，若没有给when表达式提供参数，分支条件就是任意布尔表达式。

```kotlin
fun getMnwmonic(color: Color) =
        when (color) {
            Color.RED -> "1"
            Color.ORANGE -> "2"
            Color.YELLOW -> "3"
            Color.GREEN -> "4"
            Color.BLUE -> "5"
            else -> {
                "0"
            }
        }
```

#### 6.迭代：`while` 循环和 `for` 循环

##### a. `while`

```kotlin
while(condition){
    //业务
}

do{
   //业务 
}while(condition)
```

##### b. `for` 循环

```kotlin
 for (i in 1..100) {
   print(i + "\n" )
 }
```

###三、函数的定义与调用
####1.函数的命名和调用
```kotlin
	使用常规语法时，必须按照函数声明中定义的参数顺序来给定参数，可以省略的只有排在末尾的参数，若使用命名参数，则可以省略中间的一些参数，亦可以按照想要的顺序只给定你需要的参数。
	参数的默认值是被编码到被调用的函数中，而不是调用的地方。
	若Java中调用Kotlin函数，函数有默认值，使用@JavaOverloads注解
/**
 * 集合输出格式是(1;2;3)
 * 声明带默认值的参数,避免创建重载函数
 */
    fun <T> JoinToString(collection: Collection<T>, separator: String ="", prefix: String = "", postfix: String =""): String {
        val result = StringBuffer(prefix)
        for ((index, element) in collection.withIndex()) {
            if (index > 0) {
                result.append(separator)
            }
            result.append(element)
        }
        result.append(postfix)
        return result.toString();
    }
    
    //省略末尾参数
    print(JoinToString(list)) ------1 2 3
    //给定参数
	print(JoinToString(list,prefix = "#",postfix = "#"))
	#1 2 3#
```
	当调用一个Kotlin定义的函数时，可以显示得标明一些参数的名称，若指明了一个参数的名称，为避免混淆，那它之后所有的参数都需要表明名称。
```kotlin
print(JoinToString(list, separator = ";", prefix = "(", postfix = ")"))
```
####2.消除静态工具类:顶层函数和属性
​	在Kotlin中，对于静态工具类，可以把这些函数直接放到代码文件(Kotlin文件)的顶层，不用从属于任何类，这些依旧是包内的成员，访问，import。顶层函数不能访问类的private成员。

![](/Users/candice/Downloads/Worksoace/IDEASpace/LearnKotlin/Pic/屏幕快照 2018-06-19 下午4.43.40.png)

​	要改变Kotlin顶层函数的生成类的名称，需要为这个文件添加 `@JvmName` 的注解，放在文件的开头，位于package的前面
```kotlin
@file:JvmName("StringFunctions")
package util
```
​       也可以将属性放在文件的顶层

```kotlin
/**
 * 顶层常量属性
 * 相当于 const public static final
 * 以public static final属性暴露给Java，使用const
 * const适用于所有基本数据类型的属性，以及String型
 */
const val TAG = "测试"
```

​       扩展函数是静态函数，可以使用关键字 `as` 来修改导入的类或者函数名称,不可被重写，如果一个类的成员函数和扩展函数有相同的签名，成员函数往往会被优先使用。
```kotlin
/**
 * 扩展函数，可以直接在Main方法中调用
 */
fun String.lastChar(): Char = this.get(this.length - 1)
//调用
print("Kotlin".lastChar())

//使用as修改函数名字
import util.lastChar as last
  print("Kotlin".last())

//Java类中调用Kotlin
JoinKtKt.lastCha1r(name);

```
​	扩展属性

```kotlin
/**
 * 扩展属性
 */
val String.lastChar1: Char
    get() = get(length - 1)

var StringBuilder.lastChar2: Char
    get() = get(length - 1)
    set(value: Char) {
        this.setCharAt(length - 1, value)
    }
//Java中访问 显式的调用其getter函数
JoinKtKt.getLastChar2(new StringBuilder(name));
```

#### 3.处理集合

- 可变参数关键字 `vararg` 
- 中缀表示法，当调用一些只有一个参数的函数时，使用中缀表示法，代码简练
- 解构声明：用来把一个单独的组合值展开到多个变量中

#####    (1)扩展Java集合和可变参数
```kotlin
/**
*当需要传递的参数已经包装在数组中时，需要显式的解包数组，，以便每个数组元素在函数中能作为单独
*的参数来调用,使用的时候是在对应参数前面加上 “ * ”(功能叫展开运算符)
*/
val list = listOf<String>("1", "2", "3", "4", "5", *args)
        //获取集合最后一个元素
        print(list.last())
        //获取集合最大 最小元素
        print(list.max())
        print(list.min())

```
##### (2)中缀调用和解构声明—键值对(map)

​	中缀调用： `to` ， 函数名称直接放在目标对象名称和参数之间。要允许使用中缀符号调用函数，需要使用 `infix`

```kotlin
 //map
  val map = mapOf<Int, String>(1 to "one", 2 to "two")
  mapOf<String,String>("1".to("one"),"2".to("two"))
//使用中缀符号调用to函数
  1 to "one"
//一般to函数的调用
  "1".to("one")
```

##### (3)字符串和正则表达式处理

  	分割字符串 底下二者等价，Java中使用split 分割如下字符串返回空数组

```kotlin
 print("12.345-6.A".split("\\.|-".toRegex()))
 print("12.345-6.A".split(".", "-"))
```

​	String扩展函数：Kotlin标准库中包含了一些可以用来获取在给定分隔符第一次（或最后一次）出现之前（或之后）的字符串的函数。

```kotlin
/**
 * string扩展函数解析文件路径
 */
fun parsePath(path: String) {
    val directory = path.substringBeforeLast("/")
    val fullName = path.substringAfterLast("/")
    val fileName = fullName.substringBeforeLast(".")
    val extension = fullName.substringAfterLast(".")
    print("Dir: $directory ,name : $fileName,extension: $extension")
}
```

​	扩展函数

```kotlin
//提取局部函数
fun saveTransportation(transportation: Transportation) {
    fun validate(transportation: Transportation, value: String, fileName: String) {
        if (value.isEmpty()) {
            throw IllegalArgumentException("Can't save transportation 			      ${transportation.id}:empty $fileName")
        }
    }

    validate(transportation, transportation.name, "ship")
}


//在局部函数中访问外层函数的参数
fun saveTransportation1(transportation: Transportation) {
    fun validate(value: String, fileName: String) {
        if (value.isEmpty()) {
            throw IllegalArgumentException("Can't save transportation ${transportation.id}:empty $fileName")
        }
    }

    validate(transportation.name, "ship")
}

//获取逻辑到扩展函数
fun Transportation.validateBeforeSave() {
    fun validate(value: String, fileName: String) {
        if (value.isEmpty()) {
            throw IllegalArgumentException("Can't save transportation $id:empty $fileName")
        }
    }
    validate(name, "ship")
}

fun save(transportation: Transportation) {
    transportation.validateBeforeSave()
}

```

### 四、类、对象和接口

#### 1.接口

​		Kotlin在类名后使用 `：` 来代替Java中的 `extend` 和 `implements` 关键字，多实现，单继承；在Kotlin中 `override`  修饰符是强制要求的，接口的方法可以有一个默认实现。对于实现多个接口，当有包含相同的带默认的方法，必须显式的实现该默认方法，通过调用继承的两个父类型中的实现来实现默认方法。

```kotlin
class Button : View(), Clickable, Focusable {
    override fun show() {
        super<Clickable>.show()
        super<Focusable>.show()
    }


    override fun click() {
        print("I was clicked")
    }

    override fun onClick() {
        super.onClick()
        print("Button is clicked")
    }
}
```

#### 2.open、final和abstract修饰符：默认为final

​	Kotlin默认都是 `final` .想要一个类被继承、方法或属性被重写，需要在类之前、方法或属性之前添加 `open` 修饰符。若重写了一个基类或者接口的成员，重写的成员默认是 `open` ，想要不被再重写，显式的将要重写添加 `final` 。

​     抽象类：抽象成员始终是 `open` 的，非抽象成员并不是默认的 `open`，可以显式的标注。

```kotlin
abstract class LinearLayout {
    abstract fun animate()

    fun startAnimate(){

    }

   open fun stopAnimate(){

    }
}
```

​	在接口中不能使用final、open或者abstract，接口中的成员始终是open的。

| 修饰符   | 相关成员               | 评注                                          |
| :------- | ---------------------- | --------------------------------------------- |
| final    | 不能被重写，Kotlin默认 | 类中成员默认使用                              |
| open     | 可以被重写             | 需要明确表明                                  |
| abstract | 必须被重写             | 只能在抽象类中使用，抽象成员不能有实现        |
| override | 重写父类或接口中的成员 | 如果没有使用final表明，重写的成员默认是开放的 |

#### 3.可见性修饰符：默认public

​	Kotlin中省略修饰符，默认是 `public` 。

​     `internal`：只在模块中可见。

| 修饰符       | 类成员       | 顶层声明     |
| ------------ | ------------ | ------------ |
| public(默认) | 所有地方可见 | 所有地方可见 |
| internal     | 模块中可见   | 模块可见     |
| protected    | 子类中可见   | --           |
| private      | 类中可见     | 文件中可见   |

​	类的基础类型和类型参数列表中用到的所有类，或者函数的签名都有与这个类或者函数本身相同的可见性。

​	    类的扩展函数不能访问它的 `private` 和 `protected` 成员。

​	Kotlin中一个外部类不能看到其内部（或者嵌套）类中的private成员。

#### 4.内部类和嵌套类：默认嵌套类

​	Kotlin的嵌套类不能访问外部类的实例。

​       	Kotlin中没有显式修饰符的嵌套类与Java中static嵌套类是一样的要把它变成一个内部类来持有一个外部类的引用的引用的话要使用 `inner` 修饰符。

```kotlin
class ImageButton : View {
    override fun getCurrentStata(): State {
        return ImageButtonState()
    }

    override fun restoreState(state: State) {
        print("I am ImageButton")
    }

    //嵌套类
    class ImageButtonState : State {

    }
}
```

| 类A在另一个类B中声明       | 在Java中       | 在Kotlin中    |
| -------------------------- | -------------- | ------------- |
| 嵌套类(不存储外部类的引用) | static class A | class A       |
| 内部类(存储外部类的引用)   | class A        | inner class A |

​	Kotlin中内部类引用外部类的，需要使用 `this@Outer`

```kotlin
class TextView : Focusable {

    inner class Inner {
        fun getOuterReference(): TextView = this@TextView
    }
}
```

#### 5.sealed修饰符

​	为父类添加一个sealed修饰符，对可能创建的子类做出严格的限制，所有的直接子类必须嵌套在父类中，被sealed修饰符修饰的这个类是个open类。

```kotlin
/**
*在when表达式中处理所有sealed类的子类，不再需要提供默认的else分支
*
*/
sealed class Expr {
    class Nume(val value: Int) : Expr()
    class Sume(val left: Expr, val right: Expr) : Expr()
}

fun eveal(expr: Expr): Int =
        when (expr) {
            is Expr.Nume -> expr.value
            is Expr.Sume -> eveal(expr.left) + eveal(expr.right)
        }
```

#### 6.构造方法和属性

​	Kotlin区分主构造方法(在类体外声明)和从构造方法(使用constructor)。

```kotlin
//主构造函数，若没有注解或可见性修饰符，constructor可省略
class User constructor(name: String) {
  //也可以放在init代码块中
    val name = name

    //初始化语句块，类被创建时执行的代码，可以声明多个
    init {}
    
      //自定义set
    var address: String = "unspecofied"
        set(value: String) {
            print("""Address was changed for $name:$field -> $value.""")
            field = value
        }
    
    //修改访问器的可见性
    var counter: Int = 0
    //不能再类外部修改这个属性
        private set

    fun addWord(word: String) {
        counter += word.length
    }
}
```

#### 7.数据类和类委托

 	在Kotlin中，== 运算符是比较两个对象的默认方式：本质上是调用equals来比较两个值。如果equals被重新了，就能够很安全的使用 == 比较实例了；若要进行引用比较，使用 === 运算符。	

#### 8."object"关键字

​	定义一个类并创建一个实例。

- **对象声明**：类声明和单例声明组合到一起。通过加入object关键字，用一句话来定义一盒类和一个该类的变量。对象声明，可以包含属性、方法、初始化语句块等，声明构造方法是无意义的。

	在大型软件系统中慎用对象声明，对对象实例化没有任何控制，并且不能通过构造方法指定特定的参数。

	对象名.字符 方式来调用和访问属性

	在Java中使用Kotlin对象：对象声明被编译成通过静态字段来持有它的单一实例的类，这个字段名字始终是 `INSTANCE` ,在Java中调用，则是 `对象名.INSTANCE.字符` 。

	```kotlin
	//对象声明
	object Payroll {
	
	    val allEmployees= arrayListOf<User>()
	
	    fun calculateSalary(){
	        print("工资")
	    }
	}
	//调用方式
	//调用函数
	 Payroll.calculateSalary()
	//调用属性
	 Payroll.allEmployees
	//在Java中调用
	Payroll.INSTANCE.calculateSalary();
	```

- **伴生对象**:Kotlin中类不能拥有静态成员，依赖包级别函数和对象声明，在子类中不能被重写。

	关键字：companion ，可以直接通过容器类名称来访问这个对象的方法和属性，不需要显式地指明对象的名称，可以访问类中所有private成员，包括private构造方法和属性。     

```kotlin
class Animal() {
    //实例创建成功后才会执行
    val name: String? = ""

    //直接用  Animal.getAnimal()调用
    companion object {
        fun getAnimal() {
            print("name is ${Animal().name}")
        }
    }
}

//在Java中调用
Animal.Companion.getAnimal();
```

 **工厂方法**：能够返回声明这个方法的类的子类。

```kotlin
 /**
  * 工厂方法
  * 可以指定伴生对象的名字，也可以省略
  * 省略名字，默认分配为companion
  */
    companion object Loader {
        fun newSubcribingUser(email: String) = User(email.substringBefore('@'))

        fun newFaceBookUser(accountId: String) = User("accountId is $accountId")
    }
//调用
  print(User.Loader.newSubcribingUser("12344@gamil.com").name+"\n")
  print(User.newFaceBookUser("799790").name)
```

**伴生对象实现接口**：

```kotlin
class Person(val name: String) {
    companion object : JSONFactory<Person> {
        override fun fromJSON(jsonText: String) = Person("jsonText is $jsonText")

    }
}
```

**伴生对象扩展**：为了定义一个扩展，必须在类中声明一个伴生对象，即使为空也可。

```kotlin
class Person(val name: String) {

    companion object : JSONFactory<Person> {
        override fun fromJSON(jsonText: String) = Person("jsonText is $jsonText")

    }
}

/**
 * 伴生对象扩展
 */
fun Person.Companion.fromJson(json:String):Person{
    return  Person("扩展函数")
}
```

- **对象表达式(匿名对象)：**替代了Java中匿名内部类的用法。不是单例，每次对象表达式被执行都会创建一个闲的对象实例。

	```kotlin
	    /**
	     * 对象表达式：匿名内部类 调用局部变量
	     */
	fun countClicks(window: Window) {
	    var count = 0
	     window.addMouseListener(object : MouseListener {
	         override fun mouseReleased(e: MouseEvent?) {
	         }
	
	         override fun mouseEntered(e: MouseEvent?) {
	         }
	
	         override fun mouseClicked(e: MouseEvent?) {
	                count++
	         }
	
	         override fun mouseExited(e: MouseEvent?) {
	         }
	
	         override fun mousePressed(e: MouseEvent?) {
	         }
	        })
	    }
	```

	### 

### 五、lambda表达式

#### 1.语法

```kotlin
{x:Int,y:Int -> x + y}
```

####  2.在作用域访问变量

​	Kotlin中不会仅限于访问final变量，在lambda内部也可以修改这些变量。荀彧在lambda内部访问final变量甚至修改他们。lambda访问外部变量，即lambda捕捉。在默认情况下，局部变量的声明周期被限制在声明这个变量的函数中，若被lambda捕捉，使用这个变量的代码可以被存储并稍后执行。若捕捉是final变量，它的值和使用这个值的lambda代码一起存储；对于非final变量来说，它的值被封装在一个特殊的包装器中，这样你就可以改变这个值，而对这个包装器的引用会和lambda代码一起存储。若lambda被用作时间处理器或者在其他异步执行的情况，对局部变量的修改只会在lambda执行时发生。

```kotlin
//在lambda使用后汉书参数
fun printMessage(message: Collection<String>, prefix: String) {
	message.forEach {
		print("$prefix $it")
	}
}

//在lambda中改变局部变量
	fun printProblemCounts(response: Collection<String>) {
		var clientErrors = 0
		var serverErrors = 0
		response.forEach {
			if (it.startsWith("4")) {
				clientErrors++
			} else if (it.startsWith("5")) {
				serverErrors++
			}
		}
		print("$clientErrors client errors, $serverErrors server errors ")
	}

```

#### 3.成员引用     

```kotlin
//语法
//双冒号把类名称与你要引用的成员名称隔开
Person::age
//类::成员
```

#### 4.集合函数API

##### 1. `filter` 和  `map`

​	filter函数结果返回一个集合，只包含输入集合中那些满足判断式的元素。

​	map函数对集合中的每一个元素应用个盾构的函数并把结果收集到一个新的集合。

```kotlin
		//filter
		val list = listOf<Int>(1, 2, 3, 4, 5, 6)
		val filter = list.filter { it % 2 == 0 }
//		print(filter)
		print(list.map { it * it })

		val persons = listOf<Person>(Person("Alice", 34), Person("Jack", 23), Person("John", 56))
		print(persons.filter { it.age > 30 })
		print(persons.map { it.name })
```

##### 2. `count、all、any、find`

​	 `count`  函数检查集合有多少元素满足判断式。Int

​	 `find` 函数返回第一个符合条件的元素。

​	 `all` 函数是否所有的函数都满足判断式。 boolean

 	 `any` 检查集合中是否至少存在一个匹配元素  boolean

##### 3.惰性操作：序列

​	Sequence:一个可以逐个列举元素的元素序列，调用扩展函数 `asSquence()` 把任意集合转成序列，调用   `toList()` 转为列表。

##### 4.`"with"和“apply”`

**with：**对同一个对象执行多次操作，不需要反复把对象名称写出来。

```kotlin
//this 可以省略
fun alphabet(): String {
		val stringBuilder = StringBuilder()
		return with(stringBuilder) {
			for (letter in 'A' .. 'Z') {
				this.append(letter)
			}
			append("\nNow I know the alphabet")
			this.toString()
		}
	}
```

**apply：**始终会返回作为实参传递给它的对象。

```kotlin
fun alphabet() = StringBuilder().apply {
		for (letter in 'A' .. 'Z') {
			append(letter)
		}
		append("\nNow I know the alphabet")
}.toString()
```

### 六、类型系统

#### 1.可空类型

```kotlin
语法：Type？ = Type or null
```

​	可以加在任何类型后面来表示这个类型的变量存储null引用；没有问号的类型表示这种类型的变量不能存储null引用，非空。

#### 2.安全调用运算符

![](/Users/candice/Downloads/Worksoace/IDEASpace/LearnKotlin/Pic/安全调用运算符.png)

```kotlin
//允许你把一次null检查和一次方法调用合并成一个操作。
  ?.  
 val name = ""
 print(name?.length ?: 0)
```

#### 3.Elvis运算符(null合并运算符)：“?:“

![](/Users/candice/Downloads/Worksoace/IDEASpace/LearnKotlin/Pic/Elvis运算符.png)

```kotlin
//若s为null，空字符串；若不为null，则为s
fun foo(s:String){
    val t:String =s ?: ""
}
```

#### 4.安全转换："as?"

![](/Users/candice/Downloads/Worksoace/IDEASpace/LearnKotlin/Pic/安全转换as？.png) `as?`运算符常识把值转换成指定类型，如果只不是合适的类型就返回 null。

#### 5.非空断言：”!!“

![](/Users/candice/Downloads/Worksoace/IDEASpace/LearnKotlin/Pic/非空断言!!.png)

  可以把任意值转换成非空类型。

#### 6."let"函数

![](/Users/candice/Downloads/Worksoace/IDEASpace/LearnKotlin/Pic/let函数.png)

```kotlin
//eamil为null时，当为""还是会走let函数，所以只针对null
	val email: String? = ""
	email?.let { email -> sendEmail(email) }
	email?.let { sendEmail(it) }
```

#### 7.延迟初始化属性

 	延迟初始化属性都是var，使用 lateinit(依赖注入)。

#### 8.类型参数的可空性

​	Kotlin中所欲泛型类和泛型函数的类型参数默认是可空的，任何类型，包括可空类型在内，都可以替换类型参数，使用类型参数作为类型的声明都允许为null；要使类型参数非空，必须要为它指定一个非空的上界，那样泛型会拒绝可空值作为实参。

```kotlin
//类型参数的可空性
	fun<T> printHashCode(t:T){
		print(t?.hashCode())
	}
//类型参数非空，需要指定一个上界
fun<T:Any> printHashCode(t:T){
		print(t.hashCode())
	}
```

#### 9.基本数据类型

​	Kotlin并不区分基本数据类型和包装类型。

​	基本数据类型(除boolean)转换必须显式的进行转换，都有对应的转换函数。

```kotlin
toByte()
toLong()
toChar()
toShort()
toFloat()
toInt()
toDouble()
```

​	  `Any` 是所有类型的超类型(超类型的根)

​	 `Nothing `类型没有任何值，只有被当做函数返回值使用，或者被当做泛型函数返回值的类型参数使用。

#### 10.集合和数组

​	集合列表的可空和集合元素的可空

```kotlin
//创建一个包含可空值的集合
fun readNumbers(reader: BufferedReader): List<Int?> {
	val result = ArrayList<Int?>()
	for (line in reader.lineSequence()) {
		try {
			val number = line.toIntOrNull()
			result.add(number)
		}catch (e:NumberFormatException){
			result.add(null)
		}

	}
	return result
}


//    使用可空值的集合
fun addValidNumbers(number: List<Int?>) {
	val validNums = number.filterNotNull()
//	var sumVaildNum = 0
//	var inVaildNum = 0
//	for (num in number) {
//		if (num != null) {
//			sumVaildNum += num
//		} else {
//			inVaildNum++
//		}
//	}
	print("sum of valid numbers:${validNums.sum()}" + "\n")
	print("Invalid number:${number.size-validNums.size}")
}

//集合列表可空
List<Int>?
//都可空
List<Int?>?
```

​	Kotlin把访问集合数据的接口和修改集合数据的接口分开了。

​	 访问集合的接口 `kotlin.collections.Collection`——只读集合

​	 修改集合的接口 `kotlin.collections.MutableCollection`-----修改集合 

| 集合类型 | 只读   | 可变                                              |
| -------- | ------ | ------------------------------------------------- |
| List     | listOf | mutableListOf、arrayListOf                        |
| Set      | setOf  | mutableSetOf、hashSetOf、linkedSetOf、sortedSetOf |
| Map      | mapOf  | mutableMapOf、hashMapOf、linkedMapOf、sortedMapOf |

### 六、运算符重载

#### 1.重载二元算术运算

​	重载运算符的所有函数都需要用 `operator` 关键字，Kotlin限定了你能重载哪些运算符，一级你需要再你的类中定义的对应名字的函数。

```kotlin
data class Point(val x: Int, val y: Int) {
	operator fun plus(other: Point): Point {
		return Point(x + other.x, y + other.y)
	}
}
```

| 表达式 | 函数名 |
| ------ | ------ |
| a * b  | times  |
| a / b  | div    |
| a % b  | mod    |
| a + b  | plus   |
| a - b  | minus  |

​	Kotlin运算符不会自动支持交换性，如下：

```kotlin
//定义一个运算符，不要求两个运算数是相同的类型，p*1.5
operator fun Point.times(scale:Double):Point{
	return Point((x*scale).toInt(),(y*scale).toInt())
}
//定义一个运算符，不要求两个运算数是相同的类型，1.5*p
operator fun Double.times(p:Point):Point{
	return Point((this*p.x).toInt(),(this*p.y).toInt())
}

```

​	Kotlin提供用于执行位运算的完整函数列表：

| 函数 | 作用       |
| ---- | ---------- |
| sh1  | 带符号左移 |
| shr  | 带符号右移 |
| ushr | 无符号右移 |
| and  | 按位与     |
| or   | 按位或     |
| xor  | 按位异或   |
| inv  | 按位取反   |

#### 2.重载复合赋值运算符

​	当定义了二元算术运算符，还支持+=、-=这类复合赋值运算符

​	当在代码中用到+=的时候，理论上plus和plusAssign都可能被调用，如果在这种情况下，两个函数都有定义且适用，编译器会报错。解决办法：用val替换替换var，这样plusAssign运算就不适用。

```kotlin
	val arrayList = arrayListOf<Int>(1, 2)
	arrayList += 3
	print(arrayList)
	print("\n")
	val newList = arrayList + listOf<Int>(4, 5)
	print(newList)
```

| 表达式 | 函数        |
| ------ | ----------- |
| +=     | plusAssign  |
| -=     | minusAssign |
| *=     | timeAssign  |

#### 3.重载一元运算符

| 表达式  | 函数名     |
| ------- | ---------- |
| +a      | unaryPlus  |
| -a      | unaryMinus |
| !a      | not        |
| ++a,a++ | inc        |
| --a,a-- | dec        |

#### 4.重载比较运算符

**”equals“：**==  != 可以用于可空运算数，会检查运算数是否为null。

**"compareTo":**

```kotlin
	val alice = Person("Alice", 34)
	val jack = Person("Jack", 23)
	print(alice < jac
```

#### 5.集合和区间约定

- 通过下标来访问元素：“get”和“set”

  ```kotlin
  //get
  operator fun get(index: Int): Int {
  		return when (index) {
  			0 -> x
  			1 -> y
  			else -> throw IndexOutOfBoundsException("Invaild coordinate $index")
  		}
  	}
  
  //set
  data class MutablePoint(var x: Int,var y:Int) {
  
  	operator fun set(index:Int,value:Int){
  		when(index){
  			0->x=value
  			1->y=value
  			else->throw  ArrayIndexOutOfBoundsException("Invaild coordinate $index")
  		}
  	}
  }
  ```

- "in"的约定

  ![](/Users/candice/Downloads/Worksoace/IDEASpace/LearnKotlin/Pic/in约定.png)

- rangeTo约定

  ```
  ".."运算符是调用rangeTo函数,返回一个区间(闭区间)
  ```

  ![](/Users/candice/Downloads/Worksoace/IDEASpace/LearnKotlin/Pic/rangeTo.png)

- 解构声明和组件函数

  **解构声明：**允许展开单个复合值，并使用它初始化多个单独变量。

  ​		   初始化每个变量，需调用componentN，N表示位置，但是在data数据类中，编译器会为每个主构造方法中声明的属性生成一个componentN函数,标准库只允许使用此语法访问一个对象的前五个元素。

  ```kotlin
  	var p2 = Point(24, 14)	
  	val (x, y) = p2
  		print(x)
  		print("\n")
  		print(y)
  ```

- 委托属性：操作对象不用自己执行，把工作委托给另一个辅助对象，辅助对象称为委托。

	​	关键字by对其后的表达式求值来获取这个对象，关键字可以用于任何符合属性委托约定规则的对象。

	```kotlin
	//属性p将她的访问器逻辑委托了另一个对象：Delegate类的一个新的实例
	class Foo{
	    var p :Type by Delegate()
	}
	```

### 七、高阶函数

​	 **高阶函数：**以另一个函数作为参数或者返回值的函数。

 			    任何以lambda或者函数引用作为参数的函数，或者返回值为lambda或函数引用的函数，或者两者都满足的就是高阶函数。

#### 1.函数类型

```kotlin
//参数类型      返回类型
val/var name = (Int, String) -> Unit
//函数类型声明需要一个现实的返回类型，Unit不可以省略。

val mins: (Int, Int) -> Int = { x, y -> x - y }
//可空的函数类型的变量
val funOrNull: ((Int, Int) -> Int)? = null
//返回值可空的函数类型
var canReturnNull: (Int, Int) -> Int? = { x, y -> x - y }
```

#### 2.lambda去除重复代码

```kotlin
data class SiteVisit(val path: String, val duration: Double, val os: OS) {}
//val duration = log.filter { it.os == OS.MAC }.map { it -> it.duration }.average()
fun List<SiteVisit>.averageDuration(os: OS) = 
	filter { it.os == os }
	.map { it -> it.duration }
	.average()


//val mobile = log.filter { it.os in setOf<OS>(OS.IOS, OS.ANDROID) //}.map(SiteVisit::duration).average()
fun List<SiteVisit>.averageDurationFor(pridicate: (SiteVisit)->Boolean) =
		         filter (pridicate)
				.map { it -> it.duration }
				.average()
```

#### 3.内联函数

**关键字：** `inline` 

​	当一个函数被声明为inline时，她的函数体是内联的，函数体会被直接替换到函数被调用的地方，而不是被正常调用。

```kotlin
fun foo(lock: Lock) {
	print("before sync")
	lock.lock()
	synchronized(1) {
		print("Action")
	}
	print("After sync")
}


//以下是foo函数编译的字节码
//只有synchronized被内联了，lambda才会被正常调用
fun foo(lock: Lock) {
	print("before sync")//调用者foo函数代码
	lock.lock() //被内联的synchronized函数代码

	try {
		print("Action")//被内联的lambda体代码
	}finally {
		lock.unlock()
	}
	print("After sync")//调用者foo函数代码
}
```

​	如果在两个不同的位置使用同一个内联函数，但是用的是不同的lambda，内联函数会在每一个被条用的位置被分别内联，内联函数的代码的代码会被拷贝到使用它的两个不同位置，并把不同的lambda替换其中。

#### 4.高阶函数中的控制流

**非局部返回：**只有在以lambda作为参数的韩珊瑚是内联函数的时候才能从更外层的函数返回。在一个非内联函数的lambda中使用return表达式是不允许的。

```kotlin
//在lambda中使用return关键字，它会调用lambda的函数中饭，并不只是从lambda中返回。
fun lookForOs(list: List<SiteVisit>) {
//		for (sitevisit in list){
//			if (sitevisit.os == OS.WINDOWS){
//				print("sitevisit.os is $sitevisit.os")
//				return
//			}
//		}
		list.forEach {
			if (it.os == OS.WINDOWS) {
				print("sitevisit.os is $it.os")
				return
			}
		}
	}
```

**局部返回：**类似Javafor循环中的break语句，会终止lambda的执行，并接着从调用lambda的代码出执行。区分局部返回和非局部返回，使用标签，想要从一个lambda表达式返回可以标记它，然后在return关键字后面引用这个标签。

```kotlin
//方式一：在lambda之前放一个标签名(可以是任意标识符)+@，要从lambda返回，在return+@标签名
fun lookForOs(list: List<SiteVisit>) {
//		for (sitevisit in list){
//			if (sitevisit.os == OS.WINDOWS){
//				print("sitevisit.os is $sitevisit.os")
//				return
//			}
//		}
		list.forEach label@{
			if (it.os == OS.WINDOWS) {
				print("sitevisit.os is $it.os")
				return@label
			}
		}
		print("Not found!")
	}

//方式二：使用lambda作为参数的函数的函数名可以作为标签

fun lookForOs(list: List<SiteVisit>) {
		list.forEach {
			if (it.os == OS.WINDOWS) {
				print("sitevisit.os is $it.os")
				return@forEach
			}
		}
		print("Not found!")
	}
//注：若显示地制定了lambda表达式标签，再使用函数名作为标签没有任何效果，一个lambda表达式的标签数量不能多于一个
```

**匿名函数：**和普通函数一样有相同的指定返回值类型规则，代码块体的匿名函数需要显式地指定返回类型，表达式函数体，可以省略返回类型。在匿名函数中，不带标签的return表达式会从匿名函数返回，而不是从包含匿名函数的函数返回。原因：return从最近的使用fun关键字声明的函数返回。

```kotlin
//代码块匿名函数	
log.filter(fun (sitevisit):Boolean{
			return sitevisit.os == OS.LINUX
		})

fun lookForOs(list: List<SiteVisit>) {
		list.forEach (fun(sitevisit){
			if (sitevisit.os == OS.WINDOWS) {
				print("sitevisit.os is $sitevisit.os")
				return
			}
			print("Not found!")
		})
	}
//表达式匿名函数
log.filter(fun(sitevisit)=(sitevisit.os == OS.LINUX))
```

### 八、泛型

Kotlin泛型在运行时会被擦除。

**1.** 变型

​	子类型：任何时候如果需要的是类型A的值，都能够使用类型B的值(当作A的值)，类型B就称为类型A的子类型，Int就是Number的子类型。

​	超类型：A是B的子类型，B就是A的超类型

### 九、注解和反射

#### 1.应用注解

以 `@` 字符作为(注解)名字的前缀，并放在要注解的声明最前面。

注解只能拥有以下类型的参数：基本数据类型、字符串、枚举、类引用、其他的注解类，以及前面这些类型的数组.

```kotlin
/**
 * Removes the element at the specified [index] from this list.
 * In Kotlin one should use the [MutableList.removeAt] function instead.
 */
//ReplaceWith是一个注解，定义为Deprecated的注解的实参，没有 @
@Deprecated("Use removeAt(index) instead.", ReplaceWith("removeAt(index)"), level = DeprecationLevel.ERROR)
@kotlin.internal.InlineOnly
public inline fun <T> MutableList<T>.remove(index: Int): T = removeAt(index)
```

指定注解实参语法

- 要把一个类指定为注解实参，在类名后加上 `::class:@MyAnnotation（MyClass::class）`

- 要把另一个注解指定为一个实参，去掉注解前面的 `@`

- 要把一个数组指定为一个实参，使用 `arrayOf` 函数：`@RequestMapping(path = arrayOf("/foo","/bar"))`

	注解实参在编译器就是已知的，不能引用任意的属性作为实参。要把属性当作注解实参使用，需要使用const修饰符标记它，来告知编译器这个熟悉感是编译期常量。

注解目标：使用点目标声明被用来说明要注解的元素，被房子 @ 符号和注解名称之间，并用冒号和注解名称隔开。 `@get:Rule` 其中 get 是使用点目标。支持的使用点目标列表如下：

- property—Java的注解不能应用这种使用点目标
- field—为属性生成的字段
- get——属性的getter
- set——属性的setter
- recriver——扩展会后汉书或者扩展属性的接受者参数
- param------构造方法参数
- setparam—属性setter的参数
- delegate——为委托属性存储委托实例的字段
- file——包含在文件中声明的顶层函数和属性的类

#### 2.Json序列化

把一个对象序列化成JSON

`@JsonExclude` 标记一个属性，这个属性应该排除在序列化和反序列化之外。

`@JsonName` 说明代表这个属性的JSON键值对之中的键应该是一个给定的字符串，而不是属性的名称

**声明注解：**annotation class  类名 

​	注解类知识用来定义关联到声明和表达式的元数据的结构，不包含任何代码，编译器禁止为一个注解类指定类主体。对拥有参数的注解来说，在类的住构造方法中声明这些参数。val关键字是强制的，对一个注解类的所有参数来说。

```kotlin
//声明注解 annotation class  类名
@Target(AnnotationTarget.PROPERTY)
annotation class JsonExclude
```

**元注解：**可以应用到注解类上的注解。 常见 `@Target`说明了注解可以被应用的元素类型。

```kotlin
//out关键字允许引用那些继承Any的类，而不仅仅是引用Any自己。
@Target(AnnotationTarget.PROPERTY)
annotation class DeserializeInterface(val targetClass: KClass<out Any>)

//使用类做注解参数  通常使用类名称+::class关键字来引用一个类。
data class Person(
        val name: String,
        @DeserializeInterface(CompanyImpl::class) val company: Company
)
```

**泛型做注解参数：**

```kotlin
//<*> 允许ValueSerializer序列化任何值
//out 接受任何实现了ValueSerializer的接口，不只是ValueSerializer::class
//<out ValueSerializer<*>> 接受实现了ValueSerializer接口的类作为有效实参。如DateSerializer::class
@Target(AnnotationTarget.PROPERTY)
annotation class CustomSerializer(val serializerClass: KClass<out ValueSerializer<*>>)


//DateSerializer 实现ValueSerializer接口
object DateSerializer : ValueSerializer<Date> {
    private val dateFormat = SimpleDateFormat("dd-mm-yyyy")

    override fun toJsonValue(value: Date): Any? =
            dateFormat.format(value)

    override fun fromJsonValue(jsonValue: Any?): Date =
            dateFormat.parse(jsonValue as String)
}

```

#### 3.反射

​	一种在运行时动态地访问对象属性和方法的方式，而不需要实现确定这些属性是什么。

​	只能使用反射访问定义在最外层或者类中的属性，而不能访问函数的局部变量。

```kotlin
		val person = Person("Aclice", 23)
		val kClass = person.javaClass.kotlin
		//获取类名
		print(kClass.simpleName)
		//访问成员属性
		val memberProperty = Person::name
		print(memberProperty.get(person))
		//获取属性的名称
		kClass.memberProperties.forEach { print(it.name) }
		//调用方法
		val kotlinFun =::foo1
		kotlinFun.call(32)

		//访问getter访问,成员变量
		var conter = 0	
		val kProperty = ::conter
		kProperty.setter.call(12)
		print(kProperty.get())
```

#### 4.反射实现对象序列化

默认情况下，将序列化对象的所有属性；基本数据类型和字符串将会被酌情序列化成JSON默认值、布尔值呵呵字符串值；集合将会被序列化成JSON数组；其他类型的属性将会被序列化成嵌套的对象。

#### 5.JSON解析和对象反序列化

​	JKid中JSON反序列化器使用相当普遍的方式实现，由三个主要阶段组成：词法分析器(lexer)、语法分析器或解析器，反序列化组件本身。

​	语法分析把字符组成的输入字符串切分成一个由标记组成的列表。这里有两类标记：代表JSON语法中具有特殊意义的字符(逗号、冒号、花括号和方括号)的字符标记；对应到字符串、数字、布尔值及null常量的指标记。左花括号({)、字符串值("Catch-22")和整数值(42)是不同标记的例子。

​	解析器通常负责将无格式的标记列表转换为结构化的表示法。

