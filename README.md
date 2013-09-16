js-style-guide
==============

GRT Javascript Style Guide


Sources:
 - https://github.com/airbnb/javascript
 - http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml
 - http://doc.owncloud.org/server/5.0/developer_manual/angular.html
 - https://github.com/rwaldron/idiomatic.js/


AngularJS
==============
Environment
--------------

General Style Guidelines
-------------------
For readability, controllers, services, directives and methods added to modules should be declared at the top of the page seperate from their implementations.

	//bad
	var myApp = angular.module('myApp',[]);
	 
	myApp.controller('GreetingCtrl', ['$scope', function($scope) {
	    $scope.greeting = 'Hola!';
	}]);

	//good
	var myApp = angular.module('myApp',[]);
	 
	myApp.controller('GreetingCtrl', GreetingCtrl);
	
	...
	
	function GreetingCtrl($scope){
		$scope.greeting = 'Hola!';
	}



Modules
--------------

Controllers
--------------


Models & Services
--------------


Directives
--------------


Filters
--------------
A simple filter:

	var app = angular.module('YourApp');

	app.filter('biggerThanX', biggerThanX);

	function biggerThanX(){
		
		var biggerThanX = function(elements, x){
			result = [];
			for(var i=0; i<elements.length; i++){
			  var elem = elements[i];
			  if(elem.someNumber > x){
			      results.push(elem);
			  }
			}
			return result;
		};
	
		return biggerThanX;
	}


Fiters are used like Unix pipes in Bash

	<ul ng-controller="SomeController">
	    <li ng-repeat="item in items | biggerThanX:5">{{ item.someNumber }}</li>
	</ul>

Requests
--------------
**Restangular**
Restangular is the preferred AngularJS service to handle Rest API Restful Resources


**$http**


**ngResource**



Javascript
==============

Variables
------------

**Always declare new variables with var.**
When you fail to specify var, the variable gets placed in the global context, potentially clobbering existing values. Also, if there's no declaration, it's hard to tell in what scope a variable lives (e.g., it could be in the Document or Window just as easily as in the local scope). So always declare with var.

	// bad
	superPower = new SuperPower();

	// good
	var superPower = new SuperPower();
	Use one var declaration for multiple variables and declare each variable on a newline.

	// bad
	var items = getItems();
	var goSportsTeam = true;
	var dragonball = 'z';

	// good
	var items = getItems(),
	    goSportsTeam = true,
	    dragonball = 'z';

**Declare unassigned variables last.** 
This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

	// bad
	var i, len, dragonball,
	    items = getItems(),
	    goSportsTeam = true;

	// bad
	var i, items = getItems(),
	    dragonball,
	    goSportsTeam = true,
	    len;

	
	
**Assign variables at the top of their scope.** This helps avoid issues with variable declaration and assignment hoisting related issues.

	// bad
	function() {
		test();
		console.log('doing stuff..');

  	//..other stuff..
	
	  var name = getName();
	
	  if (name === 'test') {
	    return false;
	  }
	
	  return name;
	}

	// good
	function() {
	  var name = getName();
	
	  test();
	  console.log('doing stuff..');
	
	  //..other stuff..
	
	  if (name === 'test') {
	    return false;
	  }
	
	  return name;
	}

	// bad
	function() {
	  var name = getName();
	
	  if (!arguments.length) {
	    return false;
	  }
	
	  return true;
	}

	// good
	function() {
	  if (!arguments.length) {
	    return false;
	  }
	
	  var name = getName();
	
	  return true;
	}




Hoisting
--------------------------

Variable declarations get hoisted to the top of their scope, their assignment does not.

	// we know this wouldn't work (assuming there
	// is no notDefined global variable)
	function example() {
	  console.log(notDefined); // => throws a ReferenceError
	}

	// creating a variable declaration after you
	// reference the variable will work due to
	// variable hoisting. Note: the assignment
	// value of `true` is not hoisted.
	function example() {
	  console.log(declaredButNotAssigned); // => undefined
	  var declaredButNotAssigned = true;
	}

	// The interpreter is hoisting the variable
	// declaration to the top of the scope.
	// Which means our example could be rewritten as:
	function example() {
	  var declaredButNotAssigned;
	  console.log(declaredButNotAssigned); // => undefined
	  declaredButNotAssigned = true;
	}

Anonymous function expressions hoist their variable name, but not the function assignment.

	function example() {
	  console.log(anonymous); // => undefined
	
	  anonymous(); // => TypeError anonymous is not a function
	
	  var anonymous = function() {
	    console.log('anonymous function expression');
	  };
	}
	
