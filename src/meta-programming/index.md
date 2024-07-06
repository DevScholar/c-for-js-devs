# Meta Programming

Metaprogramming can be seen as a way of writing code that writes/generates other code.

JavaScript has the concept of metaprogramming, but it refers to intercepting and defining basic language operations, which is different from metaprogramming in C, C# or Rust. There is a JavaScript source generator called [hygen](https://github.com/jondot/hygen), but it does not call itself a "metaprogramming tool".

C does not support metaprogramming natively, however, third party tools, like [metalang99](https://github.com/Hirrolot/metalang99), exists.

C does not support reflection.

## Function-like macros

Function-like macros in C are in the following form: `#define`

The following code snippet defines a function-like macro named `print_something`, which is generating a `print_it` method for printing the "Something" string.

```c
#include <stdio.h>

#define print_something() printf("Something\n")

int main() {
    print_something();
    return 0;
}
```

## Derive macros

Derive macros are not supported in C and JavaScript.

## Attribute macros

Attribute macros are not supported in C and JavaScript.