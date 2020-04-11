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
+ } catch (error) {
+   // etc.
+ }
  console.log(a);
```

What if we could capture a variable from the `try` block?

```js
try (a) {
  const b = getB();
  const a = getA(b);
} catch (error) {
  // etc.
}
console.log(a);
```

which would define a `const a` in the scope outside of the `try`.
Much cleaner diff:

```diff
+ try (a) {
    const b = getB();
    const a = getA(b);
+ } catch (error) {
+   // etc.
+ }
console.log(a);
```

Could do similar with `let a`.

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