# Strings

## Encoding issues with C language strings

In computers, characters are stored in binary encoding. A character set is a standard collection of encoded characters. In 1967, the American National Standards Institute (ANSI) released the ASCII character set that only supported the English alphabet, and then various countries and regions created various ASCII-compatible but incompatible character sets based on ASCII. These character sets are collectively known as multi-byte character sets (MBCS). In the 1990s, MBCS was the default standard for C character types and their associated functions. In 1991, the Unicode Consortium was formed to address the incompatibility of character sets, with the aim of creating a unified international character set standard. Initially, Unicode released the UTF-16 encoding standard, which is not ASCII compatible, and it has gradually and slowly gained support from the industry. Microsoft didn't release Windows 2000 with Unicode support until 2000.

However, on the Windows operating system platform, where compatibility is emphasized, the default standard for C character types and their associated functions is still the outdated MBCS (also known as narrow characters, functions end in A in the Windows API) rather than Unicode (at Microsoft, the word Unicode sometimes refers to UTF-16LE in code) (LE stands for Little Endian, which refers to the lower bytes first, The order in which the high-digit bytes are encoded after (also known as wide characters, functions end with W in the Windows API). For compatibility, Windows additionally defines the C character type 'wchar' and its associated functions for UTF-16LE strings, as well as the character type 'TCHAR' for both Unicode and non-Unicode compatibility (functions do not end with a capital A or W in the Windows API) and its related functions. This makes it difficult for Windows programmers.
Later, more advanced UTF-8 encoding was released. Unix-like operating systems (e.g., Linux, FreeBSD), who is not very compatible, changed the default standard for C character types and their associated functions to UTF-8. Due to compatibility, and a lack of emphasis on UTF-8, Windows does not support UTF-8 as a standard for encoding programs internally. Windows programmers are still miserable.


The turning point came in 2015, when Microsoft's new CEO, Satya Nadella, took office, and he launched a strategy to embrace open source software. In the open-source world, people generally use Linux. At this point, character encoding becomes a big problem. So, in 2019, starting with Windows version 1903 (May 2019 Update), you can use manifests to have processes use UTF-8 as process code pages. In order to avoid conflicts with UTF-16LE, Microsoft has modified the MBCS related APIs to do this. But for compatibility reasons, the preferred program internal encoding on Windows is still UTF-16LE. In addition, Microsoft began to develop a terminal "Windows Terminal" that supports Unicode (including UTF-16LE, UTF-8, etc.) to solve this problem. Previously, Windows Terminal did not support Unicode characters.
Now, Windows programmers can finally use UTF-8 as the default standard for C character types and their associated functions, as well as for the internal encoding of programs, by [adding a manifest].

[adding a manifest]: https://learn.microsoft.com/en-us/windows/apps/design/globalizing/use-utf8-code-page


There is a character type in C: `char`.

The mapping of those to JavaScript is shown in the following table:

| C      | JavaScript | Note        |
| ------ | ---------- | ----------- |
| `char` | `string`   | see Note 1. |

There are differences in working with strings in C and JavaScript, but the equivalents above should be a good starting point. One of the differences is that C characters are using different encodes, but JavaScript strings are UTF-16 encoded.

Notes:

1. JavaScript has only one string type, `string`. JavaScript has no pointer types.

JavaScript:

```js
let str = "Hello, World!"; 
let str = new String("Hello, World!")
```

C:

```c
char str[]= "Hello, World!"; 
```

## String Literals

String literals in JavaScript `String` types and allocated on the heap. In Rust, they are `char`, has a global lifetime and does not get allocated on the heap; they're embedded in the compiled binary.
JavaScript:

```js
let str = "Hello, World!";
```

C:

```c
char str[]= "Hello, World!"
```

JavaScript verbatim string literals are equivalent to Rust raw string literals.

JavaScript

```js
let str = `Hello, \World/!`;
```

C

```c
char *str = R"(Hello, \World/!)";
```

C UTF-8 string literals:

```c
char str[] = u8"hello";
```

## String Interpolation

JavaScript has a built-in string interpolation feature that allows you to embed expressions inside a string literal. The following example shows how to use string interpolation in JavaScript:

```javascript
let name = "John";
let age = 42;
let str = `Person Name: ${name}, Age: ${age} `;
```

C does not have a built-in string interpolation feature. Instead, the `printf` function is used to format a string. The following example shows how to use string interpolation in C:

```c
#include <stdio.h>

int main() {
    char name[] = "John";
    int age = 42;
    printf("Person Name: %s, Age: %d", name, age);
    return 0;
}
```

Custom classes and structs can also be interpolated in JavaScript due to the fact that the `toString()` method is available for each type as it inherits from `object`.

```js
class Person {
    constructor({
        name,
        age
    }) {
        this.name = name;
        this.age = age;
        this.toString = function() {
            return `Person Name: ${name}, Age: ${age}`;
        }
    }

}
let person = new Person({
    name: "John",
    age: 42
});
console.log(person);
```

In C, there is no default formatting implemented/inherited for each type. We use structs instead of classes, and function pointers to simulate methods in classes.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    char *name;
    int age;
    char* (*toString)(void);
} Person;