Named function expressions hoist the variable name, not the function name or the function body.

	function example() {
	  console.log(named); // => undefined
	
	  named(); // => TypeError named is not a function
	
	  superPower(); // => ReferenceError superPower is not defined
	
	  var named = function superPower() {
	    console.log('Flying');
	  };
	
	  // the same is true when the function name
	  // is the same as the variable name.
	  function example() {
	    console.log(named); // => undefined
	
	    named(); // => TypeError named is not a function
	
	    var named = function named() {
	      console.log('named');
	    };
	  }
	}
	
Function declarations hoist their name and the function body.

	function example() {
	  superPower(); // => Flying
	
	  function superPower() {
	    console.log('Flying');
	  }
	}





Constants
------------------------------

**Use NAMES_LIKE_THIS for constant values.**
Use @const to indicate a constant (non-overwritable) pointer (a variable or property).
Never use the const keyword as it not supported in Internet Explorer.





Semicolons
-------------------------------

**Always use semicolons.**
Relying on implicit insertion can cause subtle, hard to debug problems. Don't do it. 

JavaScript requires statements to end with a semicolon, except when it thinks it can safely infer their existence. In each of these examples, a function declaration or object or array literal is used inside a statement. The closing brackets are not enough to signal the end of the statement. Javascript never ends a statement if the next token is an infix or bracket operator.


	// bad
	(function() {
	  var name = 'Skywalker'
	  return name
	})()

	// good
	(function() {
	  var name = 'Skywalker';
	  return name;
	})();

	// good
	;(function() {
	  var name = 'Skywalker';
	  return name;
	})();


Clarification: Semicolons and functions
------------------------------------------------
Semicolons should be included at the end of function expressions, but not at the end of function declarations. The distinction is best illustrated with an example:

	var foo = function() {
	  return true;
	};  // semicolon here.

	function foo() {
	  return true;
	}  // no semicolon here.


Objects
-----------------------------
**Use the literal syntax for object creation.**

	// bad
	var item = new Object();

	// good
	var item = {};

**Don't use reserved words as keys. It won't work in IE8. More info**

	// bad
	var superman = {
	  default: { clark: 'kent' },
	  private: true
	};

	// good
	var superman = {
	  defaults: { clark: 'kent' },
	  hidden: true
	};

**Use readable synonyms in place of reserved words.**

	// bad
	var superman = {
	  class: 'alien'
	};

	// bad
	var superman = {
	  klass: 'alien'
	};

	// good
	var superman = {
	  type: 'alien'
	};





Arrays
--------------------------
**Use the literal syntax for array creation**

	// bad
	var items = new Array();

	// good
	var items = [];
	If you don't know array length use Array#push.

	var someStack = [];

	// bad
	someStack[someStack.length] = 'abracadabra';

	// good
	someStack.push('abracadabra');
	When you need to copy an array use Array#slice. jsPerf

	var len = items.length,
	    itemsCopy = [],
	    i;

	// bad
	for (i = 0; i < len; i++) {
	  itemsCopy[i] = items[i];
	}

	// good
	itemsCopy = items.slice();
	To convert an array-like object to an array, use Array#slice.

	function trigger() {
	  var args = Array.prototype.slice.call(arguments);
	  ...
	}




Strings
---------------------------
**Use single quotes '' for strings**

	// bad
	var name = "Bob Parr";

	// good
	var name = 'Bob Parr';

	// bad
	var fullName = "Bob " + this.lastName;

	// good
	var fullName = 'Bob ' + this.lastName;
	Strings longer than 80 characters should be written across multiple lines using string concatenation.


	// bad
	var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think 	about how Batman had anything to do with this, you would get nowhere fast.';

	// bad
	var errorMessage = 'This is a super long error that \
	was thrown because of Batman. \
	When you stop to think about \
	how Batman had anything to do \
	with this, you would get nowhere \
	fast.';

	// good
	var errorMessage = 'This is a super long error that ' +
	  'was thrown because of Batman.' +
	  'When you stop to think about ' +
	  'how Batman had anything to do ' +
	  'with this, you would get nowhere ' +
	  'fast.';

