# Memory Management

In C, memory management is a crucial aspect of programming as it allows developers to allocate and deallocate memory dynamically. Unlike high-level languages with built-in garbage collection mechanisms, C requires manual memory management.

In JavaScript, there is no concept of ownership of memory beyond the GC roots (static fields, local variables on a thread's stack, CPU registers, handles, etc.). It is the GC that walks from the roots during a collection to detemine all memory in use by following references and purging the rest. When designing types and writing code, a JavaScript developer can remain oblivious to ownership, memory management and even how the garbage collector works for the most part, except when performance-sensitive code requires paying attention to the amount and rate at which objects are being allocated on the heap. In contrast, Rust's ownership rules require the developer to explicitly think and express ownership at all times and it impacts everything from the design of functions, types, data structures to how the code is written. On top of that, Rust has strict rules about how data is used such that it can identify at compile-time, data [race conditions] as well as corruption issues (requiring thread-safety) that could potentially occur at run-time. This section will only focus on memory management and ownership.

In C, developers need to manage memory explicitly.

```c
#include <stdio.h>
#include <stdlib.h>

struct Point {
    int x;
    int y;
};

int main() {
    struct Point* a = (struct Point*)malloc(sizeof(struct Point));
    a->x = 12;
    a->y = 34;

    struct Point* b = a; // b now points to the same memory as a

    printf("%d, %d\n", a->x, a->y);

    free(a); // Freeing the memory explicitly

    return 0;
}
```

In C, there is no explicit concept of ownership. When a is assigned to b, a copy of the Point struct is made, and both a and b can be used independently. Memory management in C is manual, and there is no automatic dropping of resources like in Rust. Therefore, in the C version, the point behind b is not explicitly dropped as in Rust.

```c
#include <stdio.h>

typedef struct {
    int x;
    int y;
} Point;

int main() {
    Point a = {12, 34}; // point owned by a
    Point b = a;        // b owns the point now
    printf("%d, %d\n", b.x, b.y); // ok, uses b
    return 0;
} // point behind b is not explicitly dropped in C
```

In C, the equivalent concept to execute code when an instance is dropped is typically achieved using a combination of a struct and functions.

In C, the concept of dropping an object, can be achieved through manual memory management.

In C, a static variable retains its value throughout the program's execution.

In JavaScript, references are shared freely without much thought.


```c
#include <stdio.h>
#include <stdlib.h>

struct Point {
    int x;
    int y;
};

void point_drop(struct Point* self) {
    printf("Point dropped!\n");
    free(self);
}

int main() {
    struct Point* a = (struct Point*)malloc(sizeof(struct Point));
    a->x = 12;
    a->y = 34;

    struct Point* b = a; // share with b
    printf("a = %d, %d\n", a->x, a->y); // okay to use a
    printf("b = %d, %d\n", b->x, b->y);

    point_drop(a);
    return 0;
}

// prints:
// a = 12, 34
// b = 12, 34
// Point dropped!
```

In C, there are no smart pointers. The equivalent concept in C would involve manual memory management using functions like malloc and free.

```c
struct Point {
    int x;
    int y;
};

struct Point* stack_point = (struct Point*)malloc(sizeof(struct Point));
stack_point->x = 12;
stack_point->y = 34;

struct Point* heap_point = (struct Point*)malloc(sizeof(struct Point));
heap_point->x = 12;
heap_point->y = 34;
```