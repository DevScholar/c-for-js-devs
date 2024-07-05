# Exception Handling

In JavaScript, an exception should always be an [`Error`][js-system-exception] object or an instance of an `Error` subclass. Exceptions are thrown if a problem occurs in a code section. A thrown exception is passed up the stack until the application handles it or the program terminates.

In C, error handling is typically done using return values or error codes. To convert the Rust code snippet provided to C, one would need to implement error handling using return values or custom error structures.

## Custom error types

An example on [how to create user-defined exceptions][js-user-defined-exceptions]:

```js
class EmployeeListNotFoundException extends Error {
    constructor(message) {
        super(message);
        this.name = 'EmployeeListNotFoundException';
    }
}
```

In C, one can achieve similar error handling by defining custom error structures and functions.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    const char* message;
} EmployeeListNotFound;

const char* EmployeeListNotFound_message(EmployeeListNotFound* err) {
    return err->message;
}

void EmployeeListNotFound_free(EmployeeListNotFound* err) {
    free(err);
}

int main() {
    EmployeeListNotFound* err = (EmployeeListNotFound*)malloc(sizeof(EmployeeListNotFound));
    err->message = "Could not find employee list.";

    // Example usage
    printf("%s\n", EmployeeListNotFound_message(err));

    EmployeeListNotFound_free(err);

    return 0;
}
```

In C, the equivalent of the JavaScript `Error.cause` property can be achieved by defining a custom error structure that includes a field to store the error cause or source. Here is a simplified example:

```c
#include <stdio.h>

typedef struct {
    const char* cause; // Field to store the error cause/source
} Error;

int main() {
    Error customError;
    customError.cause = "Custom error message";
    
    printf("Error Cause: %s\n", customError.cause);
    
    return 0;
}
```

## Raising exceptions

To raise an error in JavaScript, throw an error:

```js
function throwIfNegative(value) {
    if (value < 0) {
        throw new Error('Value cannot be negative');
    }
}

```

To mimic recoverable errors in C, an enum is used to define custom result types. The function error_if_negative checks if the input value is negative and returns the appropriate result. The main function demonstrates how to use this error handling mechanism in C.

```c
#include <stdio.h>

typedef enum {
    OK,
    ERR
} Result;

Result error_if_negative(int value) {
    if (value < 0) {
        return ERR;
    } else {
        return OK;
    }
}

int main() {
    int input = -5;
    Result result = error_if_negative(input);

    if (result == ERR) {
        printf("Error: Specified argument was out of the range of valid values. (Parameter 'value')\n");
    } else {
        printf("No error. Value is non-negative.\n");
    }

    return 0;
}
```

In C, a way to handle unrecoverable errors is typically achieved using the abort() function from the `<stdlib.h>` library. 

```c
#include <stdio.h>
#include <stdlib.h>

void panic_if_negative(int value) {
    if (value < 0) {
        fprintf(stderr, "Specified argument was out of the range of valid values. (Parameter 'value')\n");
        abort();
    }
}

int main() {
    //Main code logic here
    return 0;
}
```

## Error propagation

In JavaScript, exceptions are passed up until they are handled or the program terminates. In Rust, unrecoverable errors behave similarly, but handling them is uncommon.

Recoverable errors, however, need to be propagated and handled explicitly. Their presence is always indicated by the Rust function or method signature. Catching an exception allows you to take action based on the presence or absence of an error in JavaScript:

```js
//JavaScript doesn't have a file system in it. People often implement file systems using the BrowserFS library that mimic Node.js APIs.
function write() {
    try {
        fs.writeFileSync('file.txt', 'content');
    } catch (error) {
        console.log('Writing to file failed.');
    }
}

```

In C, error handling is typically done using return values rather than exceptions. One would need to use functions that return error codes to indicate success or failure instead of throwing exceptions.

```c
#include <stdio.h>

int write() {
    FILE *file = fopen("file.txt", "w");
    if (file == NULL) {
        printf("Error opening file.\n");
        return -1; // Return an error code
    }

    if (fprintf(file, "content") < 0) {
        printf("Error writing to file.\n");
        fclose(file);
        return -2; // Return a different error code
    }

    fclose(file);
    return 0; // Return 0 for success
}

int main() {
    int result = write();
    if (result != 0) {
        printf("Writing to file failed.\n");
    }
    return 0;
}
```

C does not have a built-in operator like Rust's ? for error propagation. In C, error handling is typically done using return values or error codes. 

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *file = fopen("file.txt", "w");
    if (file == NULL) {
        perror("Error opening file");
        return -1; // Error code for file opening failure
    }

    if (fwrite("content", sizeof(char), 7, file) != 7) {
        perror("Error writing to file");
        fclose(file);
        return -2; // Error code for incomplete write
    }

    fclose(file);
    return 0; // Success
}
```

## Stack traces

<!--Throwing an unhandled exception in .NET will cause the runtime to print a stack
trace that allows debugging the problem with additional context.-->

In C, handling unrecoverable errors can be achieved using the abort() function, which terminates the program immediately. For recoverable errors, C does not have built-in support for backtraces. However, it is possible to implement a basic form of error handling using return codes or custom error structures to indicate and handle errors in a recoverable manner. To backtrace, developers often resort to manual logging or stack tracing techniques in C.

[js-system-exception]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error
[rust-result]: https://doc.rust-lang.org/std/result/enum.Result.html
[panic-backtrace]: https://doc.rust-lang.org/book/ch09-01-unrecoverable-errors-with-panic.html#using-a-panic-backtrace
[js-user-defined-exceptions]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error/Error
[rust-std-error]: https://doc.rust-lang.org/std/error/trait.Error.html
[provide method]: https://doc.rust-lang.org/std/error/trait.Error.html#method.provide
[question-mark-operator]: https://doc.rust-lang.org/std/result/index.html#the-question-mark-operator-
[panic]: https://doc.rust-lang.org/std/macro.panic.html
[propagating-errors-rust-book]: https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html#a-shortcut-for-propagating-errors-the--operator
[trait object]: https://doc.rust-lang.org/reference/types/trait-object.html
