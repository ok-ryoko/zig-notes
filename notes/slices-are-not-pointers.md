# Slices Are Not Pointers But Slice Types Are Pointer Types #

Created on 2024-03-21; last updated on 2024-05-13

Every [slice](./slice.md) is actually a container with two fields, `ptr` (a [many-item pointer](./many-item-pointer.md)) and `len` (a `usize`), e.g.:

```zig
const std = @import("std");
const expect = std.testing.expect;

test {
    var s: []u8 = undefined;
    _ = &s;

    try expect(@hasField(@TypeOf(s), "ptr"));
    try expect(@TypeOf(s.ptr) == [*]u8);
    try expect(@hasField(@TypeOf(s), "len"));
    try expect(@TypeOf(s.len) == usize);
}
```

However, the corresponding slice type is itself a pointer type:

```zig
const std = @import("std");
const expect = std.testing.expect;

test {
    switch (@typeInfo([]u8)) {
        .Pointer => |info| {
            try expect(info.size == std.builtin.Type.Pointer.Size.Slice);
        },
        else => unreachable,
    }
}
```

The attributes of a slice type are equal to the attributes of the underlying many-item pointer type, size notwithstanding.
