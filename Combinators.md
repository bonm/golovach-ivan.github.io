# Combinators as Lifestyle

## Brief Overview
**Generators**
```scala
  val subsetA = 𝔸
  val subsetB = ∘(100)
  val subsetC = 10 |-| 20
  val subsetD = 50 <-> 60  
```
**Combinators**
```scala
  val subset = (subsetA ∪ subsetB) ∩ ~subsetC
```

## Value and Type levels
## Generators and Combinators

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

- Value-level, One Type - finite fixed-size set
    - Example #1: Bool
    - Part I: Boolean Combinators
    - Part II: Predicate Combinators

Keywords: Boolean Algebra, Monoid, Functional Type, Product Type, 

===
#### Type Generators
```scala
  type T = Bool
  type T[A] = A → Bool
  type T[A, B] = A :+: B
  
  ∀[A].(A :: HNil) ⊂ HList
  ∀[A].∀[B ⊂ HList].(A :: B) ⊂ HList
  
  ∀[A].(A :+: CNil) ⊂ Coproduct
  ∀[A].∀[B ⊂ Coproduct].(A :: B) ⊂ Coproduct
```
#### Type Combinators
#### Value Generators
#### Value Combinators
===

|                              | Type Generators | Type Combinators | Value generators | Value Combinators |   
|------------------------------|-----------------|------------------|------------------|-------------------|
| **Boolean Algebra**          | ∅ → A: Bool     |         -        | ∅ → A: {T,F,?}   |                   |
| **Predicate Combinators**    |                 |                  |                  |                   |
| **Line sub-set Combinators** |                 |                  |                  |                   |
| **Product/Coproduct**        |                 |                  |                  |                   |

В этой статье рассмотрим
1) комбинаторы булевой алгебры
```scala
// Types
//     type A = Bool
// Generators: 
//    ∅ → A: {'T', 'F', '?'}
// Combinators: 
//     A → A: {'~'}
//     A ⨯ A → A: {'&', '|'}
val x = (~F & ?) | T
val y = (~x | ~(T & ?))
val z = (x | y | ?) & x & y
```
2) комбинаторы предикатов
```scala
// Types
//     type V[A] = A → Bool
// Generators: 
//     ...
// Combinators: 
//     V[A] ⨯ V[A] → V[A]: {'∪', '∩', '\'}
???
```
3) комбинаторы подмножеств прямой
```scala
// Types
//     type A = Int → Bool
// Generators: 
//     Int → A: {'∘'}
//     Int ⨯ Int → A: {'|-|', '<->', '<-|', '|->'}
// Combinators: 
//     A ⨯ A → A: {'∪', '∩', '\'}
val subset = (10 |-| 20) ∪ (50 <-> 60) \ ∘(55)
```
4) комбинаторы валидаторов
```scala
// Type constructors
//     type V[X] = X → Bool
// Generators
//     ...
// Combinators
//     ∀A, V[A] → V[A]: {'~'}
//     ∀A, V[A] ⨯ V[A] → V[A]: {'&', '|'}
val checkAddress: Address => Bool =
  (ω[Address](_ street) *> checkStreet) &
  (ω[Address](_ house)  *> positive) &
  (ω[Address](_ flat)   *> positive)

val checkUser: User => Bool =
  (ω[User](_ name)    *> checkName) &
  (ω[User](_ age)     *> positive) &
  (ω[User](_ address) *> checkAddress)
```
5) Расширить Logic Quantifiers as Combinators
```scala
// Type constructors
//     type V[X] = X → Bool
// Generators
//     ...
// Combinators
//     ∀A, V[A]* → V[A]: {'∀', '∃'}
```

## Type - finite fixed-size set
Рассмотрим простейший случай, тип - конечное множество фиксированного размера. В таком случае n-арные операции можно задать таблично.

### Example #1: Bool
#### Part I: Boolean Combinators
Рассмотрим логический тип c добавленным неопреленным значением `?`.
```scala
// Bool = T | F | ?
object Bool extends Enumeration {
  type Bool = Value
  val T, F, ? = Value
}
```

Ожидаемым набором операций для этого типа являются операции булевой алгебры ('~', '&', '|').
```scala
// Boolean Algebra Type Class
trait BooleanAlgebra[A] {
  def &(p: A, q: A): A
  def |(p: A, q: A): A
  def ~(p: A): A
}
```

Их табличные определения

|   ~   | T | F | ? |    
|-------|---|---|---|
|       | F | T | ? |

|   &   | F | T | ? |
|-------|---|---|---|
| **F** | F | F | F |
| **T** | F | T | ? |
| **?** | F | ? | ? |

|  \|   | T | F | ? |    
|-------|---|---|---|
| **T** | T | T | T |
| **F** | T | T | ? |
| **?** | T | ? | ? |

естественно переносятся в код
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

Мы имеем 0-арные генераторы констант {T, F, ?}: Bool и тройку комбинаторов ({~\_}: Bool => Bool, {\_&\_, \_|\_}: (Bool, Bool) => Bool)
Теперь можно писать
```scala
val x = (~F & ?) | T
val y = (~x | ~(T & ?))
val z = (x | y | ?) & x & y
```

#### Part II: Predicate Combinators

Произвольные унарный(ψ_) и бинарный(_⊙_) операторы для типа A можно для произвольного типа B естественным образом распространить на функциональный тип B => A.

binary ⊙: A ⊙ A → A

∀B. (B => A) ⊙ (B => A) → (B => A)

(p: B => A) ⊙ (q: B => A) = b => p(b) ⊙ q(b)
 
