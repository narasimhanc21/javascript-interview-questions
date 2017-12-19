# javascript-interview-questions
Explain event delegation
	Event delegation is a technique involving adding event listeners to a parent element instead of adding them to the descendant elements. The listener will fire whenever the event is triggered on the descendant elements due to event bubbling up the DOM. The benefits of this technique are:

	1. Memory footprint goes down because only one single handler is needed on the parent element, rather than having to attach event handlers on each descendant.
	2. There is no need to unbind the handler from elements that are removed and to bind the event for new elements.
	
	<ul id="parent-list">
	<li id="post-1">Item 1</li>
	<li id="post-2">Item 2</li>
	<li id="post-3">Item 3</li>
	<li id="post-4">Item 4</li>
	<li id="post-5">Item 5</li>
	<li id="post-6">Item 6</li>
	</ul>
	
	// Get the element, add a click listener...
	document.getElementById("parent-list").addEventListener("click", function(e) {
	// e.target is the clicked element!
	// If it was a list item
	if(e.target && e.target.nodeName == "LI") {
	// List item found!  Output the ID!
	console.log("List item ", e.target.id.replace("post-", ""), " was clicked!");
	}
	});
	
	a parent DIV with many children but all we care about is an A tag with the classA CSS class:
	
	// Get the parent DIV, add click listener...
	document.getElementById("myDiv").addEventListener("click",function(e) {
	// e.target was the clicked element
	  if (e.target && e.target.matches("a.classA")) {
	console.log("Anchor element clicked!");
	}
	});

Explain how this works in JavaScript
	If the new keyword is used when calling the function, this inside the function is a brand new object.
	If apply, call, or bind are used to call/create a function, this inside the function is the object that is passed in as the argument.
	If a function is called as a method, such as obj.method()?—?this is the object that the function is a property of.
	If a function is invoked as a free function invocation, meaning it was invoked without any of the conditions present above, this is the global object. In a browser, it is the window object. If in strict mode ('use strict'), this will be undefined instead of the global object.
	If multiple of the above rules apply, the rule that is higher wins and will set the this value.
	e.g  var person = {
	firstName:"John",
	lastName: "Doe",
	fullName: function() {
	return this.firstName + " " + this.lastName;
	}
	}
	var myObject = {
	firstName:"Mary",
	lastName: "Doe",
	}
	person.fullName.call(myObject);  // Will return "Mary Doe" 

	If the function is an ES2015 arrow function, it ignores all the rules above and receives the this value of its surrounding scope at the time it is created.
	const obj = {
	value: 'abc',
	createArrowFn: function() {
	return () => console.log(this);
	}
	};
	const arrowFn = obj.createArrowFn();
	arrowFn(); // -> { value: 'abc', createArrowFn: ƒ }
	
	
	https://codeburst.io/the-simple-rules-to-this-in-javascript-35d97f31bde3

Explain how prototypal inheritance works
	All JavaScript objects have a prototype property, that is a reference to another object. When a property is accessed on an object and if the property is not found on that object, the JavaScript engine looks at the object's prototype, and the prototype's prototype and so on, until it finds the property defined on one of the prototypes or until it reaches the end of the prototype chain. This behaviour simulates classical inheritance.
	
	function Rectangle( width, height ) {
	  this.width = width;
	  this.height = height;
	};

	Rectangle.prototype.area = function() {
	  return this.width * this.height;
	};

	var shape = Object.create( Rectangle.prototype );
	Rectangle.call( shape, 3, 4 );
	https://medium.com/@kevincennis/prototypal-inheritance-781bccc97edb

What do you think of AMD vs CommonJS?
	Both are ways to implement a module system, which was not natively present in JavaScript until ES2015 came along. CommonJS is synchronous while AMD (Asynchronous Module Definition) is obviously asynchronous. CommonJS is designed with server-side development in mind while AMD, with its support for asynchronous loading of modules, is more intended for browsers.

	I find AMD syntax to be quite verbose and CommonJS is closer to the style you would write import statements in other languages. Most of the time, I find AMD unnecessary, because if you served all your JavaScript into one concatenated bundle file, you wouldn't benefit from the async loading properties. Also, CommonJS syntax is closer to Node style of writing modules and there is less context-switching overhead when switching between client side and server side JavaScript development.
	
	https://auth0.com/blog/javascript-module-systems-showdown/
	
Explain why the following doesn't work as an IIFE: function foo(){ }();.What needs to be changed to properly make it an IIFE?
	IIFE stands for Immediately Invoked Function Expressions. The JavaScript parser reads function foo(){ }(); as function foo(){ } and ();, where the former is a function declaration and the latter (a pair of brackets) is an attempt at calling a function but there is no name specified, hence it throws Uncaught SyntaxError: Unexpected token ).

	Here are two ways to fix it that involves adding more brackets: (function foo(){ })() and (function foo(){ }()). These functions are not exposed in the global scope and you can even omit its name if you do not need to reference itself within the body.
	
	The most widely accepted way to tell the parser to expect a function expression is just to wrap in in parens, because in JavaScript, parens can’t contain statements. At this point, when the parser encounters the function keyword, it knows to parse it as a function expression and not a function declaration.

What's the difference between a variable that is: null, undefined or undeclared? How would you go about checking for any of these states?
	Undeclared variables are created when you assign to a value to an identifier that is not previously created using var, let or const. Undeclared variables will be defined globally, outside of the current scope. In strict mode, a ReferenceError will be thrown when you try to assign to an undeclared variable.
	
	function foo() {
	x = 1;   // Throws a ReferenceError in strict mode
	}

	foo()
	console.log(x) // 1
	
	A variable that is undefined is a variable that has been declared, but not assigned a value. It is of type undefined. 
	If a function does not return any value as the result of executing it is assigned to a variable, the variable also has the value of undefined.
	
	var foo;
	console.log(foo); // undefined
	console.log(foo === undefined); // true
	console.log(typeof foo === 'undefined'); // true
	
	function bar() {}
	var baz = bar();
	console.log(baz); // undefined
	
	A variable that is null will have been explicitly assigned to the null value. It represents no value and is different from undefined in the sense that it has been explicitly assigned. To check for null, simply compare using the strict equality operator. 

	var foo = null;
	console.log(foo === null); // true

