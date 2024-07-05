# Equality

When comparing for equality in JavaScript, this refers to testing for _equivalence_ insome cases (also known as _value equality_), and in other cases it refers to testing for _reference equality_, which tests whether two variables refer to the same underlying object in memory. In JavaScript, while there is no syntax for explicitly custom types, custom types can be simulated through constructors and prototypes. Constructors allow you to create objects with specific properties and methods, and you can use prototypes to implement inheritance and shared methods. Every "custom type" can be compared for equality because it inherits from `object`.

For example, when comparing for equivalence and reference equality in JavaScript:

```js
class Point {
    constructor(X, Y) {
        this.X = X;
        this.Y = Y;
    }
    
    equals(other) {
        return this.X === other.X && this.Y === other.Y;
    }
}

const a = new Point(1, 2);
const b = new Point(1, 2);
const c = a;

console.log(a.equals(b)); // (1) true
console.log(a.equals(new Point(2, 2))); // (1) false
console.log(a === b); // (2) false
console.log(a === c); // (2) true
```

1. In JavaScript, classes are used to implement equals methods to compare the equality of values.
2. For the comparison of reference equality, using the === operator to check if the variable points to the same object in memory.

Equivalently in C:

```c
#include <stdio.h>

typedef struct {
    int x;
    int y;
} Point;

int point_equals(Point a, Point b) {
    return a.x == b.x && a.y == b.y;
}

int main() {
    Point a = {1, 2};
    Point b = {1, 2};
    Point c = a;

    printf("%d\n", point_equals(a, b)); // true
    printf("%d\n", point_equals(a, b)); // true
    printf("%d\n", point_equals(a, (Point){2, 2})); // false
    printf("%d\n", point_equals(a, c)); // true

    return 0;
}
```