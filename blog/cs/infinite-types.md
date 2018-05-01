# Infinite Types in Java and Scala

Infinite Type - это невыразимый тип, последовательными приближениями-уточнениями которого, являются неограниченно растущие записи.

Java Language Spec 9: ["4.10.4. Least Upper Bound"](https://docs.oracle.com/javase/specs/jls/se9/html/jls-4.html#jls-4.10.4)
> It is possible that the lub() function yields an **infinite type**. This is permissible, and a compiler for the Java programming language must recognize such situations and represent them appropriately using cyclic data structures.

> The possibility of an **infinite type** stems from the recursive calls to lub(). Readers familiar with recursive types should note that an **infinite type** is not the same as a recursive type.

# Inexpressible, but not Infinite Types
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