**When programatically building up a string, use Array#join instead of string concatenation.**

	var items,
	    messages,
	    length, i;

	messages = [{
	    state: 'success',
	    message: 'This one worked.'
	},{
	    state: 'success',
	    message: 'This one worked as well.'
	},{
	    state: 'error',
	    message: 'This one did not work.'
	}];

	length = messages.length;

	// bad
	function inbox(messages) {
	  items = '<ul>';
	
	  for (i = 0; i < length; i++) {
	    items += '<li>' + messages[i].message + '</li>';
	  }
	
	  return items + '</ul>';
	}

	// good
	function inbox(messages) {
	  items = [];
	
	  for (i = 0; i < length; i++) {
	    items[i] = messages[i].message;
	  }
	
	  return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
	}




Functions
-----------------------------
**Function expressions:**

	// anonymous function expression
	var anonymous = function() {
	  return true;
	};

	// named function expression
	var named = function named() {
	  return true;
	};

	// immediately-invoked function expression (IIFE)
	(function() {
	  console.log('Welcome to the Internet. Please follow me.');
	})();
	
**Never declare a function in a non-function block (if, while, etc).** Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

Note: ECMA-262 defines a block as a list of statements. A function declaration is not a statement. Read ECMA-262's note on this issue.

	// bad
	if (currentUser) {
	  function test() {
	    console.log('Nope.');
	  }
	}

	// good
	if (currentUser) {
	  var test = function test() {
	    console.log('Yup.');
	  };
	}

**Never name a parameter arguments**, this will take precedence over the arguments object that is given to every function scope.

    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }
    
    // good
    function yup(name, options, args) {
      // ...stuff...
    }



Properties
-----------------------------
**Use dot notation when accessing properties.**

	var luke = {
	  jedi: true,
	  age: 28
	};

	// bad
	var isJedi = luke['jedi'];

	// good
	var isJedi = luke.jedi;
	Use subscript notation [] when accessing properties with a variable.

	var luke = {
	  jedi: true,
	  age: 28
	};

	function getProp(prop) {
	  return luke[prop];
	}

	var isJedi = getProp('jedi');





Conditional Expressions & Equality
--------------------------------------
**Use === and !== over == and !=.**
Conditional expressions are evaluated using coercion with the ToBoolean method and always follow these simple rules:

Objects evaluate to true
Undefined evaluates to false
Null evaluates to false
Booleans evaluate to the value of the boolean
Numbers evalute to false if +0, -0, or NaN, otherwise true
Strings evaluate to false if an empty string '', otherwise true
	if ([0]) {
	  // true
	  // An array is an object, objects evaluate to true
	}
Use shortcuts.

	// bad
	if (name !== '') {
	  // ...stuff...
	}

	// good
	if (name) {
	  // ...stuff...
	}

	// bad
	if (collection.length > 0) {
	  // ...stuff...
	}

	// good
	if (collection.length) {
	  // ...stuff...
	}
	

Some more examples:

	'' == '0'           // false
	0 == ''             // true
	0 == '0'            // true
	
	false == 'false'    // false
	false == '0'        // true
	
	false == undefined  // false
	false == null       // false
	null == undefined   // true
	
	' \t\r\n ' == 0     // true



Blocks
---------------------
**Use braces with all multi-line blocks.**

	// bad
	if (test)
	  return false;

	// good
	if (test) return false;

	// good
	if (test) {
	  return false;
	}

	// bad
	function() { return false; }

	// good
	function() {
	  return false;
	}




Comments
---------------------------
Use /** ... */ for multiline comments. Include a description, specify types and values for all parameters and return values.

	// bad
	// make() returns a new element
	// based on the passed in tag name
	//
	// @param <String> tag
	// @return <Element> element
	function make(tag) {
	
	  // ...stuff...
	
	  return element;
	}

	// good
	/**
	 * make() returns a new element
	 * based on the passed in tag name
	 *
	 * @param <String> tag
	 * @return <Element> element
	 */
	function make(tag) {
	
	  // ...stuff...
	
	  return element;
	}

Use // for single line comments. Place single line comments on a newline above the subject of the comment. Put an emptyline before the comment.

	// bad
	var active = true;  // is current tab

	// good
	// is current tab
	var active = true;

	// bad
	function getType() {
	  console.log('fetching type...');
	  // set the default type to 'no type'
	  var type = this._type || 'no type';
	
	  return type;
	}

	// good
	function getType() {
	  console.log('fetching type...');
	
	  // set the default type to 'no type'
	  var type = this._type || 'no type';
	
	  return type;
	}

Prefixing your comments with FIXME or TODO helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are FIXME -- need to figure this out or TODO -- need to implement.

**Use // FIXME: to annotate problems**

	function Calculator() {
	
	  // FIXME: shouldn't use a global here
	  total = 0;
	
	  return this;
	}
	
