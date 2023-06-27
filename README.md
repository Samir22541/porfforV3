# porffor
a **very basic** wip js *aot* optimizing wasm compiler in js. not serious/intended for (real) use<br>
age: ~3 days

## design
porffor is a very unique js engine, due a very different approach. it is seriously limited, but what it can do, it does pretty well. key differences:
- 100% aot compiled *(not jit)*
- everything is a number
- no constant runtime/preluded code

## limitations
- **only number type, no string/array/object/etc at all**
- little built-ins, no prototype
- no errors
- no async/promise/await
- no variables between scopes (except args and globals)
- literal callees only in calls (eg `print()` works, `a = print; a()` does not)
- there is no version of the spec this is based on, I add (easy) things I use

## supported
- number literals
- declaring functions
- calling functions *literal callees only*
- `return`
- `let`/`const`/`var` basic declarations
- some basic integer operators (`+-/*%`)
- some basic integer bitwise operators (`&|`)
- equality operators (`==`, `!=`, etc)
- gt/lt operators (`>`, `<`, `>=`, etc)
- some unary operators (`!`, `+`, `-`)
- logical operators (`&&`, `||`)
- declaring multiple variables in one
- global variables (`var`/none in top scope)
- functions returning 1 number
- bool literals as ints (not real type)
- `if` and `if ... else`
- anonymous functions
- setting functions using vars (`const foo = function() { ... }`)
- arrow functions
- `undefined`/`null` as ints (hack)
- update expressions (`a++`, `++b`, `c--`, etc)
- `for` loops (`for (let i = 0; i < N; i++)`, etc)
- hack for "chars" as ints (`'X'` -> `88`)
- tree shaking wasm imports
- *basic* objects (hack)
- `console.log` (hack)
- `while` loops
- `break` and `continue`
- basic `assert` func
- named export funcs

## soon todo
- support f64 as valtype (wip)
- assignment operators (`+=`, `-=`, etc)
- more math operations (`**`, etc)
- nicer errors
- opt: smarter inline selection (snapshots?)
- begin `Math` (`Math.sqrt`, etc)
- website with code input, wasm output, output and timings

## optimizations
mostly for reducing size. do not really care about compiler perf/time as long as it is reasonable.

### traditional opts
- inlining functions
- inline const math ops

### wasm transforms
- `local.set`, `local.get` -> `local.tee`
- `i32.const 0`, `i32.eq` -> `i32.eqz`
- `i64.extend_i32_u`, `i32.wrap_i64` -> ``
- `return`, `end` -> `end`
- remove some redundant sets/gets
- remove unneeded single just used vars
- remove unneeded blocks
- type cache

## usecases
basically none (other than giving people headaches). potential as a tiny fast advanced expression evaluator (for math)?

## parser
uses [acorn](https://github.com/acornjs/acorn), pretty neat.

## usage
basically nothing will work :). see files in `test` for examples.

1. clone repo
2. `npm install`
3. `node test` to run tests (all should pass)
4. `node runner path/to/code.js` to run a file (or `node runner` to use wip repl)

you can also use deno (`deno run -A ...` instead of `node ...`)

### flags
- `-raw` for no info logs (just raw js output)
- `-valtype=i32|i64|f64` to set valtype, i32 by default (experimental)
- `-O0` to disable opt
- `-O1` to enable basic opt
- `-O2` to enable advanced opt (inlining, treeshake wasm imports)
- `-O3` (default) to enable advanceder opt (precompute const math)
- `-no-run` to not run wasm output, just compile
- `-opt-log` to log some opts
- `-funcs` to log funcs (internal representations)
- `-opt-funcs` to log funcs after opt
- `-sections` to log sections as hex
- `-opt-no-inline` to not inline any funcs

## faq

### 1. why name
`purple` in Welsh is `porffor`. why purple?
- no other js engine is purple colored
- purple is pretty cool
- purple apparently represents "ambition", which is.. one word to describe this project
- the hard to speak name is also the noise your brain makes in reaction to this idea

### 2. why at all
yes.