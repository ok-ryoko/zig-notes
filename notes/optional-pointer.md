# Optional Pointer #

Created on 2024-03-19; last updated on 2024-04-09

The type `?*T` describes an optional [single-item pointer](./single-item-pointer.md) to exactly one value of type `T`.

The statements below generalize to optional [many-item pointer](./many-item-pointer.md) types but not optional [slice](./slice.md) types because [slices are not pointers](./slices-are-not-pointers.md).

Any optional pointer may have the value `null`, which is guaranteed to represent the zero memory address. Indeed, the types `?*T` and [`*allowzero T`](./allowzero.md) coerce to one another, although the primitive `null` can't be assigned to a variable of type `*allowzero T`.

It is an error to attempt memory access through a *null pointer* (an optional pointer that has the value `null`).

The type `?*T` is guaranteed to have the same size as the type `*T`.

It is possible to initialize a variable of type `?*allowzero T`. This type seems to have limited practical utility beyond testing the compiler's casting behavior.

The type `?*allowzero T` is larger than the type `?*T` because extra space is needed to store the redundant information.

The type `?*T` coerces to the type `?*allowzero T` but not *vice versa*.

What about [C pointers](./c-pointer.md)?

- The primitive `null` can be assigned to a variable of type `[*c]T`;
- `?[*c]T` is to `[*c]T` as `?*allowzero T` is to `*allowzero T`, and
- the compiler refuses to construct the type `[*c]allowzero T` because all C pointer types are necessarilly `allowzero`.