**Use // TODO: to annotate solutions to problems**

	function Calculator() {
	
	  // TODO: total should be configurable by an options param
	  this.total = 0;
	
	  return this;
	}




Whitespace
-----------------------------
**Use soft tabs set to 2 spaces**

	// bad
	function() {
	∙∙∙∙var name;
	}

	// bad
	function() {
	∙var name;
	}

	// good
	function() {
	∙∙var name;
	}

**Place 1 space before the leading brace.**

	// bad
	function test(){
	  console.log('test');
	}

	// good
	function test() {
	  console.log('test');
	}

	// bad
	dog.set('attr',{
	  age: '1 year',
	  breed: 'Bernese Mountain Dog'
	});

	// good
	dog.set('attr', {
	  age: '1 year',
	  breed: 'Bernese Mountain Dog'
	});
	
**Place an empty newline at the end of the file.**

	// bad
	(function(global) {
	  // ...stuff...
	})(this);
	// good
	(function(global) {
	  // ...stuff...
	})(this);

**Use indentation when making long method chains.**

	// bad
	$('#items').find('.selected').highlight().end().find('.open').updateCount();

	// good
	$('#items')
	  .find('.selected')
	    .highlight()
	    .end()
	  .find('.open')
	    .updateCount();

	// bad
	var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
	    .attr('width',  (radius + margin) * 2).append('svg:g')
	    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
	    .call(tron.led);

	// good
	var leds = stage.selectAll('.led')
	    .data(data)
	  .enter().append('svg:svg')
	    .class('led', true)
	    .attr('width',  (radius + margin) * 2)
	  .append('svg:g')
	    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
	    .call(tron.led);




Commas
------------------------
**No leading commas.**

	// bad
	var once
	  , upon
	  , aTime;

	// good
	var once,
	    upon,
	    aTime;

	// bad
	var hero = {
	    firstName: 'Bob'
	  , lastName: 'Parr'
	  , heroName: 'Mr. Incredible'
	  , superPower: 'strength'
	};

	// good
	var hero = {
	  firstName: 'Bob',
	  lastName: 'Parr',
	  heroName: 'Mr. Incredible',
	  superPower: 'strength'
	};

**No trailing commas**

	// bad
	var hero = {
	  firstName: 'Kevin',
	  lastName: 'Flynn',
	};
	
	var heroes = [
	  'Batman',
	  'Superman',
	];
	
	// good
	var hero = {
	  firstName: 'Kevin',
	  lastName: 'Flynn'
	};
	
	var heroes = [
	  'Batman',
	  'Superman'
	];



Type Casting & Coercion
--------------------------------
**Perform type coercion at the beginning of the statement.**
Strings:

	//  => this.reviewScore = 9;

	// bad
	var totalScore = this.reviewScore + '';

	// good
	var totalScore = '' + this.reviewScore;

	// bad
	var totalScore = '' + this.reviewScore + ' total score';

	// good
	var totalScore = this.reviewScore + ' total score';

**Use parseInt for Numbers and always with a radix for type casting.**

	var inputValue = '4';

	// bad
	var val = new Number(inputValue);

	// bad
	var val = +inputValue;

	// bad
	var val = inputValue >> 0;

	// bad
	var val = parseInt(inputValue);

	// good
	var val = Number(inputValue);

	// good
	var val = parseInt(inputValue, 10);



Naming Conventions
-------------------------------
**Avoid single letter names. Be descriptive with your naming.**

	// bad
	function q() {
	  // ...stuff...
	}
	
	// good
	function query() {
	  // ..stuff..
	}
	
**Use camelCase when naming objects, functions, and instances**

	// bad
	var OBJEcttsssss = {};
	var this_is_my_object = {};
	var this-is-my-object = {};
	function c() {};
	var u = new user({
	  name: 'Bob Parr'
	});

	// good
	var thisIsMyObject = {};
	function thisIsMyFunction() {};
	var user = new User({
	  name: 'Bob Parr'
	});

**Use PascalCase when naming constructors or classes**

	// bad
	function user(options) {
	  this.name = options.name;
	}

	var bad = new user({
	  name: 'nope'
	});

	// good
	function User(options) {
	  this.name = options.name;
	}

	var good = new User({
	  name: 'yup'
	});

