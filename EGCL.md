# EGCL support

The EGCL branches use `EGCL-DEBUG:PRINT-BACKTRACE` and `LIST-BACKTRACE`.
They support current logical stacks across interpreted, bytecode, and installed
native frames, including code running in fibers. Frame snapshots can outlive
the calls they describe; argument objects are retained references, not deep copies.

`map-backtrace` supplies available original arguments as `Arg-0`, `Arg-1`, etc.
These are positional labels, not inferred parameter names or lexical locals.
Native argument locations, source positions, and inline frames are not currently
provided by EGCL, so their corresponding fields stay absent. No Rust backtrace
or placeholder frame is substituted for missing Lisp information.

Capture inside a handler before unwinding if the failing call chain is needed.
The condition passed to `print-backtrace` is printed as a report; it does not
make an already-unwound stack available. This matches the library's current-stack
adapter interface.

The Evergreen repository carries a pinned ocicl Git-source scenario for cold and
cached ASDF loads, printed/structured traces, output streams/files, condition
reporting, and tier selection. Other implementation branches are unchanged.
