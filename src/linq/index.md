# LINQ

This section discusses LINQ within the context and for the purpose of querying or transforming sequences and typically collections like lists, sets and dictionaries.

## Enumerable items

The equivalent of enumerable items in C is `enum`. In JavaScript, there is `forEach`: 

```js
let values = [1, 2, 3, 4, 5];
let output = '';

values.forEach((value, index) => {
    if (index > 0)
        output += ', ';
    output += value;
});

console.log(output); // Outputs: 1, 2, 3, 4, 5
```

In C, the equivalent is simply `for`:

```c
u#include <stdio.h>
#include <string.h> // Include the string.h header file

int main() {
    int values[] = {1, 2, 3, 4, 5};
    char output[50] = "";
    
    for (int i = 0; i < 5; i++) {
        if (i > 0)
            sprintf(output + strlen(output), ", ");
        sprintf(output + strlen(output), "%d", values[i]);
    }

    printf("%s\n", output); // Outputs: 1, 2, 3, 4, 5

    return 0;
}
```

The `for` loop over an iterable essentially gets desuraged to the following:

```c
#include <stdio.h>
#include <string.h>
int main() {
    int values[] = {1, 2, 3, 4, 5};
    char output[50] = "";
    int i;

    for (i = 0; i < sizeof(values) / sizeof(values[0]); i++) {
        if (i > 0) {
            sprintf(output + strlen(output), ", ");
        }
        sprintf(output + strlen(output), "%d", values[i]);
    }

    printf("%s\n", output);
    return 0;
}
```

<!--

Rust's ownership and data race condition rules apply to all instances and data, and iteration is no exception. So while looping over an array might look straightforward, one has to be mindful about ownership when needing to iterate the same collection/iterable more than once. The following example iteraters the list of integers twice, once to print their sum and another time to determine and print the maximum integer: 

```rust
fn main() {
    let values = vec![1, 2, 3, 4, 5];

    // sum all values

    let mut sum = 0;
    for value in values {
        sum += value;
    }
    println!("sum = {sum}");

    // determine maximum value

    let mut max = None;
    for value in values {
        if let Some(some_max) = max { // if max is defined
            if value > some_max {     // and value is greater
                max = Some(value)     // then note that new max
            }
        } else {                      // max is undefined when iteration starts
            max = Some(value)         // so set it to the first value
        }
    }
    println!("max = {max:?}");
}
```

However, the code above is rejected by the compiler due to a subtle difference: `values` has been changed from an array to a [`Vec<int>`][vec.rs], a _vector_, which is Rust's type for growable arrays. The first iteration of `values` ends up _consuming_ each value as the integers are summed up. In other words, the ownership of _each item_ in the vector passes to the iteration variable of the loop: `value`. Since `value` goes out of scope at the end of each iteration of the loop, the instance it owns is dropped. Had `values` been a vector of heap-allocated data, the heap memory backing each item would get freed as the loop moved to the next item. To fix the problem, one has to request iteration over _shared_ references via `&values` in the `for` loop. As a result, `value` ends up being a shared reference to an item as opposed to taking its ownership.

  [vec.rs]: https://doc.rust-lang.org/stable/std/vec/struct.Vec.html

Below is the updated version of the previous example that compiles. The fix is to simply replace `values` with `&values` in each of the `for` loops.

```rust
fn main() {
    let values = vec![1, 2, 3, 4, 5];

    // sum all values

    let mut sum = 0;
    for value in &values {
        sum += value;
    }
    println!("sum = {sum}");

    // determine maximum value

    let mut max = None;
    for value in &values {
        if let Some(some_max) = max { // if max is defined
            if value > some_max {     // and value is greater
                max = Some(value)     // then note that new max
            }
        } else {                      // max is undefined when iteration starts
            max = Some(value)         // so set it to the first value
        }
    }
    println!("max = {max:?}");
}
```

The ownership and dropping can be seen in action even with `values` being an array instead of a vector. Consider just the summing loop from the above example over an array of a structure that wraps an integer:

