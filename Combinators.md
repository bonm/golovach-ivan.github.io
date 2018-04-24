# Combinators as Lifestyle

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
Рассмотрим простейший случай, тип - множество фиксированного размера (т.е. нет констант/образующих/генераторов и правил композиции), операцию можно задать таблично.

### Example #1: Bool
Bool = {T, F}

|   &   | F | T |    
|-------|---|---|
| **F** | F | F |
| **T** | F | T |

|  \|   | F | T |    
|-------|---|---|
| **F** | F | T |
| **T** | T | T |

- {Bool, '&'} - моноид, нейтральный елемент 'T'.
- {Bool, '|'} - моноид, нейтральный елемент 'F'.
- {Bool, '&', '|'} - кольцо, ???.

Почему этот тип и его комбинаторы важны? 
Потому, что они позволяют распространить ??? на предикаты - функции из A => Bool

### Example #2: Ord: {LT, EQ, GT}
|   ~>   | LT | EQ | GT |    
|--------|----|----|----|
| **LT** | LT | LT | LT |
| **EQ** | LT | EQ | GT |
| **GT** | GT | GT | GT |

|   ~>  | < | = | > |    
|-------|---|---|---|
| **<** | < | < | < |
| **=** | < | = | > |
| **>** | > | > | > |