Difference between undefined and not defined in JavaScript
	In JavaScript if you try to use a variable that doesn't exist and has not been declared, 
	then JavaScript will throw an error var name is not defined and the script will stop execute thereafter. 
	But If you use typeof undeclared_variable then it will return undefined.

	Before starting further discussion let's understand the difference between declaration and definition.

	var x is a declaration because you are not defining what value it holds yet, 
	but you are declaring its existence and the need of memory allocation.

	var x; // declaring x
	console.log(x); //output: undefined
	var x = 1 is both declaration and definition (also we can say we are doing initialisation), 
	Here declaration and assignment of value happen inline for variable x, 
	In JavaScript every variable declaration and function declaration brings to the top of its current scope 
	in which it's declared then assignment happen in order this term is called hoisting.

	A variable that is declared but not define and when we try to access it, It will result undefined.

	var x; // Declaration
	if(typeof x === 'undefined') // Will return true
	A variable that neither declared nor defined when we try to reference such variable then It result not defined.

	console.log(y);  // Output: ReferenceError: y is not defined
	Ref Link:
	http://stackoverflow.com/questions/20822022/javascript-variable-definition-declaration

What is a closure, and how/why would you use one?

	A closure is the combination of a function and the lexical environment within which that function was declared. The word "lexical" refers to the fact that lexical scoping uses the location where a variable is declared within the source code to determine where that variable is available. Closures are functions that have access to the outer (enclosing) function's variables—scope chain even after the outer function has returned.
	
	(Closures are functions that have access to variables from anthor function's scope. This is often accomplished by creating a function inside a function.)

	var basketModule = (function () {
	  // privates
	  var basket = [];
	  function doSomethingPrivate() {
	//...
	  }

	  // Return an object exposed to the public
	  return {
	addItem: function( values ) {
	  basket.push(values);
	},
	 
	getItemCount: function () {
	  return basket.length;
	},
	 
	// Public alias to a private function
	doSomething: doSomethingPrivate,
	 
	getTotal: function () {
	  var q = this.getItemCount(),
	  p = 0;
	  while (q--) {
	p += basket[q].price;
	  }
	  return p;
	}
	  };
	})();

	basketModule.addItem({
	  item: "bread",
	  price: 0.5
	});
	basketModule.addItem({
	  item: "butter",
	  price: 0.3
	});
	// Outputs: 2
	console.log( basketModule.getItemCount() );
	// Outputs: 0.8
	console.log( basketModule.getTotal() );

	// Outputs: undefined
	// This is because the basket itself is not exposed as a part of our
	// public API
	console.log( basketModule.basket );
 
	// This also won't work as it only exists within the scope of our
	// basketModule closure, but not in the returned public object
	console.log( basket );
	
	Example 2:
	Closures are created whenever a variable that is defined outside the current scope is accessed from within some inner scope.Following example shows how the variable counter is visible within the create, increment, and print functions, but not outside of them

	function create() {
	   var counter = 0;
	   return {
		  increment: function() {
			 counter++;
		  },
	  
		  print: function() {
			 console.log(counter);
		  }
	   }
	}
	var c = create();
	c.increment();
	c.print();     // ==> 1

	Ref:
	https://addyosmani.com/resources/essentialjsdesignpatterns/book/#modulepatternjavascript

Can you describe the main difference between a forEach loop and a .map() loop and why you would pick one versus the other?
	
What's a typical use case for anonymous functions?
	They can be used in IIFEs to encapsulate some code within a local scope so that variables declared in it do not leak to the global scope.

	(function() {
	  // Some code here.
	})();
	As a callback that is used once and does not need to be used anywhere else. The code will seem more self-contained and readable when handlers are defined right inside the code calling them, rather than having to search elsewhere to find the function body.

	setTimeout(function () {
	  console.log('Hello world!');
	}, 1000);
	Arguments to functional programming constructs or Lodash (similar to callbacks).

	const arr = [1, 2, 3];
	const double = arr.map(function (el) {
	  return el * 2;
	});
	console.log(double); // [2, 4, 6]

How do you organize your code? (module pattern, classical inheritance?)
	The module pattern is still great, but these days, I use the Flux architecture based on React/Redux which encourages a single-directional functional programming approach instead. I would represent my app's models using plain objects and write utility pure functions to manipulate these objects. State is manipulated using actions and reducers like in any other Redux application.

What's the difference between host objects and native objects?
	Native objects are objects that are part of the JavaScript language defined by the ECMAScript specification, such as String, Math, RegExp, Object, Function, etc.

	Host objects are provided by the runtime environment (browser or Node), such as window, XMLHTTPRequest, etc.
	
	Native objects are those objects supplied by JavaScript. Examples of these are String, Number, Array, Image, Date, Math, etc.
	Host objects are objects that are supplied to JavaScript by the browser environment. Examples of these are window, document, forms, etc.

Difference between: function Person(){}, var person = Person(), and var person = new Person()?
	var person = Person() invokes the Person as a function, and not as a constructor. Invoking as such is a common mistake if it the function is intended to be used as a constructor. Typically, the constructor does not return anything, hence invoking the constructor like a normal function will return undefined and that gets assigned to the variable intended as the instance.

	var person = new Person() creates an instance of the Person object using the new operator, which inherits from Person.prototype. An alternative would be to use Object.create, such as: Object.create(Person.prototype).

	function Person(name) {
	  this.name = name;
	}

	var person = Person('John');
	console.log(person); // undefined
	console.log(person.name); // Uncaught TypeError: Cannot read property 'name' of undefined

	var person = new Person('John');
	console.log(person); // Person { name: "John" }
	console.log(person.name); // "john"
	
What's the difference between .call and .apply?
	Both .call and .apply are used to invoke functions and the first parameter will be used as the value of this within the function. However, .call takes in a comma-separated arguments as the next arguments while .apply takes in an array of arguments as the next argument. An easy way to remember this is C for call and comma-separated and A for apply and array of arguments.
	
	These methods both call the function with a specific this value. * the apply() method accepts two arguments: the value of this and an array of arguments. * the call() method exhibits the same behavior as apply(),but arguments are passed to it differently.Using call() arguments must be enumerated specifically.

	function add(a, b) {
	  return a + b;
	}

	console.log(add.call(null, 1, 2)) // 3
	console.log(add.apply(null, [1, 2])) // 3

Explain Function.prototype.bind.
	ECMAScript 5 defines an addition method called 'bind()'.the 'bind()' method create a new function instance whose this value is bound to the value to that was passed into 'bind()'.

	this.x = 9;    // this refers to global "window" object here in the browser
	var module = {
	  x: 81,
	  getX: function() { return this.x; }
	};

	module.getX(); // 81

	var retrieveX = module.getX;
	retrieveX();   
	// returns 9 - The function gets invoked at the global scope

	// Create a new function with 'this' bound to module
	var boundGetX = retrieveX.bind(module);
	boundGetX(); // 81

Example of call apply bind
	var obj = {
	a : 1,
	b: 2
	}

	var a = 5;
	var b = 10;

	function add(x) {
	  return this.a+ this.b + x;
	}

	add(7);

	add.call(obj, 8);

	add.apply(obj, [8]);

	var adder = add.bind(obj, 7);
	adder();

When would you use document.write()?
What's the difference between feature detection, feature inference, and using the UA string?
	Feature Detection
	Feature detection involves working out whether a browser supports a certain block of code, and running different code dependent on whether it does (or doesn't), so that the browser can always provide a working experience rather crashing/erroring in some browsers. For example:

	if ('geolocation' in navigator) {
	  // Can use navigator.geolocation
	} else {
	  // Handle lack of feature
	}
	Modernizr is a great library to handle feature detection.

	Feature Inference

	Feature inference checks for a feature just like feature detection, but uses another function because it assumes it will also exist, e.g.:

	if (document.getElementsByTagName) {
	element = document.getElementById(id);
	}
	This is not really recommended. Feature detection is more foolproof.

	UA String
	This is a browser-reported string that allows the network protocol peers to identify the application type, operating system, software vendor or software version of the requesting software user agent. It can be accessed via navigator.userAgent. 
	
	Feature Detection is to identify the browser's capabilities.
	Feature Inference is guess whether browser has certain feature through others feature or UA string.
	One inappropriate use of feature detection is called feature inference. Feature inference attempts to use multiple features after validating the presence of only one. The presence of one feature is inferred by the presence of another. The problem is, of course, that inference is an assumption rather than a fact, and that can lead to maintenance issues.
	UA String is User-Agent Detection.

Explain Ajax in as much detail as possible.
	AJAX is short for Asynchronous Javascript + XML. The technique consisted of making server requests for additional data without unloading the page,resulting in a better user experience.

	Ajax (asynchronous JavaScript and XML) is a set of web development techniques using many web technologies on the client side to create asynchronous web applications. With Ajax, web applications can send data to and retrieve from a server asynchronously (in the background) without interfering with the display and behavior of the existing page. By decoupling the data interchange layer from the presentation layer, Ajax allows for web pages, and by extension web applications, to change content dynamically without the need to reload the entire page. In practice, modern implementations commonly substitute JSON for XML due to the advantages of being native to JavaScript.
	The XMLHttpRequest API is frequently used for the asynchronous communication or these days, the fetch API.

Explain how JSONP works (and how it's not really Ajax).
	JSONP (JSON with Padding) is a method commonly used to bypass the cross-domain policies in web browsers because Ajax requests from the current page to a cross-origin domain is not allowed.

	JSONP works by making a request to a cross-origin domain via a <script> tag and usually with a callback query parameter, for example: https://example.com?callback=printData. The server will then wrap the data within a function called printData and return it to the client.

	<!-- https://mydomain.com -->
	<script>
	function printData(data) {
	  console.log(`My name is ${data.name}!`);
	}
	</script>

	<script src="https://example.com?callback=printData"></script>
	// File loaded from https://example.com?callback=printData
	printData({ name: 'Yang Shun' });
	The client has to have the printData function in its global scope and the function will be executed by the client when the response from the cross-origin domain is received.

	JSONP can be unsafe and has some security implications. As JSONP is really JavaScript, it can do everything else JavaScript can do, so you need to trust the provider of the JSONP data.

	These days, CORS is the recommended approach and JSONP is seen as a hack.


Have you ever used JavaScript templating? If so, what libraries have you used?
	Yes. Handlebars, Underscore, Lodash, AngularJS and JSX. I disliked templating in AngularJS because it made heavy use of strings in the directives and typos would go uncaught. JSX is favourite as it is closer to JavaScript and there is barely any syntax to be learnt. Nowadays, you can even use ES2015 template string literals as a quick way for creating templates without relying on third-party code.

	const template = `<div>My name is: ${name}</div>`;

Explain "hoisting".
	1.There is a preproccess or precompile in javascript runtime. and 'Hoisting' occur in the preproccess.

	Function declarations and variable declarations are always moved (“hoisted”) invisibly to the top of their containing scope by the JavaScript interpreter. This means that code like this:

	function foo() {
	bar();
	var x = 1;
	}
	is actually interpreted like this:

	function foo() {
	var x;
	bar();
	x = 1;
	}
	
	2. Hoisting is a term used to explain the behavior of variable declarations in your code. Variables declared or initialized with the var keyword will have their declaration "hoisted" up to the top of the current scope. However, only the declaration is hoisted, the assignment (if there is one), will stay where it is. Let's explain with a few examples.

	// var declarations are hoisted.
	console.log(foo); // undefined
	var foo = 1;
	console.log(foo); // 1

	// let/const declarations are NOT hoisted.
	console.log(bar); // ReferenceError: bar is not defined
	let bar = 2;
	console.log(bar); // 2
	Function declarations have the body hoisted while the function expressions (written in the form of variable declarations) only has the variable declaration hoisted.

	// Function Declaration
	console.log(foo); // [Function: foo]
	foo(); // 'FOOOOO'
	function foo() {
	  console.log('FOOOOO');
	}
	console.log(foo); // [Function: foo]

	// Function Expression
	console.log(bar); // undefined
	bar(); // Uncaught TypeError: bar is not a function
	var bar = function() {
	  console.log('BARRRR');
	}
	console.log(bar); // [Function: bar]

Describe event bubbling.
	When an event triggers on a DOM element, it will attempt to handle the event if there is a listener attached, then the event is bubbled up to its parent and the same thing happens. This bubbling occurs up the element's ancestors all the way to the document. Event bubbling is the mechanism behind event delegation.

What's the difference between an "attribute" and a "property"?
	Attributes are defined on the HTML markup but properties are defined on the DOM. To illustrate the difference, imagine we have this text field in our HTML: <input type="text" value="Hello">.

	const input = document.querySelector('input');
	console.log(input.getAttribute('value')); // Hello
	console.log(input.value); // Hello
	But after you change the value of the text field by adding "World!" to it, this becomes:

	console.log(input.getAttribute('value')); // Hello
	console.log(input.value); // Hello World!
	In contrast, the value property doesn't reflect the value attribute. Instead, it's the current value of the input. When the user manually changes the value of the input box, the value property will reflect this change. So if the user inputs "John" into the input box, then:

	theInput.value // returns "John"
	whereas:

	theInput.getAttribute('value') // returns "Name:"
	The value property reflects the current text-content inside the input box, whereas the value attribute contains the initial text-content of the value attribute from the HTML source code.

	So if you want to know what's currently inside the text-box, read the property. If you, however, want to know what the initial value of the text-box was, read the attribute. Or you can use the defaultValue property, which is a pure reflection of the value attribute:

	theInput.value                 // returns "John"
	theInput.getAttribute('value') // returns "Name:"
	theInput.defaultValue          // returns "Name:"
	
	
	In HTML / Javascript the terms get confused because DOM Elements have attributes (per the HTML source) which are backed by properties when those elements are represented as Javascript objects.
	To further confuse things, changes to the properties can sometimes update the attributes.
	For example, changing the element.href property will update the href attribute on the element, and that'll be reflected in a call to element.getAttribute('href').
	However if you subsequently read that property, it will have been normalised to an absolute URL, even though the attribute might be a relative URL!

Why is extending built-in JavaScript objects not a good idea?
	Extending a built-in/native JavaScript object means adding properties/functions to its prototype. While this may seem like a good idea at first, it is dangerous in practice. Imagine your code uses a few libraries that both extend the Array.prototype by adding the same contains method, the implementations will overwrite each other and your code will break if the behavior of these two methods are not the same.

	The only time you may want to extend a native object is when you want to create a polyfill, essentially providing your own implementation for a method that is part of the JavaScript specification but might not exist in the user's browser due to it being an older browser.

Difference between document load event and document DOMContentLoaded event?
	The DOMContentLoaded event is fired when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading.

	window's load event is only fired after the DOM and all dependent resources and assets have loaded.

Difference between document load event and document ready event?	
	ready means DOM is ready.
	load means the page fully loaded. Includes inner frames, images etc.

What is the difference between == and ===?
	== is the abstract equality operator while === is the strict equality operator. The == operator will compare for equality after doing any necessary type conversions. The === operator will not do type conversion, so if two values are not the same type === will simply return false. When using ==, funky things can happen, such as:

	1 == '1' // true
	1 == [1] // true
	1 == true // true
	0 == '' // true
	0 == '0' // true
	0 == false // true
	My advice is never to use the == operator, except for convenience when comparing against null or undefined, where a == null will return true if a is null or undefined.

	var a = null;
	console.log(a == null); // true
	console.log(a == undefined); // true
	
	The == operator will compare for equality after doing any necessary type conversions. The === operator will not do the conversion, so if two values are not the same type === will simply return false. It's this case where === will be faster, and may return a different result than ==. In all other cases performance will be the same.

Explain the same-origin policy with regards to JavaScript.
	The same-origin policy prevents JavaScript from making requests across domain boundaries. An origin is defined as a combination of URI scheme, hostname, and port number. This policy prevents a malicious script on one page from obtaining access to sensitive data on another web page through that page's Document Object Model.

Make this work:
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
	function duplicate(arr) {
	  return arr.concat(arr);
	}

	duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
	
Why is it called a Ternary expression, what does the word "Ternary" indicate?
	"Ternary" indicates three, and a ternary expression accepts three operands, the test condition, the "then" expression and the "else" expression.

What is "use strict";? what are the advantages and disadvantages to using it?
	use strict' is a statement used to enable strict mode to entire scripts or individual functions. Strict mode is a way to opt in to a restricted variant of JavaScript.

	Advantages:

	Makes it impossible to accidentally create global variables.
	Makes assignments which would otherwise silently fail to throw an exception.
	Makes attempts to delete undeletable properties throw (where before the attempt would simply have no effect).
	Requires that function parameter names be unique.
	this is undefined in the global context.
	It catches some common coding bloopers, throwing exceptions.
	It disables features that are confusing or poorly thought out.
	Disadvantages:

	Many missing features that some developers might be used to.
	No more access to function.caller and function.arguments.

Create a for loop that iterates up to 100 while outputting "fizz" at multiples of 3, "buzz" at multiples of 5 and "fizzbuzz" at multiples of 3 and 5
	for (let i = 1; i <= 100; i++) {
	let f = i % 3 == 0, b = i % 5 == 0;
	console.log(f ? (b ? 'FizzBuzz' : 'Fizz') : (b ? 'Buzz' : i));
	}
Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?
	Every script has access to the global scope, and if everyone is using the global namespace to define their own variables, there will bound to be collisions. Use the module pattern (IIFEs) to encapsulate your variables within a local namespace.

Why would you use something like the load event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?
	The load event fires at the end of the document loading process. At this point, all of the objects in the document are in the DOM, and all the images, scripts, links and sub-frames have finished loading.

	The DOM event DOMContentLoaded will fire after the DOM for the page has been constructed, but do not wait for other resources to finish loading. This is preferred in certain cases when you do not need the full page to be loaded before initializing.

Explain what a single page app is and how to make one SEO-friendly.
	SPAs are reliant on JavaScript to render content, but not all search engines execute JavaScript during crawling, and they may see empty content on your page. This inadvertently hurts the Search Engine Optimization (SEO) of your app. However, most of the time, when you are building apps, SEO is not the most important factor, as not all the content needs to be indexable by search engines. To overcome this, you can either server-side render your app or use services such as Prerender to "render your javascript in a browser, save the static HTML, and return that to the crawlers".
	
What is the extent of your experience with Promises and/or their polyfills?
	A promise is an object that may produce a single value some time in the future: either a resolved value, or a reason that it’s not resolved (e.g., a network error occurred). A promise may be in one of 3 possible states: fulfilled, rejected, or pending. Promise users can attach callbacks to handle the fulfilled value or the reason for rejection.

	Promises are eager, meaning that a promise will start doing whatever task you give it as soon as the promise constructor is invoked. 
	
	Some common polyfills are $.deferred, Q and Bluebird but not all of them comply to the specification. ES2015 supports Promises out of the box and polyfills are typically not needed these days.

What are the pros of using Promises instead of callbacks?
	Avoid callback hell which can be unreadable.
	Makes it easy to write sequential asynchronous code that is readable with .then().
	Makes it easy to write parallel asynchronous code with Promise.all().

What tools and techniques do you use debugging JavaScript code?
	React and Redux
	React Devtools
	Redux Devtools
	JavaScript
	Chrome Devtools
	debugger statement
	Good old console.log debugging

What language constructions do you use for iterating over object properties and array items?
	For objects:

	for loops - for (var property in obj) { console.log(property); }. 
	However, this will also iterate through its inherited properties, and you will add an obj.hasOwnProperty(property) check before using it.
	Object.keys() - Object.keys(obj).forEach(function (property) { ... }). 
	Object.keys() is a static method that will lists all enumerable properties of the object that you pass it.
	Object.getOwnPropertyNames() - 
	Object.getOwnPropertyNames(obj).forEach(function (property) { ... }). 
	Object.getOwnPropertyNames() is a static method that will lists all enumerable and non-enumerable properties of the object that you pass it.

	For arrays:
	for loops - for (var i = 0; i < arr.length; i++). 
	The common pitfall here is that var is in the function scope and not the block scope and most of the time you would want block scoped iterator variable. 
	ES2015 introduces let which has block scope and it is recommended to use that instead. 
	So this becomes: for (let i = 0; i < arr.length; i++).
	forEach - arr.forEach(function (el, index) { ... }). 
	This construct can be more convenient at times because you do not have to use the index if all you need is the array elements.
	Most of the time, I would prefer the .forEach method, but it really depends on what you are trying to do. for loops allow more flexibility, such as prematurely terminate the loop using break or incrementing the iterator more than once per loop.

Explain the difference between mutable and immutable objects.
What is an example of an immutable object in JavaScript?
What are the pros and cons of immutability?
How can you achieve immutability in your own code?

Explain the difference between synchronous and asynchronous functions.
	Synchronous functions are blocking while asynchronous functions are not. In synchronous functions, statements complete before the next statement is run. In this case the program is evaluated exactly in order of the statements and execution of the program is paused if one of the statements take a very long time.

	Asynchronous functions usually accept a callback as a parameter and execution continues on the next line immediately after the asynchronous function is invoked. The callback is only invoked when the asynchronous operation is complete and the call stack is empty. Heavy duty operations such as loading data from a web server or querying a database should be done asynchronously so that the main thread can continue executing other operations instead of blocking until that long operation to complete (in the case of browsers, the UI will freeze).

Describe event bubbling.
	Event Flow describles the order in which events are received on the page.An event has three phases to its life cycle: capture, target, and bubbling.
	Event Bubbling mean that an event start at the most specific element(the deepest possible point to the document tree) and then flow upward toward the least specific node(the document);

What is event loop? Can you draw a simple diagram to explain event loop?
Explain Event loop. event loop explained or concurrency model and event loop
asynchronous vs synchronous e	
What is the difference between call stack and task queue

	The Call Stack
	JavaScript has a single call stack in which it keeps track of what function we’re currently executing and what function is to be executed after that. A stack is an array-like data structure but with some limitations you can only add items on top of each other and at any time you can only remove the top one.(e.g ( pile of plates))

	When you’re about to execute a function it is added on the call stack. Then if that function calls another function the other function will be on top of the first one in the call stack. When you get an error in the console you get a long message that shows you the path of execution this is what the stack looked in that exact moment. 

	The Event Table & Event Queue

	Every time you call a setTimeout function or you do some async operation it is added to the Event Table. This is a data structure which knows that a certain function should be triggered after a certain event. Once that event occurs it sends a notice. It’s sole purpose is to keep track of events and send them to the Event Queue.

	The Event Queue is a data structure similar to the stack in which you add items to the back but can only remove them from the front. It kind of stores the correct order in which the functions should be executed. It receives the function calls from the Event Table, but it needs to somehow send them to the Call Stack? This is where the Event Loop comes in.

	The Event Loop
	Event Loop is a constantly running process that checks if the call stack is empty. Imagine it like a clock and every time it ticks it looks at the Call Stack and if it is empty it looks into the Event Queue. If there is something in the event queue that is waiting it is moved to the call stack. If not, then nothing happens.

	The event loop got its name because of how it's usually implemented, which usually resembles:

	while (queue.waitForMessage()) {
	  queue.processNextMessage();
	}
	queue.waitForMessage waits synchronously for a message to arrive if there is none currently.
	The event loop is a single-threaded loop that monitors the call stack and checks if there is any work to be done in the task queue. 

	http://altitudelabs.com/blog/what-is-the-javascript-event-loop/
	https://hackernoon.com/understanding-js-the-event-loop-959beae3ac40


Explain the differences on the usage of foo between function foo() {} and var foo = function() 
	The former is a function declaration while the latter is a function expression. The key difference is that function declarations have its body hoisted but the bodies of function expressions are not (they have the same hoisting behaviour as variables). For more explanation on hoisting, refer to the question above on hoisting. If you try to invoke a function expression before it is defined, you will get an Uncaught TypeError: XXX is not a function error.

	Function Declaration
	foo(); // 'FOOOOO'
	function foo() {
	  console.log('FOOOOO');
	}

	Function Expression
	foo(); // Uncaught TypeError: foo is not a function
	var foo = function() {
	  console.log('FOOOOO');
	}

Explain the "JavaScript module pattern" and when you'd use it.
	Bonus points for mentioning clean namespacing.
	What if your modules are namespace-less?
	The module pattern use a anonymous function that returns a object. 
	Inside of the anonymous function, the private variables and functions are defined first.
	After that, an object literal is returned as the function value. That object literal contains only properties and methods that should be public. Since the object is defined inside the anonymous function, all of the public methods have access to the private variables and functions.

How do you organize your code? (module pattern, classical inheritance?)
	I developing SPA with requirejs and MVC Framework recently. So I organize my code with AMD.

When do you optimize your code?
	For release, we will compress and combine code.
	Whenever I have a time I will review my code and refactor it.

Explain how you would get a query string parameter from the browser window's URL.
	1.var queryString = location.search 2.parse queryString * queryString.split("=") * regExp 3.return specific parameter.

What is JavaScript's this keyword?
	JavaScript's this keyword normally refers to the object that owns the method, but it depends on how a function is called. Basically, it points to the currently in scope object that owns where you are in the code. When working within a Web page, this usually refers to the Window object. If you are in an object created with the new keyword, the this keyword refers to the object being created. When working with event handlers, JavaScript's this keyword will point to the object that generated the event.

======================================================================================================================

What are global variables? How are they declared? What are the problems with using globals?
	Global variables are available throughout your code: that is, the variables have no scope. Local variables scope, on the other hand, is restricted to where it is declared (like within a function). The var keyword is used to declare a local variable or object, while omitting the var keyword creates a global variable.

	Most JavaScript developers avoid globals. One reason why is they're averse to naming conflicts between local and globals, Also, code that depends on globals can be difficult to maintain and test.

	// Declare a local variable
	var localVariable = "TechRepublic"

	// Declare a global

	globalVariable = "CNet"


What are JavaScript types?
	Unlike Java or C#, JavaScript is a loosely-typed language (some call this weakly typed); this means that no type declarations are required when variables are created. Strings and numbers can be intermixed with no worries. JavaScript is smart, so it easily determines what the type should be. The types supported in JavaScript are: Number, String, Boolean, Function, Object, Null, and Undefined.

	var fName = "Mary";   //Declare a String
	var total = 100.32;    //Declare a number


	var fName = new String; //Another way to declare a string


	fName = "Mary";


	var total = new Number;


	var isIt = new Boolean;
	var names = new Array;
	var car = new Object;

What is event bubbling?
	Event bubbling describes the behavior of events in child and parent nodes in the Document Object Model (DOM); that is, all child node events are automatically passed to its parent nodes. The benefit of this method is speed, because the code only needs to traverse the DOM tree once. This is useful when you want to place more than one event listener on a DOM element since you can put just one listener on all of the elements, thus code simplicity and reduction. One application of this is the creation of one event listener on a page's body element to respond to any click event that occurs within the page's body.

How are errors gracefully handled in JavaScript?
	Exceptions that occur at runtime can be handled via try/catch/finally blocks; this allows you to avoid those unfriendly error messages. The finally block is optional, as the bare minimum to use is try/catch. Basically, you try to run code (in the try block between the braces), and execution is transferred to the catch block of code when/if runtime errors occur. When the try/catch block is finally done, code execution transfers to the finally code block. This is the same way it works in other languages like C# and Java.

	try {
	// do something


	} catch (e) {
	// do something with the exception
	} finally {
	// This code block always executes whether there is an exception or not.
	}


How do JavaScript timers work? What is a drawback of JavaScript timers?
	Timers allow you to execute code at a set time or repeatedly using an interval. This is accomplished with the setTimeout, setInterval, and clearInterval functions. 
	The setTimeout(function, delay) function initiates a timer that calls a specific function after the delay; it returns an id value that can be used to access it later. 
	The setInterval(function, delay) function is similar to the setTimeout function except that it executes repeatedly on the delay and only stops when cancelled. 
	The clearInterval(id) function is used to stop a timer. Timers can be tricky to use since they operate within a single thread, thus events queue up waiting to execute.

======================================================================================================================
Javascript Interview Questions - tutorialspoint

What is JavaScript?
	JavaScript is a lightweight, interpreted programming language with object-oriented capabilities that allows you to build interactivity into otherwise static HTML pages.
	The general-purpose core of the language has been embedded in Netscape, Internet Explorer, and other web browsers.

Following are the features of JavaScript -
    JavaScript is a lightweight, interpreted programming language.
    JavaScript is designed for creating network-centric applications.
    JavaScript is open and cross-platform.

What are the advantages of using JavaScript?
    Less server interaction - You can validate user input before sending the page off to the server. This saves server traffic, which means less load on your server.
    Immediate feedback to the visitors - They don't have to wait for a page reload to see if they have forgotten to enter something.
    Increased interactivity - You can create interfaces that react when the user hovers over them with a mouse or activates them via the keyboard.
    Richer interfaces - You can use JavaScript to include such items as drag-and-drop components and sliders to give a Rich Interface to your site visitors.

Is JavaScript a case-sensitive language?
	Yes! JavaScript is a case-sensitive language. This means that language keywords, variables, function names, and any other identifiers must always be typed with a consistent capitalization of letters.

How can you create an Object in JavaScript?
	JavaScript supports Object concept very well. You can create an object using the object literal as follows -
	var emp = {
	   name: "Zara",
	   age: 10
	};

How can you read properties of an Object in JavaScript?
	You can write and read properties of an object using the dot notation as follows -
	// Getting object properties
	emp.name  // ==> Zara
	emp.age   // ==> 10
	// Setting object properties
	emp.name = "Daisy"  // <== Daisy
	emp.age  =  20      // <== 20

How can you create an Array in JavaScript?
	var x = [];
	var y = [1, 2, 3, 4, 5];

How to read elements of an array in JavaScript?
	An array has a length property that is useful for iteration. We can read elements of an array as follows -
	var x = [1, 2, 3, 4, 5];
	for (var i = 0; i < x.length; i++) {
	   // Do something with x[i]
	}

Named and anonymous function.
	A named function has a name when it is defined. A named function can be defined using function keyword as follows -
	function named(){
	   // do some stuff here
	}
	An anonymous function can be defined in similar way as a normal function but it would not have any name.
	An anonymous function can be assigned to a variable.An anonymous function can be passed as an argument to another function.

What is arguments object in JavaScript? How can you get the type of arguments passed to a function?
	JavaScript variable arguments represents the arguments passed to a function.Using typeof operator, we can get the type of arguments passed to a function. 
	Using arguments.length property, we can get the total number of arguments passed to a function

	function func(x){
	   console.log(typeof x, arguments.length);
	}
	func();                //==> "undefined", 0
	func(1);               //==> "number", 1
	func("1", "2", "3");   //==> "string", 3

How can you get the reference of a caller function inside a function?
	The arguments object has a callee property, which refers to the function you're inside of. 
	function func() {
	   return arguments.callee; 
	}
	func();                // ==> func

Variable Scope	
The scope of a variable is the region of your program in which it is defined. 
JavaScript variable will have only two scopes.
    Global Variables - A global variable has global scope which means it is visible everywhere in your JavaScript code.
    Local Variables - A local variable will be visible only within a function where it is defined. Function parameters are always local to that function.
	A local variable takes precedence over a global variable with the same name.

What is callback?
	A callback is a plain JavaScript function passed to some method as an argument or option. Some callbacks are just events, called to give the user a chance to react when a certain state is triggered.

Built In Methods:

charAt() method returns the character at the specified index.
concat() method combines the text of two strings and returns a new string.
forEach() method calls a function for each element in the array.
indexOf() method returns the index within the calling String object of the first occurrence of the specified value, or -1 if not found.
length() method returns the length of the string.
pop() method removes the last element from an array and returns that element.
push() method adds one or more elements to the end of an array and returns the new length of the array.
reverse() method reverses the order of the elements of an array -- the first becomes the last, and the last becomes the first.
sort() method sorts the elements of an array.
substr() method returns the characters in a string beginning at the specified location through the specified number of characters.
toLowerCase() method returns the calling string value converted to lower case.
toUpperCase() method returns the calling string value converted to upper case.
toString() method returns the string representation of the number's value.

What are the variable naming conventions in JavaScript?
	You should not use any of the JavaScript reserved keyword as variable name. 
	JavaScript variable names should not start with a numeral (0-9). They must begin with a letter or the underscore character. For example, 123test is an invalid variable name but _123test is a valid one.
	JavaScript variable names are case sensitive. For example, Name and name are two different variables.

How typeof operator works?
	The typeof is a unary operator that is placed before its single operand, which can be of any type. Its value is a string indicating the data type of the operand.
	The typeof operator evaluates to "number", "string", or "boolean" if its operand is a number, string, or boolean value and returns true or false based on the evaluation.

What typeof returns for a null value?
	It returns "object".

How to print a web page using javascript?
	JavaScript helps you to implement this functionality using print function of window object. The JavaScript print function window.print() will print the current web page when executed.

What is Date object in JavaScript?
	The Date object is a datatype built into the JavaScript language. Date objects are created with the new Date( ).
	Once a Date object is created, a number of methods allow you to operate on it. Most methods simply allow you to get and set the year, month, day, hour, minute, second, and millisecond fields of the object, using either local time or UTC (universal, or GMT) time.

What is Number object in JavaScript?
	The Number object represents numerical date, either integers or floating-point numbers. In general, you do not need to worry about Number objects because the browser automatically converts number literals to instances of the number class.
	Creating a number object -
	var val = new Number(number);
	If the argument cannot be converted into a number, it returns NaN (Not-a-Number).

How to handle exceptions in JavaScript?
	The latest versions of JavaScript added exception handling capabilities. JavaScript implements the try...catch...finally construct as well as the throw operator to handle exceptions.

	You can catch programmer-generated and runtime exceptions, but you cannot catch JavaScript syntax errors.

What is purpose of onError event handler in JavaScript?
	The onerror event handler was the first feature to facilitate error handling for JavaScript. The error event is fired on the window object whenever an exception occurs on the page.
	The onerror event handler provides three pieces of information to identify the exact nature of the error -
    Error message - The same message that the browser would display for the given error.
    URL - The file in which the error occurred.
    Line number - The line number in the given URL that caused the error.

TODO

Describe a strategy for memoization (avoiding calculation repetition) in JavaScript
How could you implement cache to save calculation time for a recursive fibonacci function?
How could you cache execution of any function?

Memoization is an optimization technique used primarily to speed up computer programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again.

Memoizing in simple terms means memorizing or storing in memory. A memoized function is usually faster because if the function is called subsequently with the previous value(s), then instead of executing the function, we would be fetching the result from the cache.

// a simple pure function to get a value adding 10
const add = (n) => (n + 10);
console.log('Simple call', add(3));
// a simple memoize function that takes in a function
// and returns a memoized function
const memoize = (fn) => {
  let cache = {};
  return (...args) => {
    let n = args[0];  // just taking one argument here
    if (n in cache) {
      console.log('Fetching from cache');
      return cache[n];
    }
    else {
      console.log('Calculating result');
      let result = fn(n);
      cache[n] = result;
      return result;
    }
  }
}
// creating a memoized function for the 'add' pure function
const memoizedAdd = memoize(add);
console.log(memoizedAdd(3));  // calculated
console.log(memoizedAdd(3));  // cached
console.log(memoizedAdd(4));  // calculated
console.log(memoizedAdd(4));  // cached

What are the differences between variables created using let, var or const?
	var-let-const-doc attached.
	http://wesbos.com/let-vs-const/
	http://wesbos.com/javascript-scoping/

How would you make sure value of this works correctly inside setTimeout?
	
	It's because this in the setTimeout handler is referring to window, which is presumably not the same value as referenced by this outside the handler.

	Option 1: Cache the outer value, and use it inside...
	
	var self = this;

	var timer3 = setTimeout((function() {
		self.alive = true;
		Console.log("alive!");
	}),3000);

	2: use ES5 Function.prototype.bind

	var timer3 = setTimeout((function() {
		this.alive = true;
		Console.log("alive!");
	}.bind(this)),3000);


	3: ES6 Arrow function
	var timer3 = setTimeout(()=>{
		this.alive = true;
		Console.log("alive!");
	},3000);
	Because there's no binding of this in Arrow functions.

What is debounce and how could you implement debounce
	Debounce function limits the rate at which a function can fire. A quick example:  you have a resize listener on the window which does some element dimension calculations and (possibly)  repositions a few elements.  That isn't a heavy task in itself but being repeatedly fired after numerous resizes will really slow your site down.  Why not limit the rate at which the function can fire?

	Here's the basic JavaScript debounce function (as taken from Underscore.js):

	// Returns a function, that, as long as it continues to be invoked, will not
	// be triggered. The function will be called after it stops being called for
	// N milliseconds. If `immediate` is passed, trigger the function on the
	// leading edge, instead of the trailing.
	function debounce(func, wait, immediate) {
		var timeout;
		return function() {
			var context = this, args = arguments;
			var later = function() {
				timeout = null;
				if (!immediate) func.apply(context, args);
			};
			var callNow = immediate && !timeout;
			clearTimeout(timeout);
			timeout = setTimeout(later, wait);
			if (callNow) func.apply(context, args);
		};
	};
	You'll pass the debounce function the function to execute and the fire rate limit in milliseconds.  Here's an example usage:

	var myEfficientFn = debounce(function() {
		// All the taxing stuff you do
	}, 250);

	window.addEventListener('resize', myEfficientFn);
	The function above will only fire once every quarter of a second instead of as quickly as it's triggered; an incredible performance boost in some cases.


When you click on an element, what is the value of "this" inside the click handler?
	The value of this within the handler

	If attaching a handler function to an element using addEventListener(), the value of this inside the handler is a reference to the element. It is the same as the value of the currentTarget property of the event argument that is passed to the handler.

	If an event attribute (for example, onclick) is specified on an element in the HTML source, the JavaScript code in the attribute value is effectively wrapped in a handler function which binds the value of this in a manner consistent with the addEventListener(); an occurrence of this within the code represents a reference to the element. 

	var someInput = document.querySelector('input');
	someInput.addEventListener('click', myFunc, false);
	someInput.myParam = 'This is my parameter';
	function myFunc(evt)
	{
	  window.alert( evt.target.myParam );
	}

How could you create Custom events? 
	Creating custom events
	Events can be created with the Event constructor as follows:

	var event = new Event('build');
	// Listen for the event.
	elem.addEventListener('build', function (e) { ... }, false);
	// Dispatch the event.
	elem.dispatchEvent(event);

	This constructor is supported in most modern browsers (with Internet Explorer being the exception). For a more verbose approach (which works with Internet Explorer), see the old-fashioned way below.

	Adding custom data – CustomEvent()

	To add more data to the event object, the CustomEvent interface exists and the detail property can be used to pass custom data.For example, the event could be created as follows:

	var event = new CustomEvent('build', { detail: elem.dataset.time });
	This will then allow you to access the additional data in the event listener:

	function eventHandler(e) {
	  console.log('The time is: ' + e.detail);
	}


	The old-fashioned way

	// Create the event.
	var event = document.createEvent('Event');

	// Define that the event name is 'build'.
	event.initEvent('build', true, true);

	// Listen for the event.
	elem.addEventListener('build', function (e) {
	  // e.target matches elem
	}, false);

	// target can be any Element or other EventTarget.
	elem.dispatchEvent(event);


	Triggering built-in events
	This example demonstrates simulating a click (that is programmatically generating a click event) on a checkbox using DOM methods. View the example in action.

	function simulateClick() {
	  var event = new MouseEvent('click', {
		view: window,
		bubbles: true,
		cancelable: true
	  });
	  var cb = document.getElementById('checkbox'); 
	  var cancelled = !cb.dispatchEvent(event);
	  if (cancelled) {
		// A handler called preventDefault.
		alert("cancelled");
	  } else {
		// None of the handlers called preventDefault.
		alert("not cancelled");
	  }
	}

What is Big O notation, and why is it useful?
	Big O Notation in short, is the mathematical expression of how long an algorithm takes to run depending on how long is the input, usually talking about the worst case scenario.
	
	In practice, we use Big O Notation to classify algorithms by how they respond to changes in input size, so algorithms with the same growth rate are represented with the same Big O Notation. The letter O is used because the rate of growth of a function is also called order of the function.

What is the DOM?
	The Document Object Model (DOM) is a programming API for HTML and XML documents. It defines the logical structure of documents and the way a document is accessed and manipulated. In the DOM specification, the term "document" is used in the broad sense - increasingly, XML is being used as a way of representing many different kinds of information that may be stored in diverse systems, and much of this would traditionally be seen as data rather than as documents. Nevertheless, XML presents this data as documents, and the DOM may be used to manage this data.

	With the Document Object Model, programmers can create and build documents, navigate their structure, and add, modify, or delete elements and content. Anything found in an HTML or XML document can be accessed, changed, deleted, or added using the Document Object Model, with a few exceptions - in particular, the DOM interfaces for the internal subset and external subset have not yet been specified.

How would you compare two objects in JavaScript?
	
If you want to use an arbitrary object as value of this, how will you do that?

Does JavaScript pass parameter by value or by reference?

How would you implement currying for any functions?

=====================================================================================================================
Concepts
--------

What are the differences between functional and imperative programming styles, and explain your preference, if any.

Scope - Functions as first class objects, closures, function vs block scoping. Understand the difference between global scope, function scope, and block scope. Understand which variables are available where. Know how the JavaScript engine performs a variable lookup.

AJAX - just general stuff, how you've used it, etc, not really super technical questions here

Web Security - same origin policy, Cross Site Scripting, Cross Site Request Forgery, cookies (secure flag, http only flag, what to store in cookies, what not to, etc), basic session based security

REST API design - given a random data model that I come up with on the spot, design a good API for it

Troubleshooting techniques - race conditions, developer tools (firebug, chrome dev tools, windows script debugger), understanding that breakpoints affect the behavior of your code, proxy tools (Fiddler or the like), understanding that proxies can affect the behavior of your code (fiddler mishandles edge cases of content-encoding chunked, as an example), wireshark or similar

Code organization and Dependency management - what do you do, are you familiar with AMD/require.js, commonjs, or es6 modules?

Array functions - map/reduce/filter/reduce/sort/etc

New ES6 features - not make-or-break, but it's good to know, babel transpiler experience also good.

Build tools - webpack, grunt, etc

Server side js - node.js, npm experience

Value vs. Reference?—?Understand how objects, arrays, and functions are copied and passed into functions. Know that the reference is what's being copied. Understand that primitives are copied and passed by copying the value.

Hoisting?—?Understand that variable and function declarations are hoisted to the top of their available scope. Understand that function expressions are not hoisted.

Closures?—?Know that a function retains access to the scope that it was created in. Know what this lets us do, such as data hiding, memoization, and dynamic function generation.

this?—?Know the rules of this binding. Know how it works, know how to figure out what it will be equal to in a function, and know why it’s useful.

new?—?Know how it relates to object oriented programming. Know what happens to a function called with new. Understand how the object generated by using new inherits from the function’s prototype property.

apply, call, bind Know how each of these functions work. Know how to use them. Know what they do to this.

Prototypes & Inheritance Understand that inheritance in JavaScript works through the [[Prototype]] chain. Understand how to set up inheritance through functions and objects and how new helps us implement it. Know what the __proto__ and prototype properties are and what they do.

Asynchronous JS Understand the event loop. Understand how the browser deals with user input, web requests, and events in general. Know how to recognize and correctly implement asynchronous code. Understand how JavaScript is both asynchronous and single-threaded.

Higher Order Functions Understand that functions are first-class objects in JavaScript and what that means. Know that returning a function from another function is perfectly legal. Understand the techniques that closures and higher order functions allow us to use.

=====================================================================================================================
System design

Describe a few ways to communicate between a server and a client. Describe how a few network protocols work at a high level (IP, TCP, HTTP/S/2, UDP, RTC, DNS, etc.)
What is REST, and why do people use it?
My website is slow. Walk me through diagnosing and fixing it. 
What are some performance optimizations people use, and when should they be used?

Talk through a full stack implemention of an autocomplete widget. 
A user can type text into it, and get back results from a server. How would you design a frontend to support the following features:
Fetch data from a backend API
Render results as a tree (items can have parents/children - it’s not just a flat list)
Support for checkbox, radio button, icon, and regular list items - items come in many forms
What perf considerations are there for complete-as-you-type behavior? Are there any edge cases (for example, if the user types fast and the network is slow)?
How would you design the network stack and backend in support of fast performance: how do your client/server communicate? How is your data stored on the backend? How do these approaches scale to lots of data and lots of clients?

How do you fetch and render tweets?
How do you update tweets as new ones come in? How do you know when new ones came in?
How do you search tweets? How do you search by author? Talk me through your database, backend, and API designs
=================================================================================================================
Reference

https://github.com/bcherny/frontend-interview-questions
https://performancejs.com/post/hde6d32/The-Best-Frontend-JavaScript-Interview-Questions-(written-by-a-Frontend-Engineer)
https://dev.to/arnavaggarwal/10-javascript-concepts-you-need-to-know-for-interviews
For coding questions (https://github.com/narasimhanc/front-end-Interview-Questions)
==================================================================================================================
