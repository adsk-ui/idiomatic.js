# Principles of Writing Consistent, Idiomatic JavaScript
------------------------------------------------

1. <a name='whitespace'>Whitespace</a>
  - Never mix spaces and tabs.
  - Configure your editor to use soft tabs with an indent size of **four** characters &mdash; this means four spaces or four spaces representing a real tab.
  - Use [Editorconfig](http://editorconfig.org/) when possible.  It supports most IDEs and handles most whitespace settings.

2. <a name='beautiful-syntax' href="#beautiful-syntax">Beautiful Syntax</a>

    <a name="parens-braces" href="#parens-braces">A. Parens, Braces, Linebreaks</a>

    ```javascript

    // if/else/for/while/try always have spaces, braces and span multiple lines
    // this encourages readability

    // 2.A.1.1
    // Examples of really cramped syntax

    if(condition) doSomething();

    while(condition) iterating++;

    for(var i=0;i<100;i++) someIterativeFn();


    // 2.A.1.1
    // Use whitespace to promote readability

    if ( condition ) {
      // statements
    }

    while ( condition ) {
      // statements
    }

    for ( var i = 0; i < 100; i++ ) {
      // statements
    }

    // Even better:

    var i,
      length = 100;

    for ( i = 0; i < length; i++ ) {
      // statements
    }

    var prop;

    for ( prop in object ) {
      // statements
    }


    if ( true ) {
      // statements
    } else {
      // statements
    }
    ```


    <a name="assignments-declarations" href="#assignments-declarations">B. Assignments, Declarations, Functions ( Named, Expression, Constructor )</a>

    ```javascript

    // 2.B.1.1
    // Variables
    var foo = 'bar',
      num = 1,
      undef;

    // Literal notations:
    var array = [],
      object = {};


    // 2.B.1.2
    // Using only one `var` per scope (function) promotes readability
    // and keeps your declaration list free of clutter (also saves a few keystrokes)

    // Bad
    var foo = '';
    var bar = '';
    var qux;

    // Good
    var foo = '',
      bar = '',
      qux;

    // or..
    var // Comment on these
    foo = '',
    bar = '',
    quux;

    // 2.B.1.3
    // var statements should always be in the beginning of their respective scope (function).


    // Bad
    function foo() {

      // some statements here

      var bar = '',
        qux;
    }

    // Good
    function foo() {
      var bar = '',
        qux;

      // all statements after the variables declarations.
    }

    // 2.B.1.4
    // const and let, from ECMAScript 6, should likewise be at the top of their scope (block).

    // Bad
    function foo() {
      let foo,
        bar;
      if (condition) {
        bar = '';
        // statements
      }
    }
    // Good
    function foo() {
      let foo;
      if (condition) {
        let bar = '';
        // statements
      }
    }
    ```

    ```javascript

    // 2.B.2.1
    // Named Function Declaration
    function foo( arg1, argN ) {

    }

    // Usage
    foo( arg1, argN );


    // 2.B.2.2
    // Named Function Declaration
    function square( number ) {
      return number * number;
    }

    // Usage
    square( 10 );

    // Really contrived continuation passing style
    function square( number, callback ) {
      callback( number * number );
    }

    square( 10, function( square ) {
      // callback statements
    });


    // 2.B.2.3
    // Function Expression
    var square = function( number ) {
      // Return something valuable and relevant
      return number * number;
    };

    // Function Expression with Identifier
    // This preferred form has the added value of being
    // able to call itself and have an identity in stack traces:
    var factorial = function factorial( number ) {
      if ( number < 2 ) {
        return 1;
      }

      return number * factorial( number - 1 );
    };


    // 2.B.2.4
    // Constructor Declaration
    function FooBar( options ) {

      this.options = options;
    }

    // Usage
    var fooBar = new FooBar({ a: 'alpha' });

    fooBar.options;
    // { a: 'alpha' }

    ```
    
    C. Exceptions, Slight Deviations

    ```javascript

    // 2.C.1.1
    // Functions with callbacks
    foo(function() {
      // Note there is no extra space between the first paren
      // of the executing function call and the word 'function'
    });

    // Function accepting an array, no space
    foo([ 'alpha', 'beta' ]);

    // 2.C.1.2
    // Function accepting an object, no space
    foo({
      a: 'alpha',
      b: 'beta'
    });

    // Single argument string literal, no space
    foo('bar');

    // Inner grouping parens, no space
    if ( !('foo' in obj) ) {

    }

    ```

    D. Consistency Always Wins

    In sections 2.A-2.C, the whitespace rules are set forth as a recommendation with a simpler, higher purpose: consistency.

    ```javascript

    // 2.D.1.1

    if (condition) {
      // statements
    }

    while (condition) {
      // statements
    }

    for (var i = 0; i < 100; i++) {
      // statements
    }

    if (true) {
      // statements
    } else {
      // statements
    }

    ```

    E. Quotes

    For the sake of consistency **use single quotes**. Double quotes should only be used if nested within single quotes.

    F. End of Lines and Empty Lines

    Whitespace can ruin diffs and make changesets impossible to read. Consider incorporating a pre-commit hook that removes end-of-line whitespace and blanks spaces on empty lines automatically. **TO DO**

3. <a name='type'>Type Checking</a>

Please use the type checking methods provided by [Underscore](http://underscorejs.org/).

    A. Actual Types

    String:

        _.isString(variable)

    Number:

        _.isNumber(variable)

    Boolean:

        _.isBoolean(variable)

    Object:

        _.isObject(variable)

    Array:

        _.isArray(variable)

    null:

        _.isNull(variable)

    undefined:

      _.isUndefined(variable)


4. <a name='cond'>Conditional Evaluation</a>

    ```javascript

    // 4.1.1
    // When only evaluating that an array has length,
    // instead of this:
    if ( array.length > 0 ) ...

    // ...evaluate truthiness, like this:
    if ( array.length ) ...


    // 4.1.2
    // When only evaluating that an array is empty,
    // instead of this:
    if ( array.length === 0 ) ...

    // ...evaluate truthiness, like this:
    if ( !array.length ) ...


    // 4.1.3
    // When only evaluating that a string is not empty,
    // instead of this:
    if ( string !== '' ) ...

    // ...evaluate truthiness, like this:
    if ( string ) ...


    // 4.1.4
    // When only evaluating that a string _is_ empty,
    // instead of this:
    if ( string === '' ) ...

    // ...evaluate falsy-ness, like this:
    if ( !string ) ...


    // 4.1.5
    // When only evaluating that a reference is true,
    // instead of this:
    if ( foo === true ) ...

    // ...evaluate like you mean it, take advantage of built in capabilities:
    if ( foo ) ...


    // 4.1.6
    // When evaluating that a reference is false,
    // instead of this:
    if ( foo === false ) ...

    // ...use negation to coerce a true evaluation
    if ( !foo ) ...

    // ...Be careful, this will also match: 0, '', null, undefined, NaN
    // If you _MUST_ test for a boolean false, then use
    if ( foo === false ) ...


    // 4.1.7
    // When only evaluating a ref that might be null or undefined, but NOT false, '' or 0,
    // instead of this:
    if ( foo === null || foo === undefined ) ...

    // ...take advantage of == type coercion, like this:
    if ( foo == null ) ...

    // Remember, using == will match a `null` to BOTH `null` and `undefined`
    // but not `false`, '' or 0
    null == undefined

    ```
    ALWAYS evaluate for the best, most accurate result - the above is a guideline, not a dogma.

5. <a name='practical'>Practical Style</a>

Portal uses RequireJS to manage dependencies. Therefore files should be written as AMD modules. Use [simplified CommonJS wrapper](http://requirejs.org/docs/api.html#cjsmodule) for requiring dependencies. 

    ```javascript

    // 5.1.1
    // A Practical Module

    define(function(require){
      var dependency = require('dependency'),
          privateVar;
      return {
        public API here...
      };
    });

    ```

6. <a name='naming'>Naming</a>

    A. You are not a human code compiler/compressor, so don't try to be one.

    The following code is an example of egregious naming:

    ```javascript

    // 6.A.1.1
    // Example of code with poor names

    function q(s) {
      return document.querySelectorAll(s);
    }
    var i,a=[],els=q('#foo');
    for(i=0;i<els.length;i++){a.push(els[i]);}
    ```

    Here's the same piece of logic, but with kinder, more thoughtful naming (and a readable structure):

    ```javascript

    // 6.A.2.1
    // Example of code with improved names

    function query( selector ) {
      return document.querySelectorAll( selector );
    }

    var idx = 0,
      elements = [],
      matches = query('#foo'),
      length = matches.length;

    for ( ; idx < length; idx++ ) {
      elements.push( matches[ idx ] );
    }

    ```

    A few additional naming pointers:

    ```javascript

    // 6.A.3.1
    // Naming strings

    `dog` is a string


    // 6.A.3.2
    // Naming arrays

    `dogs` is an array of `dog` strings


    // 6.A.3.3
    // Naming functions, objects, instances, etc

    camelCase; function and var declarations


    // 6.A.3.4
    // Naming constructors, prototypes, etc.

    PascalCase; constructor function


    // 6.A.3.6
    // From the Google Closure Library Style Guide

    functionNamesLikeThis;
    variableNamesLikeThis;
    ConstructorNamesLikeThis;
    EnumNamesLikeThis;
    methodNamesLikeThis;
    SYMBOLIC_CONSTANTS_LIKE_THIS;

    ```

    B. Faces of `this`

    Beyond the generally well known use cases of `call` and `apply`, always prefer [Underscore](http://underscorejs.org/)'s `_.bind( functionName, this )`, for creating `BoundFunction` definitions for later invocation. Only resort to aliasing (ie. var that = this) when no preferable option is available.

    ```javascript

    // 6.B.1
    function Device( opts ) {

      this.value = null;

      // open an async stream,
      // this will be called continuously
      stream.read( opts.path, _.bind(function( data ) {

        // Update this instance's current value
        // with the most recent value from the
        // data stream
        this.value = data;

      }, this) );

      // Throttle the frequency of events emitted from
      // this Device instance
      setInterval(_.bind(function() {

        // Emit a throttled event
        this.emit('event');

      }, this), opts.freq || 100 );
    }

    // Just pretend we've inherited EventEmitter ;)

    ```
    As a last resort, create an alias to `this` using `self` as an Identifier. This is bug prone and should be avoided whenever possible.

    ```javascript

    // 6.B.3

    function Device( opts ) {
      var self = this;

      this.value = null;

      stream.read( opts.path, function( data ) {

        self.value = data;

      });

      setInterval(function() {

        self.emit('event');

      }, opts.freq || 100 );
    }

    ```


    C. Use `thisArg`

    Several prototype methods of ES 5.1 built-ins come with a special `thisArg` signature, which should be used whenever possible

7. <a name='dependency-injection'>Dependency Injection</a>

    When authoring AMD-style modules (as with RequireJS), long lists of dependencies should be written using [simplified CommonJS wrapping](http://requirejs.org/docs/whyamd.html#sugar) to promote readability.

    ```javascript
    // 7.A.1.1
    // Bad
    define([ 'require', 'jquery', 'blade/object', 'blade/fn', 'rdapi',
         'oauth', 'blade/jig', 'blade/url', 'dispatch'],
    function (require,   $,        object,         fn,         rdapi,
          oauth,   jig,         url,         dispatch) {

    });

    // Good
    define(function(require){
        var $ = require('jquery'),        
            object = require('blade/object'),         
            fn = require('blade/fn'),         
            rdapi = require('rdapi'),
            oauth = require('oauth'),   
            jig = require('blade/jig'),         
            url = require('blade/url'),         
            dispatch = require('dispatch');

    });
    ```
10. <a name='comments'>Comments</a>
    - Single line above the code that is subject
    - Multiline is good
    - End of line comments are prohibited!
    - JSDoc style is good, but requires a significant time investment
