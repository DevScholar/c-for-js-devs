# Environment and Configuration

## Accessing environment variables

JavaScript doesn't provide access to environment variables natively. However, some non-browser JavaScript runtimes, such as Node.js and Node provides access to environment variables.

In Node.js:

```js
const name = "EXAMPLE_VARIABLE";

let value = process.env[name];
if (!value) {
    console.log(`Variable '${name}' not set.`);
} else {
    console.log(`Variable '${name}' set to '${value}'.`);
}
```
In Deno:

```js
const name = "EXAMPLE_VARIABLE";

let value = Deno.env.get(name);
if (!value) {
    console.log(`Variable '${name}' not set.`);
} else {
    console.log(`Variable '${name}' set to '${value}'.`);
}
```

In C programming, environmental variables can be accessed using the getenv() function provided by the standard library <stdlib.h>. This function allows a program to retrieve the value of a specific environmental variable by providing its name as an argument.

Here is a simple example demonstrating how to retrieve the value of an environmental variable named PATH:

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    char *path_value = getenv("PATH");

    if (path_value != NULL) {
        printf("The value of PATH is: %s\n", path_value);
    } else {
        printf("PATH is not set.\n");
    }

    return 0;
}
```

In C, accessing environment variables at compile time involves utilizing preprocessor directives and macros to incorporate the values of environment variables during the compilation phase. This process allows for the configuration of the program based on the environment where it will run without the need for runtime modifications.

One common approach to achieve this functionality is by using the -D flag in the compiler command to define a macro with the value of the desired environment variable. For instance, consider an environment variable MY_ENV_VAR that you want to access at compile time. You can pass this variable's value to the compiler using the -D flag as follows:

```c
#include <stdio.h>

#ifndef MY_ENV_VAR
    #define MY_ENV_VAR "default_value"
#endif

int main() {
    printf("Value of MY_ENV_VAR: %s\n", MY_ENV_VAR);
    return 0;
}
```
When compiling the program, it is possible to set the value of MY_ENV_VAR by defining it during compilation:

```bash
gcc -o myprogram myprogram.c -DMY_ENV_VAR='"custom_value"'
```

## Configuration

JavaScript doesn't support configurations, neither nor C.