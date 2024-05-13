# Type-erased Pointer #

Created on 2024-03-12; last updated on 2024-05-13

The type `*anyopaque` describes a [pointer](./pointer.md) to an arbitrary type of unknown nonzero size (see [anyopaque](./anyopaque.md)).

The type `?*anyopaque` is analogous to and is used to interoperate with C's `void*` type.

Explicit casting is required to reinterpret a type-erased pointer as a pointer to a type of known size. This involves at least an [alignment cast](./pointer-alignment.md) with `@alignCast`, since `anyopaque` can represent an arbitrarily aligned type, followed by a pointer cast with `@ptrCast`.

It is an error to dereference a type-erased pointer.

The types `*T` and `[*]T` coerce to `*anyopaque` unless `T` is itself a pointer type, since it is an error to coerce a double pointer to a type-erased pointer.

It is an error to implicitly or explicitly cast `*T` or `[*]T` to `[]anyopaque`, as well as to coerce `[]T` to `*anyopaque`.

Because [many-item pointers](./many-item-pointer.md) must have a child type of known size, it isn't possible to create a value of type `[*]anyopaque`, not even an uninitialized variable. On the other hand, it's possible to create an uninitialized variable of type `[]anyopaque`, but such variables don't seem to have any practical utility beside unit-testing the compiler for illegal casts.

Type-erased pointers are used along with [function pointers](./function-pointer.md) to implement polymorphism dynamically using vtables. We see this pattern in the definitions of [`std.mem.Allocator`] and [`std.rand.Random`].

[`std.mem.Allocator`]: https://github.com/ziglang/zig/blob/0.12.0/lib/std/mem/Allocator.zig
[`std.rand.Random`]: https://github.com/ziglang/zig/blob/0.12.0/lib/std/Random.zig
