# Structured Types

Commonly used object and collection types in JavaScript and their mapping to Rust.

| JavaScript | C                                        |
| ---------- | ---------------------------------------- |
| `array`    | (The type of an array varies with types) |

## Array

Fixed arrays are supported the same way in C as in JavaScript.

JavaScript:

```js
let someArray = [1,2];
```

C:

```c
int someArray[2] = {1, 2};
```

## List

In C the equivalent of a `array` is an array.

JavaScript:

```js
let something = ["a", "b"];
something.push("c");
```

c:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char **something = malloc(2 * sizeof(char*));
    something[0] = "a";
    something[1] = "b";
    int size = 2;
    
    // Before adding an element
    for (int i = 0; i < size; i++) {
        printf("%s ", something[i]);
    }
    
    // Adding an element
    char *new_element = "c";
    size++;
    char **temp = realloc(something, size * sizeof(char*));
    if (temp != NULL) {
        something = temp;
        something[size - 1] = new_element;
    }
    
    // After adding an element
    printf("\n");
    for (int i = 0; i < size; i++) {
        printf("%s ", something[i]);
    }
    
    // Release the memory.
    free(something);
    
    return 0;
}
```

## Tuples

JavaScript:

```js
const let something = [1, 2];
console.log(`a = ${something[0]} b = ${something[1]}`);
```

In C, arrays are used to simulate the concept of tuples:

```c
#include <stdio.h>

int main() {
    int something[] = {1, 2};
    printf("a = %d b = %d", something[0], something[1]);
    return 0;
}

// deconstruction supported
#include <stdio.h>

int main() {
    // Define a tuple
    struct Tuple {
        int a;
        int b;
    };

    // Initialize the tuple
    struct Tuple something = {1, 2};

    // Deconstruct tuples
    int a = something.a;
    int b = something.b;
    printf("a = %d b = %d\n", a, b);

    return 0;
}

```

> **NOTE**: C tuple elements cannot be named. The only way to access a tuple element is by using the index of the element or deconstructing the tuple.

## Dictionary

In Rust the equivalent of a `Dictionary` is a `object`.

JavaScript:

```js
var something = {
    "Foo": "Bar",
    "Baz": "Qux"
};

something["hi"] = "there";
```

c:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define INITIAL_CAPACITY 2

typedef struct {
    char* key;
    char* value;
} Entry;

typedef struct {
    Entry* entries;
    int capacity;
    int size;
} HashMap;

void insert(HashMap* map, char* key, char* value) {
    if (map->size >= map->capacity) {
        map->capacity *= 2;
        map->entries = realloc(map->entries, map->capacity * sizeof(Entry));
    }
    map->entries[map->size].key = strdup(key);
    map->entries[map->size].value = strdup(value);
    map->size++;
}

void printHashMap(HashMap* map) {
    printf("HashMap Contents:\n");
    for (int i = 0; i < map->size; i++) {
        printf("Key: %s, Value: %s\n", map->entries[i].key, map->entries[i].value);
    }
}


int main() {
    HashMap something;
    something.entries = malloc(INITIAL_CAPACITY * sizeof(Entry));
    something.capacity = INITIAL_CAPACITY;
    something.size = 0;

    insert(&something, "Foo", "Bar");
    insert(&something, "Baz", "Qux");
    insert(&something, "hi", "there");

    // Add any other actions here or print the code for the HashMap
    printHashMap(&something); 
    return 0;
}


```

See also:

- [Struct declaration - C Reference](https://en.cppreference.com/w/c/language/struct)
