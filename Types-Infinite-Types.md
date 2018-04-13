# Infinite Types in Java and scala

JLS 9:
> It is possible that the lub() function yields an infinite type. This is permissible, and a compiler for the Java programming language must recognize such situations and represent them appropriately using cyclic data structures.
> The possibility of an infinite type stems from the recursive calls to lub(). Readers familiar with recursive types should note that an infinite type is not the same as a recursive type.

JLS 9:
> Like lub (§4.10.4), upward projection and downward projection may produce infinite types, due to the recursion on type variable bounds.

## Links
- [JLS 9, "4.10.4. Least Upper Bound"](https://docs.oracle.com/javase/specs/jls/se10/html/jls-4.html#jls-4.10.4)
