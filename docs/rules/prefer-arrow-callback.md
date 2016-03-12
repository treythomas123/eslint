# Suggest using arrow functions as callbacks. (prefer-arrow-callback)

Arrow functions are suited to callbacks, because:

- `this` keywords in arrow functions bind to the upper scope's.
- The notation of the arrow function is shorter than function expression's.

## Rule Details

This rule is aimed to flag usage of function expressions in an argument list.

The following patterns are considered problems:

```js
/*eslint prefer-arrow-callback: 2*/

foo(function(a) { return a; });
foo(function() { return this.a; }.bind(this));
```

The following patterns are not considered problems:

```js
/*eslint prefer-arrow-callback: 2*/
/*eslint-env es6*/

foo(a => a);
foo(function*() { yield; });

// this is not a callback.
var foo = function foo(a) { return a; };

// using `this` without `.bind(this)`.
foo(function() { return this.a; });

// recursively.
foo(function bar(n) { return n && n + bar(n - 1); });
```

## Options

`prefer-arrow-callback` receives a single, optional argument which is an options object.

### allowThis

This defaults to `true`. When set to `false`, callbacks that use `this` without also using `bind` are not allowed. In that case, the value of `this` is not known until execution time so changing to an arrow function must be done carefully to avoid errors.

The following patterns will not be considered problems

```js
/*eslint prefer-arrow-callback: [2, { allowThis: false }] */
/*eslint-env es6*/

foo(function() { this.a; });

foo(function() { (() => this); });

someArray.map(function (itm) { return this.doSomething(itm); }, someObject);
```

## When Not To Use It

This rule should not be used in ES3/5 environments.

In ES2015 (ES6) or later, if you don't want to be notified about function expressions in an argument list, you can safely disable this rule.
