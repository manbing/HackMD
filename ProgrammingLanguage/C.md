---
title: Programming Language C
tags: [Programming Language]

---

[ISO C](https://www.open-std.org/jtc1/sc22/wg14/www/projects#9899)

Qualifier: const, static

# Programming Language C

# Data type

## Character
Strings are arrays of char(ASCII) whose last element is a null character '\0' with an ASCII value of 0. C has no native string data type, so strings must always be treated as character(ASCII) arrays.

# Memory

## Dereference `*`
`[]`(Array subscripting) is syntax sugar. it can uses on some data type. e.g., `int **` (pointer to pointer to int) or `int (*)[3]` (vector of pointer to int). `[]` will do differnet operation, according to the data type of object, but the efficiency is same.

# [Operators in C](https://en.wikipedia.org/wiki/Operators_in_C_and_C%2B%2B)
During expression evaluation, the order in which sub-expressions are evaluated is determined by **recedence** and **associativity**. An operator with higher precedence is evaluated before a operator of lower precedence and the operands of an operator are evaluated based on associativity.

# Bitwise Operator
`&` AND 
`|` OR
`^` XOR
`~` NOT

# Logic Operator
`&&`
`||`
`!`

# Terms, definitions, and symbols
## behavior
* **implementation-defined behavior**
unspecified behavior where each implementation documents how the choice is made.
EXAMPLE: An example of implementation-defined behavior is the propagation of the high-order bit when a signed integer is shifted right.

* **undefined behavior**
behavior, upon use of a nonportable or erroneous program construct or of erroneous data, for which this document imposes no requirements.
EXAMPLE: An example of undefined behavior is the behavior on dereferencing a null pointer


# Include Guards (Header Guards)
> Classic include guards take three lines of preprocessor directives, work identically across all C compiler implementations, and do not inject any non-portable source code into your project. On the other hand, the use of “#pragma once” instantly makes your header file non-portable. Writing three lines of preprocessor code instead of one in a header file is a very small price to pay for source code portability.
> 
> Classic include guards are not archaic, any more than source code portability is archaic. And source code portability is not archaic at all. It’s very important in modern software development.


``` c
#ifndef __INCLUDE_GUARDS_H
#define __INCLUDE_GUARDS_H

void api_function(void);
struct api_struct {
    ....
};

#endif /* !__INCLUDE_GUARDS_H */
```

# Bit Fields
``` c
#include <stdio.h>

struct CarFeatures {
    unsigned int hasAC : 1;            // 1 bit for true/false
    unsigned int hasSunroof : 1;       // 1 bit
    unsigned int hasAutoTransmission : 1; // 1 bit
    unsigned int hasLeatherSeats : 1;  // 1 bit
};

int main() {
    struct CarFeatures car;
    car.hasAC = 1;
    car.hasSunroof = 0;
    
    // The total size of 'car' on a typical system will be 4 bytes (size of an unsigned int), 
    // but only 4 bits are used in total for the fields.
    printf("Size of struct CarFeatures: %zu bytes\n", sizeof(car));

    return 0;
}
```

# Array Decay
In ISO C, array decay (more formally, "array-to-pointer conversion") is an implicit conversion where an expression of an array type is converted into an expression of a pointer type. The resulting pointer points to the first element of the original array. 

In C, the array decays to pointers. It means that array decay is the process in which an array gets converted to a pointer. This leads to the loss of the type and dimension of the array.

Array decay generally happens when the array is passed to the function as the parameters[ISO C](https://www.open-std.org/jtc1/sc22/wg14/www/projects#9899)

Qualifier: const, static

# Programming Language C

# Data type

## Character
Strings are arrays of char(ASCII) whose last element is a null character '\0' with an ASCII value of 0. C has no native string data type, so strings must always be treated as character(ASCII) arrays.

# Memory

## Dereference `*`
`[]`(Array subscripting) is syntax sugar. it can uses on some data type. e.g., `int **` (pointer to pointer to int) or `int (*)[3]` (vector of pointer to int). `[]` will do differnet operation, according to the data type of object, but the efficiency is same.

# [Operators in C](https://en.wikipedia.org/wiki/Operators_in_C_and_C%2B%2B)
During expression evaluation, the order in which sub-expressions are evaluated is determined by **recedence** and **associativity**. An operator with higher precedence is evaluated before a operator of lower precedence and the operands of an operator are evaluated based on associativity.

# Bitwise Operator
`&` AND 
`|` OR
`^` XOR
`~` NOT

# Logic Operator
`&&`
`||`
`!`

# Terms, definitions, and symbols
## behavior
* **implementation-defined behavior**
unspecified behavior where each implementation documents how the choice is made.
EXAMPLE: An example of implementation-defined behavior is the propagation of the high-order bit when a signed integer is shifted right.

* **undefined behavior**
behavior, upon use of a nonportable or erroneous program construct or of erroneous data, for which this document imposes no requirements.
EXAMPLE: An example of undefined behavior is the behavior on dereferencing a null pointer


# Include Guards (Header Guards)
> Classic include guards take three lines of preprocessor directives, work identically across all C compiler implementations, and do not inject any non-portable source code into your project. On the other hand, the use of “#pragma once” instantly makes your header file non-portable. Writing three lines of preprocessor code instead of one in a header file is a very small price to pay for source code portability.
> 
> Classic include guards are not archaic, any more than source code portability is archaic. And source code portability is not archaic at all. It’s very important in modern software development.


``` c
#ifndef __INCLUDE_GUARDS_H
#define __INCLUDE_GUARDS_H

void api_function(void);
struct api_struct {
    ....
};

#endif /* !__INCLUDE_GUARDS_H */
```

# Bit Fields
``` c
#include <stdio.h>

struct CarFeatures {
    unsigned int hasAC : 1;            // 1 bit for true/false
    unsigned int hasSunroof : 1;       // 1 bit
    unsigned int hasAutoTransmission : 1; // 1 bit
    unsigned int hasLeatherSeats : 1;  // 1 bit
};

int main() {
    struct CarFeatures car;
    car.hasAC = 1;
    car.hasSunroof = 0;
    
    // The total size of 'car' on a typical system will be 4 bytes (size of an unsigned int), 
    // but only 4 bits are used in total for the fields.
    printf("Size of struct CarFeatures: %zu bytes\n", sizeof(car));

    return 0;
}
```

# Array Decay
In ISO C, array decay (more formally, "array-to-pointer conversion") is an implicit conversion where an expression of an array type is converted into an expression of a pointer type. The resulting pointer points to the first element of the original array. 

In C, the array decays to pointers. It means that array decay is the process in which an array gets converted to a pointer. This leads to the loss of the type and dimension of the array.

Array decay generally happens when the array is passed to the function as the parameters