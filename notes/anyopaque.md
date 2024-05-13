# anyopaque #

Created on 2024-03-12; last updated on 2024-05-13

The type `anyopaque` is used to describe [type-erased pointers](./type-erased-pointers.md) and interoperate with C's `void` type.

The type `anyopaque` has an [alignment](./alignment.md) of 1 byte.

It is an error to use `anyopaque` for function parameter and return types, although it is possible to declare a constant and uninitialized variable of type `anyopaque`.
