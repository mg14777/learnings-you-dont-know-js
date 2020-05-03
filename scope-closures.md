
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

### Chapter 2: Lexical Scope
Javascript employs lexical scope model. Lexical scope means scope is defined at lexing time aka it is based on where variables and blocks of scope are defined by you while writing the source code.

In lexical scope model, scope can be determined by looking at the source code itself. Take a look at the example below:

``` javascript
var magic = 42;

function foo() {
	console.log(magic);
}

function bar() {
	var magic = 10;
	foo();
}

function baz() {
	var magic = 20;
	foo();
}

bar(); // prints 42
baz(); // prints 42
```
The above example will print 42 twice. The scope of magic in foo will always be the same i.e. global definition of magic (42) since its set in stone during compile time itself and doesn't depend on where/how foo is called. If dynamic scope was in effect here the printed values would have been 10 and 20 since scope would be determined based on call site.

"No matter where a function is invoked from, or even how it is invoked, its lexical scope is only defined by where the function was declared."

#### Scope Lookups

* JavaScript starts at the innermost scope and searches outwards until it finds the variable it was looking for.

* Scope lookup stops once it finds the first match

* **Variable Shadowing**: If the same variable is defined at multiple levels in a nested scope the innermost variable will be picked up and any outer variables will be shadowed. For example:
	```javascript
	var a = 10;
	function foo() {
		var a = 20;
		function bar() {
			var a = 30;
			function baz() {
				console.log(a); // 30
			}
		}
	}
	```
	The printed value of ```a``` will always be 30 since the lookup stops at ```var a = 30;``` and the outer values of ```a``` are shadowed.
	
* All global variables are by default part of the global object (```window``` in most browsers) which means in the above example you can still access global value of ```a``` as ```window.a```. There's no way to access ```var a = 20;``` though.

*  Lexical scope look-up process only applies to first-class identifiers. If you were accessing object properties like ```x.y.z``` the scope lookup would only go as far as finding ```x```. On finding the first occurence, object property access rules take over to resolve ```y``` and ```z```.
	```javascript
	var x = { y: { z: 4 } };
	function foo() {
	    var x = { y: {} };
	    console.log(x.y.z); // undefined
	}
	foo();
	```