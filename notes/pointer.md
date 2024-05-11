# Pointer #

Created on 2024-03-12; last updated on 2024-05-11

A pointer is a memory address allowing us to read and possibly write data at that address.

Every pointer has a pointer type.

Zig makes a distinction between four types of pointer type:

- [single-item pointer](./single-item-pointer.md);
- [many-item pointer](./many-item-pointer.md);
- [slice](./slice.md), and
- [C pointer](./c-pointer.md).

In addition, [optional](./optional-pointer.md), [function](./function-pointer.md) and [type-erased](./type-erased-pointer.md) pointers deserve mention.

Every pointer type has the following attributes:

- size, which determines the pointer type;
- child type, which tells us how to interpret the data to which the pointer points;
- alignment, which holds the alignment of the pointer;
- [address space](./pointer-address-space.md), which communicates the segment of memory in which the pointer is valid;
- a flag to indicate whether the pointer can't be used to mutate the data to which it points, i.e., whether the pointer is [`const`](./pointer-const.md);
- a flag to indicate whether reads and writes via the pointer have side effects (as in direct hardware access via MMIO), i.e., whether the pointer is [`volatile`](./pointer-volatile.md);
- a flag to indicate whether the pointer may hold the zero memory address, i.e., whether the pointer is [`allowzero`](./allowzero.md), and
- sentinel value, which appears immediately after the last element in a sentinel-terminated array, many-item pointer or slice.

The size of each pointer type is machine-dependent. Single-item, many-item and C pointer types have a size equal to the size of the `usize` integral type. Slice types, representing fat pointers, have a size equal to twice the size of `usize`.

Pointers can be used at compile time as long as their child type has a well-defined memory layout.

We use the `@ptrCast` builtin function to reinterpret pointers as having a different pointer type. `@ptrCast` changes a pointer's child type while also performing any additional necessary casts that would have taken place implicitly, such as:

- casting an optional pointer to a nonoptional pointer (with safety checking);
- decreasing pointer alignment, and
- discarding the sentinel value in a single-item pointer to a sentinel-terminated array, a sentinel-terminated many-item pointer or sentinel-terminated slice.

We can also use `@ptrCast` to restructure (slices of) compile time-only arrays. This functionality allows us to, e.g., index a one-dimensional array as if it were an equally sized multidimensional array.

We can't supply an undefined pointer to `@ptrCast`.

Currently, `@ptrCast` allows the casting of double pointers to single pointers but there is an [accepted proposal][ziglang/zig issue 1890] to prohibit this.

Interconversion between integers and pointers is possible with the `@ptrFromInt` and `@intFromPtr` builtin functions. Conversions involving the zero memory address are checked for illegal behavior. Conversions of integer addresses to pointers are checked for proper alignment.

There is an [accepted proposal][ziglang/zig issue 2414] to check for illegal behavior when using `@ptrCast` and `@intToPtr`. If implemented, this proposal would make it a compile-time error to call `@ptrCast` or `@intToPtr` when the pointed-to memory is undefined or the in-memory layout of the source child type does not match the layout of the destination child type.

There is an [accepted proposal][ziglang/zig issue 1738] to add support for pointer subtraction.

Undefined pointers can represent any value, even the zero memory address. What does it mean for a pointer that is not optional to be undefined? This has been identified as a [design flaw][ziglang/zig issue 3325].

[ziglang/zig issue 1738]: https://github.com/ziglang/zig/issues/1738
[ziglang/zig issue 1890]: https://github.com/ziglang/zig/issues/1890
[ziglang/zig issue 2414]: https://github.com/ziglang/zig/issues/2414
[ziglang/zig issue 3325]: https://github.com/ziglang/zig/issues/3325
