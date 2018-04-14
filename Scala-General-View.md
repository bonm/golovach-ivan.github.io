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
