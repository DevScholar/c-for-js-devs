# Discards

Discards express to the compiler and others to ignore the results (or parts) of an expression.

There are multiple contexts where to apply this, for example as a basic example, to ignore the result of an expression. JavaScript doesn't have discards, but you can call a function without assigning a value to any variable to emulate discards. In JavaScript this looks like:

```js
city.getCityInformation(cityName);
```

In C, discarding the result of a function call is typically achieved by assigning the result to a variable and not using that variable further. In C, casting the result to (void) indicates to the compiler that the return value is intentionally being ignored. 

```c
(void)city_get_city_information(city_name);
```

Discards are also applied for deconstructing "tuples" in JavaScript:

```js
const [_, second] = ["first", "second"];
```

In C, tuple deconstruction like in JavaScript can be achieved using a similar approach. However, C does not have built-in tuple types like JavaScript. To mimic tuple deconstruction, one can use arrays or structures:

```c
#include <stdio.h>

int main() {
    char* values[] = {"first", "second"};
    char* second = values[1];

    printf("Second value: %s\n", second);

    return 0;
}
```

To achieve struct destructuring similar to Rust in C, one can use a similar approach by defining a struct and then accessing its members selectively. 

```c
#include <stdio.h>

struct Point {
    int x;
    int y;
    int z;
};

int main() {
    struct Point origin = {0, 0, 0};

    switch (origin.x) {
        case 0:
            printf("x is %d\n", origin.x);
            break;
    }

    return 0;
}
```

When pattern matching, it is often useful to discard or ignore part of a matching expression. But since there are no discards in JavaScript, and the switch statement of js cannot be used in the same way as rust, you have to emulate this feature in an awkward way:

```js
const _ = ("first", "second");
const result = (_ => {
    switch(true) {
        case _.includes("first"):
            return "first element matched";
        default:
            return "first element did not match";
    }
})();

console.log(result);
```

and again, in C:

```c
#include <stdio.h>
#include <string.h>

int main() {
    const char* first = "first";
    const char* second = "second";
    const char* result;

    if (strcmp(first, "first") == 0) {
        result = "first element matched";
    } else {
        result = "first element did not match";
    }

    printf("%s\n", result);

    return 0;
}
```
