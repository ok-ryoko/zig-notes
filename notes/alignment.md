# Alignment #

Created on 2024-05-13

The *alignment* of a type is a number of bytes that divides the memory address of any object of that type evenly.

CPUs access data on word boundaries. Depending on the target architecture, misaligned data access can trigger a hardware exception or increase the number of instructions needed to access the data.

In Zig, alignment is always power of two of type `u29`.

The alignment of each type defaults to its *natural alignment*.

Every primitive type has a natural alignment equal to the type's size.

The natural alignment of any struct or union is equal to the natural alignment of the largest of the field types.

Enumerated types (enums) have a natural alignment of 1.

Zero-bit types such as `void`, `u0` and `struct {}` have a natural alignment of 1.

Plain Zig structs are guaranteed to have ABI-aligned fields.

If the `align(A)` qualifier appears in a variable, function or struct field declaration, then taking the address of that variable, function or field, respectively, always yields a pointer whose [alignment](./pointer-alignment.md) defaults to `A`. For example, if a variable `x` is declared with type `T align(A)`, then the expression `&x` evaluates to a value of type `*align(A) T`.

It is illegal to set the alignment in function declarations for Wasm.

Taking the address of a non-byte-aligned field of type `T` in a packed struct yields a value of type `*align(A:O:B) T`, where:

- `A` is the alignment of the host value (the struct) in bytes;
- `O` is the offset of the non-byte-aligned field into the host value in bits, and
- `B` is the size of the host value in bytes.

This kind of pointer is called a *bit-aligned pointer* or *bit pointer*.

Use the `@alignOf` builtin function on any type (except `noreturn`) to get that type's matching C ABI alignment for the current target.

For any variable `x`, the run-time expression `@intFromPtr(&x) % @alignOf(@TypeOf(x))` is equal to zero.

Use the `@setAlignStack` builtin function inside a function body to set the alignment of the stack (pointer) for the function up to 256 bytes. `@setAlignStack` may be called only once per function body and may not be called in the body of an inline or a naked function.

The `zig translate-c` subcommand translates C declarations containing the `aligned` and `aligned (A)` attributes into Zig declarations containing the `align` keyword.
