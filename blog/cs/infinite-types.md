# Infinite Types in Java and Scala

Infinite Type - это невыразимый тип, последовательными приближениями-уточнениями которого, являются неограниченно растущие записи.

Java Language Spec 9: ["4.10.4. Least Upper Bound"](https://docs.oracle.com/javase/specs/jls/se9/html/jls-4.html#jls-4.10.4)
> It is possible that the lub() function yields an **infinite type**. This is permissible, and a compiler for the Java programming language must recognize such situations and represent them appropriately using cyclic data structures.

> The possibility of an **infinite type** stems from the recursive calls to lub(). Readers familiar with recursive types should note that an **infinite type** is not the same as a recursive type.

## Example #1
```java
class A<T> {}
class B extends A<B> {}
class C extends A<C> {}
```
```java
<T> T choose(T x, T y) {
  return x;
}
```
```java
  B b = new B();
  C c = new C();
  A<?> approx0 = choose(b, c);
  A<? extends A<?>> approx1 = choose(b, c);
  A<? extends A<? extends A<?>>> approx2 = choose(b, c);
  A<? extends A<? extends A<? extends A<?>>>> approx3 = choose(b, c);
  A<? extends A<? extends A<? extends A<? extends A<?>>>>> approx4 = choose(b, c);
  ...
```

## Inexpressible, but not Infinite Types
### Structural Type example
Следующий код компилируется и запускается в Java.
```java
(new Object() {void run() {}}).run();
```

Однако невозможно в текущей системе типов Java выразить тип выражения
```java
new Object() {void run() {}}
```

В скала это возможно с ипользованием Structural Types
```scala
(new {def run(): Unit = {}}).run()
```
```slala
val x: {def run(): Unit} = new {def run(): Unit = {}}
x.run()
```
### Null Type example
Также в Java невозможно точно выразить тип литерала 'null'.

JLS 9: ["3.10.7. The Null Literal"](https://docs.oracle.com/javase/specs/jls/se9/html/jls-3.html#jls-3.10.7)
> The **null type** has one value, the null reference, represented by the *null literal* ```null```, which is formed from ASCII characters.

> A null literal is always of the null type.

JLS 9: ["4.1. The Kinds of Types and Values"](https://docs.oracle.com/javase/specs/jls/se9/html/jls-4.html#jls-4.1)
> There is also a special *null* type, the type of the expression ```null```, which has no name.

> Because the null type has no name, it is impossible to declare a variable of the null type or to cast to the null type.

> The null reference is the only possible value of an expression of null type.

JLS 9: ["15.8.1. Lexical Literals"](https://docs.oracle.com/javase/specs/jls/se9/html/jls-15.html#jls-15.8.1)
> The type of the null literal ```null``` is the null type; its value is the null reference.

В Scala это возможно
```scala
val x: Null = null
```

## Links
- Java Bug Database["JDK-4993221 : Produce infinite types from lub as required by JLS"](https://bugs.java.com/bugdatabase/view_bug.do?bug_id=4993221)
- 