```rust
struct Int(i32);

impl Drop for Int {
    fn drop(&mut self) {
        println!("{} dropped", self.0)
    }
}

fn main() {
    let values = [Int(1), Int(2), Int(3), Int(4), Int(5)];
    let mut sum = 0;

    for value in values {
        sum += value.0;
    }

    println!("sum = {sum}");
}
```

`Int` implements `Drop` so that a message is printed when an instance get dropped. Running the above code will print:

    value = Int(1)
    Int(1) dropped
    value = Int(2)
    Int(2) dropped
    value = Int(3)
    Int(3) dropped
    value = Int(4)
    Int(4) dropped
    value = Int(5)
    Int(5) dropped
    sum = 15

It's clear that each value is acquired and dropped while the loop is running. Once the loop is complete, the sum is printed. If `values` in the `for` loop is changed to `&values` instead, like this:

```rust
for value in &values {
    // ...
}
```

then the output of the program will change radically:

    value = Int(1)
    value = Int(2)
    value = Int(3)
    value = Int(4)
    value = Int(5)
    sum = 15
    Int(1) dropped
    Int(2) dropped
    Int(3) dropped
    Int(4) dropped
    Int(5) dropped

This time, values are acquired but not dropped while looping because each item doesn't get owned by the interation loop's variable. The sum is printed once the loop is done. Finally, when the `values` array that still owns all the the `Int` instances goes out of scope at the end of `main`, its dropping in turn drops all the `Int` instances.

These examples demonstrate that while iterating collection types may seem to have a lot of parallels between Rust and JavaScript, from the looping constructs to the iteration abstractions, there are still subtle differences with respect to ownership that can lead to the compiler rejecting the code in some instances.

See also:

- [Iterator][iter-mod]
- [Iterating by reference]

[into-iter.rs]: https://doc.rust-lang.org/std/iter/trait.IntoIterator.html
[iter.rs]: https://doc.rust-lang.org/core/iter/trait.Iterator.html
[iter-mod]: https://doc.rust-lang.org/std/iter/index.html
[iterating by reference]: https://doc.rust-lang.org/std/iter/index.html#iterating-by-reference
-->
## Operators

JavaScript and C don't natively support LINQ, but there is a project called [LINQ.js](https://github.com/mihaifm/linq) that implements LINQ in C# for JavaScript, and a project called [linq4c](https://github.com/haifenghuang/linq4c) that implements LINQ in C# for C.

_Operators_ in LINQ are implemented in the form of LINQ.js extension methods that can be chained together to form a set of operations, with the most common forming a query over some sort of data source. LINQ.js also offers a SQL-inspired _query syntax_ with clauses like `from`, `where`, `select`, `join` and others that can serve as an alternative or a companion to method chaining. Many imperative loops can be re-written as much more expressive and composable queries in LINQ.

## Deferred execution (laziness)

Many operators in LINQ are designed to be lazy such that they only do work when absolutely required. This enables composition or chaining of several operations/methods without causing any side-effects.

In both cases, this allows _infinite sequences_ to be represented, where the underlying sequence is infinite, but the developer decides how the sequence should be terminated. The following example shows this in JavaScript:

```js
function* infiniteRange() {
    for (let i = 0; ; ++i) {
        yield i;
    }
}

for (let x of infiniteRange()) {
    if (x < 5) {
        console.log(x); // Prints "0 1 2 3 4"
    } else {
        break;
    }
}
```

C supports the same concept through `for`:

```c
#include <stdio.h>

int main() {
    int value;
    for (value = 0; value < 5; value++) {
        printf("%d ", value); // Prints "0 1 2 3 4"
    }
    return 0;
}
```

## Iterator Methods (`yield`)

JavaScript has the `yield` keword that enables the developer to quickly write an _iterator method_. C doesn't support coroutines natively, but there is a [tutorial of implementing coroutines in C](https://www.chiark.greenend.org.uk/~sgtatham/coroutines.html).
