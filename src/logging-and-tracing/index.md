# Logging and Tracing

For most cases, `console.log()` is a good default choice for JavaScript, since it works with a variety of built-in and third-party logging providers. In JavaScript, a minimal example for structured logging could look like:

```js
let day = "Thursday";
console.log("Hello ", day); // Hello Thursday.
```

In C, there are no built-in loggers. Developers use `printf` based methods to log.
