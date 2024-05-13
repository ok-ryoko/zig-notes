# C Pointer #

Created on 2024-03-12; last updated on 2024-05-13

The type `[*c]T` describes a C [pointer](./pointer.md).

The only use for C pointers is in the machine translation of C source code to Zig source code, where it isn't always possible to determine unambiguously whether a pointer should be represented using a Zig [single](./single-item-pointer.md)- or [many](./many-item-pointer.md)-item pointer. There is an [accepted proposal][ziglang/zig issue 2984] to prohibit the use of C pointers outside translated code.

C pointers support the same syntax as single- and many-item pointers.

For any C pointer `p` to a single struct containing a field `f`, `p.*.f` accesses `f` and is syntactically analogous to the expression `p->f` in C.

For any C pointer `p` to an array of structs, each containing a field `f`, `p[i].f` accesses `f` on the `i`th struct in the array, where `i` is a `usize` holding a valid index into the array. This is the same syntax as in C.

All C pointers are [`allowzero`](./allowzero.md) and may be initialized with the `null` primitive. (The type `@TypeOf(null)` coerces to the type `[*c]T` for any choice of `T`.) For this reason, the compiler refuses to construct the type `[*c]allowzero T`. However, it is possible to initialize an [optional](./optional-pointer.md) C pointer, i.e., value of type `?[*c]T`.

Null C pointers can't be dereferenced, indexed or sliced.

C pointers can't point to types that are incompatible with the C ABI, e.g., it isn't possible to create a value of type `[*c]void`.

C pointers can't point to opaque types, i.e., it isn't possible to create a value of type `[*c]anyopaque`.

C pointers can't be sentinel-terminated.

C pointers coerce to and from (null-terminated) single-item and many-item pointers as long as the child type is C ABI-compatible.

Coercing a C pointer to a nonoptional pointer type triggers a check for illegal behavior.

The type `[:0]T` coerces to the type `[*c]T`.

The type `[*c]T` coerces to neither the type `[]T` nor the type `[:0]T`.

Use the [`std.mem.span`] function to create a value of type `[:0]T` from a nonzero value of type `[*c]T` while preserving the other attributes of the source pointer.

C pointers coerce to and from integers and can be compared with integers. There is an [accepted proposal][ziglang/zig issue 2057] to abolish this behavior.

[`std.mem.span`]: https://github.com/ziglang/zig/blob/0.12.0/lib/std/mem.zig#L780-L797
[ziglang/zig issue 2057]: https://github.com/ziglang/zig/issues/2057
[ziglang/zig issue 2984]: https://github.com/ziglang/zig/issues/2984
