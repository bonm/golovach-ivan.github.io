# Existential Types in Scala

## Java intro
## Java Wildcards
> ... by means of a novel notion of **wildcard capture**, polymorphic methods can be used to give symbolic names 
> to unspecified types, in a manner similar to the **“open” construct** known from existential types.

> The decision to include parametric polymorphism – also known as genericity or generics – 
> in the Java programming language was preceded by a long academic debate. ... It became increasingly clear 
> that the mechanism on its own, imported as it were from a functional context, lacked some of 
> the flexibility associated with object-oriented subtype polymorphism.

> ... an approach by Thorup and Torgersen [30], which we shall refer to as use-site variance, seems
> particularly successful in mediating between the two types of polymorphism without imposing penalties on other parts
> of the language. The approach was later developed, formalized, and proven type sound by Igarashi and Viroli [17]
> within the Featherweight GJ calculus [15].

> The central idea of wildcards is pretty simple. Generics in the Java programming language allow classes like the Java
> platform API class List to be parameterized with different element types, e.g., List<Integer> and List<String>. In GJ there
> is no general way to abstract over such different kinds of
> lists to exploit their common properties, although polymorphic methods may play this role in specific situations. A
> wildcard is a special type argument ‘?’ ranging over all possible specific type arguments, so that **List<?> is the type of
> all lists, regardless of their element type**.

> The motivation behind wildcards is to increase the flexibility of generic types by abstracting over the actual arguments of a parameterized type.



## Scala Existential Types: 'forSome' and 'wildcard'
## Scala: Existential Types \ Type Members duality
## 5
## 6
## Skolemization
## Links
- ["Adding Wildcards to the Java Programming Language"](http://www.gafter.com/~neal/wildcards.pdf)
