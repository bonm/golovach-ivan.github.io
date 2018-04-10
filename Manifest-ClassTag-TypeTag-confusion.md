
# Manifest/ClassManifest/ClassTag/TypeTag Confusion

## java.lang.Class<T>
- know some type signatures of class, no run-time representation
- jackson pattern
  
## Scala 2.7.2: Manifest / ClassManifest / OptManifest / NoManifest 

Механизм Manifest (scala.reflect.{Manifest, ClassManifest, OptManifest, NoManifest}) появился в scala 2.7.2 c основной задачей - через подстановку implicit parameters компилятором обеспечить run-time information для native java array creation (в Java 9 типов массивов = 8 для примитивов + Object). Т.е. для Java важно отличать `new Array[Int]` от `new Array[String]`, но не `Array[Option[Int]]` от `Array[Option[String]]`.

ClassManifest - ослабленная версия Manifest. Старается, по-возможности, заполнить все type parameters. Если не получается (например, в случае с existential types) - использует `object NoManifest` (через механизм OptManifest).
```scala
type ClassManifest[T]  = scala.reflect.ClassTag[T]
...
trait ClassTag[T] extends ClassManifestDeprecatedApis[T] ... {...}
...
trait ClassManifestDeprecatedApis[T] ... {
  def typeArguments: List[OptManifest[_]] = List()
}
```
```scala
trait Manifest[T] extends ClassManifest[T] ... {
  def typeArguments: List[Manifest[_]] = Nil
  ...
}
```

OptManifest = NoManifest | ClassManifest | Manifest (Algebraic Data Type)

Для создания массива необходимо и достаточно ClassManifest/Manifest powerful enough.
```scala
import scala.reflect.Manifest

object Demo extends App {
  // Error: cannot find class tag for element type T
  // def newArray0[T](len: Int) = new Array[T](len)
  def newArray1[T: ClassManifest](len: Int) = new Array[T](len)
  def newArray2[T: Manifest](len: Int) = new Array[T](len)

  //>> [null, null, null]
  println(newArray1[Option[String]](3).mkString("[", ", ", "]"))
  //>> [null, null, null]
  println(newArray2[Option[String]](3).mkString("[", ", ", "]"))
}
```

Однако, например, в случае с existential types сила Manifest **слишком велика**
```scala
import scala.reflect.Manifest

object Demo extends App {
  def newArray1[T: ClassManifest](len: Int) = new Array[T](len)
  def newArray2[T: Manifest](len: Int) = new Array[T](len)

  type O = Option[_] // existential type

  //>> [null, null, null]
  println(newArray1[O](3).mkString("[", ", ", "]"))
  // Compile-time error
  // println(newArray2[O](3).mkString("[", ", ", "]"))
}
```

ClassManifest

> Starting with version 2.7.2, Scala has added manifests, an undocumented (and still experimental) 
> feature for reifying types. They take advantage of a pre-existing Scala feature: implicit parameters.

> The only remaining question is how to implement generic array creation. Unlike
> Java, Scala allows an instance creation new Array[T] where T is a type parameter. How can this be
> implemented, given the fact that there does not exist a uniform array representation in Java? The only
> way to do this is to require additional runtime information which describes the type T. Scala 2.8 has a
> new mechanism for this, which is called a Manifest. **An object of type Manifest[T] provides complete
> information about the type T. Manifest values are typically passed in implicit parameters; and the compiler
> knows how to construct them for statically known types T. There exists also a weaker form named
> ClassManifest which can be constructed from knowing just the top-level class of a type, without necessarily
> knowing all its argument types.** It is this type of runtime information that’s required for array
> creation

### scala.reflect.NoManifest
```scala
package scala.reflect
object NoManifest extends OptManifest[Nothing] with Serializable {
  override def toString = "<?>"
}
```
Do you want to see **NoManifest**? Use existential types for example.
```scala
object Demo App {
  val m = classManifest[Option[_]]
  
  //>> scala.Option[<?>]
  println(m)
  //>> List(<?>)
  println(m.typeArguments)
  //>> true
  println(m.typeArguments(0) eq NoManifest)
}
```

### scala.reflect.Manifest incorrect work
Do you want to see Manifest incorrect work? 
Use variance for example.
```scala
object Demo extends App {
  def isSubType[A: Manifest, B: Manifest] = manifest[A] <:< manifest[B]
  class Foo[K]
  //>> true - wrong! (Foo - invariant not co-variant)
  println(isSubType[Foo[Int], Foo[Any]])
}
```
Or generics and subtyping
```scala
object Demo extends App {
  def isSubType[A: Manifest, B: Manifest] = manifest[A] <:< manifest[B]
  class Parent[A]
  class Child[A] extends Parent[A]
  //>> false - wrong!
  println(isSubType[Child[String], Parent[String]])
}
```

- Jorge Ortiz, 2008, ["Manifests: Reified Types"](http://archive.li/X4CzM)
- Martin Odersky, 2009, ["Scala 2.8 Array paper"](http://www.scala-lang.org/old/sid/7)
- stackoverflow: ["What's Scala's OptManifest and NoManifest for?"](https://stackoverflow.com/questions/12651542/whats-scalas-optmanifest-and-nomanifest-for)
