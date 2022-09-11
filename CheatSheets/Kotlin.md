# Kotlin Cheatsheet

##### Table of contents
- [Variables](#variables)
- [Types](#types)
- [Functions](#functions)
- [Collections](#collections)
- [Control Flow](#control-flow) 
- [Classes](#classes)
- [Objects](#objects)
- [Interfaces](#interfaces)
- [Extensions](#extensions)
- [Lambda](#lambda)
- [Generics](#generics)

## Variables

```kotlin
// var is mutable and can be reassigned.
var a = "test"
a = "passed"

// val is not mutable and cannot be reassigned once given a value.
val b = "test"
b = "fail" // Compile time error.

// lateinit states that the variable will be initialized later in the
// program, but reserves the variable name.
lateinit var name: String

// const is a compile time known value, it cannot be changed and
// is static through the duration of the program.
const val AGE: Int = 25

// Delegate lazy variables, they are only instantiated once accessed.
val lazyVal by lazy {
    10
}
```

## Types

### Primitives
Kotlin doesn't have any primitives, instead it has a class wrapping around them and a super type that every class extends by default, the Any class.

### Numerical
| Signed| Unsigned|
| --- | --- |
| Byte  | UByte |
| Short  | UShort |
| Int  | UInt |
| Long  | ULong |

### Floating Point
| Signed | Unsigned |
| --- | --- |
| Float  | UFloat |
| Double  | UDouble |

### Strings

#### String interpolation

```kotlin
val age = 10
val message = "Hello, i'm $age years old"

// Use {} to resolve expressions inside a string interpolation.
val message2 = "Hello, i'm ${age*2} years old"
```

#### Raw Strings
```kotlin
// Raw strings are delimited by """, and you can use newlines
// Here "|" is the default margin prefix, and trimMargin will remove all |
val text = """
    |Tell me and I forget.
    |Teach me and I remember.
    |Involve me and I learn.
    |(Benjamin Franklin)
    """.trimMargin()
```

### Convert Numerical Types

```kotlin
// Numerical
10.toByte()
10.toShort()
10f.toInt()
10.toLong()

// Floating point
10.toFloat()
10.toDouble()
```

## Functions
```kotlin
// All function parameters and returns needs to be typed.
// Normal function with no return.
fun greet(name: String) {
    println("Hello $name")
}

// Function that returns a value.
fun add(a: Int, b: Int): Int {
    return a + b
}

// Function comprehension with inferred return type.
fun add(a: Int, b: Int) = a + b
```

## Collections

### Array
Arrays are fixed size value containers.
```kotlin
val a: Array<Int> = arrayOf(1, 2, 3) // [1, 2, 3]
val b: ByteArray = byteArrayOf(1, 2, 3) // [1, 2, 3]

// Primitive Array
ByteArray()
ShortArray()
IntArray()
LongArray()

// Access by index
a[0] // 1

// Array of int of size 5 with values [0, 0, 0, 0, 0].
val arr = IntArray(5)

// Array of int of size 5 with values [42, 42, 42, 42, 42].
val arr = IntArray(5) { 42 }

// Array of int of size 5 with values [0, 1, 2, 3, 4] (values initialized to their index value)
var arr = IntArray(5) { it * 1 }
```

### ArrayList
ArraysLists have the same functionality as Arrays but they are dynamically resized 

### Map
Hashmaps are a data storage in key-value pairs paradigm.
```kotlin
val a = HashMap<String, Int>()
// Add a new key with a given value.
a.put(key="Test", value=123)
// or
a["Test"] = 1

// Access the value from a key
val testValue = a["Test"]
```

## Control Flow

### If
```kotlin
val a = 5
val b = 10
var max = b

if (a > b) {
    max = a
} else {
    max = b
}

// One line assignment.
var max = if (a > b) a else b
```

### When
```kotlin
val number = 3

when(number) {
    1 -> println("Number 1.")
    2 -> println("Number 2.")
    3 -> println("Number 3.")
    in 5..10 -> println("Between 5 and 10.")
    else -> println("Not one of the above.")
} // after the -> arrow function can be called too

// when assignement.
val result = when(number) { 
    1 -> "one"
    2 -> "two"
    3 -> "three"
    else -> "higher than three"
} // three
```
### For
```kotlin
// Everything that can be iterated over can be used in a for statement.
val message = "Hello"

for (character in message) {
    pritnln(character)
}

// Output:
// H
// e
// l
// l
// o
```

### forEach
```kotlin
// Used on iterable objects.
val numbers = listOf(1, 2, 3)
numbers.forEach { // Access each element in the list with "it".
    println(it)
}

// Iterate with index
numbers.forEachIndexed { index, element ->
    println("$element at position $index")
}
```

## Classes

### Class

```kotlin
class MyClass(var myInt: Int, var myString: String)
// A class doesn't need a body, the primary constructor is defined
// between the parentheses.

val classInstance = MyClass(myInt = 5, myString = "String")

class MyOtherClass {
    // Initializing the variables inside the class body.
        // Getters and setters are automatic and can be overwritten.
    var myInt: Int
    var myString: String
        set(value) {
            field = value.lowercase()
      }
    // a set function receives the value of the field,
    // in this case 'myString', this can parse values automatically
    // without treatment on the constructor.

    // Creating a constructor to get the values from the initialized
    // variables. Only the first constructor is needed.
    constructor(myInt: Int, myString: string) {
        this.myInt = myInt
        this.myString = myString
    }

    constructor() // The class can have as many constructors you want.
}
```

### Extending and implementing
```kotlin
// Extending from other classes or implementing interfaces uses the same syntax.
// You can only extend from one class, and implement as many interfaces you want.
// Use a semicolon to start extending/implementing classes and a colon for each interface you want to implement.
class MyClass() : MyOtherClass(), MyInterface {}
```

### Abstract class

```kotlin
// Abstract classes cannot be directly instantiated and only have methods declaration.
// It's main purpose is to be extended from and its methods be overwritten.

abstract class MyAbstract(var name: String) {
    abstract fun greet(name: String): String
    abstract fun add(n1: Int, n2: Int): Int
}
```

### Open class

```kotlin
// Open classes can be instantiated and extended from.

open class MyOpen(var name: String) {

    // Open methods can be used or overwritten by the class extending an
    // open class.
    open fun greet(name: String): String {
        return "Hello $name!"
    }

    open fun add(n1: Int, n2: Int): Int {
        return n1 + n2
    }
}
```

### Data class

```kotlin
// Data classes are ideal to store data
data class ImageInfo(
    val width: Int,
    val height: Int,
    val creationDate: Long,
    val pixelDensity: Float,
)
```

### Enum class

```kotlin
// Enum classes are used to provide values like constants.
enum class Movement {
    RIGHT, LEFT, UP, DOWN
}

// Enum classes can have values too
enum class Movement(val key: Char) {
    RIGHT('d'), LEFT('a'), UP('w'), DOWN('s')
}
```

### Sealed class

```kotlin
// Sealed classes are like enums, you create only a subset of types you can use.
// In this example the type ApiResponse can only be of two types, Success or Error.
// Like abstract classes, they cannot be instantiated on their own, so
// other values need to extend the sealed class.
sealed class ApiResponse<out T: Any> {
    data class Success<T : Any>(val body: T) : ApiResponse<T>()
    data class Error<T : Any>(val exception: HttpException) : ApiResponse<T>()
}
```

### Anonymous classes
Anonymous classes can inline, extend and instantiate abstract classes.

```kotlin
val detector = object : Detector {
    override fun detect() {}
} 
```

### Static methods
```kotlin
// Static methods and variables can be placed inside the compaion object scope.
// This is most used for constant values that live inside a class namespace, you don't need
// to create an instance of Person to access NAME.
class Person() {
    companion object {
        const val NAME = "alex"
    }
}
```

## Objects
Objects mostly replace the use of static methods that are used in Java.
```kotlin
// Objects are singletons, meaning that only one instance of them exists.
object Constants {
    const val NUMBER = 42
}

// Objects can extends classes and implement interfaces.
object Constants : OtherClass {
    override fun overFun() {}
}
```

## Interfaces

```kotlin
// Interfaces provide templates so classes can override their method
// signatures.

interface Bank {
    fun createAccount(name: String, password: Int)
    fun deposit(amount: Long)
    fun withdraw(amount: Long)
    fun closeAccount()
}
```

## Extensions

Extensions are methods on already defined classes.

```kotlin
// Access the String with 'this'
fun String.sayHello() {
    println("hello ${this}")
}

// calling the extension
val name = "john"
name.sayHello

// Output: hello john
```

## Lambda
Lambdas are anonymous functions that can be passed as arguments to other functions.

```kotlin
// Declaring a lambda function
() -> Unit // Doesn't return anything

// Example on functions
fun myLambda(lambda: () -> Unit) {
    lambda.invoke() // null-safe lambda invocation
}

// Calling myLambda function
myLambda {
    print("Hello")
}

// Multiple parameters
fun sum(lambda: (Int, Int) -> Int) {
    lambda.invoke(5, 5) // null-safe lambda invocation
}

val add = sum { number1, number2 ->
    number1 + number2
} // add will have the value of 10

// Returning a lambda
fun add(n1: Int): (Int) -> Int {
    return {
        n1 + it
    }
}

add(2)(3) // Calling the returned lambda in place
```

## Generics

```kotlin
// Kotlin generics needs to extend the Any type which all types extend from.
class Box<T : Any>(t: T) {
    var value = t
}
```
