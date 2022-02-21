# Kotlin

## 基础

### 基本类型

参考 ：https://www.kotlincn.net/docs/reference/basic-types.html

### 控制流程

参考：https://www.kotlincn.net/docs/reference/control-flow.html



## 类与对象

### 修饰符

参考：https://www.kotlincn.net/docs/reference/visibility-modifiers.html

如果没有显式指定修饰符的话，默认可见性是 `public`。

- `private` 意味着只在这个类内部（包含其所有成员）可见。

- `protected`—— 和 `private`一样 + 在子类中可见。
- `internal` —— 能见到类声明的 *本模块内* 的任何客户端都可见其 `internal` 成员。
- `public` —— 能见到类声明的任何客户端都可见其 `public` 成员。



### 类（Class）

类声明由类名、类头（指定其类型参数、主构造函数等）以及由花括号包围的类体构成。类头与类体都是可选的； 如果一个类没有类体，可以省略花括号。

```kotlin
calss Person{/*...*/}
```



### 构造函数

在 Kotlin 中的一个类可以有一个**主构造函数**以及一个或多个**次构造函数**。主构造函数是类头的一部分：它跟在类名（与可选的类型参数）后。

```kotlin
//如果主构造函数没有任何注解或者可见性修饰符，可以省略这个 constructor 关键字。
class Person constructor(name: String) { /*……*/ }
class Person(name: String) { /*……*/ }


class Person constructor(name: String) { 
    //主构造的参数可以在初始化块中使用。它们也可以在类体内声明的属性初始化器中使用
    val name = name;
    
    //主构造函数不能包含任何的代码。初始化的代码可以放到以 init 关键字作为前缀的初始化块（initializer blocks）中。
    init {
        println("First initializer block that prints ${name}")
    }
}

//声明属性以及从主构造函数初始化属性，Kotlin 有简洁的语法
class Person(val name: String,  var age: Int) { /*……*/ }

//如果构造函数有注解或可见性修饰符，这个 constructor 关键字是必需的，并且这些修饰符在它前面
class Customer public @Inject constructor(name: String) { /*……*/ }
```



### 属性

Kotlin 类中的属性既可以用关键字 *var* 声明为可变的，也可以用关键字 *val* 声明为只读的。

```kotlin
class Person{
    var name : String= "name" //不能为空
    var name: String? = null  //可以为空
}
```



### 继承

在 Kotlin 中所有类都有一个共同的超类 `Any`，这对于没有超类型声明的类是默认超类。`Any` 有三个方法：`equals()`、 `hashCode()` 与 `toString()`，所有 Kotlin 类都定义了这些方法。

```kotlin
//默认情况下，Kotlin 类是最终（final）的：它们不能被继承。 要使一个类可继承，请用 open 关键字标记它。
open class Base{}
//如需声明一个显式的超类型，把超类型放到冒号之后
class Derived(p: Int) : Base()
```



### 抽象类

类以及其中的某些成员可以声明为 *abstract*。 抽象成员在本类中可以不用实现。 需要注意的是，不需要用 `open` 标注一个抽象类或者函数。

```kotlin
//可以用一个抽象成员覆盖一个非抽象的开放成员
open class Polygon {
    open fun draw() {}
}

abstract class Rectangle : Polygon() {
    abstract override fun draw()
}
```



### 接口

参考：https://www.kotlincn.net/docs/reference/interfaces.html

Kotlin 的接口可以既包含抽象方法的声明也包含实现。与抽象类不同的是，接口无法保存状态。它可以有属性但必须声明为抽象或提供访问器实现。

```kotlin
//使用关键字 interface 来定义接口
interface MyInterface {
    fun bar()
    fun foo() {
      // 可选的方法体
    }
}

//一个类或者对象可以实现一个或多个接口。
class Child : MyInterface {
    override fun bar() {
        // 方法体
    }
}
```



### 扩展函数

Kotlin 能够扩展一个类的新功能而无需继承该类或者使用像装饰者这样的设计模式。 这通过叫做 *扩展* 的特殊声明完成。 例如，你可以为一个你不能修改的、来自第三方库中的类编写一个新的函数。 这个新增的函数就像那个原始类本来就有的函数一样，可以用普通的方法调用。 这种机制称为 *扩展函数* 。此外，也有 *扩展属性* ， 允许你为一个已经存在的类添加新的属性。

*声明一个扩展函数，我们需要用一个 接收者类型 也就是被扩展的类型来作为他的前缀。*

```kotlin
open class Demo 

fun main(){
    fun Demo.getValue() = "扩展函数"
    val value = Demo().getValue()
    println(value)
}
```

*扩展函数是由函数调用所在的表达式的类型来决定的， 而不是由表达式运行时求值结果决定的。*

```kotlin
open class Shape

class Rectangle: Shape()

fun Shape.getName() = "Shape"

fun Rectangle.getName() = "Rectangle"

fun printClassName(s: Shape) {
    println(s.getName())
}    

printClassName(Rectangle()) //打印结果为Shape
```

参考：https://www.kotlincn.net/docs/reference/extensions.html



### 数据类

我们经常创建一些只保存数据的类。 在这些类中，一些标准函数往往是从数据机械推导而来的。在 Kotlin 中，这叫做 *数据类* 并标记为 `data`

```kotlin
data class User(val name: String, val age: Int)
```

参考：https://www.kotlincn.net/docs/reference/data-classes.html



### 密封类

密封类用来表示受限的类继承结构：当一个值为有限几种的类型、而不能有任何其他类型时。在某种意义上，他们是枚举类的扩展：枚举类型的值集合也是受限的，但每个枚举常量只存在一个实例，而密封类的一个子类可以有可包含状态的多个实例。

*要声明一个密封类，需要在类名前面添加 `sealed` 修饰符。虽然密封类也可以有子类，但是所有子类都必须在与密封类自身相同的文件中声明*

```kotlin
sealed class Expr
data class Const(val number: Double) : Expr()
data class Sum(val e1: Expr, val e2: Expr) : Expr()
object NotANumber : Expr()
```

参考：https://www.kotlincn.net/docs/reference/sealed-classes.html



### 枚举类

```kotlin
//每个枚举常量都是一个对象，枚举常量用逗号分隔。
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}

//初始化（因为每一个枚举都是枚举类的实例，所以他们可以是这样初始化过的）
enum class Color(val rgb: Int) {
        RED(0xFF0000),
        GREEN(0x00FF00),
        BLUE(0x0000FF)
}

//匿名类（枚举常量还可以声明其带有相应方法以及覆盖了基类方法的匿名类）
enum class ProtocolState {
    abstract fun signal(): ProtocolState

    WAITING {
        override fun signal() = TALKING
    },

    TALKING {
        override fun signal() = WAITING
    };
}

```

参考：https://www.kotlincn.net/docs/reference/enum-classes.html



### 嵌套类与内部类

```kotlin
//类可以嵌套在其他类中
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}

val demo = Outer.Nested().foo() // == 2

//内部类：标记为 inner 的嵌套类能够访问其外部类的成员。内部类会带有一个对外部类的对象的引用
class Outer {
    private val bar: Int = 1
    inner class Inner {
        fun foo() = bar
    }
}

val demo = Outer().Inner().foo() // == 1

```

参考：https://www.kotlincn.net/docs/reference/nested-classes.html



### 泛型



## 集合

参考：https://www.kotlincn.net/docs/reference/collections-overview.html