char* Person_toString() {
    return "Person Name: %s, Age: %d";
}

Person* new_Person(char* name, int age) {
    Person* person = (Person*)malloc(sizeof(Person));
    person->name = name;
    person->age = age;
    person->toString = Person_toString;
    return person;
}

int main() {
    Person* person = new_Person("John", 42);
    printf(person->toString(), person->name, person->age);
    free(person);
    return 0;
}

```

## Text Replacing

C doesn't have a built-in string replacement feature, so replacing strings is very complex in C. Here's the code for a C function to replace a string:

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

/**
 * @brief Replace a substring in a string with another substring.
 * @param str The original string.
 * @param find The substring to find.
 * @param replace The substring to replace.
 * @param case_sensitive Flag to indicate case sensitivity (1 for case-sensitive, 0 for case-insensitive).
 * @param global_replace Flag to enable global replacement (1 for global, 0 for first occurrence only).
 * @return The modified string after replacement.
 */
char* replace_str(const char* str, const char* find, const char* replace, int case_sensitive, int global_replace) {
    char* result;
    int i, count = 0;
    int find_len = strlen(find);
    int replace_len = strlen(replace);

    for (i = 0; str[i] != '\0';) {
        if ((case_sensitive ? strncmp(&str[i], find, find_len) : strncasecmp(&str[i], find, find_len)) == 0) {
            count++;
            if (!global_replace) break;
            i += find_len;
        } else {
            i++;
        }
    }

    result = (char*)malloc(strlen(str) + count * (replace_len - find_len) + 1);

    i = 0;
    while (*str) {
        if ((case_sensitive ? strncmp(str, find, find_len) : strncasecmp(str, find, find_len)) == 0) {
            strcpy(&result[i], replace);
            i += replace_len;
            str += find_len;
            if (!global_replace) break;
        } else {
            result[i++] = *str++;
        }
    }
    result[i] = '\0';
    return result;
}

int main() {
    const char* original_str = "Hello, World!";
    const char* find_str = "World";
    const char* replacement_str = "C Language";

    char* new_str = replace_str(original_str, find_str, replacement_str, 0, 1); // Case-insensitive, global replacement
    printf("Original String: %s\n", original_str);
    printf("New String: %s\n", new_str);

    free(new_str);
    return 0;
}
```
<!--
Another option is to use the `std::fmt::Debug` trait. The `Debug` trait is implemented for all standard types and can be used to print the internal representation of a type. The following example shows how to use the `derive` attribute to print the internal representation of a custom struct using the `Debug` macro. This declaration is used to automatically implement the `Debug` trait for the `Person` struct:

```rust
#[derive(Debug)]
struct Person {
    name: String,
    age: i32,
}

let person = Person {
    name: "John".to_owned(),
    age: 42,
};

println!("{person:?}");
```

> Note: Using the :? format specifier will use the `Debug` trait to print the struct, where leaving it out will use the `Display` trait.

See also:

- [Rust by Example - Debug](https://doc.rust-lang.org/stable/rust-by-example/hello/print/print_debug.html?highlight=derive#debug)
-->