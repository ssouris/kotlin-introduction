## A Practical intro to Kotlin

![LOGO](https://kotlinlang.org/assets/images/open-graph/kotlin_250x250.png)

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

### The Java Platform is...

- ...a mature and extensive hardware independent platform.
- ...evolving very, very slowly
- ...a platform for many programming languages:
- Java, Scala, Kotlin, Ceylon, Frege, Groovy, Fantom, Gosu, ...
- Groovy, Clojure, JRuby, Jython, Golo

attributes to Russel Winder talk on DevoxUK

---
 
 - Extension Functions
 - No semi-colon
 - single expression functions
 - when expression
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

## Compared to other runtimes

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
var mutableString = "Lorem ipsum"
```

---

#### type inference

```kotlin
val anInt = 42 // <- compiler infers type to Int
val someString = "Lorem Ipsum"
```

---

#### functions

```kotlin
fun add(left: Int, right: Int): Int {
    return left + right
}
```

---

#### named & default args

```kotlin
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

## If statement as an expression 1/3

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

## If statement as an expression 2/3

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

### If statement as an expression 3/3

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

  fun Date.isTuesday() : Boolean {
    return day == 2
  }

  val epoch: Date = Date(1970, 0, 0)
  if (epoch.isTuesday()) {
    println("The epoch was a Tuesday.")
  } else {
    println("The epoch was not a Tuesday.")
  }
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
// decomposing parameters
val (name, email, id) = Tuple("Stathis", "stathis.souris@stathis.com", 1);

val (firstName, lastName) = Customer(firstName="lol", lastName = "lol1")

val countryAndCity = listOf(Pair("Madrid", "Spain"), "Paris" to "France")
for ((city, country) in countryAndCity) {
  ...
}

// ranges
val listOfNumers = 1..100
for (numer in listOfNumers) {
  println(numer)
}

```

---

### Smart cast

```kotlin
// classes by default final, add `open` to inherit from
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

### Safe navigation

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

SAM Conversions:

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
             
fun useFoo() = listOf(
        foo("a"),
        foo("b", number = 1),
        foo("c", toUpperCase = true),
        foo(name = "d", number = 2, toUpperCase = true))
```

---

### Null checks in Java

```java
public void sendMessageToClient(
    @Nullable Client client,
    @Nullable String message) {
    if (client == null || message == null) return;

    PersonalInfo personalInfo = client.getPersonalInfo();
    if (personalInfo == null) return;

    String email = personalInfo.getEmail();
    if (email == null) return;

    sendMessage(email, message);
}
```

---

###

```kotlin
fun sendMessageToClient(
        client: Client?, message: String?) {
    sendMessage(client?.personalInfo?.email ?: return, message ?: return );
}

class Client (val personalInfo: PersonalInfo?)

class PersonalInfo (val email: String?)

```

---

### Delegation in Kotlin

```kotlin
class CopyPrinter(copier: Copy, printer: Print) : Copy by copier, Print by printer

interface Copy {
    fun copy(page: Page): Page
}

interface Print {
    fun print(page: Page)
}
```

---

### Java decompiled code

```java
public final class CopyPrinter implements Copy, Print {
   private final Copy $$delegate_0;// $FF: synthetic field
   private final Print $$delegate_1;// $FF: synthetic field

   public CopyPrinter(@NotNull Copy copier, @NotNull Print printer) {
      Intrinsics.checkParameterIsNotNull(copier, "copier");
      Intrinsics.checkParameterIsNotNull(printer, "printer");
      super();
      this.$$delegate_0 = copier;
      this.$$delegate_1 = printer;
   }

   @NotNull
   public Page copy(@NotNull Page page) {
      Intrinsics.checkParameterIsNotNull(page, "page");
      return this.$$delegate_0.copy(page);
   }

   public void print(@NotNull Page page) {
      Intrinsics.checkParameterIsNotNull(page, "page");
      this.$$delegate_1.print(page);
   }
}
....
```
--- 

### Sealed classes

Defining restricted class hierarchies

```kotlin
sealed class Expr {
  class Num(val value: Int): Expr()
  class Sum(val left: Expr, val right: Expr): Expr()
}
fun eval(e: Expr): Int =
  when (e) {
    is Expr.Num -> e.value
    is Expr.Sum -> eval(e.right) + eval(e.left)
  }
```

---

### Don't do this :)

```kotlin
// method names can be free text
infix fun CharSequence.`are you ok man`(arg1 : String) 
        = println("Yeap")

fun main(args: Array<String>) {
    // with infix methods can skip the '(', ')'
    "Stathis Souris " `are you ok man` "?"
}
```
---

Q/A
