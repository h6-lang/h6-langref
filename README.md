## syntax
```
symbol: 1.23
func: { 1 + }
# note that bindings only bind a single expression

"this is:\na \"string\""

{'H 'e 'l 'l 'o }
```

Code is executed from the left to right, top to bottom.

## types
there are only two types:
- fixed point (16.16 bits) number
- array / bytecode / code block

## operations
most operations don't work on both arrays and numbers!

- push number: `3.1`
- array: `{ op1 op2 op3 }`
- duplicate: `1 2 .` -> `1 2 2`
- pop: `1 2 ;` -> `1`
- swap: `1 2 $` -> `2 1`
- left-rotate 3: `1 2 3` -> `2 3 1`
- right-rotate 3: `1 2 3` -> `3 1 2`
- ref-planet: `1 2 3 4 &-v-v` -> `1 2 3 4  1 3`
- less-than: `1 2 <` -> `1`
- greater-than: `1 2 >` -> `0`
- equal: `1 2 =` -> `0`
- not: `1 ~` -> `0`
- plus: `1 2 +` -> `3`
- minus: `1 2 -` -> `-1`
- multiply: `1 2 *` -> `2`
- select: `1 2 1 ?` -> `2`, and `1 2 0 ?` -> `1`
- execute / unpack: `{1 2 3} !` -> `1 2 3`
- pack: `1 _` -> `{1}` (works on arrays too)
- first: `{1 2 3} @0` -> `1` (also works on n-d arrays: `{{1 2} 3} @0` -> `{1 2}`)
  note that this does NOT execute the bytecode of the whole array, but instead only executes the first op
- skip1: `{1 2 3} @<` -> `{2 3}`
  similarly to first, does not execute the bytecode of the whole array
- concat: `{1 2} {3} @+` -> `{1 2 3}`
  only works if both args are array
- length: `{1 2 3} @*` -> `3`
- typeid: `100 typeid` -> `0`, and `{1 2 3} typeid` -> `1`
- system: `system N` -> call system function N

## linking
since code gets self-linked, it is possible to reference symbols that get declared later in the code, as well as reference the current declaring symbol:
```
a: { a }
```

note that this also means that referencing undeclared symbols, even if the code that does so is never executed, will produce linker errors:
```
{ thisSymbolDoesNotExist } ;
# this will produce a linker error
```
