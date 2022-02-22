# CEC 320 & 322 -- C Programming Primer & Resources
This document is a work in progress. If you find any typos or resources that you think would be a good addition to this document, open an issue [here](https://github.com/AVanDeBrook/c-primer/issues), [submit a pull-request](https://github.com/**AVanDeBrook**/c-primer/pulls) with your proposed changes, or send me a message (via Email or Canvas).

# Hello World
```c
#include <stdio.h>

int main()
{
    printf("Hello World\n");
    return 0;
}
```
- Every programmer's first step. Don't worry about the why. Just copy, paste and try running this code. If it works, then we're all set up and ready to go.

# Basic Program Structure
- C programs usally contain these 5 parts:
  1. **Main()** - This is where we write all the code to be executed
  2. **Variables** - Labels to store data where we can access and or modify them later
  3. **Functions** - Pieces of code that execute a defined set of instructions
  4. **Operations / assignments** - How we interact with variables and data 
  5. **Preprocessor directives** - Instructions that we pass to the compiler that let us add macros, change program flow, and manage external libraries/header files

**Example Program:**

```c
#include <stdio.h> 
 
/* print Fahrenheit-Celsius table 
      for fahr = 0, 20, ..., 300 */ 
int main() 
{ 
     int fahr, celsius; 
     int lower, upper, step; 
 
     lower = 0;      /* lower limit of temperature scale */ 
     upper = 300;    /* upper limit */ 
     step = 20;      /* step size */ 
 
     fahr = lower; 
     while (fahr <= upper) { 
         celsius = 5 * (fahr-32) / 9; 
         printf("%d\t%d\n", fahr, celsius); 
         fahr = fahr + step; 
	} 
} 
```

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

**Arithmetic Operations:**
* `+` -- Addition operator
* `-` -- Subtraction operator
* `*` -- Multiplication operator
* `%` -- Modulus operator (remainder after division)

**Logical Operations:**
* `&&` -- Logical and
* `||` -- Logical or
* `!` -- Logical not

**Bitwise Operations:**
* `&` -- Bitwise and
* `|` -- Bitwise or
* `~` -- Bitwise not
* `^` -- Bitwise xor

**Comparison Operations:**
* `>` -- Greater than
* `>=` -- Greater than or equal to
* `<` -- Less than
* `<=` -- Less than or equal to
* `==` -- Equal to
* `!=` -- Not equal to

**Misc. Operators:**
* `*` -- Dereference operator 
  * To dereference and memory address to a value; when used in the context of a pointer
* `&` -- Reference operator 
  * To get the memory address of a variable; when used before a variable, with no whitespace between the ampersand and the variable name (e.g. `&foo`)

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
  <data_type> <var_name> = <value>; //Initialized with value
  
  <data_type> <var_name>; //Not initialized
  ```

`data_type` can be any of the following (there are more, but these are the basics):
* `int` -- 32-bit signed integer
* `short` -- 16-bit signed integer
* `char` -- 8-bit signed integer
* `double` -- 64-bit floating point number
* `float` -- 32-bit floating point number

`var_name` can be virtually any name containing letters, numbers, underscores, hyphens, etc. There are limitations on what characters can and cannot be included.

Optionally, variables may be initialized to a specific `value` upon declaration. This is not required when declaring (creating) variables, however, you will often see variables initialized to 0 when they are declared (this is also a recommended habit just in case that variable takes up a spot in memory that previously had data in it).

Typically, in this course you will see the following data types (from `stdint.h`):
* Unsigned numbers:
    * `uint64_t` -- Unsigned 64-bit integer
    * `uint32_t` -- Unsigned 32-bit integer
    * `uint16_t` -- Unsigned 16-bit integer
    * `uint8_t` -- Unsigned 8-bit integer
* Signed numbers:
    * `int64_t` -- Signed 64-bit integer
    * `int32_t` -- Signed 32-bit integer
    * `int16_t` -- Signed 16-bit integer
    * `int8_t` -- Signed 8-bit integer

**NOTE:** If you ever are unsure about the size of a variable, you can use the `sizeof(<data_type>);` or `sizeof(<var_name>);` function to return the amount of memory in bytes that variable is using.
## Variable Examples
**Various variable declarations:**
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

**Variable declarations w/ initialization:**
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

**Separate declaration and initialization:**
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


# Conditional and Logic Statements
## Conditionals
The syntax of conditionals will be broken up into several groups and described below. However, the one thing in common between them is the `condtion` that is tested to determine which code to execute.

A `condition` is a boolean condition or result from comparisons (e.g. `a > b`, `b <= c`, etc.).

Here is a list of common conditional operators:
| Operator  | Function   |
|---|---|
| `<`|  less than |
|  `<=`|  less than or equal to |
|   `>`|  greater than |
|   `>=`|  greater than or equal to |
|  `==` | equal to  |
|   `!=` | not equal to  |
| `!` | Logical not (or negation operator)|
| `&&` | Logical and |
| <code>&#124;&#124;</code> | Logical or|

**NOTE:** that in C there are no `true` or `false` values (unless included through `stdbool.h`), instead `0` can be considered to be `false` and anything greater than or equal to `1` can be considered `true`. However, the `true`/`false` convention will continue to be used throughout this document in order to maintain consistency and clarity.

## If Statements
Syntax of an if-statement:
```c
if ( condition ) {
    ...
}
```

The code within the curly braces (`{` ... `}`) will be executed if the specified `condition` is `true`, otherwise, the code within the curly braces will be skipped.

## If-else Statements
Syntax of an if-else statement:
```c
if ( condition ) {
    ...
} else {
    ...
}
```

The code within the first set of curly braces (the if-clause) will be executed if the specified `condition` is `true`, otherwise, the code within the second set of curly braces will be executed (the else-clause).

## Cascading If-Statements
Syntax of a cascading if-statement:
```c
if ( condition ) {
    ...
} else if ( else-if-condition ) {
    ...
} else {
    ...
}
```

The code within the first set of curly braces (the if-clause) will be executed if the specified `if-condition` is `true`. The code within the second set of curly braces (the else-if-clause) will be executed if the specified `else-if-condition` is `true`. The code within the last set of curly braces (the else-clause) will be executed if none of the previous `condition`s are `true`.

Make note of a few things:
* The number of else-if-clauses can continue on indefinitely
* There can only be one if-clause
* There can only be one else-clause
* Only one of these `condition`s will execute e.g. if the `if-condition` is `true`, the else-if-clause and else-clause will be skipped

## Ternary Operator
Ternary operator syntax:
```c
variable = ( condition ) ? : false_value
```
The ternary operator is a useful albeit confusing type of conditional. They do not show up very often due to their fundamentally nondescriptive nature, however, it is useful to know what they are and how they work for the instances in which they do show up.

`variable` is any valid variable to assign a value to. This can be done during declaration or as part of a normal assignment operation (see [variables section](#variables) above).

- `condition` is a boolean condition, as described above.

- `true_value` is a specific value to assign to `variable` given that `condition` is `true`.

- `false_value` is a specific value to assign to `variable` given that `condition` is `false`.

Make note of the following:
* `true_value` and `false_value` can be either constant expressions or variables
* Ternary operators can be chained together to mimic the behavior of cascading if-statements

## Switch Statements
- Switch statement is another decision making structure in C that can be used instead of several if else statements.
  
Switch statement syntax:
```c
switch ( value_var ) {
    case <value> :
        ...
        break;
    case <value> :
        ...
        break;
        ...
    default:
        ...
        break;
}
```

Switch statements are also sometimes referred to as switch-case statements, but both names refer to the same concept.

Switch statements differ slightly from if-statements in that they require a specific value in order to execute a branch of code rather than a `condition`, however, they can generally be thought of as large cascading if-statements.

`value_var` is a variable to "switch" on that can generally be considered to have one of many possible values. Both `value_var` and value must be of integer type. Additionally, `value` must be a constant expression. In other words, `value` cannot be a variable; it must be either a defined constant, a number, or an enumerated value/set of values (see enumerators). **If the expression returns a value not defined by any of the case statements, the default branch is will be executed** While default is not required, it is good practice to include the default branch.

Make note of the following:
* `break` is not required in the `case` statements, however you will get 'fall-through' behavior if you do not include them (i.e. all the code under that `case` will execute regardless of whether `value` matches `value_var`)
* There may be an indefinite number of `case` statements
* There may be only one `default` case
* Switch statements are generally the only acceptable instance of the use of `break`

## Conditional Examples
**If-statement example:**
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

**If-else statement example:**
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

**Cascading if-statement example:**
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
```

## Conditional Resources
* [TutorialsPoint if-statement reference](https://www.tutorialspoint.com/cprogramming/if_statement_in_c.htm)
* [TutorialsPoint switch-statement reference](https://www.tutorialspoint.com/cprogramming/switch_statement_in_c.htm)

# Loops
There are 3 types of loops in C. All operate similarly, but have specific use cases. All 3 have a boolean condition that is checked through each iteration of the loop to decide whether to keep looping or stop. This operates exactly the same as the `condition` described in the [conditionals section](#conditionals).

## While Loops
While loop syntax:
```c
while ( condtion ) {
    ...
}
```

While loops will continue repeating the code within the curly braces (`{`...`}`) until the specified `condition` is `false`.

## Do-While Loops
Do-While loop syntax:
```c
do {
    ...
} while ( condition ) ;
```
Do-while loops operate exactly the same as while loops with the only exception being that the code within the curly braces is guaranteed to execute at least once. If the condition is true, then the loop will repeat otherwise the loop will end.
## For Loops
For loop syntax:
```c
for ( [start] ; [stop] ; [step] ) {
    ...
}
```
For loops are for executing a set of instructions a specific number of times. You will most often see them used for stepping through strings and arrays.

- `start` is a variable with a specific starting value (usually `0`). This variable can be declared within the for-loop or before-hand.

- `stop` is a boolean condition that must be met in order for the loop to continue iterating.

- `step` is a specific action to perform to increment the variable in `start` (usually something along the lines of `i++`).

Useful notes:
* `start`, `stop`, and `step` are included in brackets (`[` and `]`) because they are not technically required in order for the loop to be valid
## Loop Examples
**While loop example:**
```c
int a = 0, b = 10;

while (a < b) {
    a += 1;
}

printf("a = %d, b = %d\n", a, b);

// Output:
// a = 9, b = 10
```

**Do-while loop example:**
```c
int a = 10, b = 10;

do {
    a += 1;
} while (a < b);

printf("a = %d, b = %d\n", a, b);

// Output:
// a = 11, b = 10
```

**For loop example:**
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

# Functions:

- Functions in C consist of two parts:
  <ol>
  <li>Prototype - The declaration of the function including the return type, function name, and any input arguments </li>
  <li>Definition - The algorithm or implementation for the function</li>
  </ol>

## Function Prototype
- The function protype lets the compiler know about the return data type, argument list and their data types, and the order of arguments being passed to the function

There are some instances where you will need to use or create function prototypes. You will normally use these in header files `(.h)` or cases where the functions are declared after `main`. Basically the compiler is seeing the function call before it is seeing its declaration (which will lead to implicit definition errors). To get around this, the function is declared before it is called and then defined later. That way the compiler has a knowledge of the functions existence and parameter/return type format.

Prototypes are written exactly like function declarations (note that the function prototype and definition need to match exactly):

**Function prototype syntax:**

```c
<return_type> <function_namae> (<parameters>); 

 //Example:
int64_t math_mul(int a, int b);
```

- Placing function prototypes in header (.h) files will satisfy requirement for declaring function prototypes
- **Don't forget to add the semi colon at the end :)!**

## Function Definition
- The actual algorithm/code written for a function enclosed in curly brackets

`return_type` is the type of value that the function will return (similar to the way variables are declared; note that this can also be `void` or `void *`, see [`malloc`](https://en.cppreference.com/w/c/memory/malloc)).

`function_name` is any valid name (see [variables](#variables)).

`parameters` is a comma separated list of variable declarations. There is no upper limit on the number of parameters you can include in a function.

A function can be called using its name and passing it the required parameters.
```c
function_name ( <paramters> );
```

**Function definition syntax:**
```c
<return_type> <function_namae> (input parameters)
{
    // Implementation/code of the function
}

//Example:
long long int math_mul(int a, int b){
    return (long long int) a * b;
}
```

`return-type` is the type of value that the function will return (similar to the way variables are declared; note that this can also be `void` or `void *`, see `malloc`).

`name` is any valid name (see [variables](#variables)).

`parameters` is a comma separated list of variable declarations. There is no upper limit on the number of parameters you can include in a function.

A function can be called using its name and passing it the required parameters.
```c
function_name ( <paramters> ) ;
```
See definitions above for `return_type`, `function_name`, and `parameters`. 

## Function Examples

Function declaration & implementation:
```c
#include <stdio.h>

//Function Prototypes:
int string_length(char *str);
void print_string(char *array, int length);

//Function definitions:
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

<!-- # Typedef, Structs -->
# Structs:
- Structures or structs are a data structures used to create user-defined data types in C. Think of a more primitive type of object. Structures also allow us to combine data of different types

General struct syntax:
```c
struct tag_name
{
    member_element1;
    member_element2;
    member_element3;
    member_element4;
}
```

Struct Example: 
```c
struct carModel
{
    unsigned int carNumber;
    uint32_t carPrice;
    uint16_t carMaxSpeed;
    float carWeight;
}
```
### Declaring/Initializing a struct:

#### Declaration
```c 
struct structName Var_name;

//Example:
struct carModel BMW;
```
- The data type of every variable in a struct is the **struct** keyword followed by the struct name!

#### Initialization:
- At least 3 ways of handling initialization and assigning values to each struct element/variable

1. C89 Method:
```c 
struct structName varName = {element1, element2, element3, ... elementN};

//Example:
struct carModel BMW = {2021, 15000, 220, 1330};
```
-  Similar to an array declaration
- **The order is important**, it must match the order how the members were defined in the struct

1. C99 Method: 
```c
struct structName BMW = {.member1 = value, .member2 = value, .member3 = value};

//Example:
struct carModel BMW = {.carNumber = 2021, .carWeight = 1330, .carMaxSpeed = 220};

```

- Order is not important
- Don't forget the **dot operator (.) before every element inside the curly braces!**

### Accessing structure variable attributes
```c

struct carModel
{
    unsigned int carNumber;
    uint32_t carPrice;
    uint16_t carMaxSpeed;
    float carWeight;
}
struct carModel BMW = {5, 10, 15, 20};

//Accessing variables:
BMW.carNumber;
BMW.carPrice;
BMW.carMaxSpeed;
BMW.carWeight;
```

- **NOTE: Place structures inside header files (.h) or outside the main methods. It's not good practice to place inside the main methods.**

# Typedef
- The C programming language provides a keyword called 'typedef', which can be used to give a type a new name. These can be a lot more convenient and eaiser to use than structs when done properly. 
  
**Example:** Define a term 'BYTE' for one byte numbers:
```c
typedef unsigned char BYTE
```
- After the definition, the identifier **BYTE** can be used as an abbreviation for the type unsigned char:
```c
typedef unsigned char BYTE

BYTE b1, b2;
```
- No need to use unsigned char (unless appropriate or intended)
<br>

- Typedef can also be used to give a name to structs:

```c
typedef struct
{
    unsigned int carNumber;
    uint32_t carPrice;
    uint16_t carMaxSpeed;
    float carWeight;
} Car_Model_t;

//Declaration:
CarModel_t BMW;
```

- Typedef is used to give an alias name to primitive and user defined data type
- There is not an inherent advantage to using a typedef over a struct but the typedef can make your code slightly easier to read.

# Bitwise Operations

- We use bitwise operations to perform binary operations on variables and data. Bitwise operators work on the data **bit by bit from the least significant bit (LSB) to the most significant bit (MSB)**. There are six bitwise operators in C that can be used for testing (`&`), setting (`|`), clearing (`~` and `&`), and toggling (`^`) of bits. The table and figures below shows list the operator, their function, and a usage example. 
<br>

- [Watch this for a brief summary on binary numbers](https://www.youtube.com/watch?v=sXxwr66Y79Y)

<!-- - Bitwise operations perform binary operations on variables/data at the bit level. 
- Bitwise operators can be used for: 
  - Testing of bits (`&`)
  - Setting of bits (` | `)
  - Clearing of bits (`~` and `&`)
  - Toggling of bits (`^`)
- 6 operators in C  -->

| Operator    | Symbol |
| ----------- | ------ |
| AND         | `&`      |
| OR          | `|` |
| NOT         | `~`      |
| XOR         | `^`      |
| Left shift  | `<<`     |
| Right shift | `>>`     |

Note the difference between logical operators and bitwise operators:
    - `&&` is a **logical AND** operator
    - `&` is a **bitwise AND** operator

<!-- ### Binary logic operators (AND OR NOT XOR): 
- Work just like the logic gate equivalent:
  
![ghrr4sq1](uploads/236bc3f96898ac4011c5418f68c7021b/ghrr4sq1.bmp) -->

### Bitwise AND:
```c
#include <stdint.h>
#include <stdio.h>

int main()
{
    int num1 = 10;
    int num2 = 27;
    int output = (num1 & num2);
    printf("What is the value of output: %d", output);

    /*  10 = 0000 1010   */
    /*& 27 = 0001 1011   */ 
    //Expected output = 10 (0000 1010)
    
    return 0;
}
```

### Bitwise OR:
```c
#include <stdint.h>
#include <stdio.h>

int main()
{
    int num1 = 15;
    int num2 = 67; 
    int output = (num1 | num2);
    printf("What is the value of num2: %d", output);

    /*  15 = 0000 1111   */
    /*& 67 = 1000 0011   */  
    //Expected output = 79 (1000 1111)
    //If there is a 1 in the bit position between both numbers the output will be a 1
    
    return 0;
}
```

### Bitwise XOR:
```c
#include <stdint.h>
#include <stdio.h>

int main()
{
    int num1 = 15;
    int num2 = 67; 
    int output = (num1 ^ num2);
    printf("What is the value of output: %d", output);

    /*  15 = 0000 1111   */
    /*& 67 = 1000 0011    */ 
    //Expected output = 76 (1000 1100)
    //If there is a 1 and a 0 in the same bit position between both numbers the output will be a 1 otherwise 0.

    return 0;
}
```

### Bitwise NOT:
```c
#include <stdint.h>
#include <stdio.h>

int main() {
    uint8_t num1 = 10;
    uint8_t output = (~num1);
    printf("What is the value of output: %d", output);
    
    /*  10 = 0000 1010   */
    //Expected output = 245 (1111 0101)
    //Flip all the bits (1 --> 0, 0 --> 1)

    return 0;
}
```
- **NOTE**: Typically a 1 in the MSB position means that the number is negative. An unsigned integer data type was used here to prevent the output of a negative number. If your application requires negative numbres, use a signed integer data type (`int`, `int16_t`, `int32_t`).
<br>
- [Read this](https://www.electronics-tutorials.ws/binary/signed-binary-numbers.html) for a better breakdown on negative numbers in binary

### Left Shift:
 - Operator takes 2 operands
 - Bits of the 1st operand will be shifted left by the amount decided by the 2nd operand
 - Syntax: ``` operand1 << operand2```

```c
#include <stdint.h>
#include <stdio.h>

int main()
{
    uint8_t a = 111; //0x6F (hex), 0110 1111 (binary)
    printf("Value of a left shifted by 0: %d\n", (a << 0));
    printf("Value of a left shifted by 1: %d\n", (a << 1));
    printf("Value of a left shifted by 2: %d\n", (a << 2));
    printf("Value of a left shifted by 3: %d\n", (a << 3));
    printf("Value of a left shifted by 4: %d\n", (a << 4));

    //Expected Outputs
    // a << 0: 0110 1111 (Does nothing)
    // a << 1: 1101 1110
    // a << 2: 1011 1100
    // a << 3: 0111 1000
    // a << 4: 1111 0000
}

```
  
### Right Shift: 
 - Operator takes 2 opperands
 - Bits of the 1st operand will be shifted left by the amount decided by the 2nd operand
 - Syntax: ```operand1 >> operand2```
  
```c
#include <stdint.h>
#include <stdio.h>

int main()
{
    uint8_t a = 111; //0x6F (hex), 0110 1111 (binary)
    printf("Value of a right shifted by 0: %d\n", (a >> 0));
    printf("Value of a right shifted by 1: %d\n", (a >> 1));
    printf("Value of a right shifted by 2: %d\n", (a >> 2));
    printf("Value of a right shifted by 3: %d\n", (a >> 3));
    printf("Value of a right shifted by 4: %d\n", (a >> 4));

    //Expected Outputs:
    // a >> 0: 0110 1111 (Does nothing)
    // a >> 1: 0011 0111
    // a >> 2: 0001 1011
    // a >> 3: 0000 1101
    // a >> 4: 0000 0110
}
```

## Bitwise Operators Resources:
- [Tutorialspoint Bitwise Operators in C](https://www.tutorialspoint.com/cprogramming/c_bitwise_operators.htm)
- [GeeksforGeeks Bitwise Operators in C/C++](https://www.geeksforgeeks.org/bitwise-operators-in-c-cpp/)
- [Bitwise Operators in C](https://www.guru99.com/c-bitwise-operators.html)

# Pointers
Pointer syntax:
```c
data_type * var_name [= value];
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

# Preprocessor directives
- In C programming, pre-processor directives are used to affect compiler settings, create macros, and manage external external and standard libraries.
  
- **Pre-processor directives begin with '#' symbol**


| Type    | Syntax of pre-processor directive |
| ----------- | ------ |
| Macros         | `#define <identfier> <value>` <br>Used for textual replacement|
| File inclusion          | `#include <std_lib_file_name>` or `#include "user_defined file_name"` <br>Used for file unclusion |
| Conditional compilation | `#ifdef, #endif, #if, #else, #ifndef, #undef` <br> Used to direct the compiler about code compilation      |
| Others         | `#error, #pragma`      |

## Macros:
- Macros are used for textual replacement in the code, commonly used to define constants
  
```c
#define <identifier> <value>
#define MAX_RECORD 10
```
<!-- - Write macros in all uppercase letters -->
  
- We can also make [function-like macros](https://gcc.gnu.org/onlinedocs/cpp/Function-like-Macros.html) that execute small snippets of code  

 **Best Practices while writing macros in C:**
1. Use meaningful macro names
2. Use uppercase letters for macro names to distinguish them from variables
3. **MACROS ARE NOT VARIABLES!** They do not consume any code space or ram during compile/run time

## Conditional compilation:
- These directives help to include or exclude individual code blocks based on various conditions set in the program

### #if and #endif
```c
#if <constant expression>
//Code block
#endif

#if 0 
//code block
#endif
```

- `#if` are like `#endif` are like if-statements for the compiler:
  - If a non zero value is present, the code block is included/compiled
  - If a zero value is evaluated, that block of code will not be included

### if... #else... #endif
```c
#if 1
// Code block 1
#else
// Code block 2
#endif
```

### #ifdef
```c
#ifdef NEW_FEATURE
//Code block
#endif
```
- ```#ifdef``` directive checks whether the identifier is defined in the program or not. If defined, the code block will be included for compilation

### #ifndef
```c
#ifndef NEW_FEATURE
//Code block
#endif
```
- ``` #ifndef``` directive checks whether the identifier is defined or not within the progam. If the identifier is not defined then the code block will be included for compilation

# Header Files

> A header file is a file with extension **.h** which contains C function declarations and macro 
> definitions to be shared between several source files. There are two ypes of header files: 
> the files that the programmer writes and the files that comes with your compiler.
> `-` [www.tutorialspoint.com](https://www.tutorialspoint.com/cprogramming/c_header_files.htm)

## [Standard library header files](https://en.cppreference.com/w/c/header)
| Header File Name  | Description |
|---|---|
| ``` <stdio.h> ```  | Input/output  |
| ```<stdlib.h>```  | General utilities, memory management, algorithms  |
| ```<stdint.h>```  | Fixed-width integer types, alias data types  |
| ```<time.h>```  | Time/date utilites  |
| ```<math.H>```  | Common mathematics functions  |
| ```<string.h>```  | String handling |

- Remember the hello world example? We had to include <stdio.h> in order to use printf().

## How to use / include:

`#include` Syntax: ``` #include <file_name> ```

```c
#include <stdint.h>
#include <stdio.h>
int main()
{
    uint8_t num1 = 35;
    uint32_t num2 = 0xFF;

    printf("Value of num1: %d\n Value of num2: %d\n", num1, num2);
    return 0;
}
```

<!-- ### User-defined header files

  
### Example header file:



```c
/** @file stm32f407xx_base_driver.h
 * 
 * @brief Header file for stm32f4xx boards containing peripheral register locations
 * and functions to enable peripheral clocks
 *
 * @par
 * 
 *  Created on: Feb 18, 2021
 *      Author: milesosborne
 */

#include <stdint.h>
#include <stddef.h>

#ifndef INC_STM32F407XX_H_
#define INC_STM32F407XX_H_

//Some generic macros
#define ENABLE 1
#define DISABLE 0
#define SET ENABLE
#define RESET DISABLE
#define GPIO_PIN_SET SET
#define GPIO_PIN_RESET RESET
#define FLAG_RESET RESET
#define FLAG_SET SET


/*
 * Peripheral Clock Setup
 */
void GPIO_clock_enable(GPIO_RegDef_t *pGPIOx);
void GPIO_clock_disable(GPIO_RegDef_t *pGPIOx);
void GPIO_reset(GPIO_RegDef_t *pGPIOx);

#endif
``` -->
