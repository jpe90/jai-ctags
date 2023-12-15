This is a fork of [jai-ctags](https://github.com/rluba/jai-ctags) that I'm using for code navigation in Emacs. It also fixes ctags-based code navigation in other editors such as VS Code (with extensions such as [this](https://github.com/gediminasz/ctags-companion)).

https://github.com/jpe90/jai-ctags/assets/9307830/3372d4d0-d75d-4188-b2f0-1763dab6bc0a

Originally, I intended to add compatability for the [etags format](https://en.wikipedia.org/wiki/Ctags#Etags_2), but that format requires a byte offset for each symbol. Jai's Compiler module mentions that byte offsets may be included in the future, but currently it tracks column and line values.

Instead of trying to calculate byte offsets for symbols, I ended up just slightly modifying the tag address that this module writes out to be compatible with [Citre](https://github.com/universal-ctags/citre) which is an Emacs package written by the authors of Universal Ctags. It uses the ctags format for navigation rather than etags.

Although vim seems to handle the tag addresses (empasized with arrows below) that this module originally wrote out:

```
froop	hello.jai	/\%6l\%1c/;"	kind:f	arity:1
main	hello.jai	/\%10l\%1c/;"	kind:f	arity:0
                        ^^^^^^^^^^
```

Citre chokes on them with "invalid escape sequence". I didn't really want to go down a rabbit hole of fixing that, but I knew Citre can handle ctags' --excmd=number option, so I just updated the tag address format to match it:

```
froop	hello.jai	6;	kind:f	arity:1
main	hello.jai	10;	kind:f	arity:0
                        ^^^
```

If the compiler is updated to use byte offsets, I'll update this repository to generate a TAGS file in the etags format.