unary ψ: ψ(A)→ A

∀B. ψ(B => A) → (B => A)

ψ(p: B => A) = b => ψ(p(b))
 
Для произвольного типа B, тип B => Bool имеет естественную семантику валидатора екземпляра типа B.
Таким образом мы получаем комбинаторы валидаторов.

Добавим неявное техническое преобразование Boolean -> Bool
```scala
implicit def boolean2bool(b: Boolean): Bool = if (b) T else F
```

И определим BooleanAlgebra для ∀A, ∀B:BooleanAlgebra  => (A => B):BooleanAlgebra
```scala
  implicit def funcBooleanAlgebra[A, B: BooleanAlgebra] = new BooleanAlgebra[A => B] {
    override def &(p: A => B, q: A => B): A => B = a => p(a) & q(a)
    override def |(p: A => B, q: A => B): A => B = a => p(a) | q(a)
    override def ~(p: A => B): A => B = a => ~p(a)
  }
  ```

Определив небольшую библиотеку сомбинаторов для интервалов
```scala
  import Ordered._
  def gte[A: Ordering](min: A): A => Bool = _ >= min
  def lte[A: Ordering](max: A): A => Bool = _ <= max
  ```

Можем целых множество точек на линии \[10, 11 .. 19, 20\] ∪ \[50, 51 .. 59, 60\] задать как
```scala
val in10to20or50to60 = (gte(10) & lte(20)) | (gte(50) & lte(60))
```

Определив более интересные имена для комбинаторов, сможем говорить про комбинаторы полупрямых 
```scala
  implicit class X[A: Ordering](min: A) {
    def |->(): A => Bool = _ >= min
  }
  def <-|[A: Ordering](max: A): A => Bool = _ <= max
```

Можем писать
```scala
val in10to20or50to60 = (10.|-> & <-|(20)) | (50.|-> & <-|(60))
```

Можем перейти к алгебре подмножеств.

Генераторы
```scala
implicit class IntervalOps[A: Ordering](min: A) {
  /** Interval: (min ... max) */
  def <->	(max: A) = gt(min) & lt(max)
  /** Interval: [min ... max) */
  def |->	(max: A) = gte(min) & lt(max)
  /** Interval: (min ... max] */
  def <-|	(max: A) = gt(min) & lte(max)
  /** Interval: [min ... max] */
  def |-|	(max: A) = gte(min) & lte(max)
}
```
  
И комбинаторы
```scala
  implicit class LineSubsetOps[A: BooleanAlgebra](p: A) {
  def ∩(q: A): A = implicitly[BooleanAlgebra[A]].&(p, q)
  def ∪(q: A): A = ~(~p ∩ ~q)
  def \(q: A): A = p ∩ ~q
}
```
 
Теперь ту же пару отрезков можно записать как
```scala
val in10to20or50to6a = (10 |-| 20) ∪ (50 |-| 60)
```
или как
```scala
val in10to20or50to6b = (10 |-| 60) \ (20 <-> 50)
```

Добавив генератор точки
```scala
def ∘(point: Int) = point |-| point
```

можем писать так
```scala
val in10to20or50to6 = (10 |-| 20) ∪ (50 |-| 61) \ ∘(61)
```

#### Bisiness Model Validators Combinators
пусть у нас есть модель, заданная в виде иерархии Algebraic data Types
```scala
case class Address(street: String, house: Int, flat: Int)
case class User(name: String, age: Int, address: Address)
```

И валидаторы
```scala
val checkName: String => Bool = _.length > 3
val checkStreet: String => Bool = _.length > 3
val positive: Int => Bool = gte(0)
```

Используя Field Extractor Operator 'ω'
```scala
class ω[A] {
  def apply[B](f: A => B) = new {
    def *>[C](p: B => C): (A => C) = a => p(f(a))
  }
}
object ω {
  def apply[A]: ω[A] = new ω[A]
}
  ```

Сможем декларативно строить сложные валидаторы с использованием композиции
```scala
val checkAddress: Address => Bool =
  (ω[Address](_ street) *> checkStreet) &
  (ω[Address](_ house)  *> positive) &
  (ω[Address](_ flat)   *> positive)

val checkUser: User => Bool =
  (ω[User](_ name)    *> checkName) &
  (ω[User](_ age)     *> positive) &
  (ω[User](_ address) *> checkAddress)
```

Или монолитом
```scala
val checkUserX: User => Bool =
  (ω[User](_ name) *> checkName) &
  (ω[User](_ age)  *> positive) &
  (ω[User](_.address.street) *> checkStreet) &  
  (ω[User](_.address.house)  *> positive) &  
  (ω[User](_.address.flat)   *> positive)
```

#### Logic Quantifiers as Combinators
Обратим внимание, что даже с добавленным знаком '?'
{Bool, '&'} - это моноид с нейтральным елементом 'T'
{Bool, '|'} - это моноид с нейтральным елементом 'F'

Это позволяет нам строить сверточные комбинаторы 
```scala
  // '∀' - allOf
  def ∀[A](xs: (A => Bool)*): A => Bool =
    (xs :\ ((_: A) => T: Bool)) (_ & _)
  // '∃' - anyOf
  def ∃[A](xs: (A => Bool)*): A => Bool =
    (xs :\ ((_: A) => F: Bool)) (_ | _)
```

и записывать
```scala
val allOf = ∀((_: Int) > 0, (_: Int) < 10, (_: Int) % 2 == 0)
val anyOf = ∃((_: Int) > 0, (_: Int) < 10, (_: Int) % 2 == 0)
```

