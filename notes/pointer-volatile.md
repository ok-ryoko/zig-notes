# volatile (Pointer) #

Created on 2024-04-04; last updated on 2024-05-09

If the type of a pointer has the `volatile` qualifier, then loads and stores with that pointer are guaranteed by the compiler to take place and to take place in the same order as in the source code relative to other volatile loads and stores.

The `volatile` qualifier on a pointer type communicates that the data referenced by pointers of that type may change outside the program's control flow. This has the effect of suppressing optimizations by the compiler (and only the compiler).

For example, the type `*volatile T` describes a [single-item pointer](./single-item-pointer.md) to data that may change outside the program's control flow.

Pointers that do not have the `volatile` qualifier coerce to pointer types that do have the `volatile` qualifier. For example, pointers of type `*T` coerce to the type `*volatile T`.

The `@volatileCast` builtin function removes the `volatile` qualifier from a pointer.

Pointers having the `volatile` qualifier should be used for only memory-mapped input/output (MMIO).

There is [discussion][ziglang/zig issue 4284] about introducing two builtin functions named `@volatileLoad` and `@volatileStore` that operate on pointers. These functions may or may not replace the `volatile` pointer attribute.

## Further reading ##

The `volatile` keyword was initially proposed in and implemented to close [ziglang/zig issue 238].

[ziglang/zig issue 238]: https://github.com/ziglang/zig/issues/238
[ziglang/zig issue 4284]: https://github.com/ziglang/zig/issues/4284
