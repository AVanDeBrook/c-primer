# CEC 320 & 322 - C Programming Primer & Resources

## Table of Contents
* [Variables](#variables)
  * [Examples](#variable-examples)
  * [Resources](#variable-resources)
* [Pointers](#pointers)
* [Conditionals](#conditionals)
  * [If-Statements](#if-statements)
  * [If-else Statements](#if-else-statements)
  * [Cascading If-Statements](#cascading-if-statements)
  * [Ternary Operators](#ternary-operator)
  * [Switch Statements](#switch-statements)
  * [Examples](#conditional-examples)
  * [Resources](#conditional-resources)
* [Loops](#loops)

## Variables
General syntax:
```
type name ['=' value]';'
```

`type` can be any of the following (there are more, but these are the basics):
* `int` - 32-bit signed integer
* `short` - 16-bit signed integer
* `char` - 8-bit signed integer
* `double` - 64-bit floating point number
* `float` - 32-bit floating point number

`name` can be virtually any name containing letters, numbers, underscores, hyphens, etc. There are limitations on what characters can and cannot be included.

Optionally, variables may be initialized to a specific `value` upon declaration. This is not required when declaring (creating) variables, however, you will often see variables initialized to 0 when they are declared (this is also a recommended habit just in case that variable takes up a spot in memory that previously had data in it).

Typically in this course you will see the following data types (from `stdint.h`):
* Unsigned numbers:
    * `uint64_t` - unsigned 64-bit integer
    * `uint32_t` - unsigned 32-bit integer
    * `uint16_t` - unsigned 16-bit integer
    * `uint8_t` - unsigned 8-bit integer
* Signed numbers:
    * `int64_t` - signed 64-bit integer
    * `int32_t` - signed 32-bit integer
    * `int16_t` - signed 16-bit integer
    * `int8_t` - signed 8-bit integer

## Variable Examples
Various variable declarations:
```c
// Integer variables (whole numbers e.g. 1, 2, 3, etc.)
int foo;
char bar;
uint32_t sum;
uint64_t max_value;
// Floating point variables (e.g. 1.0, 2.0001, 3.3333, etc.)
float average;
double moving_average;
```

Variable declarations w/ initialization:
```c
int foo = 0;
char bar = 'a';
// Integer variables can be set to hexadecimal values by prefixing values with '0x'
// Likewise, you can set variables to binary and octal values using '0b' and '00', respectively
uint32_t sum = 0x1234;
uint64_t max_value = 0xFFFFFFFFFFFFFFFF;
// Floating point variables can be set to either floating point values or whole values 
// (these will both be interpreted the same way as 0.0)
float average = 0.0;
double moving_average = 0;
```

Separate declaration and initialization:
```c
int foo;
char bar;

foo = 10;
bar = '\0';
```

## Variable Resources
* [TutorialsPoint Reference for variables, types, etc.](https://www.tutorialspoint.com/cprogramming/c_variables.htm)
* [GeeksforGeeks article on variables in C including an explanation of variable scope](https://www.geeksforgeeks.org/variables-and-keywords-in-c/)
* [More in-depth explanation of variable data types](https://www.tutorialspoint.com/cprogramming/c_data_types.htm)

## Pointers
Pointer syntax:
```
type '*' name ['=' value]';'
```

Pointers are technically a subsection of variables, but due to their relative complexity and tendency to confuse people, I am including them in their own section.

Simply put, a pointer is a specific type of variable that is used to store a memory address. When using the memory address stored in the pointer, it can be thought of as **referencing** that specific location in memory. Thus, when using the value in the memory location referenced by the pointer, it can be thought of a **dereferencing**.

## Conditionals
The syntax of conditionals will be broken up into several groups and described below. However, the one thing in common between them is the `condtion` that is tested to determine which code to execute.

A `condition` is a boolean condition or result from comparisons (e.g. `a > b`, `b <= c`, etc.).

Here is a list of common conditional operators:
* `>` - greater than
* `<` - less than
* `>=` - greater than or equal to
* `<=` - less than or equal to
* `==` - equal to
* `!=` - not equal to
* `!` - logical not (or negation operator)
* `&&` - logical and
* `||` - logical or

**Note** that in C there are no `true` or `false` values (unless included through `stdbool.h`), instead `0` can be considered to be `false` and anything greater than or equal to `1` can be considered `true`. However, the `true`/`false` convention will continue to be used throughout this document in order to maintain consistency and clarity.

## If Statements
Syntax of an if-statement:
```
'if' '(' condition ')' '{'
    ...
'}'
```

The code within the curly braces (`{` and `}`) will be executed if the specified `condition` is `true`, otherwise, the code within the curly braces will be skipped.

## If-else Statements
Syntax of an if-else statement:
```
'if' '(' condition ')' '{'
    ...
'}' 'else' '{' 
    ...
'}'
```

The code within the first set of curly braces (the if-clause) will be executed if the specified `condition` is `true`, otherwise, the code within the second set of curly braces will be executed (the else-clause).

## Cascading If-Statements
Syntax of a cascading if-statement:
```
'if' '(' if-condition ')' '{'
    ...
'}' 'else' 'if' '(' else-if-condition ')' '{'
    ...
'}' 'else' '{'
    ...
'}'
```

The code within the first set of curly braces (the if-clause) will be executed if the specified `if-condition` is `true`. The code within the second set of curly braces (the else-if-clause) will be executed if the specified `else-if-condition` is `true`. The code within the last set of curly braces (the else-clause) will be executed if none of the previous `condition`s are `true`.

Make note of a few things:
* The number of else-if-clauses can continue on indefinitely
* There can only be one if-clause
* There can only be one else-clause
* Only one of these `condition`s will execute e.g. if the `if-condition` is `true`, the else-if-clause and else-clause will be skipped

## Ternary Operator
Ternary operator syntax:
```
variable '=' '(' condition ')' '?' true_value ':' false_value
```

The ternary operator is a useful albeit confusing type of conditional. They do not show up very often due to their fundamentally nondescriptive nature, however, it is useful to know what they are and how they work for the instances in which they do show up.

`variable` is any valid variable to assign a value to. This can be done during declaration or as part of a normal assignment operation (see [variables section](#variables) above).

`condition` is a boolean condition, as described above.

`true_value` is a specific value to assign to `variable` given that `condition` is `true`.

`false_value` is a specific value to assign to `variable` given that `condition` is `false`.

Make note of the following:
* `true_value` and `false_value` can be either constant expressions or variables
* Ternary operators can be chained together to mimic the behavior of cascading if-statements

## Switch Statements
Switch statement syntax:
```
'switch' '(' value_var ')' '{'
    'case' value':'
        ...
        'break;'
    'case' value':'
        ...
        'break;'
    ...
    'default:'
        ...
        'break;'
'}'
```

Switch statements are also sometimes referred to as switch-case statements, but both names refer to the same concept.

Switch statements differ slightly from if-statements in that they require a specific value in order to execute a branch of code rather than a `condition`, however, they can generally be thought of as large cascading if-statements.

`value_var` is a variable to "switch" on that can generally be considered to have one of many possible values. Both `value_var` and value must be of integer type. Additionally, `value` must be a constant expression. In other words, `value` cannot be a variable; it must be either a defined constant, a number, or an enumerated value/set of values (see enumerators).

Make note of the following:
* `break` is not required in the `case` statements, however you will get 'fall-through' behavior if you do not include them (i.e. all the code under that `case` will execute regardless of whether `value` matches `value_var`)
* There may be an indefinite number of `case` statements
* There may be only one `default` case
* Switch statements are generally the only acceptable instance of the use of `break`

## Conditional Examples
If-statement example:
```c
int a = 0, b = 5;

if (a < b) {
    printf("a < b\n");
}

if (a < b && b >= 5) {
    printf("b >= 5\n");
}

// Output should be:
// a < b
// b >= 5
```

If-else statement example:
```c
int c = 10, d = 20;

if (c > d) {
    printf("c > d");
} else {
    printf("c <= d");
}

// Output should be:
// c <= d
```

Cascading if-statement example:
```c
int a = 10, b = 5;

// Keep in mind that only one of these printf's will execute 
// (the first condition that is found to be true)
if (a < b) {
    printf("a=%d", a);
} else if (a > b) {
    printf("b=%d", b);
} else {
    printf("a=%d, b=%d", a, b);
}

// Output should be:
// b=5
```

```c
int a = 5, b = 10;

if (a < b) {
    printf("a=%d", a);
} else if (a > b) {
    printf("b=%d", b);
} else {
    printf("a=%d, b=%d", a, b);
}

// Output should be:
// a=5
```

Switch statement example:
```c
int value = 0x1F;

switch (value) {
    case 10:
        printf("value = 10");
        break;
    case 0x1F:
        printf("value = 0x1F");
        break;
    default:
        printf("value = %d", value);
        break;
}

// Output should be:
// value = 0x1F
```
Note the difference in output between the above example and the one below:
```c
int value = 0x1F;

switch (value) {
    case 0x1F:
        printf("value = 0x1F\n");
    case 10:
        printf("value = 10\n");
    default:
        printf("value = %d\n", value);
}

// Output should be:
// value = 0x1F
// value = 10
// value = 31
//
```

## Conditional Resources
* [TutorialsPoint if-statement reference](https://www.tutorialspoint.com/cprogramming/if_statement_in_c.htm)
* [TutorialsPoint switch-statement reference](https://www.tutorialspoint.com/cprogramming/switch_statement_in_c.htm)

## Loops

