
## Id-like Monads
### Id
### TupleN

## Sum-like Monads
### Option
### Try
### Either

## Function-like Monads
### Reader
### Parser
### State

??? cats.effect.IO, SDK.Future, scalaz.effect.IO, scalaz.concurrent.Task
??? cats.Eval, cats.Async

> This is precisely why scala.concurrent.Future is not a suitable type for encapsulating effects in this way: constructing a Future that will eventually side-effect is itself a side-effect! Future evaluates eagerly (sort of, see below) and memoizes its results, meaning that a println inside of a given Future will only evaluate once, even if the Future is sequenced multiple times. This in turn means that val x = Future(...); f(x, x) is not the same program as f(Future(...), Future(...)), which is the very definition of a violation of referential transparency.
> ["An IO monad for cats"](https://typelevel.org/blog/2017/05/02/io-monad-for-cats.html)
