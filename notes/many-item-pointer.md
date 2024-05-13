# Many-item Pointer #

Created on 2024-03-12; last updated on 2024-05-13

The type `[*]T` describes a [pointer](./pointer.md) to an unknown number of (zero or more) items of a type `T` of known size.

A many-item pointer is one of the components making up a [slice](./slice.md).

Because the child type `T` in a many-pointer type `[*]T` must have a known size, it is not possible to construct the types `[*]anyopaque` (describing a [type-erased](./type-erased-pointer.md) many-item pointer) and `[*]opaque {}` (describing a many-item pointer to an opaque type).

Many-item pointers support pointer arithmetic, indexing and slicing. Given a many-item pointer `p` of type `[*]T` and two index variables `i` and `j` of type `usize` such that `i <= j` equals `true`:

- `p + i` and `p - i` evaluate to the memory addresses at positive and negative offsets of `i` from `p`, respectively;
- `p[i]` interprets the data at `p + i` as a value of type `T`;
- `p[i..]` interprets the data from `p + i` as a value of type `[*]T`;
- `p[i..j]` interprets the data from `p + i` up to but not including `p + j` as a value of type `*[j - i]T` if `i` and `j` are known at compile time and a slice of type `[]T` having length `j - i` otherwise.

Many-item pointers don't support bounds checking with one exception: When `p` and `i` are known at compile time, the expression `p - i` is bounds-checked.

The syntax `(p + i).*` is disallowed for memory access. It is instead necessary to modify `p` in place and then index `p`.

Many-item pointers can't be dereferenced; they must be indexed or sliced.

The type `[*]T` does not coerce to the type `*T`, i.e., many-item pointers don't coerce to [single-item pointers](./single-item-pointer.md), presumably because the types have a multiplicity mismatch.

The type `[*]T` does not coerce to the type `[]T`, i.e., many-item pointers don't coerce to slice types, because the number of items is unknown.

Pointer arithmetic involving a many-item pointer may alter the alignment of the pointer. For any many-item pointer `p` and `usize` `i`, the alignment of `p + i` is equal to the value of the following Zig expression:

```zig
std.math.gcd(@typeInfo(@TypeOf(p)).Pointer.alignment, @intFromPtr(p + i) - @intFromPtr(p))
```

## Sentinel-terminated many-item pointer ##

The type `[*:Z]T` describes a many-item pointer for which the number of items is determined by a sentinel value `Z` of type `T`.

Given a variable `p` of type `[*:Z]T` and two index variables `i` and `j` of type `usize` such that `i <= j` equals `true`:

- `p[i..]` and `p[i.. :Z]` interpret the data from `p + i` as a value of type `[*:Z]T`;
- `p[i..j]` evaluates to a value of type `*[j - i]T` if `i` and `j` are known at compile time and a slice of type `[]T` having length `j - i` otherwise, and
- `p[i..j :Z]` evaluates to a value of type `*[j - i:Z]T` if `i` and `j` are known at compile time and a slice of type `[:Z]T` having length `j - i` otherwise while asserting that `p[j]` is equal to `Z`.

Moreover, let `z` be the integer offset of the sentinel value `Z` from `p`. If `z` is known at compile time, then `p[z .. z + 1]` evaluates to a pointer of type `*[1]T` to an array containing only the sentinel value. Otherwise, if `z` becomes known at run time, then `p[z .. z + 1]` evaluates to a slice of type `[]T` having length 1.

The type `[*:Z]T` coerces to the type `[*]T` but not *vice versa*.

Use the [`std.mem.span`] function to create a value of type `[]T` from a value of type `[*:Z]T` while preserving the other attributes of the source pointer.

The most common sentinel-terminated many-item pointer type found in the source code for the Zig programming language and standard library is `[*:0]const u8`, which is used to represent C-style strings (often written `const char*` or `char* const` in C).

Sentinel-terminated many-item pointers don't support iteration with `for` loops.

[`std.mem.span`]: https://github.com/ziglang/zig/blob/0.11.0/lib/std/mem.zig#L713-L730
