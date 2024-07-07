# Generics

Generics provide a way to create definitions for types and methods that can be parameterized over other types. This improves code reuse, type-safety and performance (e.g. avoid run-time casts). Consider the following example of a generic type that adds a timestamp to any value. However, JavaScript does not have the concept of generics.

```js
class Timestamped {
    constructor(value) {
        this.Timestamp = new Date();
        this.Value = value;
    }
}

```

C does not have built-in support for generics. In C, preprocessor macros or void pointers are typically used to achieve a similar effect.

Here's a simplified example in C that mimics the Rust code using a void pointer to achieve a generic-like behavior:

```c
#include <stdio.h>
#include <time.h>

typedef struct {
    void* value;
    time_t timestamp;
} Timestamped;

Timestamped new(void* value) {
    Timestamped ts;
    ts.value = value;
    ts.timestamp = time(NULL);
    return ts;
}

int main() {
    int intValue = 42;
    Timestamped intTimestamped = new(&intValue);

    char charValue = 'A';
    Timestamped charTimestamped = new(&charValue);

    return 0;
}
```

## Generic type constraints

JavaScript has no concept of generics, and it is a weakly typed scripting language that makes it impossible to add type constraints to it.

```js
class Timestamped {
    constructor(value) {
        this.value = value;
        this.timestamp = Date.now();
    }

    equals(other) {
        return this.value === other.value && this.timestamp === other.timestamp;
    }
}
```

The same can be achieved in C:

```c
#include <stdio.h>
#include <time.h>

typedef struct {
    void* value;
    time_t timestamp;
} Timestamped;

Timestamped new(void* value) {
    Timestamped ts;
    ts.value = value;
    ts.timestamp = time(NULL);
    return ts;
}

int equal(Timestamped* ts1, Timestamped* ts2) {
    return ts1->value == ts2->value && ts1->timestamp == ts2->timestamp;
}

int main() {
    void* val = NULL;
    Timestamped ts1 = new(val);
    Timestamped ts2 = new(val);

    if (equal(&ts1, &ts2)) {
        printf("Timestamped values are equal.\n");
    } else {
        printf("Timestamped values are not equal.\n");
    }

    return 0;
}
```
