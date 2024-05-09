# const (Pointer) #

Created on 2024-04-03; last updated on 2024-05-09

If the type of a pointer has the `const` qualifier, then that pointer may not be used to mutate the referenced data.

For example, the type `*const T` represents a [single-item pointer](./single-item-pointer.md) allowing loads but not stores. In C, we would call this a *pointer to `const`*. In C++, we would say that the pointer has *low-level `const`*.

If `x` is a `const` variable of type `T`, then the expression `&x` evaluates to a value of type `*const T`.

Pointers that do not have the `const` qualifier coerce to pointer types that do have the `const` qualifier. For example, pointers of type `*T` coerce to the type `*const T`.

The `@constCast` builtin function removes the `const` qualifier from a pointer. It is appropriate to call `@constCast` on a pointer to data that was declared mutable but not data that was declared constant.

## Further reading ##

- [ziggit.dev: Mutable and Constant Pointer Semantics][ziggit-mutable-and-constant-pointer-semantics]
- [ziggit.dev: Using @constCast][ziggit-using-constcast]

[ziggit-mutable-and-constant-pointer-semantics]: https://ziggit.dev/t/3172
[ziggit-using-constcast]: https://ziggit.dev/t/3425
