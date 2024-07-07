# Structures (`struct`)

In JavaScript, there is no direct concept of a structure, but you can use objects to model similar structures.
Structures in C:

- In C, `struct` simply defines the data/fields. Developers can encapsulate data by defining structs and manipulating that data with functions, which is similar to impl in rust.

- They cannot be sub-classed.

- They are allocated on stack by default.

In C, a `struct` is the primary construct for modeling any data structure (the other being an `enum`).

In C, structs do not need to implement traits, as C does not support the concept of traits in object-oriented programming.

```c
#include <stdio.h>

struct Point {
    int x;
    int y;
};

int main() {
    struct Point p;
    p.x = 10;
    p.y = 20;

    printf("Point coordinates: (%d, %d)\n", p.x, p.y);

    return 0;
}
```

Value types in JavaScript are usually designed by a developer to be mutable. It's considered best practice speaking semantically, but the language does not prevent designing a `struct` that makes destructive or in-place modifications. 

Since C doesn't have classes and consequently type hierarchies based on sub-classing, shared behaviour is achieved via traits and generics and polymorphism via virtual dispatch using [trait objects].

  [trait objects]: https://doc.rust-lang.org/book/ch17-02-trait-objects.html#using-trait-objects-that-allow-for-values-of-different-types

In JavaScript:

```js
class Rectangle {
    constructor(x1, y1, x2, y2) {
        this.x1 = x1;
        this.y1 = y1;
        this.x2 = x2;
        this.y2 = y2;
    }

    length() {
        return this.y2 - this.y1;
    }

    width() {
        return this.x2 - this.x1;
    }

    top_left() {
        return [this.x1, this.y1];
    }

    bottom_right() {
        return [this.x2, this.y2];
    }

    area() {
        return this.length() * this.width();
    }

    is_square() {
        return this.width() === this.length();
    }

    toString() {
        return `(${this.x1}, ${this.y1}), (${this.x2}, ${this.y2})`;
    }
}

const rect = new Rectangle(0, 0, 4, 4);
console.log(rect.area());
console.log(rect.toString());
```

The equivalent in C would be:

```c
#include <stdio.h>

typedef struct {
    int x1, y1, x2, y2;
} Rectangle;

int length(Rectangle rect) {
    return rect.y2 - rect.y1;
}

int width(Rectangle rect) {
    return rect.x2 - rect.x1;
}

int area(Rectangle rect) {
    return length(rect) * width(rect);
}

int is_square(Rectangle rect) {
    return width(rect) == length(rect);
}

void display(Rectangle rect) {
    printf("(%d, %d), (%d, %d)\n", rect.x1, rect.y1, rect.x2, rect.y2);
}

int main() {
    Rectangle rect = {0, 0, 4, 4};
    
    printf("Length: %d\n", length(rect));
    printf("Width: %d\n", width(rect));
    printf("Area: %d\n", area(rect));
    printf("Is Square: %s\n", is_square(rect) ? "Yes" : "No");
    
    display(rect);
    
    return 0;
}
```

Since there is no inheritance in C, the way a type advertises support for some _formatted_ representation is by defining a struct for Rectangle and a function to print its contents.:

```c
#include <stdio.h>

typedef struct {
    int x;
    int y;
    int width;
    int height;
} Rectangle;

void displayRectangle(Rectangle rect) {
    printf("Rectangle = { x: %d, y: %d, width: %d, height: %d }\n", rect.x, rect.y, rect.width, rect.height);
}

int main() {
    Rectangle rect = {12, 34, 56, 78};
    displayRectangle(rect);
    return 0;
}
```
