
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


- Burmako, Shabalin, June 2014, "Easy Metaprogramming For Everyone!" [PDF](http://scalamacros.org/paperstalks/2014-06-17-EasyMetaprogrammingForEveryone.pdf), [VIDEO](https://www.youtube.com/watch?v=twokmzbDzqA)
- Burmako, June 2016, "Metaprogramming 2.0" [PDF](http://scalamacros.org/paperstalks/2016-06-17-Metaprogramming20.pdf), [VIDEO](https://www.youtube.com/watch?v=IPnd_SZJ1nM)
- Fengyun Liu, Eugene Burmako, 2017, ["Two Approaches to Portable Macros"](https://www.dropbox.com/s/2xzcczr3q77veg1/gestalt.pdf)
