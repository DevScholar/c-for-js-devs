# Enumeration types (`enum`)

To convert a Rust enum to JavaScript, you can represent it using an object with key-value pairs.

```js
const DayOfWeek = {
    Sunday: 0,
    Monday: 1,
    Tuesday: 2,
    Wednesday: 3,
    Thursday: 4,
    Friday: 5,
    Saturday: 6
};
```

C syntax for doing the same:

```c
enum DayOfWeek
{
    Sunday = 0,
    Monday = 1,
    Tuesday = 2,
    Wednesday = 3,
    Thursday = 4,
    Friday = 5,
    Saturday = 6,
};
```

When dealing with an enum type in C, there is no inherent behavior or predefined functionality associated with instances of the enum.

In C, enums do not have built-in support for coercion to integral values. Developers need to manually implement functions to achieve this behavior. Here is an example of how to achieve similar functionality in C:

```c
#include <stdio.h>

typedef enum {
    Sunday = 0,
    Monday = 1,
    Tuesday = 2,
    Wednesday = 3,
    Thursday = 4,
    Friday = 5,
    Saturday = 6
} DayOfWeek;

int main() {
    DayOfWeek dow = Wednesday;
    printf("Day of week = %d\n", dow);

    if (dow == Friday) {
        printf("Yay! It's the weekend!\n");
    }

    // Coerce to integer
    int dowInt = (int)dow;
    printf("Day of week = %d\n", dowInt);

    // Manually coerce back to DayOfWeek
    DayOfWeek newDow = (DayOfWeek)dowInt;
    printf("Day of week = %d\n", newDow);

    return 0;
}
```

In C, enums can be casted manually to integral types and vice versa. This approach gives developers control over the conversion process, similar to the Rust example provided.

In C, to convert an integral type to an enum, one can create a function that maps integral values to an enum:

```c
#include <stdio.h>

typedef enum {
    Sunday = 0,
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday
} DayOfWeek;

DayOfWeek try_from_i32(int n) {
    if (n >= 0 && n <= 6) {
        return (DayOfWeek)n;
    } else {
        return -1; // Or any other error code indicating failure
    }
}

int main() {
    DayOfWeek dow = try_from_i32(5);
    if (dow != -1) {
        printf("Day of the week: %d\n", dow);
    } else {
        printf("Error: Invalid day\n");
    }

    dow = try_from_i32(50);
    if (dow != -1) {
        printf("Day of the week: %d\n", dow);
    } else {
        printf("Error: Invalid day\n");
    }

    return 0;
}
```

In this C code snippet, the try_from_i32 function maps the integer input to the corresponding DayOfWeek enum value if valid. It handles out-of-range values by returning an error code. 

In C, enum types can be utilized to create union types, allowing for different variants to store variant-specific data. This concept is similar to Rust's discriminated union types. Below is an example illustrating the use of enum for union types in C:

```c
#include <stdio.h>
#include <stdint.h>
#include <string.h>

enum IpAddrTag {
    V4,
    V6
};

struct IpAddrV4 {
    uint8_t octet1;
    uint8_t octet2;
    uint8_t octet3;
    uint8_t octet4;
};

struct IpAddrV6 {
    char address[40]; // Assuming a maximum IPv6 address length
};

union IpAddr {
    struct IpAddrV4 v4;
    struct IpAddrV6 v6;
};

int main() {
    union IpAddr home;
    home.v4.octet1 = 127;
    home.v4.octet2 = 0;
    home.v4.octet3 = 0;
    home.v4.octet4 = 1;

    union IpAddr loopback;
    strcpy(loopback.v6.address, "::1");

    return 0;
}
```

This form of `enum` declaration does not exist in JavaScript, but it can be emulated with classes:

```js
class IpAddr {
    constructor(v4, v6) {
        this.v4 = v4;
        this.v6 = v6;
    }
}

const home = new IpAddr([127, 0, 0, 1], null);
const loopback = new IpAddr(null, "::1");
```
