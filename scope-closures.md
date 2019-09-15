# You Don't Know JS notes

## Scope and Closures

### Chapter 1: What is scope ?
* Although Javascript is considered as an interpreted language, it technically is compiled right before (even milliseconds) execution. With the emergence of JIT (Just In Time) compilers the lines between interpretation and compilation have blurred. More details [here](https://softwareengineering.stackexchange.com/questions/138521/is-javascript-interpreted-by-design).

* The cast behind the scenes:
  1. Engine: responsible for execution and overlooking compilation of the program from start to finish
  2. Compiler: responsible for actual compilation involving parsing and code generation
  3. Scope: collects and maintains a lookup list of identifiers/variables declared. Governs the rules of how and when different variables can be accessed.

* A statement like `var a = 2;` is processed in 2 phases:
  1. During compilation phase, Compiler sees `var a` and asks scope if the variable has been declared already. If yes Compiler ignores and moves on otherwise it asks scope to declare it. It then generates the code for `a = 2` for execution later.
  2. During execution phase, Engine looks up the variable via Scope and assigns to it.

* Scopes can be nested. The scope lookup by the engine starts with the currently executing scope and keeps going a level up until it either finds the variable or reaches the global level scope.
	``` javascript
	function foo() {
		var a = 2;
	    console.log(a + b);
	}
	var b = 2;
	foo();
	// Output: 4
	```
