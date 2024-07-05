# Variables

Consider the following example around variable assignment in JavaScript:

```js
let x = 5;
```

And the same in C:

```c
int x = 5;
```

C is not type-safe: the compiler guarantees that the value stored in a variable is always of the designated type. The example can be simplified by using the compiler's ability to automatically infer the types of the variable. In JavaScript: 

```js
let x = 5;
```

In C:

```c
auto x = 5;
```

When expanding the first example to update the value of the variable (reassignment), the behavior of JavaScript and Rust differ:

```js
let x = 5;
x = 6;
console.log(x); // 6
```

In C, the identical statement will compile:

```c
#include <stdio.h>

int main() {
    int x = 5;
    x = 6; // Variable 'x' is mutable in C
    printf("%d", x);
    return 0;
}
```

In C, variables are _mutable_ by default. Once a value is bound to a name, the variable's value can be changed:

```c
#include <stdio.h>

int main() {
    int x = 5;
    x = 6;
    printf("%d", x);
    return 0;
}
```

In C, the concept of variable shadowing is not directly supported. However, a similar effect can be achieved by declaring a new variable with the same name in a nested scope:

```c
#include <stdio.h>

int main() {
    int x = 5;
    {
        int x = 6;
        printf("%d", x); // Output: 6
    }
    return 0;
}
```

JavaScript also supports shadowing, e.g. locals can shadow fields and type members can shadow members from the base type. In Rust, the above example demonstrates that shadowing also allows to change the type of a variable without changing the name, which is useful if one wants to transform the data into different types and shapes without having to come up with a distinct name each time.

See also:

- [Data races and race conditions] for more information around the implications
  of mutability
- [Scope and shadowing]
- [Memory management][memory-management-section] for explanations

[memory-management-section]: ../memory-management/index.md
[data races and race conditions]: https://stanford-cs242.github.io/f18/lectures/06-2-concurrency.html
