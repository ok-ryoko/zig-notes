# Function Pointer #

Created on 2024-03-20; last updated on 2024-05-13

The type `*const P`, where `P` is a function prototype, describes a pointer to a compile time-known function body. The type `P` coerces to the type `*const P`.

The function prototype in a function pointer may not contain an inferred error set.

Any pointer to an inline function must have a compile time-known value.

It is an error to call a function pointer at compile time.

In dynamic implementations of polymorphism, function pointers populate vtables and accept a [type-erased pointer](./type-erased-pointer.md) to an interface implementation (see, for example, the definition of [`std.mem.Allocator`]).

[`std.mem.Allocator`]: https://github.com/ziglang/zig/blob/0.12.0/lib/std/mem/Allocator.zig
