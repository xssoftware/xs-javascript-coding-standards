Code Conventions for the JavaScript Programming Language
========================================================

The intent of this guide is to reduce cognitive friction when scanning code
from different authors. It does so by enumerating a shared set of rules and
expectations about how to format Javascript code.

The style rules herein are derived from commonalities among the various member
projects. When various authors collaborate across multiple projects, it helps
to have one set of guidelines to be used among all those projects. Thus, the
benefit of this guide is not in the rules themselves, but in the sharing of
those rules.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119][].

This is a set of coding conventions and rules for use in JavaScript programming. It is inspired by [Crockford's Standard][] and [Google's Javascript Style Guide][].

[Crockford's Standard]: http://javascript.crockford.com/code.html
[Google's Javascript Style Guide]: http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml
[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt

JavaScript Files
----------------

- JavaScript programs should be stored in and delivered as .js files.

- JavaScript code should not be embedded in HTML files unless the code is specific to a single session. Code in HTML adds significantly to pageweight with no opportunity for mitigation by caching and compression.

- \<script src="filename.js" type="text/javascript"\> tags should be placed as late in the body as possible. This reduces the effects of delays imposed by script loading on other page components.

Indentation
-----------

The unit of indentation is tabs.

Line Length
-----------

There MUST NOT be a hard limit on line length.

The soft limit on line length MUST be 120 characters; automated style checkers
MUST warn but MUST NOT error at the soft limit.

Lines SHOULD NOT be longer than 80 characters; lines longer than that SHOULD
be split into multiple subsequent lines of no more than 80 characters each.

There MUST NOT be trailing whitespace at the end of non-blank lines.

Blank lines MAY be added to improve readability and to indicate related
blocks of code.

There MUST NOT be more than one statement per line.

When a statement will not fit on a single line, it may be necessary to break it. Place the break after an operator, ideally after a comma. A break after an operator decreases the likelihood that a copy-paste error will be masked by semicolon insertion.

Comments
--------

Be generous with comments. It is useful to leave information that will be read at a later time by people (possibly yourself) who will need to understand what you have done. The comments should be well-written and clear, just like the code they are annotating. An occasional nugget of humor might be appreciated. Frustrations and resentments will not.

It is important that comments be kept up-to-date. Erroneous comments can make programs even harder to read and understand.

Make comments meaningful. Focus on what is not immediately visible. Don't waste the reader's time with stuff like

```js
    i = 0; // Set i to zero.
```

Generally use line comments. Save block comments for formal documentation and for commenting out.

### Comments Syntax

Use [JSDoc](http://code.google.com/p/jsdoc-toolkit/). The JSDoc syntax is based on JavaDoc . Many tools extract metadata from [JSDoc](http://code.google.com/p/jsdoc-toolkit/) comments to perform code validation and optimizations. These comments must be well-formed.

Variable Declarations
---------------------

All variables should be declared before used. JavaScript does not require this, but doing so makes the program easier to read and makes it easier to detect undeclared variables that may become implied globals. Implied global variables should never be used.

The var statements should be the first statements in the function body.

It is preferred that each variable be given its own line and comment. They should be listed in alphabetical order.

```js
    var currentEntry; // currently selected table entry
    var level;        // indentation level
    var size;         // size of table
```

JavaScript does not have block scope, so defining variables in blocks can confuse programmers who are experienced with other C family languages. Define all variables at the top of the function.

Use of global variables should be minimized. Implied global variables should never be used.

Constants
---------

Use NAMES_LIKE_THIS for constants. Use @const where appropriate. Never use the const keyword.

For simple primitive value constants, the naming convention is enough.

```js
/**
 * The number of seconds in a minute.
 * @type {number}
 */
goog.example.SECONDS_IN_A_MINUTE = 60;
```

For non-primitives, use the @const annotation.

```js
/**
 * The number of seconds in each of the given units.
 * @type {Object.<number>}
 * @const
 */
goog.example.SECONDS_TABLE = {
  minute: 60,
  hour: 60 * 60
  day: 60 * 60 * 24
}
```

This allows the compiler to enforce constant-ness.
As for the const keyword, Internet Explorer doesn't parse it, so don't use it.

Function Declarations
---------------------

All functions should be declared before they are used. Inner functions should follow the var statement. This helps make it clear what variables are included in its scope.

There should be no space between the name of a function and the ( (left parenthesis) of its parameter list. There should be one space between the ) (right parenthesis) and the { (left curly brace) that begins the statement body. The body itself is indented with 1 tab. The } (right curly brace) is aligned with the line containing the beginning of the declaration of the function.

```js
    function outer(c, d) {
        var e = c * d;

        function inner(a, b) {
            return (e * a) + b;
        }

        return inner(0, 1);
    }
```

This convention works well with JavaScript because in JavaScript, functions and object literals can be placed anywhere that an expression is allowed. It provides the best readability with inline functions and complex structures.

```js
    function getElementsByClassName(className) {
        var results = [];
        walkTheDOM(document.body, function (node) {
            var a;                  // array of class names
            var c = node.className; // the node's classname
            var i;                  // loop counter

// If the node has a class name, then split it into a list of simple names.
// If any of them match the requested name, then append the node to the set of results.

            if (c) {
                a = c.split(' ');
                for (i = 0; i < a.length; i += 1) {
                    if (a[i] === className) {
                        results.push(node);
                        break;
                    }
                }
            }
        });
        return results;
    }
```

If a function literal is anonymous, there should be one space between the word function and the ( (left parenthesis). If the space is omited, then it can appear that the function's name is function, which is an incorrect reading.

```js
    div.onclick = function (e) {
        return false;
    };

    that = {
        method: function () {
            return this.datum;
        },
        datum: 0
    };
```

Use of global functions should be minimized.

When a function is to be invoked immediately, the entire invocation expression should be wrapped in parens so that it is clear that the value being produced is the result of the function and not the function itself.

```js
var collection = (function () {
    var keys = [], values = [];

    return {
        get: function (key) {
            var at = keys.indexOf(key);
            if (at >= 0) {
                return values[at];
            }
        },
        set: function (key, value) {
            var at = keys.indexOf(key);
            if (at < 0) {
                at = keys.length;
            }
            keys[at] = key;
            values[at] = value;
        },
        remove: function (key) {
            var at = keys.indexOf(key);
            if (at >= 0) {
                keys.splice(at, 1);
                values.splice(at, 1);
            }
        }
    };
}());
```
Method Definitions
------------------

While there are several methods for attaching methods and properties to a constructor, the preferred style is:

```js
Foo.prototype.bar = function() {
  /* ... */
};
```

Names
-----

Names should be formed from the 26 upper and lower case letters (A .. Z, a .. z), the 10 digits (0 .. 9), and _ (underbar). Avoid use of international characters because they may not read well or be understood everywhere. Do not use $ (dollar sign) or \ (backslash) in names.

Do not use _ (underbar) as the first character of a name. It is sometimes used to indicate privacy, but it does not actually provide privacy. If privacy is important, use the forms that provide private members. Avoid conventions that demonstrate a lack of competence.

Most variables and functions should start with a lower case letter.

Constructor functions which must be used with the new prefix should start with a capital letter. JavaScript issues neither a compile-time warning nor a run-time warning if a required new is omitted. Bad things can happen if new is not used, so the capitalization convention is the only defense we have.

Global variables should be in all caps. (JavaScript does not have macros or constants, so there isn't much point in using all caps to signify features that JavaScript doesn't have.)

In general, use functionNamesLikeThis, variableNamesLikeThis, ClassNamesLikeThis, EnumNamesLikeThis, methodNamesLikeThis, and SYMBOLIC_CONSTANTS_LIKE_THIS.

Statements
----------

### Simple Statements

Each line should contain at most one statement. Put a ; (semicolon) at the end of every simple statement. Note that an assignment statement which is assigning a function literal or object literal is still an assignment statement and must end with a semicolon.

JavaScript allows any expression to be used as a statement. This can mask some errors, particularly in the presence of semicolon insertion. The only expressions that should be used as statements are assignments and invocations.

### Compound Statements

Compound statements are statements that contain lists of statements enclosed in { } (curly braces).

The enclosed statements should be indented with 1 more tab.
The { (left curly brace) should be at the end of the line that begins the compound statement.
The } (right curly brace) should begin a line and be indented to align with the beginning of the line containing the matching { (left curly brace).
Braces should be used around all statements, even single statements, when they are part of a control structure, such as an if or for statement. This makes it easier to add statements without accidentally introducing bugs.
Labels

Statement labels are optional. Only these statements should be labeled: while, do, for, switch.

### return Statement

A return statement with a value should not use ( ) (parentheses) around the value. The return value expression must start on the same line as the return keyword in order to avoid semicolon insertion.

if Statement

The if class of statements should have the following form:

```js
    if (condition) {
        statements
    }
    
    if (condition) {
        statements
    } else {
        statements
    }
    
    if (condition) {
        statements
    } else if (condition) {
        statements
    } else {
        statements
    }
```

### for Statement

A for class of statements should have the following form:

```js
    for (initialization; condition; update) {
        statements
    }

    for (variable in object) {
        if (filter) {
            statements
        } 
    }
```

The first form should be used with arrays and with loops of a predeterminable number of iterations.

The second form should be used with objects. Be aware that members that are added to the prototype of the object will be included in the enumeration. It is wise to program defensively by using the hasOwnProperty method to distinguish the true members of the object:

```js
    for (variable in object) {
        if (object.hasOwnProperty(variable)) {
            statements
        } 
    }
```

### while Statement

A while statement should have the following form:

```js
    while (condition) {
        statements
    }
```

### do Statement

A do statement should have the following form:

```js
    do {
        statements
    } while (condition);
```

Unlike the other compound statements, the do statement always ends with a ; (semicolon).

### switch Statement

A switch statement should have the following form:

```js
    switch (expression) {
    case expression:
        statements
    default:
        statements
    }
```

Each case is aligned with the switch. This avoids over-indentation.

Each group of statements (except the default) should end with break, return, or throw. Do not fall through.

### try Statement

The try class of statements should have the following form:

```js
    try {
        statements
    } catch (variable) {
        statements
    }

    try {
        statements
    } catch (variable) {
        statements
   } finally {
        statements
    }
```

### continue Statement

Avoid use of the continue statement. It tends to obscure the control flow of the function.

### with Statement

The with statement should not be used.

Using with clouds the semantics of your program. Because the object of the with can have properties that collide with local variables, it can drastically change the meaning of your program. For example, what does this do?

```js
with (foo) {
  var x = 3;
  return x;
}
```

Answer: anything. The local variable x could be clobbered by a property of foo and perhaps it even has a setter, in which case assigning 3 could cause lots of other code to execute. Don't use with.

### Whitespace

Blank lines improve readability by setting off sections of code that are logically related.

Blank spaces should be used in the following circumstances:

A keyword followed by ( (left parenthesis) should be separated by a space.

```js
        while (true) {
```

A blank space should not be used between a function value and its ( (left parenthesis). This helps to distinguish between keywords and function invocations.
All binary operators except . (period) and ( (left parenthesis) and [ (left bracket) should be separated from their operands by a space.
No space should separate a unary operator and its operand except when the operator is a word such as typeof.
Each ; (semicolon) in the control part of a for statement should be followed with a space.
Whitespace should follow every , (comma).

Bonus Suggestions
-----------------

### {} and []

Use {} instead of new Object(). Use [] instead of new Array().

Use arrays when the member names would be sequential integers. Use objects when the member names are arbitrary strings or names.

### , (comma) Operator

Avoid the use of the comma operator except for very disciplined use in the control part of for statements. (This does not apply to the comma separator, which is used in object literals, array literals, var statements, and parameter lists.)

### Block Scope

In JavaScript blocks do not have scope. Only functions have scope. Do not use blocks except as required by the compound statements.

### Assignment Expressions

Avoid doing assignments in the condition part of if and while statements.

Is

```js

    if (a = b) {
```

a correct statement? Or was

```js
    if (a == b) {
```

intended? Avoid constructs that cannot easily be determined to be correct.

### === and !== Operators.

It is almost always better to use the === and !== operators. The == and != operators do type coercion. In particular, do not use == to compare against falsy values.

### Confusing Pluses and Minuses

Be careful to not follow a + with + or ++. This pattern can be confusing. Insert parens between them to make your intention clear.

```js
    total = subtotal + +myInput.value;
```

is better written as

```js
    total = subtotal + (+myInput.value);
```

so that the + + is not misread as ++.

### eval is Evil

The eval function is the most misused feature of JavaScript. Avoid it.

eval has aliases. Do not use the Function constructor. Do not pass strings to setTimeout or setInterval.

### this

Only in object constructors, methods, and in setting up closures
The semantics of this can be tricky. At times it refers to the global object (in most places), the scope of the caller (in eval), a node in the DOM tree (when attached using an event handler HTML attribute), a newly created object (in a constructor), or some other object (if function was call()ed or apply()ed).

Because this is so easy to get wrong, limit its use to those places where it is required:
- in constructors
- in methods of objects (including in the creation of closures)

### Associative Arrays
Never use Array as a map/hash/associative array
Associative Arrays are not allowed... or more precisely you are not allowed to use non number indexes for arrays. If you need a map/hash use Object instead of Array in these cases because the features that you want are actually features of Object and not of Array. Array just happens to extend Object (like any other object in JS and therefore you might as well have used Date, RegExp or String).

### Multiline string literals

Do not use multiline string literals:

```js
var myString = 'A rather long string of English text, an error message \
                actually that just keeps going and going -- an error \
                message to make the Energizer bunny blush (right through \
                those Schwarzenegger shades)! Where was I? Oh yes, \
                you\'ve got an error and all the extraneous whitespace is \
                just gravy.  Have a nice day.';
```

The whitespace at the beginning of each line can't be safely stripped at compile time; whitespace after the slash will result in tricky errors; and while most script engines support this, it is not part of ECMAScript.

Use string concatenation instead:

```js
var myString = 'A rather long string of English text, an error message ' +
    'actually that just keeps going and going -- an error ' +
    'message to make the Energizer bunny blush (right through ' +
    'those Schwarzenegger shades)! Where was I? Oh yes, ' +
    'you\'ve got an error and all the extraneous whitespace is ' +
    'just gravy.  Have a nice day.';
```

### Array and Object literals

Use Array and Object literals instead of Array and Object constructors.

Array constructors are error-prone due to their arguments.

```js
// Length is 3.
var a1 = new Array(x1, x2, x3);

// Length is 2.
var a2 = new Array(x1, x2);

// If x1 is a number and it is a natural number the length will be x1.
// If x1 is a number but not a natural number this will throw an exception.
// Otherwise the array will have one element with x1 as its value.
var a3 = new Array(x1);

// Length is 0.
var a4 = new Array();
```

Because of this, if someone changes the code to pass 1 argument instead of 2 arguments, the array might not have the expected length.

To avoid these kinds of weird cases, always use the more readable array literal.

```js
var a = [x1, x2, x3];
var a2 = [x1, x2];
var a3 = [x1];
var a4 = [];
```

Object constructors don't have the same problems, but for readability and consistency object literals should be used.

```js
var o = new Object();

var o2 = new Object();
o2.a = 0;
o2.b = 1;
o2.c = 2;
o2['strange key'] = 3;
```

Should be written as:

```js
var o = {};

var o2 = {
  a: 0,
  b: 1,
  c: 2,
  'strange key': 3
};
```
### Modifying prototypes of builtin objects

Modifying builtins like Object.prototype and Array.prototype are strictly forbidden. Modifying other builtins like Function.prototype is less dangerous but still leads to hard to debug issues in production and should be avoided.

### Prefer ' over "

For consistency single-quotes (') are preferred to double-quotes ("). This is helpful when creating strings that include HTML:

```js
var msg = 'This is some HTML';
```
