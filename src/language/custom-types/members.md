# Members

## Constructors

In C programming, constructors are not explicitly defined. Instead, you can achieve similar functionality by using factory functions that create and initialize instances of a struct. These factory functions can be standalone functions or associated functions of the struct. Conventionally, if there is only one factory function for a struct, it is commonly named new.

```c
#include <stdio.h>

typedef struct {
    int x1, y1, x2, y2;
} Rectangle;

Rectangle newRectangle(int x1, int y1, int x2, int y2) {
    Rectangle rect;
    rect.x1 = x1;
    rect.y1 = y1;
    rect.x2 = x2;
    rect.y2 = y2;
    return rect;
}

int main() {
    Rectangle myRect = newRectangle(0, 0, 100, 100);
    printf("Rectangle coordinates: (%d, %d), (%d, %d)\n", myRect.x1, myRect.y1, myRect.x2, myRect.y2);
    return 0;
}
```
<!--
Since Rust functions (associated or otherwise) do not support overloading; the factory functions have to be named uniquely. For example, below are some examples of so-called constructors or factory functions available on `String`:

- `String::new`: creates an empty string.
- `String::with_capacity`: creates a string with an initial buffer capacity.
- `String::from_utf8`: creates a string from bytes of UTF-8 encoded text.
- `String::from_utf16`: creates a string from bytes of UTF-16 encoded text.

In the case of an `enum` type in Rust, the variants act as the constructors. See [the section on enumeration types][enums] for more.

See also:

- [Constructors are static, inherent methods (C-CTOR)][rs-api-C-CTOR]

  [enums]: enums.md
  [rs-api-C-CTOR]: https://rust-lang.github.io/api-guidelines/predictability.html?highlight=new#constructors-are-static-inherent-methods-c-ctor
-->
## Methods (static & instance-based)

C types (both `enum` and `struct`), can have static and instance-based methods. <!--In Rust-speak, a _method_ is always instance-based and is identified by the fact that its first parameter is named `self`. The `self` parameter has no type annotation since it's always the type to which the method belongs. A static method is called an _associated function_. In the example below, `new` is an associated function and the rest (`length`, `width` and `area`) are methods of the type:-->

```c
#include <stdio.h>

typedef struct {
    int x1, y1, x2, y2;
} Rectangle;

Rectangle new_rectangle(int x1, int y1, int x2, int y2) {
    Rectangle rect;
    rect.x1 = x1;
    rect.y1 = y1;
    rect.x2 = x2;
    rect.y2 = y2;
    return rect;
}

int length(Rectangle *rect) {
    return rect->y2 - rect->y1;
}

int width(Rectangle *rect) {
    return rect->x2 - rect->x1;
}

int area(Rectangle *rect) {
    return length(rect) * width(rect);
}

int main() {
    Rectangle rect = new_rectangle(0, 0, 4, 3);
    printf("Length: %d\n", length(&rect));
    printf("Width: %d\n", width(&rect));
    printf("Area: %d\n", area(&rect));
    return 0;
}
```

## Constants

A type in C can have constants. However, the most interesting aspect to note is that C allows a type instance to be defined as a constant too:

```c
struct Point {
    int x;
    int y;
};

const struct Point ZERO = {0, 0};
```

In JavaScript, you need to define a Point class, and then, simulate the construction behavior in Rust by setting a static property ZERO directly on the Point class.

```js
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
}

Point.ZERO = new Point(0, 0);
```

## Events

C has no built-in support for type members to adverstise and fire events.

## Properties

In C, there are no built-in properties. To mimic property-like behavior, the user can use getter and setter functions.

```c
#include <stdio.h>

typedef struct {
    int x1, y1, x2, y2;
} Rectangle;

Rectangle newRectangle(int x1, int y1, int x2, int y2) {
    Rectangle rect = {x1, y1, x2, y2};
    return rect;
}

// Getter functions
int getX1(Rectangle *rect) { return rect->x1; }
int getY1(Rectangle *rect) { return rect->y1; }
int getX2(Rectangle *rect) { return rect->x2; }
int getY2(Rectangle *rect) { return rect->y2; }

// Setter functions
void setX1(Rectangle *rect, int val) { rect->x1 = val; }
void setY1(Rectangle *rect, int val) { rect->y1 = val; }
void setX2(Rectangle *rect, int val) { rect->x2 = val; }
void setY2(Rectangle *rect, int val) { rect->y2 = val; }

// Computed properties
int length(Rectangle *rect) { return rect->y2 - rect->y1; }
int width(Rectangle *rect) { return rect->x2 - rect->x1; }
int area(Rectangle *rect) { return length(rect) * width(rect); }

int main() {
    Rectangle rect = newRectangle(0, 0, 10, 20);
    printf("Area of the rectangle: %d\n", area(&rect));
    return 0;
}
```

