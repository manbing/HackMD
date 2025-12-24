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
EXAMPLE: An example of undefined behavior is the behavior on dereferencing a null pointer[ISO C](https://www.open-std.org/jtc1/sc22/wg14/www/projects#9899)

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