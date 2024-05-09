# Address Space #

Created on 2024-04-04; last updated on 2024-05-09

An *address space* is any logical range of memory addresses.

Global constant values, global mutable values, function-local values and functions each have their own target-dependent default address space.

The `addrspace` keyword can be used to qualify the address space of a global variable, function body or pointer but not a function-local variable or function pointer.

For any variable `x` of type `T addrspace(A)`, where `A` is a `std.builtin.AddressSpace`, the expression `&x` evaluates to a value of type `*addrspace(A) T`. Similarly, taking the address of a function body qualified to be in a particular address space yields a [function pointer](./function-pointer.md) to that address space.

The semantics of and relationships between address spaces are target-dependent.

For example, each target determines whether a given variable or function can be initialized in a given address space supported by that target.

For another example, some address spaces on a given target may have different load–store semantics than other address spaces on the same target. Indeed, address spaces are a useful abstraction when loads and stores for a particular type of memory require special CPU instructions, as they relieve the programmer of having to write (inline) assembly to work with said memory.

Zig exposes the following address spaces:

- `generic`: all targets
- `fs`, `gs`, `ss`: x86\_64 and x86
- `global`, `constant`, `local`, `shared`: spirv32, spirv64 and amdgcn
- `param`: nvptx and nvptx64
- `flash`, `flash1`, `flash2`, …, `flash5`: avr

The default address space for all values, functions and pointers is `generic`, which represents the unique *generic address space* for a given target. For example, if `x` is a mutable variable of type `T` that isn't qualified to be in a particular address space, then the expression `&x` evaluates to a value of type `*T`, which is implicitly the type `*addrspace(.generic) T`.

On avr, the default address space for functions is `flash`.

Zig's model of address spaces doesn't account for the following behaviors exhibited by some targets:

- different address spaces have a different address size;
- some but not all address spaces allow the zero address (see [`allowzero`](./allowzero.md));
- different address spaces use a different value for the null pointer (see [optional pointer](./optional-pointer.md)), and
- function-local variables may be declared to be in a particular address space.

Zig also doesn't translate address space qualifiers from C source code.

There is an [accepted proposal][ziglang/zig issue 9815] to implement I/O address spaces to support reads from and writes to I/O ports that require special CPU instructions on some targets (x86 and avr).

Address spaces are principally of concern to people who are programming embedded systems or GPUs, or developing standalone/freestanding programs, operating systems, compilers, etc.

The concept of an address space was first described by ISO/IEC JTC1 SC22 WG14 in the context of embedded C in [N946], which was subsequently worked into an early draft of [TR 18037:2008]. A later refinement (probably draft [N1275]) became the basis for address space support in LLVM, on which Zig's model of address spaces is based.

## Examples of address spaces ##

For graphical targets, Zig uses the [LLVM AMDGPU backend's address spaces][llvm-amdgpu-address-spaces]. The address spaces `global` and `constant` both map to the global virtual address space on the device. However, data declared to be in the constant address space is assumed by the kernel to remain constant during execution, enabling the use of scalar read instructions. These instructions can use cached data, potentially improving performance.

In AVR microcontrollers, flash memory typically holds program code but may also hold read-only data. In some 8- and 16-bit models, this memory is not mapped to data memory and must be accessed using special CPU instructions. For these models, LLVM (and Zig, by extension) uses the data memory as the basis for the generic address space and defines one extra named address space for each 64-KB page of available flash memory up to 384 KB.

x86\_64 CPUs running in 64-bit mode expose two segments for optional data in virtual memory, FS and GS, that can be treated as address spaces. [On x86\_64 Linux, programs typically use the FS segment for thread-local storage][linux-x86-64-fsgs].

## Further reading ##

Address spaces in Zig were first proposed in and implemented to close [ziglang/zig issue 653]. There is an [accepted proposal][ziglang/zig issue 15232] for finalizing the semantics of address spaces.

[linux-x86-64-fsgs]: https://www.kernel.org/doc/html/v6.8/arch/x86/x86_64/fsgs.html
[llvm-amdgpu-address-spaces]: https://releases.llvm.org/17.0.1/docs/AMDGPUUsage.html#address-spaces
[N1275]: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1275.pdf
[N946]: https://www.open-std.org/jtc1/sc22/wg14/www/docs/n946.pdf
[TR 18037:2008]: https://www.iso.org/standard/51126.html
[ziglang/zig issue 15232]: https://github.com/ziglang/zig/issues/15232
[ziglang/zig issue 653]: https://github.com/ziglang/zig/issues/653
[ziglang/zig issue 9815]: https://github.com/ziglang/zig/issues/9815
