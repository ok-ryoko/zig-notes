# Pointers Are a Source of Checked Illegal Behavior #

Created on 2024-03-19; last updated on 2024-04-01

It is illegal to change the alignment of a [pointer](./pointer.md) to a new alignment with which the pointer's memory address is inconsistent.

It is illegal to cast a pointer holding the zero address to a pointer type that doesn't support the zero address.
