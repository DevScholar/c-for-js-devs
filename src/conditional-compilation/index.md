# Conditional Compilation

Both JavaScript and C are providing the possibility for compiling specific code based on external conditions.

JavaScript doesn't support conditional compilation natively. However, it is possible to use some third-party tool like [`babel-plugin-preprocessor`][preproc-dir] in order to control conditional compilation. 

```js
//#if DEBUG
    console.log("Debug");
//#else
    console.log("Not debug");
//#endif
```

An example that uses vanilla JavaScript:

```js
let isDebug = true;

if(isDebug)
{
    window.eval(`
    console.log("Debug");
    `);
} else {
    window.eval(`
    console.log("Not debug");
    `);
}
```

Conditional compilation in C allows developers to include or exclude sections of code during the compilation process based on certain conditions. This feature is particularly useful when different versions of a program need to be generated for various platforms or configurations without modifying the source code.

In C, conditional compilation is achieved using preprocessor directives, which are processed before the actual compilation of the code. The #ifdef, #ifndef, #else, #elif, and #endif directives are commonly used for conditional compilation.

Here is a simple example to illustrate conditional compilation in C:

```c
#include <stdio.h>

#define DEBUG 1

int main() {
    #ifdef DEBUG
        printf("Debug mode is enabled.\n");
    #else
        printf("Debug mode is disabled.\n");
    #endif

    return 0;
}
```
