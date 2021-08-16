## This is a small gdb extension that introduces a few gdb commands to be used in the [TVM Project](https://github.com/apache/tvm)

There are 3 major commands introduced in this extension.
`tvm_dump` - Calls `p tvm::Dump(<value>)` for a given value

Eg:
```c++
(gdb) tvm_dump index
(((j.outer*64) + (i*n)) + j.inner)
```

`tvm_type` - Prints the original object type of a given value. When a function gets a PrimExpr as argument, this commands prints which sub-class of PrimExpr the given value is. This is in same spirit to whatis command in gdb
For example, AddNode could be the underlying original type of the object, declared using it's parent PrimExprNode

Usage:
```c++
(gdb) whatis index
type = tvm::PrimExpr
(gdb) tvm_type index
tvm::tir::AddNode
```

`tvm_attr` - This commmand extends the use of tvm_type and tries to access the underlying attributes/members of the original object.
For example, AddNode has the members `a` and `b`, so this allows us to access those members.

This prints out 3 lines, where the first line shows the access string used to access the member, second line shows the type of the object, and the last line prints a dump of the object

```c++
(gdb) tvm_attr index.a
access string '((tvm::tir::AddNode*)index).a'
Type of object: 'tvm::tir::AddNode'
((j.outer*64) + (i*n))
```

### Installation

The simplest way to install the plugin is to source the commands.py file, either directly in your gdb session or adding the below line to your `.gdbinit`

`source /path/to/commands.py`
