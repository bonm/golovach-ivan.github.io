# Scala General View

## Soundness

## Syntax

## Compilers
- scalac for 1.x 
  - written in java-with-patterns/pizza
- nsc compiler for 2.0-2.10
  - some use of functional capabilities of Scala
- scalac for 2.11 (ncs-based)
  - REPL
  - presentation compiler for IDE (Eclipse, Ensime)
  - run-time metaprogramming with toolboxes

## Dotty Architecture
> Dotty treats positions a bit differently from scalac. The architecture is as follows:
> 1. There are untyped and typed trees. Both kinds of trees are immutable. The job of the typer is to map one to the other.
> 2. Untyped trees are very close to source and have precise range positions. For instance, for-expression generators and filters are expressed as particular untyped trees.
> 3. Typed trees are a desugared subset of untyped trees. An untyped tree may map to several typed trees (example: an untyped case class def maps into a class def and a module def of the companion object).
> 4. Typed trees also have range positions which are copied from the positions of the untyped trees from which they are generated.
> 5. There is a navigation API which works with the positions in order to:
>   - map a typed tree to the untyped tree from which it is derived,
>   - map an untyped tree to the set of typed trees that derive from it.

## Dictionary
> The Scala IDE for Eclipse uses the Scala Presentation Compiler, a faster asynchronous version of the Scala Compiler. The presentation 
> compiler only runs the phases up until and including the typer phase, that is, the first 4 of the 27 scala compilation phases. The IDE 
> uses the presentation compiler to provide semantic features such as live error markers, inferred type hovers, and semantic highlighting. 

> The presentation compiler was introduced so that external tools could get access to the compiler's internal model.

> The source for the presentation compiler can be found in the Scala repository in the package scala.tools.nsc.interactive. The most 
> important file for this discussion is Global.scala. If you open Global.scala, you'll see that it extends the standard Scala compiler, 
> scala.tools.nsc.Global, and mixes in several helpers.

## Compiler tricks
> You can get a list of all the phases of the compiler by passing the **-Yshow-phases** option to scalac.

## Links
- Odersky, 2015, ["JVMLS 2015 - Compilers are Databases"](https://www.youtube.com/watch?v=WxyyJyB_Ssc)
- [Dotty Presentation Compiler discussion](https://github.com/lampepfl/dotty/issues/1523)