**Use a leading underscore _ when naming private properties**

	// bad
	this.__firstName__ = 'Panda';
	this.firstName_ = 'Panda';

	// good
	this._firstName = 'Panda';
	When saving a reference to this use _this.

	// bad
	function() {
	  var self = this;
	  return function() {
	    console.log(self);
	  };
	}

	// bad
	function() {
	  var that = this;
	  return function() {
	    console.log(that);
	  };
	}

	// good
	function() {
	  var _this = this;
	  return function() {
	    console.log(_this);
	  };
	}

**Name your functions.** This is helpful for stack traces.

	// bad
	var log = function(msg) {
	  console.log(msg);
	};

	// good
	var log = function log(msg) {
	  console.log(msg);
	};




Accessors
----------------------------
Accessor functions for properties are not required

**If you do make accessor functions use getVal() and setVal('hello')**

	// bad
	dragon.age();

	// good
	dragon.getAge();

	// bad
	dragon.age(25);

	// good
	dragon.setAge(25);

**If the property is a boolean, use isVal() or hasVal()**

	// bad
	if (!dragon.age()) {
	  return false;
	}

	// good
	if (!dragon.hasAge()) {
	  return false;
	}

It's okay to create get() and set() functions, but be consistent.

	function Jedi(options) {
	  options || (options = {});
	  var lightsaber = options.lightsaber || 'blue';
	  this.set('lightsaber', lightsaber);
	}

	Jedi.prototype.set = function(key, val) {
	  this[key] = val;
	};

	Jedi.prototype.get = function(key) {
	  return this[key];
	};





Constructors
---------------------------
Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

	function Jedi() {
	  console.log('new jedi');
	}

	// bad
	Jedi.prototype = {
	  fight: function fight() {
	    console.log('fighting');
	  },
	
	  block: function block() {
	    console.log('blocking');
	  }
	};

	// good
	Jedi.prototype.fight = function fight() {
	  console.log('fighting');
	};

	Jedi.prototype.block = function block() {
	  console.log('blocking');
	};
	
Methods can return this to help with method chaining.

	// bad
	Jedi.prototype.jump = function() {
	  this.jumping = true;
	  return true;
	};

	Jedi.prototype.setHeight = function(height) {
	  this.height = height;
	};

	var luke = new Jedi();
	luke.jump(); // => true
	luke.setHeight(20) // => undefined

	// good
	Jedi.prototype.jump = function() {
	  this.jumping = true;
	  return this;
	};

	Jedi.prototype.setHeight = function(height) {
	  this.height = height;
	  return this;
	};

	var luke = new Jedi();
	
	luke.jump()
	  .setHeight(20);



Events
----------------------------
When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

	// bad
	$(this).trigger('listingUpdated', listing.id);
	
	...
	
	$(this).on('listingUpdated', function(e, listingId) {
	  // do something with listingId
	});

prefer:

	// good
	$(this).trigger('listingUpdated', { listingId : listing.id });
	
	...
	
	$(this).on('listingUpdated', function(e, data) {
	  // do something with data.listingId
	});




jQuery
-------------------------------
**Prefix jQuery object variables with a $.**

	// bad
	var sidebar = $('.sidebar');

	// good
	var $sidebar = $('.sidebar');

Cache jQuery lookups.

	// bad
	function setSidebar() {
	  $('.sidebar').hide();
	
	  // ...stuff...
	
	  $('.sidebar').css({
	    'background-color': 'pink'
	  });
	}

	// good
	function setSidebar() {
	  var $sidebar = $('.sidebar');
	  $sidebar.hide();
	
	  // ...stuff...
	
	  $sidebar.css({
	    'background-color': 'pink'
	  });
	}

For DOM queries use Cascading $('.sidebar ul') or parent > child $('.sidebar > ul'). jsPerf

Use find with scoped jQuery object queries.

	// bad
	$('.sidebar', 'ul').hide();

	// bad
	$('.sidebar').find('ul').hide();

	// good
	$('.sidebar ul').hide();

	// good
	$('.sidebar > ul').hide();

	// good (slower)
	$sidebar.find('ul');

	// good (faster)
	$($sidebar[0]).find('ul');


CSS/LESS/SASS
==============

CSS
----------------------

Don’t bind your CSS too much to your HTML structure and try to avoid IDs. Also try to make your CSS reusable by grouping common attributes into classes.

DO:

	.list {
	    list-style-type: none;
	}
	
	.list > .list_item {
	    display: inline-block;
	}
	
	.important_list_item {
	    color: red;
	}

DON’T:

	#content .myHeader ul {
	    list-style-type: none;
	}
	
	#content .myHeader ul li.list_item {
	    color: red;
	    display: inline-block;
	}

