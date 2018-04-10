
# Manifest/ClassManifest/ClassTag/TypeTag Confusion

## java.lang.Class<T>
- know some type signatures of class, no run-time representation
- jackson pattern
  
## Scala 2.7.2: Manifest / ClassManifest / OptManifest / NoManifest 

Рекомендуется использовать ClassTag/TypeTag.

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

- Jorge Ortiz, 2008, ["Manifests: Reified Types"](http://archive.li/X4CzM)
- Martin Odersky, 2009, ["Scala 2.8 Array paper"](http://www.scala-lang.org/old/sid/7)
- stackoverflow: ["What's Scala's OptManifest and NoManifest for?"](https://stackoverflow.com/questions/12651542/whats-scalas-optmanifest-and-nomanifest-for)
