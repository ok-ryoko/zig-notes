# Alignment (Pointer) #

Created on 2024-05-13

Every [pointer type](./pointer.md) has an [alignment](./alignment.md) attribute.

The type `*align(A) T`, where `T` is a type and `A` is a `u29`-typed power of two, represents pointers having alignment `A` to objects of type `T`.

The alignment of a pointer type defaults to the alignment of the underlying type, i.e., the type `*T` is implicitly the type `*align(@alignOf(T)) T`. However, if a pointee is a variable, function or struct field whose declaration contains the `align(A)` qualifier, then the pointer's default alignment is `A` instead.

Pointers having larger alignment coerce to pointer types having smaller alignment but not *vice versa*.

Use the `@alignCast` builtin function to increase the alignment of a pointer. Calling `@alignCast` does nothing at run time but inserts a safety check during code generation to ensure that the new alignment divides the memory address evenly.
