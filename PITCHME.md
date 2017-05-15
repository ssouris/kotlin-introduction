## Intro to Kotlin

![LOGO](https://kotlinlang.org/assets/images/open-graph/kotlin_250x250.png)

A practical intro to kotlin for java devs.

---

### Why Kotlin?
  - Conciseness & Smarter API
  - Null Safety & Immutability protection
  - Type Inference, Statis Typing & Smart Casts
  - Open programming styles
  - Java Interop

---

### Why Kotlin?
  - Fully open source (built by Jetbrains)
  - Run on Java 6+
  - Static typing & type inference
  - Modern language features
  - 1st grade tooling

---

### More about Kotlin

 - String templates
 - Properties
 - Lamdas
 - Data class
 - Smart cast
 - Null Safety
 - Lazy property
 - Default values for function parameters
 
---
 
 - Extension Functions
 - No semi-colon
 - single expression functions
 - when expression
 - let, apply, use, with
 - collections

---

### Kotlin runtime
<table>
  <tr>
    <th>Library</th>
    <th>Jar Size</th> 
    <th>Dex Size</th>
    <th>Method count</th>
    <th>Field count</th>
  </tr>
  <tr>
    <td>kotlin-runtime-0.10.195</td>
    <td>354 KB</td>
    <td>282 KB</td>
    <td>1071</td>
    <td>391</td>
  </tr>
  <tr>
    <td>kotlin-stdlib-0.10.195</td>
    <td>541 KB</td>
    <td>835 KB</td>
    <td>5508</td>
    <td>458</td>
  </tr>
</table>

---

## Compared to other runtimes and guava :P

<table>
  <tr>
    <th>Library</th>
    <th>Jar Size</th> 
    <th>Dex Size</th>
    <th>Method count</th>
    <th>Field count</th>
  </tr>
  <tr>
    <td>scala-library-2.11.5</td>
    <td>5.3 MB</td>
    <td>4.9 MB</td>
    <td>50801</td>
    <td>5820</td>
  </tr>
  <tr>
    <td>groovy-2.4.0-grooid</td>
    <td>4.5 MB</td>
    <td>4.5 MB</td>
    <td>29636</td>
    <td>8069</td>
  </tr>
</table> 

---

#### val/var

```kotlin
// val = immutable object
val aString = "Lorem ipsum" // no semicolons

// var = mutable obect
var anotherString = aString
```

---

#### type inference

```kotlin
val anInt = 42 // <- compiler detucts type to Int
val someString = "Lorem Ipsum"
```

---

#### function structure

```kotlin
fun add(left: Int, right: Int): Int {
    return left + right
}
```

---

#### named & default args

```kotlin
// note optional return type
fun sayHi(greeting: String = "Howdy",
          name: String = "Honored Guest") {
    println("$greeting, $name!")
}

fun main(args: Array<String>) {
    sayHi()
    sayHi(name = "Shane")
    sayHi(name = "Dave",
            greeting = "Welcome to the conference room")
}
```

---

## If statement as an expression stage 1

```kotlin
fun getServiceResult(condition: Boolean): String {
    val result = null
    if (condition) {
        result = "Service OK"
    } else {
        result = "Service Failure"
    }
    return result
}
```

---

## If statement as an expression stage 2

```kotlin
// or even better:
fun getServiceResult(condition: Boolean): String {
    return if (condition) {
        "Service OK"
    } else {
        "Service Failure"
    }
}
```
---

### If statement as an expression stage 3

```kotlin
// whaaat
fun getServiceResult(condition: Boolean) = 
  if (condition) "Service OK" else "Service Failure"
```

---

### String interpolation

```kotlin
val customer = Customer("Stathis", "Souris")

class MainClass {
    fun main (args: List<String>) {
        println("Name is ${customer.firstName}")
        // or if i want the toString I just omit the properties
        println("Name is $customer")
    }
}
```

---

### Extension methods

```kotlin
fun `extension methods`() {
  // there are also extension methods
  // that were solely missed in Java
  // and why we see a lot of utility classes

  fun Date.isTuesday() : Boolean {
    return day == 2
  }

  val epoch: Date = Date(1970, 0, 0)
  if (epoch.isTuesday()) {
    println("The epoch was a Tuesday.")
  } else {
    println("The epoch was not a Tuesday.")
  }
  // and it end up being a statis method in bytecode
}
```

---

### Higher order functions

```kotlin
fun `Higher order functions`() {
  
  // this actually means a function that accepts a function

  fun <T> List<T>.filter(predicate: (T) -> Boolean): List<T> {
    // ...
    return emptyList()
  }
}
```

---

### Other goodies

```kotlin
// deconstruction
val (name, email, id) = tuple("Stathis", "stathis.souris@stathis.com", 1);

val (firstName, lastName) = Customer(firstName="lol", lastName = "lol1")

val listOfNumers = 1..100
for (numer in listOfNumers) {
  println(numer)
}
    
val countryAndCity = listOf(Pair("Madrid", "Spain"), "Paris" to "France")
for ((city, country) in countryAndCity) {
  ...
}
```

---

### Smart cast

```kotlin
// castings
// by default final cannot inherit from them
open class Person { }
class Employee(val vacationDays: Int): Person() {}
class Contractor: Person()

fun validateVacations(person: Person) {
  if (person is Employee) {
    // casting is inferred by the compiler (smart cast)
    if (person.vacationDays != 20) {
      println("You need to take some more time off!")
    }
  }
}
```

---

### Safe navigation and forcing navigastion even in nullable type

```kotlin
class CustomerServiceInKotlin {
    fun validateCustomer(customer: Customer) {
        if (customer.firstName.startsWith("A")) { .. }
        if (customer.lastName.startsWith("B")) { .. }
    }

    fun validateCustomer1(customer: Customer?) {
        if (customer?.firstName!!.startsWith("A")) { .. }
        if (customer!!.lastName.startsWith("B")) { .. }
    }
    // anything that comes form java comes with null
}
```

---

### Micro DSL 

```kotlin
// micro DSL to my lang
fun using(obj: Closeable, action: () -> Unit) {
  try {
    action()
  } finally {
    obj.close()
  }
}

using(o) {
  // do something with the object
}
```

---

# Type Alias

```kotlin
typealias MapOfStringsToObjects = HashMap<String, Object>
```

---

### When

```kotlin
val validNumbers = arrayOf(100, 42, 5566, 234)

fun tryWhen(val x: Int): Unit {
    when(x) {
        in 1..10 -> print("x == 1")
        in validNumbers -> print("x is valid range")
        !in 10..20 -> print("x is ouside range")
        else -> print("None of the above")
    }
}
```

---

### 

SAM Conversions
Just like Java 8, Kotlin supports SAM conversions. This means that Kotlin function literals can be automatically converted into implementations of Java interfaces with a single non-default method, as long as the parameter types of the interface method match the parameter types of the Kotlin function.
You can use this for creating instances of SAM interfaces:

```kotlin
val runnable = Runnable { println("This runs in a runnable") }
val executor = ThreadPoolExecutor()
// Java signature: void execute(Runnable command)
executor.execute { println("This runs in a thread pool") }
```

---

### Java method overloading

```java
public String foo(String name, int number, boolean toUpperCase) {
    return (toUpperCase ? name.toUpperCase() : name) + number;
}
public String foo(String name, int number) {
    return foo(name, number, false);
}
public String foo(String name, boolean toUpperCase) {
    return foo(name, 42, toUpperCase);
}
public String foo(String name) {
    return foo(name, 42);
}
```

---

### Can be replaced with one function

```kotlin
fun foo(name: String, 
        number: Int = 42, 
        toUpperCase: Boolean = false) =
             (if (toUpperCase) name.toUpperCase() else name) + number
```
```java
fun useFoo() = listOf(
        foo("a"),
        foo("b", number = 1),
        foo("c", toUpperCase = true),
        foo(name = "d", number = 2, toUpperCase = true)
)
```