## Extension Methods

In JavaScript, you can use prototype to add new methods to existing classes. This approach allows you to add new behavior to an existing class without changing the existing class definition:

```js
//JavaScript doesn't have a StringBuilder class. This code is only used to demonstrate adding a new method to an existing class.
class StringBuilder {
    constructor(initialString) {
        this.value = initialString;
    }

    toString() {
        return this.value;
    }
}
StringBuilder.prototype.wrap = function (left, right) {
        this.value = left + this.value + right;
    }
const sb = new StringBuilder("Hello, World!");
sb.wrap(">>> ", " <<<");
console.log(sb.toString()); 
```

C does not have built-in support for extension methods or traits. To achieve similar functionality in C, one can use function pointers or structures to mimic extension methods. Here's a simplified example in C to demonstrate extending a type with a method:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Define the StringExt structure before using it.
typedef struct StringExt {
    char* data;
    // The wrap function should take a pointer to StringExt to modify the original structure.
    void (*wrap)(struct StringExt*, const char*, const char*);
} StringExt;

// The wrap function now correctly takes a pointer to a StringExt structure.
void wrap(struct StringExt* self, const char* left, const char* right) {
    if (self == NULL || self->data == NULL || left == NULL || right == NULL) {
        return; // Safety check to avoid dereferencing NULL pointers.
    }
    // Allocate memory for the new string, including space for the null terminator.
    char* temp = malloc(strlen(left) + strlen(self->data) + strlen(right) + 1);
    if (temp == NULL) {
        return; // Check for failed memory allocation.
    }
    // Concatenate the strings in the correct order.
    strcpy(temp, left);
    strcat(temp, self->data);
    strcat(temp, right);
    // Free the old data and update the StringExt structure with the new data.
    free(self->data);
    self->data = temp;
}

int main() {
    // Declare the StringExt variable as an actual structure, not just a type.
    StringExt s = { .data = strdup("Hello, World!"), .wrap = wrap };

    // Call the wrap function with the address of the StringExt structure.
    s.wrap(&s, ">>> ", " <<<");
    printf("%s\n", s.data); // Should now print: >>> Hello, World! <<<

    free(s.data); // Free the allocated memory for the data string.
    return 0;
}
```

<!--Just like in C#, for the method in the extension trait to become available
(2), the extension trait must be imported (1).--> Also note, the extension trait identifier `StrWrapExt` can itself be discarded via `_` at the time of import without affecting the availability of `wrap` for `String`.

## Visibility/Access modifiers

In JavaScript, there is no explicit visibility modifier like in C#, but similar functionality can be achieved with some conventions.

In C, visibility and access control are primarily achieved through the use of header files and the concept of translation units. By declaring functions and variables in header files and including them in source files, C provides a way to control visibility. To mimic private members, one can use static variables or functions within a source file, limiting their scope to that file. For public visibility, declaring functions and variables in header files and including those headers in multiple source files allows for shared access. While C lacks explicit modifiers like pub in Rust, the structuring of code using header files and source files provides a similar level of control over visibility and access.
<!--
For more details, see the [Visibility and Privacy][privis] section of The Rust
Reference.

  [privis]: https://doc.rust-lang.org/reference/visibility-and-privacy.html

  <!--

The table below is an approximation of the mapping of C# and Rust modifiers:

| C#                            | Rust         | Note        |
| ----------------------------- | ------------ | ----------- |
| `private`                     | (default)    | See note 1. |
| `protected`                   | N/A          | See note 2. |
| `internal`                    | `pub(crate)` |             |
| `protected internal` (family) | N/A          | See note 2. |
| `public`                      | `pub`        |             |

1. There is no keyword to denote private visibility; it's the default in Rust.

2. Since there are no class-based type hierarchies in Rust, there is no
   equivalent of `protected`.
-->
## Mutability

When designing a type in JavaScript, it is not the responsiblity of the developer to decide whether the a type is mutable or immutable; whether it supports destructive or non-destructive mutations. In Rust, mutability is expressed on methods through the type of the parameters as shown in the example below:

```c
#include <stdio.h>

