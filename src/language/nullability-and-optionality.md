# Nullability and Optionality

In JavaScript, `null` is often used to represent a value that is missing, absent or logically uninitialized. For example:  

```js
let some = 1;
let none = null;
```

Rust has no `null` and consequently no nullable context to enable. Optional or missing values are instead represented by [`Option<T>`][option]. The equivalent of the JavaScript code above in Rust would be:  

```c
#include <stddef.h>
//...
int some = 1;
int *none = NULL;
```


## Control flow with optionality

In JavaScript, you may have been using `if`/`else` statements for controlling the flow when using nullable values.

```js
let max = 10;
if (max !== null && max !== undefined) {
    let someMax = max;
    console.log(`The maximum is ${someMax}.`); // Output：The maximum is 10.
}
```

C does not have built-in support for null or undefined values like JavaScript. Instead, it typically uses specific values or flags to indicate absence or special conditions:

```c
#include <stdio.h>

int main() {
    int max = 10;
    if (max != 0) {
        int someMax = max;
        printf("The maximum is %d.\n", someMax); // Output: The maximum is 10.
    }
    return 0;
}
```

## Null-conditional operators

 The null-conditional operators (`?.`) make dealing with `null` in JavaScript more ergonomic.

In C language, there is no direct equivalent to the null-conditional operator ?. found in JavaScript. To handle similar scenarios in C, one can use conditional statements to check for null pointers before accessing members.

```js
let some = "Hello, World!";
let none = null;
console.log(some?.length); // 13
console.log(none?.length); // undefined
```

```c
#include <stdio.h>
#include <string.h>

int main() {
    char* some = "Hello, World!";
    char* none = NULL;

    printf("%d\n", (some != NULL) ? (int)strlen(some) : -1);
    printf("%d\n", (none != NULL) ? (int)strlen(none) : -1);

    return 0;
}
```

## Null-coalescing operator

The null-coalescing operator (`??`) is typically used to default to another value when a nullable is `null`:

```js
let some = 1;
let none = null;
console.log(some ?? 0); // 1
console.log(none ?? 0); // 0
```

In C, there is no direct equivalent to the null-coalescing operator (??) as in JavaScript. However, it is possible to achieve similar functionality using conditional operators.

```c
#include <stdio.h>

int main() {
    int some = 1;
    int *none = NULL;
    
    printf("%d\n", some); // 1
    printf("%d\n", none != NULL ? *none : 0); // 0
    
    return 0;
}
```

## Null-forgiving operator

In C, there is no direct equivalent to the null-forgiving operator (!) found in languages like C# or Rust. To handle null pointers or avoid null references, C programmers typically use conditional statements or pointer checks to ensure safe memory access. Unlike Rust, C does not have built-in mechanisms for static flow analysis to handle null values implicitly. Therefore, developers need to implement explicit null checks and error handling in C code to manage potential null pointer exceptions effectively.
