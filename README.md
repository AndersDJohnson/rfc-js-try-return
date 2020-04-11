# rfc-js-try-return
A proposal for JS `try` to return a variable.

Often we start with code like:

```js
const b = getB();
const a = getA(b);
console.log(a);
```

But then we need to refactor to catch errors in `getA(b)`,
which results in a very noisy diff (even whitespace-insensitive),
and forces us to make `a` a mutable `let` reference:

```diff
+ let a;
+ try {
    const b = getB();
-   const a = getA(b);
+   a = getA(b);
+ } catch (error) {}
  console.log(a);
```

What if we could capture a variable from the `try` block to be hoisted to the upper scope?

```js
try (a) {
  const b = getB();
  const a = getA(b);
} catch (error) {}
console.log(a);
```

which would define a `const a` in the scope outside of the `try`.

This was possible with `var a`, but with those we don't get the other advantages of `const` & `let`.

Much cleaner diff:

```diff
+ try (a) {
    const b = getB();
    const a = getA(b);
+ } catch (error) {}
console.log(a);
```

Could do similar with `let a`.

Or alternative syntaxes:

```js
try (const a) {
  const b = getB();
  a = getA(b);
}
```

Or:

```js
const a = try {
  const b = getB();
  a = getA(b);
}
```

Or maybe some new keyword like `hoist` (either pulls it up one scope or to function scope like `var`):

```js
try {
  const b = getB();
  hoist const a = getA(b);
}
```

Even more clean if combined with [rfc-js-try-no-catch](https://github.com/AndersDJohnson/rfc-js-try-no-catch):

```diff
+ try (a) {
    const b = getB();
    const a = getA(b);
+ }
console.log(a);
```

Related:
* https://github.com/AndersDJohnson/rfc-js-catch-no-parentheses
* https://github.com/AndersDJohnson/rfc-js-try-no-catch
* https://github.com/tc39/proposal-optional-catch-binding
