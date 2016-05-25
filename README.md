# ECMAScript const function parameter
ECMAScript Proposal, specs, and reference implementation for const function parameter.

Spec drafted by [@zeitiger](https://github.com/zeitiger)

## Rationale
ECMAScript 2015 introduce with `let` and `const` block scope variable definition.
`const` give additional the guarantee that a variable itself could not be changed.

But there is a leak of `const` within function parameter list. It's possible to define default assignment and
destructuring, but not to define a function parameter as `const`.

To avoid accidentally reassignment of function parameter is should be possible to declare parameter as `const` without
creating `const` variables at the beginning of a function with keeping the mutable function parameter.

## Code examples
```js
/**
 * Example basic
 */
function ordinaryFunc(const foo) {
    // ...
}
const arrowFunc = (const foo) => foo;
```

```js
/**
 * Example runtime check
 */
function ordinaryFunc(const foo) {
    foo = 42; // => TypeError: Assignment to constant variable.
}
```

```js
/**
 * Example combination with default parameter
 */
function ordinaryFunc(const bar=42) {
    // ...
}
ordinaryFunc(23);
ordinaryFunc();
```

```js
/**
 * Example combination with parameter destructuring
 */
function ordinaryFunc(const {foo: [bar]}) {
    bar = 42; // => TypeError: Assignment to constant variable.
}
```

## Open question

- Should it be possible to define `const` parameter without default value?
- If so what should the implicit default? `undefined`?

**Background:** `const` variables need a explicit value assignment, but function parameters are optional by design.

## Todo

- Write babel plugin to extend parser to accept this syntax
- Write babel plugin to transform to ES5 or ES2015