# Pointers Are a Source of Checked Illegal Behavior #

Created on 2024-03-19; last updated on 2024-05-13

It is illegal to change the [alignment of a pointer](./pointer-alignment.md) to a value that doesn't divide the memory address evenly.

It is illegal to cast a pointer holding the zero address to a pointer type that doesn't support the zero address.
