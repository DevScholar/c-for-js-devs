# Operator overloading

JavaScript doesn't support operator overloading. Consider the following example in JavaScript:

```js
class Fraction {
    constructor(numerator, denominator) {
        this.numerator = numerator;
        this.denominator = denominator;
    }

    static add(a, b) {
        return new Fraction(a.numerator * b.denominator + b.numerator * a.denominator, a.denominator * b.denominator);
    }

    toString() {
        return `${this.numerator}/${this.denominator}`;
    }
}

console.log(Fraction.add(new Fraction(5, 4), new Fraction(1, 2)).toString());  // Output "14/8"
```

In C, operator overloading is not directly supported. However, it is possible to achieve similar functionality by defining functions that mimic operator behavior.

```c
#include <stdio.h>

typedef struct {
    int numerator;
    int denominator;
} Fraction;

Fraction addFractions(Fraction f1, Fraction f2) {
    Fraction result;
    result.numerator = f1.numerator * f2.denominator + f2.numerator * f1.denominator;
    result.denominator = f1.denominator * f2.denominator;
    return result;
}

int main() {
    Fraction f1 = {5, 4};
    Fraction f2 = {1, 2};
    
    Fraction result = addFractions(f1, f2);
    
    printf("%d/%d\n", result.numerator, result.denominator); // Output: 14/8
    
    return 0;
}
```