struct Point {
    int x;
    int y;
};

struct Point new_point(int x, int y) {
    struct Point p;
    p.x = x;
    p.y = y;
    return p;
}

int get_x(struct Point *p) {
    return p->x;
}

int get_y(struct Point *p) {
    return p->y;
}

void set_x(struct Point *p, int val) {
    p->x = val;
}

void set_y(struct Point *p, int val) {
    p->y = val;
}

int main() {
    struct Point p = new_point(3, 4);
    printf("Point coordinates: (%d, %d)\n", get_x(&p), get_y(&p));
    set_x(&p, 7);
    set_y(&p, 8);
    printf("Updated point coordinates: (%d, %d)\n", get_x(&p), get_y(&p));
    return 0;
}

```

In JavaScript, use ES6's destructuring assignment and object extension syntax to implement non-destructive mutation:

```js
class Point {
    constructor(X, Y) {
        this.X = X;
        this.Y = Y;
    }
}

let pt = new Point(123, 456);
pt = { ...pt, X: 789 };
console.log(pt); // prints: Point { X = 789, Y = 456 }
```

There is no `with` in C, but to emulate something similar in C, it has to be baked into the type's design:

```c
#include <stdio.h>

typedef struct {
    int x;
    int y;
} Point;

Point new_point(int x, int y) {
    Point p;
    p.x = x;
    p.y = y;
    return p;
}

int get_x(Point p) {
    return p.x;
}

int get_y(Point p) {
    return p.y;
}

Point set_x(Point p, int val) {
    p.x = val;
    return p;
}

Point set_y(Point p, int val) {
    p.y = val;
    return p;
}

int main() {
    Point p = new_point(3, 4);
    printf("Initial Point: x=%d, y=%d\n", get_x(p), get_y(p));

    p = set_x(p, 7);
    p = set_y(p, 9);
    printf("Modified Point: x=%d, y=%d\n", get_x(p), get_y(p));

    return 0;
}
```

In JavaScript, classes are used to simulate structs, and destructuring objects is assigned to implement something like `with`.

```js
class Point {
    constructor(x, y) {
        this.X = x;
        this.Y = y;
    }

    toString() {
        return `(${this.X}, ${this.Y})`;
    }
}

let pt = new Point(123, 456);
console.log(pt.toString()); // prints: (123, 456)
pt = { ...pt, X: 789 };
console.log(pt.toString()); // prints: (789, 456)

```

C has another syntax that may seem similar:

```c
#include <stdio.h>

typedef struct {
    int x;
    int y;
} Point;

Point createPoint(int x, int y) {
    Point pt;
    pt.x = x;
    pt.y = y;
    return pt;
}

void printPoint(Point pt) {
    printf("Point { x: %d, y: %d }\n", pt.x, pt.y);
}

Point set_x(Point p, int val) {
    p.x = val;
    return p;
}

Point set_y(Point p, int val) {
    p.y = val;
    return p;
}

int get_x(Point p) {
    return p.x;
}

int get_y(Point p) {
    return p.y;
}

int main() {
    Point p = createPoint(3, 4);
    printf("Initial Point: x=%d, y=%d\n", get_x(p), get_y(p));

    p = set_x(p, 7);
    p = set_y(p, 9);
    printf("Modified Point: x=%d, y=%d\n", get_x(p), get_y(p));

    return 0;
}
```

<!--However, while `with` in C# does a non-destructive mutation (copy then update), the [struct update syntax] does (partial) _moves_ and works with fields only. Since the syntax requires access to the type's fields, it is generally more common to use it within the Rust module that has access to private details of its types.

  [struct update syntax]: https://doc.rust-lang.org/stable/book/ch05-01-defining-structs.html#creating-instances-from-other-instances-with-struct-update-syntax
-->