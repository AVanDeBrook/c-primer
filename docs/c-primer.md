# CEC 320 & 322 -- C Programming Primer & Resources
This document is a work in progress. If you find any typos or resources that you think would be a good addition to this document, open an issue [here](https://github.com/AVanDeBrook/c-primer/issues), [submit a pull-request](https://github.com/AVanDeBrook/c-primer/pulls) with your proposed changes, or send me a message (via Email or Canvas).

## Main Function
Arguably the most important part of any C program is the main function. This is the entry point (aka reset vector) of your program. It generally takes one of the following two forms:
```c
int main(int argc, char *argv[])
{
    ...
}
```
or
```c
int main(void)
{
    ...
}
```

For this course you will see the latter form more often than not. The former form is meant for passing command line arguments to the program and this is simply not an option on microprocessors. Additionally, since microprocessors generally repeat a set of instructions constantly, there is usually no return from `main`. An infinite loop is used instead (or in the case of more sophisticated systems, this is where the task scheduler would be started). Programs will usually look something like this:
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <stdbool.h>

int main(void)
{
    // some kind of initialization

    while (1) {
        // do something
    }
}
```

## Operators
There are many operators in C ranging from basic arithmetic to bitwise logic operators. Below is a list of the most common operators that you will see during this course:
Arithmetic Operations:
* `+` -- addition operator
* `-` -- subtraction operator
* `*` -- multiplication operator
* `%` -- modulus operator (remainder after division)
Logical Operations:
* `&&` -- logical and
* `||` -- logical or
* `!` -- logical not
Bitwise Operations:
* `&` -- bitwise and
* `|` -- bitwise or
* `~` -- bitwise not
* `^` -- bitwise xor
Comparison Operations:
* `>` -- greater than
* `>=` -- greater than or equal to
* `<` -- less than
* `<=` -- less than or equal to
* `==` -- equal to
* `!=` -- not equal to
Misc. Operators:
* `*` -- dereference operator (to dereference and memory address to a value; when used in the context of a pointer)
* `&` -- reference operator (to get the memory address of a variable; when used before a variable, with no whitespace between the ampersand and the variable name e.g. `&foo`)

Make note of the following:
* The difference between `=` (variable assignment) and `==` (value equivalency)
* Some languages have both `==` and `===`, C only has `==`
* There is a specific precedence level for each operator (see chart in [resources](#operator-resources))
* The order of operations can be modified using parenthesis (just like mathematical operations)
## Operator Resources
* [Operator Precedence (from cppreference.com)](https://en.cppreference.com/w/c/language/operator_precedence)
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

Typically, in this course you will see the following data types (from `stdint.h`):
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

Each variable type has its corresponding pointer type (e.g. `int` has a corresponding `int *` type for pointers to `int`s). The importance of distinguishing between types of pointers will become evident when discussing pointer arithmetic and data structures such as arrays and strings.

Pointers can be used to **reference** memory addresses by using the variable name. For example:
```c
int array[2] = {5, 10};
// Declare a int pointer that points to the beginning of `array`
int *ptr = array;

// Print the memory addresses of each element in `array`
for (int i = 0; i < 2; i++) {
    // Print the memory address stored in `ptr`
    printf("0x%p\n", ptr);
    // Increment `ptr` to the next element in `array`
    ptr++;
}
```

The values within memory addresses stored in pointer variables can be accessed by **dereferencing** the pointer. For example:
```c
char str[] = "some string";
// Declare char pointer that points to the beginning of the string
char *ptr = str;

printf("%c\n", *ptr);

// Output:
// s
```

## Pointer Arithmetic
It is possible to perform arithmetic operations on pointers, however, the behavior is not immediately intuitive. This is best illustrated through example.

The following arithmetic operation (on normal variables) results in the expected values.
```c
uint32_t foo = 0x20000000U;
printf("foo = 0x%x\n", foo);

foo += 1;
printf("foo = 0x%x\n", foo);

// Output:
// foo = 0x20000000
// foo = 0x20000001
```

However, the same operations done using pointers yields an entirely different result.
```c
uint32_t *foo = (uint32_t *)0x20000000U;
printf("foo = 0x%p\n", foo);

foo += 1;
printf("foo = 0x%p\n", foo);

// Output:
// foo = 0x20000000
// foo = 0x20000004
```

This is because pointer treat memory as blocks according to the size of the specific pointer, so when the pointer is incremented by 1, it will move to the next integer in memory (i.e. since `uint32_t`s take up 4 bytes in memory, when the pointer is incremented it will increase by 4 bytes at a time). This is consistent across variable types (e.g. `uint8_t`s take up 1 byte in memory and increment by 1 byte at a time, `uint16_t`s take up 2 bytes and increment 2 bytes at a time, etc.).

## Pointer Examples
Declaring and initializing a pointer:
```c
int array[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};

// Declare an int pointer that points to the beginning of `array`
int *array_ptr = array;

// Employing pointer arithmetic to move the pointer to
// reference the address of a specific value in the array
while (*array_ptr++ != 9);
```

Employing pointers and pointer arithmetic to find the length of a string:
```c
char string[] = "some string";
char *string_pointer = string;
int count = 0;

while (*string_pointer++)
    count += 1;

printf("string length = %d\n", count);
printf("string length (from strlen) = %d\n", strlen(string));

// Output:
// string length = 11
// string length (from strlen) = 11
```

## Pointer Resources
* [TutorialsPoint page on pointers](https://www.tutorialspoint.com/cprogramming/c_pointers.htm)
* [Programiz Pointers Page -- Decent explanation w/ good supporting material and examples](https://www.programiz.com/c-programming/c-pointers)
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
There are 3 types of loops in C. All operate similarly, but have specific use cases. All 3 have a boolean condition that is checked through each iteration of the loop to decide whether to keep looping or stop. This operates exactly the same as the `condition` described in the [conditionals section](#conditionals).

## While Loops
While loop syntax:
```
'while` '(' condtion ')' '{'
    ...
'}'
```

While loops will continue repeating the code within the curly braces (`{` and `}`) until the specified `condition` is `false`.

## Do-While Loops
Do-While loop syntax:
```
'do' '{'
    ...
'}' 'while' '(' condition ')' ';'
```
Do-while loops operate exactly the same as while loops with the only exception being that the code within the curly braces is guaranteed to execute at least once.

## For Loops
For loop syntax:
```
'for' '(' [start] ';' [stop] ';' [step] ')' '{'
    ...
'}'
```
For loops are for executing a set of instructions a specific number of times. You will most often see them used for stepping through strings and arrays.

`start` is a variable with a specific starting value (usually `0`). This variable can be declared within the for-loop or before-hand.

`stop` is a boolean condition that must be met in order for the loop to continue iterating.

`step` is a specific action to perform to increment the variable in `start` (usually something along the lines of `i++`).

Useful notes:
* `start`, `stop`, and `step` are included in brackets (`[` and `]`) because they are not technically required in order for the loop to be valid
## Loop Examples
While loop example:
```c
int a = 0, b = 10;

while (a < b) {
    a += 1;
}

printf("a = %d, b = %d\n", a, b);

// Output:
// a = 9, b = 10
```

Do-while loop example:
```c
int a = 10, b = 10;

do {
    a += 1;
} while (a < b);

printf("a = %d, b = %d\n", a, b);

// Output:
// a = 11, b = 10
```

For loop example:
```c
int array[] = {2, 3, 5, 7, 11, 13};

for (int i = 0; i < 6; i++) {
    array[i] += i;
    printf("array[%d] = %d\n", i, array[i]);
}

// Output:
// array[0] = 2
// array[1] = 4
// array[2] = 7
// array[3] = 10
// array[4] = 15
// array[5] = 18
```

## Functions
Function syntax:
```
return-type name'(' parameters ')'
'{'
    ...
'}'
```

`return-type` is the type of value that the function will return (similar to the way variables are declared; note that this can also be `void` or `void *`, see `malloc`).

`name` is any valid name (see [variables](#variables)).

`parameters` is a comma separated list of variable declarations. There is no upper limit on the number of parameters you can include in a function.

A function can be called using its name and passing it the required parameters.
```
name '(' paramters ')' ';'
```

## Function Prototypes
There are some instances where you will need to use or create function prototypes. You will normally use these in header files or cases where the functions are declared after `main`. Basically the compiler is seeing the function call before it is seeing its declaration (which will lead to implicit definition errors). To get around this, the function is declared before it is called and then defined later. That way the compiler has a knowledge of the functions existence and parameter/return type format.

Prototypes are written exactly like function declarations (note that the function prototype and definition need to match exactly):
```
return-type name '(' parameters ')' ';'
```

See definitions above for `return-type`, `name`, and `parameters`.
## Function Examples
Function declaration:
```c
int string_length(char *str)
{
    int length = 0;

    while (*str++)
        length += 1;

    return length;
}

void print_string(char *array, int length)
{
    for (int i = 0; i < length; i++)
        printf("%d ", array[i]);
}
```

Calling previously declared functions:
```c
char string[] = "abcd";

int length = string_length(string);
printf("length = %d\n", length);

print_string(string, length);

// Output:
// length = 4
// 97 98 99 100
```

## Function Resources
* [University of Utah -- More in-depth explanation of functions](https://www.cs.utah.edu/~germain/PPS/Topics/C_Language/c_functions.html)
