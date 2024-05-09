# Address Space (Pointer) #

Created on 2024-05-09

Every [pointer type](./pointer.md) has an attribute that indicates the [address space](./address-space.md) in which every pointer of that type is meaningful.

## Address space cast ##

Pointers do not coerce to pointer types having a different address space. It is instead necessary to perform an explicit cast using the `@addrSpaceCast` builtin function, which requires the LLVM code generation backend.

The behavior of `@addrSpaceCast` depends on the target and address space pair.

It is undefined behavior to dereference the result pointer of a legal address space cast if the referenced data does not exist at that address in that address space.

If an address space cast is legal and returns a dereferenceable result pointer, then the result pointer points to the same memory location as the operand pointer and the cast is assumed to be reversible.

It is always legal to cast a pointer to its current address space.

It is an error to cast a pointer to an address space not supported by the target or to call `@addrSpaceCast` at compile time.
