# Slice #

Created on 2024-03-20; last updated on 2024-05-11

The type `[]T` describes a fat pointer comprising a [many-item pointer](./many-item-pointer.md) of type `[*]T` and run time-known length of type `usize`.

Every slice value has two fields, `ptr` and `len`, that store the underlying many-item pointer and length, respectively.

[Slices are not pointers but slice types are pointer types](./slices-are-not-pointers.md).

Slices support indexing and slicing with bounds checking. Given a slice `s` of type `[]T` and two valid run time-known index variables `i` and `j` of type `usize` into `s` such that `i <= j` equals `true`:

- `s[i]` evaluates to a value of type `T`;
- `s[i..]` evaluates to a value of type `*[s.len - i]T` if `i` is known at compile time and a slice of type `[]T` having length `s.len - i` otherwise, and
- `s[i..j]` evaluates to a value of type `*[j - i]T` if `i` and `j` are known at compile time and a slice of type `[]T` having length `j - i` otherwise.

Slices can't be dereferenced; they must be indexed or sliced.

For any nonempty slice `s`, the expressions `&s[0]` and `s.ptr` both yield a pointer to the first element of `s`.

We can loop over any slice in a `for` expression or statement.

Compile time-known slices can be concatenated and multiplied to yield a single-item pointer to an array. If `s` and `t` are compile time-known slices of type `[]T` having length `M` and `N`, respectively, then `s ++ t` evaluates to a value of type `*[M + N]T`. Moreover, if `K` is a compile time-known `usize`, then `s ** K` evaluates to a value of type `*[K * M]T`.

The type `[]T` does not coerce to the type `[*]T`, presumably because information (the number of elements) would be discarded.

Slices don't have a well-defined layout in memory at compile time. Thus, a single-item pointer to an array that has been reinterpreted as a single-item pointer to a slice via `@ptrCast` can't be dereferenced at compile time.

## Sentinel-terminated slice ##

The type `[:Z]T` describes a slice that guarantees a sentinel value `Z` of type `T` at the element indexed by the length of the slice. In other words, for any slice `s` of type `[:Z]T`, `s[s.len]` is guaranteed to evaluate to `Z`.

The sentinel value may also appear at earlier positions in the slice.

For any empty sentinel-terminated slice `s`, the expressions `&s[0]` and `s.ptr` both yield a pointer to the sentinel value.

Given a slice `s` of type `[:Z]T` and two valid index variables `i` and `j` of type `usize` into `s` such that `i <= j` equals `true`:

- `s[i..]` and `s[i.. :Z]` each evaluate to a value of type `*[s.len - i:Z]T` if `i` is known at compile time and a slice of type `[:Z]T` having length `s.len - i` otherwise;
- `s[i..j]` evaluates to a value of type `*[j - i]T` if `i` and `j` are known at compile time and a slice of type `[]T` having length `j - i` otherwise, and
- `s[i..j :Z]` evaluates to a value of type `*[j - i:Z]T` if `i` and `j` are known at compile time and a slice of type `[:Z]T` having length `j - i` otherwise while asserting that `s[j]` is equal to `Z`.

Moreover, let `n` be a `usize` equal to `s.len`. If `n` is known at compile time, then `s[n .. n + 1]` evaluates to a pointer of type `*[1]T` to an array containing only the sentinel value. Otherwise, if `n` becomes known at run time, then `s[n .. n + 1]` evaluates to a slice of type `[]T` having length 1.

Like non-sentinel-terminated slices, compile time-known sentinel-terminated slices can be concatenated and multiplied, resulting in single-item pointers to sentinel-terminated arrays with all intervening sentinel values discarded.

The implementation of the [`std.posix.toPosixPath`] function shows how to get a `[:Z]T` from a `[]T` on the stack.

The type `[:Z]T` coerces to the type `[]T`.

The type `[:Z]T` also coerces to the type `[*:Z]T`. Even though the length of the slice is lost in this kind of conversion, the number of elements remains implied by the sentinel value.

[`std.posix.toPosixPath`]: https://github.com/ziglang/zig/blob/0.12.0/lib/std/posix.zig#L7326-L7334
