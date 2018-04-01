# Pimp-my-library Pattern

Problem with pimp - implicit resolution order!

### History / Definition
* Martin Odersky, 2006, ["Pimp my Library"](https://www.artima.com/weblogs/viewpost.jsp?thread=179766), Artima

### Examples
```scala
// scala.runtime.RichInt
for (k <- 1 to 10) println(k)
```
### Links
* J. Suereth, 2012, **"Implicits without import tax"**, NEScala 2011, ([video](http://vimeo.com/20308847), [slides](https://docs.google.com/present/view?id=dfqn4jb_106hq4mvbd8)) 
* Eugene Yokota, **"revisiting implicits without import tax"** ([blog post](http://eed3si9n.com/revisiting-implicits-without-import-tax))
* Travis Brown, 2016, **"Generic Derivation: The Hard Parts"**, Scala World, ([video](https://www.youtube.com/watch?v=80h3hZidSeE))
* Miles Sabin, **Export-hook** [GihHub project](https://github.com/milessabin/export-hook), minimal infrastructure for type class providers to support the inclusion of derived, subclass and other orphan instances in their implicit scope.

