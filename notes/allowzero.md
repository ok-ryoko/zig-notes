# allowzero #

Created on 2024-04-04; last updated on 2024-04-09

If the type of a pointer has the `allowzero` qualifier, then that pointer may hold the zero address.

For example, the type `*allowzero T` represents a [single-item pointer](./single-item-pointer.md) that may hold the zero address.

Pointers that do not have the `allowzero` qualifier coerce to pointer types that do have the `allowzero` qualifier. For example, pointers of type `*T` coerce to the type `*allowzero T`.

Zig provides the primitive `null` for representing null pointers. `null` is guaranteed to represent the zero memory address but may be assigned to only [optional pointers](./optional-pointer.md). Use optional pointer types—not `allowzero` pointer types—when working with null pointers.

Pointers of type `*allowzero T` coerce to the type `?*T` and pointers of type `?*T` coerce to the type `*allowzero T`. This behavior generalizes to [many-item pointers](./many-item-pointer.md).

All [C pointers](./c-pointer.md) are implicitly `allowzero` and the type `[*c]allowzero T` cannot be constructed explicitly.

It is possible to initialize a variable of type `?*allowzero T`. This type seems to have limited practical utility beyond testing the compiler's casting behavior.

The `allowzero` attribute is meaningful only for standalone/freestanding programs and on systems that allow mapping the zero address for, e.g., emulating legacy environments.

## Further reading ##

The `allowzero` qualifier was initially proposed in and implemented to close [ziglang/zig issue 1953].

[ziglang/zig issue 1953]: https://github.com/ziglang/zig/issues/1953
