
> Scalameta based syntactic and semantic APIs that cross-compile against Scala 2.12, 2.13 and Dotty. The corresponding library is quite slim, being less than 500Kb in size.

> A prototype implementation of the new macro engine for Scala 2.12.3 that supports **macro annotations** and **def macros**.

> We describe two approaches to the portability problem based on two different views on macros: 
> 1. the **tree-based approach**, which views macros as operations on **abstract syntax trees**, solves the problem by defining standard abstract syntax trees; 
> 2. the **syntax-based approach**, which views macros as operations on **abstract syntax**, solves the problem by defining standard abstract syntax.

> With significant benefits over the treebased approach, the syntax-based approach has been adopted in the new Scala macro system.

> Typical operations on ASTs are 
> 1. compose new trees; and 
> 2. inspect structures of trees.

+ concrete syntax tree (parse tree, parsing tree, derivation tree)
+ Abstract syntax
  + first-order abstract syntax (FOAS)
  +  higher-order abstract syntax (HOAS)

## V2: def macro, macro annotations

> **Annotation macros** are expanded before type checking, thus the trees provided to annotation macros are devoid of type information. 
> Sometimes, we say annotation macros are syntactical, because they can only use syntactical information of trees to transform user code

> **Def macros** expand after the arguments are type checked, thus they can benefit from the type information of trees to transform user code.

> Def macros replace well-typed terms with other well-typed terms.
> Generated code can contain arbitrary Scala constructs.
> Code generation can involve arbitrary computations.
> Local expansion of method calls.
> Well-formed and well-typed arguments.

> ... the hard to comprehend notion of metaprogramming expressed in def macros piggybacks on the familiar concept of a typed method call.

## Crazy ideas for future
Odersky, ["Principled Meta Programming for Scala"](https://gist.github.com/odersky/f91362f6d9c58cc1db53f3f443311140)

## Links
- Burmako, Shabalin, June 2014, "Easy Metaprogramming For Everyone!" [PDF](http://scalamacros.org/paperstalks/2014-06-17-EasyMetaprogrammingForEveryone.pdf), [VIDEO](https://www.youtube.com/watch?v=twokmzbDzqA)
- Burmako, June 2016, "Metaprogramming 2.0" [PDF](http://scalamacros.org/paperstalks/2016-06-17-Metaprogramming20.pdf), [VIDEO](https://www.youtube.com/watch?v=IPnd_SZJ1nM)
- Fengyun Liu, Eugene Burmako, 2017, ["Two Approaches to Portable Macros"](https://www.dropbox.com/s/2xzcczr3q77veg1/gestalt.pdf)
