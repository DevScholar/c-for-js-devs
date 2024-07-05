# Conversion and Casting

C is statically-typed at compile time. Hence, after a variable is declared, assigning a value of a value of a different type (unless it's implicitly convertible to the target type) to the variable is prohibited. There are several ways to convert types in C.

## Implicit conversions

Implicit conversions exist in JavaScript as well as in C (called [type coercions]).
Consider the following example:

```js
let intNumber = 1;
let longNumber = intNumber;
```

In C:

```c
#include <stdio.h>

int main() {
    int int_number = 1;
    long long_number = int_number; // No error in C
    printf("int_number: %d, long_number: %ld\n", int_number, long_number);
    return 0;
}
```

By assigning the address of s to t, it is possible to achieve a similar implicit conversion in C:

```rust
void bar() {
    const char *s = "hi";
    const char *t = s;
}
```

See also:

- [Deref coercion]

[type coercions]: https://www.geeksforgeeks.org/type-conversion-c/

## Explicit conversions

If converting could cause a loss of information, JavaScript requires explicit
conversions using a casting expression:

```js
let a = 1.2;
let b = parseInt(a);
```

C does not handle exceptions during conversions:

```c
#include <stdio.h>

int main() {
    int int_number = 1;
    long long_number = (long)int_number;
    
    // Additional code for demonstration
    printf("Integer: %d\n", int_number);
    printf("Long: %ld\n", long_number);
    
    return 0;
}
```

## Custom conversion

In C, type conversion is typically achieved through explicit casting:

```c
#include <stdio.h>
#include <string.h>

typedef struct {
    char value[100];
} MyId;

void fromMyIdToString(MyId myId, char* result) {
    strcpy(result, myId.value);
}

int main() {
    MyId myId;
    strcpy(myId.value, "id");

    char result[100];
    fromMyIdToString(myId, result);

    printf("%s\n", result);

    return 0;
}
```
