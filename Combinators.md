# Combinators as Lifestyle

- Value Combinators
  - Data-Value Combinators
    - Fixed-size set (Example: Bool, Ord)
    - yyy
  - Data-Function Combinators (Function, Monad, Applicative, Finagle)
    - Function
    - Parser
    - Monad
- Type Combinators
  - Product/Copoduct
    - AlgDT
    - Finch
  - Subtype-of
  - Be-TypeClass-for

## What is Combinators?
[First](https://wiki.haskell.org/Combinator)
> Combinator - a function or definition with no free variables (a pure lambda-expression that refers only to its arguments).

[Second](https://wiki.haskell.org/Combinator)
> The second meaning of "combinator" is a more informal sense referring to the combinator pattern, a style of organizing 
> libraries centered around the idea of combining things. This is the meaning of "combinator" which is more frequently 
> encountered in the Haskell community. Usually there is some type T, some functions for constructing "primitive" values 
> of type T, and some "combinators" which can combine values of type T in various ways to build up more complex values of type T.

Итак, Combinator - это операция на типе (моносортная) или типах (полисортная).

## Type - fixed-size set
Рассмотрим простейший случай, тип - множество фиксированного размера, операцию можно задать таблично.

### Example #1: Bool
Рассмотрим булевский тип, но для интереса рассмотрим не scala.Boolean, а наш собственный Bool. Добавим в него для интереса неопреленный тип `?`.
Bool = {T, F, ?}

```scala
object Bool extends Enumeration {
  type Bool = Value
  val T, F, ? = Value
}
```

зададим Boolean Algebra - набор из унарного и пары бинарных операций (~, &, |)
```scala
  trait BooleanAlgebra[A] {
    def &(p: A, q: A): A
    def |(p: A, q: A): A
    def ~(p: A): A
  }
```

создадим для Booleaan Algebra - переход к префиксному/инфиксному виду операторов
```scala
  implicit class BoolAlgebraOps[A](p: A)(implicit alg: BooleanAlgebra[A]) {
    def &(q: A): A = alg.&(p, q)
    def |(q: A): A = alg.|(p, q)
    def unary_~(): A = alg.~(p)
  }
```
And: '&'

|   &   | F | T | ? |
|-------|---|---|---|
| **F** | F | F | F |
| **T** | F | T | ? |
| **?** | F | ? | ? |

Or: '|'

|  \|   | T | F | ? |    
|-------|---|---|---|
| **T** | T | T | T |
| **F** | T | T | ? |
| **?** | T | ? | ? |

Not: '~'

|   ~   | T | F | ? |    
|-------|---|---|---|
|       | F | T | ? |

Можем создать BooleanAlgebra для типа Bool. Значения оператора задаем просто таблично.
```scala
  implicit val boolBooleanAlgebra = new BooleanAlgebra[Bool] {
    override def &(p: Bool, q: Bool): Bool = (p, q) match {
      case (T, T) => T
      case (F, _) => F
      case (_, F) => F
      case (_, _) => ?
    }

    override def |(p: Bool, q: Bool): Bool = (p, q) match {
      case (F, F) => F
      case (T, _) => T
      case (_, T) => T
      case (_, _) => ?
    }

    override def ~(p: Bool): Bool = p match {
      case F => T
      case T => F
      case _ => ?
    }
  }
```

Теперь можно писать
```scala
val x = (~F & ?) | T
```

- {Bool, '&'} - моноид, нейтральный елемент 'T'.
- {Bool, '|'} - моноид, нейтральный елемент 'F'.
- {Bool, '&', '|'} - кольцо, ???.

Почему этот тип и его комбинаторы важны? 
Потому, что они позволяют распространить ??? на предикаты - функции из A => Bool

