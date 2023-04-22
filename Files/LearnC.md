### Index

1. [Makefile](#Using-makefile-to-Build)
2. [Formatted Printing](#Formatted-Printing)

   1. [Format Specifiers](#Format-Specifiers-in-C)

   2. [Escape Codes](#Escape-Codes)

3. [Using the Debugger](#Using-the-debugger)
4. [Operators](#Operators-in-C)
5. [Keywords in C](#keywords-in-C)
6. [Syntax Structure](#Syntax-Structure)
7. [Variables and datatypes](#Variables-and-datatypes)
8. [If else statement](#if-elseif-else-statements)
9. [While loop and Booleans](#While loop and Booleans)
10. [Switch Statements](#Switch)
11. [Array and Strings](#Arrays-and-Strings)
12. [Sizes and Arrays](#Sizes-and-Arrays)
13. [for loops and array of strings](#For-loops-and-Array-of-Strings)
14. [Writing and Using Functions](#Writing and Using Functions)
15. [Pointers and Dreaded Pointers](#Pointers-and-Dreaded-Pointers)
16. [Struct and Pointers](#Struct-and-Pointers)
17. [Heap and Stack Memory Allocation](#Heap-and-Stack-Memory-Allocation)
18. [Function Pointers](#Function-Pointers)
19. [C Error Handling Problem](#C-Error-Handling-Problem)
20. [Advance Debugging Techniques](#Advance-Debugging-Techniques)
21. [Advance Data Types and Flow Control](#Advance-Data-Types-and-Flow-Control)
22. [The Stack Scope and Globals](#The-Stack-Scope-and-Globals)
23. [Meet Duff's Device](#Meet-Duff's-Device)
24. [Input Output Files](#Input-Output-Files)
25. [Variable Argument Functions](#Variable-Argument-Functions)
26. [Logfind](#Logfind)
27. [Creative and defensive Programming](#Creative-and-defensive-Programming)
28. [Intermediate Makefiles](#Intermediate-Makefiles)
29. [Libraries and Linking](#Libraries-and-Linking)
30.

- [Pointers](#Pointers-in-C)

# Learn C the Hard Way

Due to **UB (Undefined Behaviour)**, a standard set by ANSI (American National Standard Institure), of solid C code, it can disregard everything you have written.

**In C language every string ends with NUL (0 byte)**, but many strings come outside of the program which doesn't ends with NUL and compiler tries to read the end of the string and it causes program to crash.

Below is a simple C code with it's breakdown below

```c
#include<stdio.h>
/* multiline comment */
int main(int argc, char *args[]){
    int a = 12;
    printf("value is %d", a);
    return 0;
}
```

> To compile --> make filename (without .c) will make another file 'filename'
>
> To run --> ./filename will execute the compiled file

1. include is the way to import content of another file to the source file

2. main function has return type of int and is the function which will be executed when program runs. This function takes 2 arguments, _argc ->argument count_ and _char of string as arguments_

3. a return from main gives operating system an exit value as UNIX software uses return codes.

## Using makefile to Build

Make is ancient tool which knows how to make some type of softwares

##### How make works

```bash
$ make code
$ CFLAGS="-Wall -g" make code
```

make code will tell compiler to create a file named code, and to do so it will follow some steps like:-

1. Does any file with the name code exist

2. If not, it will try to find any file starting with name code.

3. If yes, it will check if make can build .c files

4. if yes then run the command `cc code.c -o code `

5. it will create a file named code using cc

The 2nd line **CFLAGS="-Wall" make code** will set environment variable CFLAGS to Wall which stands for _Warning All_ (which isn't actually all the warnings possible), and -g in CFLAGS will set the debugging symbol, which is useful while debugging the c code.

##### How actually a makefile is built and made

the standard form for writing a makefile block is the command before dependencies seprated by colon i.e.

```makefile
target_name: dependency1 dependency 2
    <commands to run>
```

If we only run make without specifying any file than make will automatically runs the very 1st target. Usually all is used as very 1st target.

```makefile
CFLAGS=-Wall -g

clean:
    rm -f code
list:
    ls -a
```

Save this file with the name Makefile and ensure that consistent tabs are used otherwise be ready for an error.

- 1st line is used to set the CFLAGS with make

- clean and list are the commands which can be used with make to execute the code corresponding to those

- running **make clean or make list** will execute **_rm -f code or ls -a_** respectively

```makefile
all: code.c
    code
```

- by including above code in makefile will make running **make** to execute **_make code_** , but we need to ensure all is the very 1st target.

- we can also write **_all: code_** this will do the same work as code is the prerequisite

- > man make    # to learn more about make file
  >
  > man cc            # to learn more about flags used in CFLAGS variable

##

## Formatted Printing

```c
int a = 15;
printf("Value is %d", a);
```

in above code printf is using a formatted string and than we are putting variables that should be replaced in format string. Here printf is given some variables and it's constructing a new string then printing it to the terminal.

- if we didn't intialized 'a' it will have garbage value (for static 'a' value will be 0)

- if we didn't add 'a' in printf it will give garbage value too.

#### Format Specifiers in C

- %c for a character (char ch = 'B';)

- %i or %d is for ints (%d assumes base 10 but %i detects the base, but this happens with scanf, for a printf statement both detects the base of the number)

- %ld or %li (used for a long integer)

- %u is used for unsigned int (for a negative value there will be unexpected results)

- %f, %e, %E are floating-point format specifiers (%e will give result in exponent form)

- %o is used for octal numbers

- %x, %X is used for hexa-decimal numbers

- %s is used to specify strings (strings are defined as : _char[] value = "i am string"_)

- %p is used to print any address

```c
int main(){
    char string[] = "Responsibility";
    printf("%9s\n", string); // string of len 9 with left focused spaces
    printf("%-9s\n", string);// string of len 9 with right focues spaces
    printf("%9.3\n", string); // left string of len 9, sliced to 3 index
}
```

above code will output:

> $     Responsibility
>
> $ Responsibility
>
> $        Resp

#### Escape Codes

- /t for horizontal tab

- /v for vertocal tab

- /r for carriage return

- /n for newline

- /b for backspace

## Using the debugger

There are many tools which can be used as debugger like **_gdb, Valgrind, AddressSanitizer, Lint etc._**

##### GDB (GNU Debugger)

To start gdb for any file run gdb appended with file name in terminal something like below

```bash
$ CFLAGS="-Wall -g" make code
$ gdb ./code
# will start the gdb interactive shell where we can run commands
(gdb) run
```

the above code will start the debugger for code file in interactive mode and we can do different things using the commands which gdb provides. like

- run    --> To run the code (will stop at any error)

- bt / backtrace --> To get the stack trace (line num where debugger is)

- b / break function `break main` will set a break point at main function.

  - running code with run will stop at main function.

- s / step --> next line with stepping into function calls

- n /next --> next line with stepping over function calls

- c / continue --> continue running the program from debugger location

- p / print _expr_ --> will print the value of expression _expr_

- clear --> clear a breakpoint (need debugger on that line)

- pwd, cd, make --> works just like in normal shell

- help

- shell --> quickly start a shell session.

There are few things which are useful while running the gdb debugger

1. `gdb --batch --ex r --ex bt --ex q ./code` . This will run the debugger on code file while executing run, back trace than quit. If --batch option isn't used, gdb will 1st list out its information, after that will do the same thing. But to get cleaner output use batch option. **in simple words this command is useful for checking if the code is running fine without doing debugging step by step**

2. the above command is similar like how Valgrind works and this is really important when monitoring segment faults.

##### Valgrind:

Valgrind is similar as gdb but it is a richer debugger than gdb. It has alot more options and very useful when there is need to monitor memory releases.

##### Lint:

There is program spling, which works directly on c code and won't check by running the code. It will list you all the potential errors.

##### Address Sanitizer

This is alternative of Valgrind on OS X as it uses clang and clang is a part of OS X.

##

## Operators in C

**Arithmetic Operators**

> +, -, \*, /, % (Modulo), ++ (Increment), -- (Decrement)

**Relation Operators**

> ==, !=, > (greater than), < (less than), >= (greater than equal), <= (less than equal)

**Logical Operators**

> && (Logical AND), || (Logical OR), ! (NOT), ? : (Logical Ternary)

**Bitwise Operators**

- & (Bitwise AND)

- | (Bitwise OR)

- ^ (Bitwise XOR)

- ~ (Bitwise one's complement)

- << (Bitwise shift left)

- \>> (Bitwise right Shift)

**Assignment Operator**

Assignment operators are used to assign a value to a variable, but these can be combined with logical, arithmetic or logical operators like below

- &= (and equal) which is ultimately a bitwise operator

- += (plus equal)

- \>>= (right-shift equal) a bitwise operator again

- ^= (xor equal) acts as logical operator

**Some other but really important operators are below**

- \*p --> value of p

- &k --> address of k

- sizeof() --> get the size of

- \-> (Structure dereference)

- . (Structure reference)

## keywords in C

There are many keywords in c some of them are below

- auto (to give a variable local lifetime)

- enum (define a set of int constants)

- extern

- goto (jump to a label)

- register (declare a variable be stored in a CPU register)

- static (preserve variable value after its scope exits)

- struct (combine variables in single record)

- typedef (create a new type)

- union (start a union statement)

- volatile (declaring a variable might be modified elsewhere)

## Syntax Structure

**If statement**

```c
if (condition){

 code;

}

else if (condition) {

 code;

}

else {

 code;

}
```

**switch statement**

```c
switch (operand){
    case value:
        code;
        break;
    default:
        code;
}
```

Even if 1st case came out to be true switch statement will always excute other cases and to avoid that it is a good practise to use break within all the case statements.

**while loop**

```c
while (TEST) {
    code;
}
```

A loop can use _break and continue_ statement

**do while loop**

```c
do {
    code;
} while (TEST)
```

**for loop**

```c
for (int value = 0; condition; postLoop){
    code;
}
```

**enum**

```c
enum {CONST1, CONST2, CONST3} NAME;
```

**goto**

```c
if (k == 23){
    goto test
}
test:
    code;
```

**function**

```c
int sum(int a, int b){
    code;
    return 0;
}
```

**typedefs**

```c
// typedef DEFINATION IDENTIFIER;
typedef unsigned char byte;
```

**struct**

```c
struct NAME {
    ELEMENTS;
} [VARIABLE_NAME]

// Struct with typedef
typedef struct [STRUCT_NAME] {
    ELEMENTS;
} IDENTIFIERS;
```

**union**

It is similar like struct but elements will overlap in memory

```c
union NAME {
    ELEMENTS;
} [VARIABLE_NAME];
```

##

## Variables and datatypes

- _short_ test = 12;

- _int distance = 23;_

- _long_ value = 123; or _long_ value = 123L;

- _float_ percent = 65.33F;

- _double_ percent = 83.5234;

- _bool_ value = true; (value can be **true/false**)

  - 1 and 0 represent true or false in C resp. Unlike python 23 || 15 will return 1 not 23. Similarly 62 && 73 will return 1 not 73 as in python.

- _char_ A = 'K';

- _char_ name[] = "String in C";

Few things to be noted is that a character is just a number in C like in some other languages. We can use chars in mathematical calculations as they are treated as just some integers here. Number associated with a char is always same and is the ASCII code for that number

```c
char test = 'P';    // ASCII value for P is 80
int sum = 3 + test;
printf("sum for char and int is %d percentage: %%", sum);
```

_print statement above will print 83 as ASCII value for P is 80_. Another thing to be noted here is that to print a percentage symbol it is required to use 2 percentage symbols as above.

**Warning**

```c
void main(){
    char warn = 'P';
    printf("This will give error %s", warn);
}
```

The above code will give error due to segmentation fault. If a program is trying to access or write a memory which it is not supposed to modify or access, the compiler will give segmentation fault. There are many ways segmentation fault can be created i.e. below code

```c
int main(){
    int str*;
    str = "String";

    *(str + 1) = "k";
}
```

This happened because str is written in read-only memory but \*(str + 1) = "k" is trying to modify the content in read only memory that is causing a segmentation fault

## if elseif else statements

Like in some other languages booleans are only a numeric value in C. 0 means false and any other value is true. We can test it with below code.

```c
#include<stdio.h>
#include<stbool.h>

int main(int argc, char* args[]){
    int value = 12;
    bool make = true;
    bool made = false;

    int check = value * make;
    printf("value is %d\n", check);

    check = value * made;
    printf("value is %d\n", check);
}
```

> output:
>
> value is 12
>
> value is 0

So as we saw for calculations **true will be treated as 1 and false will be treated as 0**, To pass the arguments while running the code pass them along as in bash

> $ ./code arg1 arg2 arg3

If else is very similar in C as per in some other languages. A simple C code for if-else will look like this

```c
int main(int argc, char* args[]){

    int k = 12;

    if (k < 10) {
        printf("The value is above 9");
    }
    else if (k >= 10 && k <= 15){
        printf("The value is bet. 10 and 14");
    }
    else {
        printf("Value is greater than 15");
    }
}
```

## While loop and Booleans

while loop code looks like this

```c
int main(int argc, char* args[]){
    int a = 5;
    while (a < 8){
        printf("value of a : %d", a);
        a++;
    }
}
```

This is similar to for loop but it is required to manually set the increment and initializing the variable

## Switch

In C switch statement is actually a jump table. **_in C switch statement will only take expressions which can be converted to int like int itself or char_**

```c
int main(int argc, char* argv[]){
    char letter = 'k';
    switch(letter){
        case 'i':
            printf("this is code");
            break;
        case 'a':
            printf("this is a");
            break;
        default:
            printf("nothing happened");
    }
}
```

**This is how switch statement works in C:**

- Compiler will mark the point where switch statement starts lets say Y

- than it will come up with a number for _switch(letter)_, i.e. it will convert letter 'i' into its corresponding ASCII number.

- Now compiler will translate each case block to a location in program that is exactly that far away from Y i.e. `case 'a'` will be saved in a location that is Y + a in the program

- it then does the math to figure out where `Y + letter` is located in switch statement, and if it is too far than it adjust it to `Y + default` value

- now as it knows the location it will jump to that spot and then contiue running, if there is no break it will jump to the next case.

There are few things which should be kept in mind while comparing data types.

```c
int main(int argc, char* argv[]){
    for (int i = 0; i < argv[1]; i++){
 // for (int i = 0; i < argv; i++)
       printf("%c\n", argv[1][i]);
    }
}
```

The above code will throw an error because we are trying to compare an integer with a pointer. Even if we try to compare it with a string it will also give **segmentation fault**.

## Arrays-and-Strings

C stores strings as an char array which ends with _\0 (null byte)_

> int array_name[size] = {elements in csv};

```c
int main(int argc, char* argv[]){
    int array[4] = {5};
    char urray[4] = {'k'};

    printf("%d, %d, %d, %d", array[0], array[1], array[2], array[3]);
    printf("%d, %d, %d, %d", urray[0], urray[1], urray[2], urray[3]);

    urray[0] = 'v';
    urray[1] = 'a';
    urray[2] = 'l';
    urray[3] = 'v';
    printf("%s\n", urray);

    char *color = "red";
    printf("color %s\n", color);
    printf("%c, %c, %c, %c", color[0], color[1], color[2],color[3]);
}
```

By running above code we can make some conclusions

- arrays are automatically initialized to 0 if one element is set manually

- When each character in urray (char array) is printed only 1st char will be printed in this case 'k' but all other chars are also initialized to '/0' (nul byte), but as /0 is a special character it is not visible.

- We can still modify the elements of array after initializing it.

- We can print char array by using string identifier, it will keep printing until it encounter a nul byte (\0)

- Another way to create a string is by using pointers

> char \*word = "something";

By creating string like that in c, memory address of very 1st element of the string will be saved on variable name ('word' in this case). By using %s identifier we can print whole string or %c to print characters in string using array indexing (word[2] = 'm').

This can be checked with below code and this is how one should define strings in C.

```c
int main(int argc, char *string[]){
    char *string = "odd";
    printf("location of string %p", string);
    printf("elems are %c%c%c%c", string[0], string[1], string[2]);
    printf("string is %s", string);
}
```

---

**Code to convert char array to single int**

```c
// with sscanf
char myarray[5] = {'-', '1', '2', '3', '\0'};
int i;
sscanf(myarray, "%d", &i);

// with atoi (array to integer)
char myarray[4] = {'-','1','2','3'};
int i = atoi(myarray);
printf("%d\n", i);
```

## Sizes and Arrays

```c
int array[] = {1,2,3,4,5};
```

When C sees the syntax `type name[] = {initializer};`. What C does is

- look at the type i.e. int in this case.

- Look at [], if there is no length check the number of elements in intializer. 5 in above code.

- Create 5 slots in memory to save all 5 integers.

- take name array and assign it this location.

```c
int array[] = {1,2,3,4,5};

printf("size of array in byte %ld", sizeof(array));
printf("num of ele in array %ld", sizeof(array)/sizeof(int));

char string[] = {'v', 'a', 'l', 'u', 'e'};
char stress[] = {'o', 'u', 't', '\0'};

printf("size of string %ld", sizeof(string));
printf("size of char %ld", sizeof(char));
```

sizeof is a function which will return the size of anything in bytes, this is important as C has much relation with memory and sizes.

One more thing which can be noticed is, size of string will be returned as 6 but for stress it will be 4. In case of stress actual size is 4 and returned size is also 4, but for string it is different. The reason being `\0 (nul)` as there is no nul value in string, So C is creating one by itself so size is one more than expected.

## For loops and Array of Strings

Any String is similar to a byte array. By keeping this in mind we can create various types of arrays.

While writing our main function we are using argument `char *argv[]` which is acutally an array of strings.

```c
int main(int argc, char *argv[]){
    char *strArray[] = {"something", "testing"};
    printf("%s, %s\n", strArray[0], strArray[1]);
}
```

This is how one can define string array in C.

```c
#include<stdio.h>

int main(int argc, char *argv[]){
    if (argc != 2) {
        printf("you need to pass only 1 args");
        return 21;
    }

    for (int i = 0; argv[1][i] != '\0'; i++){
        printf("entered char is %c\n", argc[1][i]);
    }
}
```

This is done in series of tasks

- OS passes every command line argument as string to the string array

- OS also sets argc as the number of elements in argv, remember the very qst argument saved in argv is the program name itself i.e. ./test

#### Understanding Array of Strings

In c array is made with `char array[]` and a string can be created with `char *string` but when these are combined it will create a multidimensional array. Each String is an element for array itself and this array is defined with `char *array[] = {"some", "talk ", "task"}`.

**Creating Multidimensional arrays in C**

```c
int array[][3] = {{1,2,3},{9,7,2}};
```

remember that it is required to specify the size of internal arrays in C, otherwise there will be a compilation error.

###### Multidimensional Arrays and Pointers in C

`int array[2][3] = {{ 1, 2, 3}, {4, 5, 6}}.` This is how a multidimensional array is created. Pointer for a multidimensional array is defined like `int *ptr[3]`, this shows that ptr is a pointer for an array of 3 elements. Multidimensional arrays can be used with pointers as in below code.

```c
int array[2][3] = {{ 1, 2, 3}, {4, 5, 6}};
int (*ptr)[3] = array
....
```

1st see how multidimensional array is layed out in memory. Above array is actually an array consisting of 2 arrays i.e. **{array0, array1}**

| Mem Add -> | 100    | 112    |
| ---------- | ------ | ------ |
| **Value**  | array0 | array1 |

| Mem Add -> | 100 | 104 | 108 | 112 | 116 | 120 |
| ---------- | --- | --- | --- | --- | --- | --- |
| **value**  | 1   | 2   | 3   | 4   | 5   | 6   |

As per memory layout above array0 will be stored from 100 to 108 as it has 3 integer values in it with each taking 4 bytes. When array0 will end array1 will start filling memory exactly after that. **Now using pointers with this multidimensional array**. Simply using array will give memory address of array's 1st element which is array0. And incrementing the value of array will give address of next element i.e. array1 --> 112

```c
...
printf("%d\n", array); // memory addr of array0 --> 100
printf("%d\n", array + 1); // mem add of array1 --> 112
```

using a derefrence for array will give array[0]\(1st element of array itself) which is array0. array0 is {1, 2, 3}. So in array[0] memory address of starting element of array0 will be stored which is 100. And pointer arithmetic will work on array0's elements. i.e. \*array + 1 will give address of array0[1].

```c
printf("%d\n", *array); // memory addr of array0[0] --> 100
printf("%d\n", *array + 1); // mem add of array0[1] --> 104
```

To know the value at any address:

```c
printf("%i\n", *(*array)); // value at addr. 100 = 1
printf("%i\n", *(*(array + 1))); // value at add 112 = 4
printf("%i\n", *(*array + 1)) // value at add 104 = 2
printf("%i\n", *(*(array + 1) + 1); // value at add 116 = 5
```

1. **\*(\*array)** : array is starting add. of array i.e. 100. derefrencing it gives array0 which will give starting add. of array0 i.e. 100 again. Derefrencing it will give value at add. 100, which is 1.

2. **\*(\*(array+1))** : (array + 1) will give memory add. of array incremented by 1. So 1 element in array is taking 12 bytes so (array + 1) will be 112 (as 100 is the add of array). Derefrencing it will give value of array1, which is the mem addr of starting of array1. Derefrencing it again will give value at 112 i.e. 4

So for 2-d arrays we can say **Array[i][j] = \*(Array[i] + j)**.

#### Try:

Try printing argv[2], argv[3]....argv[n], but pass no argument to the program. It will print random strings associated with OS.

```c
char array[] = {'a', 'a', 'a', 'a'};
// part 1
printf("address of array: %p\n", &array);
printf("array: %p\n", array);
// part 2
printf("array[0]: %c\n", array);
printf("array[0]: %c\n", *array);
// part 3
printf("string %s\n", array);
printf("string %s\n", *array);
// part 4
for (int i = 0; i < 4; i++){
    printf("address of array[%i]: %p", i, array[i]);
}
```

- In 1st part of the code both print statements will return address of array's 0th index. This shows that variable array will store the address of 0th index of array. So printing address of array and value of array will be same. `&array[0] == &array == array` will be true and `*array == array[0]` will also be true

- When we try to print the value of character we need to use derefrence operator for array. 1st statement will give unexpected as array will give any address value. But in 2nd, when we use \*array it will print the expected result. So array is an address So using \*array will print value of array.

- For part 3, this is different when identifier is a string. There is no need of using derefrence operator with string as identifier. Using derefrence with a string identifier will give an segmentation fault. This is because, a string identifier expects a char[] but it is getting a char or an int here.

- This will return addresses of the array elements and all addresses printed will be in sequence as char will take only 1 byte.

## Writing and Using Functions

Defining a function is similar as we defined main function in C

```c
#include<stdio.h>
#include<ctype.h>    // to use isalpha, isblank functions

int sum(int array);    // forward declaration of function sum

int main(int argc, char *argv[]){
    int array[] = {2,5,2,7,9};
    sum(array);
}

int sum(int array[]){
    int sum = 0;
    for (int i = 0; i < (sizeof(array)/4); i++){
        sum += array[i];
    }
    return sum;
}
```

The sum array is defined as another function here.

We have declared sum function before using it in above program, this is called **forward declaration**. Not using this will give implicit defination warning in the program but not any major error and this is helpful for compilor too.

**TRY**

Try this code below

```c
#include<stdio.h>

int main(){
    int sum = fun(2,3,5);
    printf("%i\n", sum);
}

int fun(int a){
    return a;
}
```

In above code a function fun is called in main before it is defined. So compilor will assume the declaration as `int fun()` which can take indefinate number of arguments. That's the reason above code will not give any kind of error even fun is called with 3 arguments instead of 1. As it assumes the declaration to return int, it return type in defination is other than int, there will be an error. So this is better to declare a function before it is used if in case you are using it before its defination. If a function is taking 0 arguments than it should be specified in declaration as `int fun(void)` otherwise for a defination like `int fun()` compilor will assume it will take indefinate number of arguments, but again a declaration is necessory only if a function is used before it is defined in program.

## Pointers and Dreaded Pointers

A pointer to string array can be defined as `char *(*string)`.

**The pointer lexemes**

- type \*ptr    (declaring a pointer of type(int, char) )

- \*ptr (value at address given by ptr)

- \*(ptr + i) value of next value from address given by ptr.

- &value --> address of value.

- type \*ptr = &thing --> saving address of thing in ptr.

- ptr++ --> incrementing a pointer

## Struct and Pointers

Struct is a data structure which is simlilar like classes in other languages. struct is kind of a structure where each element have some variables associated with it, like class variables (instance variables).

```c
#include<stdio.h>
#include<assert.h>

struct Person{
    char *name;
    int age;
    int height;
    int weight;
}

struct Person *create(char *name, int age, int height, int weight){
    struct Person *who = malloc(sizeof(struct Person));

    assert (who != NULL);
    who->name = strdup(name);
    who->age = age;
    who->height = height;
    who->weight = weight;

    return who;
}

int destroy(struct Person *ptr){
    assert (ptr = NULL);

    free(who->name);
    free(who);
}

void personPrint(struct Person *who){
    printf("name : %s\n", who->name);
    printf("age : %i\n", who->age);
    printf("weight : %i\n", who->weight);
    printf("height : %i\n", who->height);
}
```

A struct named Person is defined above which have properties name, age, height and weight. This is how a struct is defined. To set a property `x->y` syntax is used. Now there is a function `create` which returns `struct Person*` (a pointer to struct person data type created using struct). This is also the function which we used to set variables inside the struct person data type.

Using struct we are creating a new data type which can be used like other data types. To access a value in struct `struct->name` syntax is used and same syntax is used to initialize it.

```c
//setting a value in struct
rohit->name = "Rohit";
//accessing the same value
printf("%s\n", rohit->name);
```

There is another standard approach to use structs

```c
struct Player{
    char *name[3];
    int jersey[3];
}

void display(struct Player player){
    for (int i = 0; i < 3; i++){
        printf("Name: %s ", player.name[i]);
        printf("jersey; %i\n", player.jersey[i]);
    }
}
int main(){
    struct Player group = {{"Rohit", "Virat", "Pant"}, {45, 18, 17}};
    display(group);
}
```

**initializing can be done in other way also**

```c
struct Player group = {.name = {"Rohit", "Virat", "Pant"}
                       .jersey = {45, 18, 17}
                      };
```

This will do the same work as above 2 type declarations.

## Heap and Stack Memory Allocation

```c
#include <stdio.h>
#include <assert.h>
#include <stdlib.h>
#include <errno.h>
#include <string.h>

#define MAX_DATA 512
#define MAX_ROWS 100

struct Address{
    int id;
    int set;
    char name[MAX_DATA];
    char email[MAX_DATA];
};

struct Database{
    struct Address rows[MAX_ROWS];
};

struct Connection {
    FILE *file;
    struct Database *db;
};

void die(const char *message){
    if (errno) {
        perror(message);
    }
    else{
        printf("ERROR: %s\n", message);
    }
    exit(1);
}

void Address_print(struct Address *addr) {
    printf("%d %s %s\n", adddr->id, addr->name, addr->email);
}

void Database_load(struct Connection *conn){
    int rc = fread(conn->db, sizeof(struct Database), 1, conn->file);

    if (rc != 1)
        die("Failed to load databse");
}

struct Connection *Database_open(const char *filename, char mode){
    struct Connection *conn = malloc(sizeof(struct Connection));
    if (!conn)
        die("Memory error");

    conn->db = malloc(sizeof(struct Database));

    if (!conn->db)
        die("Memory Error");

    if (mode == 'c'){
        conn->file = fopen(filename, "w");
    }
    else{
        conn->file = fopen(filename, "r+");

        if (conn->file) {
            Database_load(conn);
        }
    }

    if (!conn->file)
        die("Failed to open the file");

    return conn;
}

void Database_close(struct Conneciton *conn) {
    if (conn) {
        if (conn->file)
            fclose(conn->file);
        if (conn->db)
            free(conn->db);
        free(conn);
    }
}

Database_write(struct Connection *conn){
    rewind(conn->file);

    int rc = fwrite(conn->db, sizeof(struct Database), 1, conn->file);

    if (rc != 1)
        die("Failed to write database");

    rc = fflush(conn->file);
    if (rc == -1)
        die("Cannot flush database");
}

void Database_create(struct Connection *conn){
    int i = 0;

    for(i = 0; i < MAX_ROWS; i++){
        struct Address addr = {.id = i, .set = 0};
        conn->db->rows[i] = addr;
    }
}

void Database_set(struct Connection *conn, int id, const char *name, const char *email) {
    struct Address *addr = &conn->db->rows[id];
    if (addr->set)
        die("Already set, delete it first");

    addr->set = 1;
    // Warning: bug
    char *res = strncpy(addr->name, name, MAX_DATA);
    if (!res)
        die("Name copy failed");

    res = strncpy(addr->email, email, MAX_DATA);
    if (!res)
        die("Email copy failed");
}

void Database_get(struct Connection *conn, int id) {
   struct Address *addr = &conn->db->rows[id];

   if (addr->set){
        Address_print(addr);
   }
   else{
        die("ID is not set");
   }
}

void Database_delete(struct Connection *conn, int id){
    struct Address addr = {.id = id, .set = 0};
    conn->db->rows[id] = addr;
}

void Database_list(struct Connection *conn){
    int i = 0;
    struct Datbase *db = conn->db;

    for (i = 0; i < MAX_ROWS; i++){
        struct Address *cur = &db->rows[i];

        if (cur->set) {
            Address_print(cur);
        }
    }
}

int main(int argc, char *argv[]){
    if (argc < 3)
        die("USAGE: ex17 <dbfile> <action> [action params]");

    char *filename = argv[1];
    char action = argv[2][0];
    struct Connction *conn = Database_ope(filename, action);
    int id = 0;

    if (argc > 3) id = atoi(argv[3]);
    if (id >= MAX_ROWS) die("There's not that many records.");

    switch (action) {
        case 'c':
            Database_create(conn);
            Database_write(conn);
            break;

        case 'g':
            if (argc != 4)
                die("Need an id to get");
            Database_get(conn, id);
            break;

        case 's':
            if (argc != 6)
                die("Need id, name, email to set");
            Database_set(conn, id, argv[4], argv[5]);
            Database_write(conn);
            break;

        case 'd':
            if (argc != 4)
                die("Need id to delete");

            Database_delete(conn, id);
            Database_write(conn);
            break;

        case '1':
            Database_list(conn);
            break;

        default:
            die("Invalid action: c=create, g=get, s=set, d=delete, l=list");
    }

    Database_close(conn);

    return 0;
}
```

`#define MAX_DATA 500` will be processed by preprocessor to create constant settings of MAX_DATA for the entire program.

Address struct is being created which is a constanct size struct, making it less efficient but easy to read and store. Database struct is also a fixed size struct as it is using only 2 pointers as input.

die function which will handle any error and kills the program in case if something is not right. There are few values like errno which is a number returned by a function if it somehow fails. perror is used to print the error message by itself. it only takes a string which will be printed just before the error message

FILE is a struct which is used to declare various operations on files such as fread, fopen, fwrite, rewind

If there is something on the stack to which a function is returning a pointer. After function gets killed it will give **segmentation fault** as the function will be pointing to a dead space.

```c
int* segment(){
    int a;
    int* ptr = &a;
    *a = 12;
    return ptr;
}
int main(){
    int *ptr = segment();    // this will give segmentation fault.
}
```

## Function Pointers

`int (*addition)(int, int)`. This is the way to declare a function pointer. Function name itself is an address so there is no need to use & to get the address of the function simply name will work i.e. `int (*addition)(int, int) = sum;`. Here sum is a function but word **sum** will be treated as an address.

To call a function using its pointer we can simply use `addition(2,3) or (*addition)(2,3)` .

```c
int sum(int a, int b){
    return a + b;
}
int main(){
    int (*addition)(int, int) = sum;
    printf("addition()-> %d, sum()-> %d\n", addition(2,3), sum(2,3));
    // result:- addition() -> 5, sum -> 5
}
```

lets suppose sum has a value 0xff123 than addition will also have a value of 0xff123, that's the reason calling addition(2,3) without dereferencing works.

One more thing to note here is that if addition is dereferenced than it will also return the same value i.e. 0xff123. So ***addition(2,3) == \(*addition)(2,3)\***.

as passing a function pointer to another function will look bad, **typedef** can be used to make it neat. We can use typedef to declare function pointer as a type so that while passing arguments one don't have to go through messy arguments.

```c
typedef int (*fun)(int, int);
// this will define a new type fun which is a pointer to a function returning
// int value and taking 2 ints as input
int sum(int a, int b){
    return a + b;
}

int main(int argc, char *argv[]){
    int array[3] = {1,2,3};
    fun value = sum;
    printf("%p\n", sum);    // suppose 0xfff123
    printf("%p\n", value);  // also print 0xff123
}
```

**typedef**

This can be used to define a new type in the program i.e. define a new datatype string which will be simlilar to char\*.

```c
typedef unsigned int unit;
typedef char *string;
typedef struct Person {
    string Name;
    int age;
}Person;

int main(){
    string name = "Dennis Ritchie";
    Person Sean = {.Name = "Sean", .age = 23};
}
```

in above program instead of writing `unsigned int value = 23;` we can use `unit value = 23;` as unit is defined as a unsigned int datatype. Similarly we can use `string` instead of char\* and `Person` instead of `struct Person`.

**DIFFERENCE BETWEEN DEFINE AND TYPEDEF**

#define just gives an alias to something like `#define VAL char*`. Now if preprocessor encounters VAL anywhere in the program it will convert it into char\* but typedef acutally defines a type in program.

```c
#define STR char*
typedef char *string;
int main(){
    STR a,b,c;
    string e,f,g;
    printf("%d, %d, %d\n", sizeof(a), sizeof(b), sizeof(c));
    printf("%d, %d, %d\n", sizeof(e), sizeof(f), sizeof(g));
}
```

Now this will give unexpected results as preprocessor will replace all occurences of STR to char\*. So `STR a,b,c;` will be converted to `char* a,b,c;`. So now a will be declared as a string but b and c will be declared as char values.

Now when we go to the typedef case there sizeof all the values will be as sizeof a pointer this is because string has been declared as a datatype of char\*.

## C Error Handling Problem

When C encounters an error it sets variable errno to some value which is defined as `extern int` in errno.h header file. After writing more and more C code one will realise eventually that c code should be written as follows :-

> 1. Call a function
>
> 2. Check if the return value is an error
>
> 3. Clean up all resources as much as you can
>
> 4. Lastly print out an error message to get more info about error.

If Error in a program isn't handled correctly it will die eventually. Author uses some macros to handle exceptions, **The Debug Macros**.

> HOW THESE MACROS WORK: WORDS BY AUTHOR
>
> It does this by adopting the convention that whenever there’s an error, your
> function will jump to an error: part of the function that knows how to clean
> up everything and return an error code. You can use a macro called check to
> check return codes, print an error message, and then jump to the cleanup section.
> You can combine that with a set of logging functions for printing out useful
> debug messages.
> I’ll now show you the entire contents of the most awesome set of brilliance
> you’ve ever seen.

```c
// debug macros of author
#ifndef __dbg_h__
#define __dbg_h__

#include <stdio.h>
#include <errno.h>
#include <string.h>

#ifdef NDEBUG
#define debug(M, ... )
#else
#define debug(M, ... ) fprintf(stderr, "DEBUG %s:%d: " M "\n",__FILE__,\
                                __LINE__, ##__VA_ARGS__)
#endif

#define clean_errno() (errno == 0 ? "None":strerror(errno))
#define log_err(M, ... ) fprintf(stderr, \
                            "[ERROR] (%s:%d: errno: %s) " M "\n", __FILE__, __LINE__,\
                            clean_errno(), ##__VA_ARGS__)

#define log_warn(M, ... ) fprintf(stderr, "[WARN] (%s:%d: errno: %s)" M "\n",\
                            __FILE__, __LINE__, ##__VA_ARGS__)
#define log_info(M, ... ) fprintf(stderr, "[INFO] (%s:%d)" M "\n",\
                            __FILE__, __LINE__, ##__VA_ARGS__)
#define check(A, M, ... ) if (!(A)) {\
                    log_err(M, ##__VA_ARGS__); errno=0; goto error;}
#define sentinel(M, ... ) {log_err(M, ##__VA_ARGS__);\
                        errno=0; goto error;}
#define check_mem(A) check((A), "Out of Memory!")
#define check_debug(A, M, ... ) if (!(A)){debug(M, ##__VA_ARGS__);\
                        errno=o;goto error; }
#endif
```

Below macros are defined with **include guards**.

```c
#ifndef NAME
#define NAME
#else
#define OTHERNAME
#endif
```

This simply means if NAME is not defined define it or in case of any problem define OTHERNAME and endif is used to end the if statement. This above code is a defence so that header files will not get defined twice. In case a file is defined twice, when it will be preprocessed it will give declaration for same function or variables twice. As in the end all these files are converted into one big file. It will give error due to double declaration of some functions. So to prevent this **include guards are used.**

- #ifdef NDEBUG

  > If your program is compiled with NDEBUG than there will be no debug messages. That's why #define debug(...) is not replaced with anything. else debug(...) will be replaced with fprintf(....)..

- #define debug(M, ...) is very important to notice here. M is injector message.

> This is very less known that one can give multiple arguments to a function with define. This all is possible because of \_\_VA_ARGS\_\_ but this is not supported by all compilers, but this is very useful. Something like this can be used with `stdarg.h`, which has following arguments va_list, va_start, va_end. Function defined for variable arguments will have a structure like this `int add(int num, ...)`. Here 1st argument will be number of arguments which will always be passed automatically and 2nd argument is ... which is also known as **ellipses**.
>
> 1. We declare a function with ellipses and than create a variable of type va_list i.e. `va_list values` denoting a list of varaibles
> 2. va_start is used to start accessing values. `va_start(values, num)` Here num is the size of list or number of arguments passed to function.
> 3. va_arg is used to access a single element from the list. `va_arg(values, int)`. 1st argument is the list of values and 2nd argument is the type of values in list.
> 4. va_end is used to remove the memory set for the variables `va_end(values)`

- #define clean_errno()

  > it is just a clean version of errno. If errno is 0 than there is no error and it will print None else it will print the errno.

- \_\_FILE\_\_ , \_\_LINE\_\_ , ##\_\_VA_ARGS\_\_

  > The use of \_\_VA_ARGS\_\_ is very important here as in these macro functions (...) part says all the extra arguements it receives will be passed as VA ARGS. Also notice the use of FILE AND LINE. This is useful when someone wants to know the line number and file which caused the error.

- log warn, loginfo, log err

  > All these are log messages for end user with simple fprintf statements to write to stderr file.

- check function

  > this macro will check if variable A passed to it is true or not if not it will call log_err. Similarly check_mem and check_debug can be interpreted.

adding those all macros to a file i.e. dbg.h and than using that including this file will also add all our defined functions in the file. `#define "dbg.h"`. This is what a header file does. It brings the defination of functions in program.

In all above code there is a statement **goto error**. Here author is using error as a tag to jump onto i.e.

```c
int main(){
    FILE *file = open("test.c", "r+");
    char *string = (char*) malloc(50*sizeof(char));
    check_mem(file);
    error:
    	puts("You are not important enough for your computer. LOL\n")
}
```

check_mem will call check which on not getting memory will jump to error, which is tag defined in the program and will print statement in puts.

#### How C Pre Processor expands macros

`#define log_err(int num, ...) fprintf(stderr, "[ERROR]%s:%d", __FILE__, __LINE__, __VA_ARGS__);`. Here all the values given to log_err will be replaced with **VA_ARGS\_\_. So we can write log_err("Your name %s and age %d\n", name ,age) and it will be converted to `fprintf(stderr, "[ERROR]%s:%d "Your name %s and age %d\n"", **FILE**, **LINE\_\_, name ,age);` What macros actually do is replace the macro with right hand side of the macro.

Similar thing is happening with check. It is replacing macro with it's defination or right hand side. So this is how macros works. Pre-processor replaces macros with its defination in the code recursively. This can also be done with a function like write but as it will be a function it will have limited scope so there are few thing which one will miss like \_\_FILE\_\_ and \_\_LINE\_\_.

To discard debug messges one can put #define NDEBUG on top of code file or in the Makefile add -DNDEBUG as a CFLAG.

\__FUNCTION\_\_ can be used to also print function name in the terminal as \_\_FILE\_\_ or _\_LINE\_\_.

## Advance Debugging Techniques

One can use debugger to run the code line by line. For a code like below

```c
#include <stdio.h>
//#include "dbg.h"
#include <assert.h>
#include <error.h>

int host(int a){
    int k = 62;
    puts("well played");
    assert(a==4);
    puts("something");
}

int main(int argc, char *argv[]){
    printf("%s:%d\n", __FILE__, __LINE__);
    host(5);
}
```

debugger can be used to see how code is working line by line. There are many C debuggers like gdb, lldb etc. To start debugger with a given program one can run `gdb test`. This will run the above program with debugger (test will be executable for the program above).

Setting a break point: `break test.c:8` or `break host`.

a value can be printed with: `print a` or `print k`

run the debugger with `run`

```c
GNU gdb (GDB) 11.2
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-pc-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from test...
(gdb) break main

Breakpoint 1 at 0x11c2: file test.c, line 14.
(gdb) break test.c:6

Breakpoint 2 at 0x1164: file test.c, line 8.
(gdb) run   backtrace

(gdb) run

Starting program: /home/bhavuksharma2202/Workspace/C/test
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/usr/lib/libthread_db.so.1".

Breakpoint 1, main (argc=1, argv=0x7fffffffe208) at test.c:14
14	    printf("%s:%d\n", __FILE__, __LINE__);
(gdb) backtrace

#0  main (argc=1, argv=0x7fffffffe208) at test.c:14
(gdb) s
15	    test(5);
(gdb) step
Breakpoint 2, test (a=5) at test.c:8
8	    puts("well played");
(gdb) s
(gdb) print a
 (gdb) 5
 (gdb) print k
 (gdb) 62
9	    assert(a==4);
(gdb) s
Program received signal SIGABRT, Aborted.
0x00007ffff7e1a34c in __pthread_kill_implementation () from /usr/lib/libc.so.6
(gdb) s
Single stepping until exit from function __pthread_kill_implementation,
which has no line number information.

Program terminated with signal SIGABRT, Aborted.
The program no longer exists.
(gdb) backtrace
(gdb) q
```

This is how one can find the flow of the program and once an error is detected it can be fixed with appropriate measures.

backtrace is used to get the flow of the program. like how program is running, starting from main till it fails.

## Advance Data Types and Flow Control

To check the max and min value of any data type we can use limits.h header file i.e. INT_MIN for minimum value of int and INT_MAX for max value of int. limits for other variables can also be seen with UINT_MAX for unsigned int. SHRT_MIN (min value of short), USHRT_MIN (unsigned min value). CHAR_BIT (for char size). SCHAR_MIN (CHAR_MIN is also same) will give min value for signed char.

##### Basic Available Data Types

| Data Type | Description                                                                    |
| --------- | ------------------------------------------------------------------------------ |
| int       | Stores a normal int usually with a size of 32bit (4bytes)                      |
| double    | Stores a floating point value with a size of 64bit (8bytes)                    |
| float     | Stores a floaing point value with a size half of double usually 32bit (4bytes) |
| char      | Stores a char value with a size of 8bit (1byte)                                |
| void      | indicated no type and used to say that function returns nothing                |
| enum      | These work as integer but with a given symbolic values                         |

##### Type modifiers

| Modifier | Description                                                                                                                                 |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| unsigned | To double the upper bound and reducing the lower bound to 0 so that data type doesn't have a negative value                                 |
| signed   | Gives negative and positive numbers but reducing the upper bound to half and lower bound to a negative value                                |
| long     | Doubles the size of the postfixed data type. i.e. int have a size of 4 bytes using _long int_ will increase the size to 8 bytes.            |
| short    | Reduces the size to half of the postfixed data type. _short int_ will have a size of 2 bytes if int has size of 4 bytes for a given system. |

##### Type Qualifiers

| Qualifier | Description                                                                                                                                        |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| const     | indicates the value won't change after being initialized                                                                                           |
| volatile  | indicates compiler shouldn't do any optimizations on the variable declared as volatile. This is useful only when code optimizations are turned on. |
| register  | Forces compiler to save the value in a cpu register.                                                                                               |

##### Type Conversion

C uses type conversion where it looks at 2 operands on either side and convert the smaller one into the larger one and sizes are in the order. `long double > double > float > int(only char and short int) > long `. So for example we have an equation like `long + char - int * double`

- 1st int \* double will result into double as int will be converted to double
- long + char will converted to long and now we will be left with `long - double`
- finally we will get a result in double

**stdint.h** defines a set of typedefs for exact-sized integer types i.e. int of size 8, 16 etc. and those are defined as **_int8_t, uint8_t, int16_t, uint16_t_**, following a pattern **_(u)int(BITS)\_t_** where u indicates unsigned and BITS indicates size of the data type. This also defines the size of each variable using the pattern (U)INT(BITS)\_MAX, (U)INT(BITS)\_MIN i.e. INT16_MAX, UINT16_MAX etc.

There are also macros defined in stdint which defines **_size_t_** (size_t is an unsigned int (32-bit system) or unsigned long long (64-bit compiler) which is used to define size of any object or return type of a function returning size of an object).

## The Stack Scope and Globals

consider below files(file.h, file.c, file_main.c) with codes

```c
// file.h
#ifndef __ex22_h__
#define __ex22_h__

extern int THE_SIZE;

int get_age();
void set_age(int age);

double update_ratio(double ratio);

void print_size();

#endif
```

```c
// file.c
#include <stdio.h>
#include "ex22.h"

int THE_SIZE=1000;

static int THE_AGE = 37;

int get_age(){
    return THE_AGE;
}

void set_age(int age){
    THE_AGE = age;
}

double update_ratio(double new_ratio){
    static double ratio = 1.0;

    double old_ratio = ratio;
    ratio = new_ratio;

    return old_ratio;
}

void print_size(){
    printf("I think ratio is: %d\n", THE_SIZE);
}
```

In file file.h there is a variable defined as `extern int THE_SIZE`.

- **extern** is a way to tell compiler that this variable does exist but in another external location more precisly in another file.
- **static (at file level)** is kind of opposite of extern as it tells that variable should be accessible to only this file not others **(static at file level is different than at other places)**
- **static (at function level)** will set a variable static for file but can only be accessed in that function only.
- **THE_SIZE** is defined as extern variable which will be used in **file_main.c**. This simply says that THE_SIZE variable is going to be used which is declared in another file.
- **get_age, set_age** are taking static variable **THE_AGE** and modifying or printing them as those are static variables and can't be excessed outside of the file. But to be used in another file these needs to be declared in that file.
- **update_ratio** takes a new ratio value and returns the old one. It uses a static variable ratio to keep track of old ratio.
- **print_size** prints the value of THE_SIZE

One thing to note here is that if a variable is set as static, it can only be declared once and if compiler encounters the declaration again it will ignore the declaration and set the new value assigned to it.

```c
//file_main.c
#include <stdio.h>
#include "ex22.h"

const char *MY_NAME = "Something";

void scope_demo(int count){
    printf("count is %d\n", count);
    if (count > 10){
        int count = 100;
        printf("count in this scope is %d\n", count);
    }

    printf("count is at exit: %d\n", count);
    count = 3000;
    printf("count after reassigning: %d\n", count);
}

int main(){
    printf("My name is %s and age is %d\n", MY_NAME, get_age());

    set_age(100);
    printf("My age now is %d\n", get_age());

    printf("The size is %d\n", THE_SIZE);
    print_size();

    THE_SIZE = 9;

    printf("The size now is %d\n", THE_SIZE);
    print_size();

    printf("Ratio at first: %f\n", update_ratio(2.0));
    printf("Ratio again: %f\n", update_ratio(10.0));
    printf("Ratio once more: %f\n", update_ratio(300.0));

    int count = 4;
    scope_demo(count);
    scope_demo(count * 20);

    printf("count after calling count_demo: %d\n", count);

    return 0;
}
```

To compile a file which is a link file we use `gcc -c -o file.o file.c` and than compiling a file using link file `gcc file_main.c file.o -o file_main`.

Few things to learn from this is:

- a variable defined outside any function has the global scope for file but for other files to access it, it should be declared as extern variable in that file i.e. THE_SIZE here.
- A variable declared as static outside of a function is global in that file's scope but can't be used outside of that file
- A variable declared as static inside a function is a global variable for that function but if it is declared once it can't be declared twice when calling that function again. If function see the declaration again it will simply ignore the declaration i.e. **ratio** here.
- Scope of a variable declared in a function or passed to a function will have scope to the whole function but if there is another variable declared inside the function with the same name than for that particular block, it will be preffered than variable passed i.e. **count in scope_demo function**.

**Things to avoid while writing C code**

- Do not shadow a variable like I’ve done here with count in scope_demo. It leaves you open to subtle and hidden bugs where you think you’re changing a value but you’re actually not.
- Avoid using too many globals, especially if across multiple files. If you have to use them, then use accessor functions like I’ve done with get_age. This doesn’t apply to constants, since those are read-only. I’m talking about variables like THE_SIZE. If you want people to modify or set this variable, then make accessor functions.
- When in doubt, put it on the heap. Don’t rely on the semantics of the stack or specialized locations. Just create things with malloc.
- Don’t use function static variables like I did in update_ratio. They’re rarely useful and end up being a huge pain when you need to make your code concurrent in threads. They’re also hard as hell to find compared to a well-done global variable.
- Avoid reusing function parameters. It’s confusing as to whether you’re just reusing it or if you think you’re changing the caller’s version of it.

## Meet Duff's Device

This was a device which was invented by Tom Duff, and is simply a way to do something with c compiler. It basically break the loops into smaller parts of procedural program so that loop has to run less time because a loop can be expensive for CPU. This is just a program to copy content of an array to another

```c
int duffs_device(int *from, int *to, int count)
{
    {
        int n = (count + 7) / 8;
        switch (count % 8)
        {
        case 0:
            do
            {
                *to++ = *from++;
            case 7:
                *to++ = *from++;
            case 6:
                *to++ = *from++;
            case 5:
                *to++ = *from++;
            case 4:
                *to++ = *from++;
            case 3:
                *to++ = *from++;
            case 2:
                *to++ = *from++;
            case 1:
                *to++ = *from++;
            } while (--n > 0);
        }
    }
    return count;
}
```

Here while copying content of a string to another part instead of writing all the content one by one in each iteration we are copying 8 chars in 1 single iteration. **remember when in switch statement a case is met all the cases below it will execute automatically even if the don't satisfy the condition.** Like here if count % 8 is 3 than the case 3 will run but case 2 and case 1 will also run this is why it is recommended to use break within every case.
One more thing one can notice here is that switch statement's some cases are in do-while loop which shouldn't be valid but is completely valid in C. So there is no effect of loop for swtich statement.

We are copying chars with pointer while incrementing them. So a line `*to++ = *from++` will copy the char from address of **to pointer** (take as 100) to **from pointer**(take as 200). after copying it will increment the values of both **to and from** with the use of unary increment ++. So when next time to will be pointing to 101 and from will be pointing at 201. Similarly it will copy all the values with incrementing the base address of both pointers.

## Input Output Files

As per above program a file is opened and things are written into it.

- `FILE \*fopen( const char \*filename, const char \*mode)`

  - **r, w, a, r+, w+, a+** . write and append works differently

  - w -> will write to the file from beginning and will create a file if there is no file

  - w+ -> will create a file and open for read and write but also remove all the content from the file at the time of opening the file

  - a+ -> will start reading from the 1st line but writing can be done after the content file had before.

  - if there is a need to open a file in binary just append b with a,w,r `ab, wb, rb+ etc.`

  **Closing a file**

- `int fclose(FILE *file)` -> this will close the file by taking address of file.

  **Writing to a file**

- `int fputc(int c, FILE *fp);` -> this will write char c into the file

- `int fputs(char* str, FILE *fp)`-> will write whole line into the file

- `int fprintf(FILE *fp, const char* format, ....)`-> can also be used to write to a file

- `fwrite(*addressdata, sizeofdata, times, *file)`-> This is used to write data in binary format

**Reading a file**

- `int fgetc(FILE *fp)`-> will read a character from the input file

- `char *fgets(char *buff, int n, FILE *fp)`-> This function will read the file fp upto n-1 chars and put all those values in buff array. while loop can be stopped by equating with **Null**

- `char* fscanf(FILE *fp,const char* format, char *buff)`-> This will do the same function as fgets.

- `fread(*addressdata, sizeofdata, times, *file)`-> used to read from a binray file

- `fseek(FILE* file, long int offset, int whence)` -> to move pointer to somewhere in file

  - SEEK_SET -> Start offset from start of file

  - SEEK_END -> Start offset from end of file

  - SEEK_CUR -> Start the offset from current pointer position

```c
int main(){
   FILE *fp;
   char buff[255];

   fp = fopen("/tmp/test.txt", "r");
   fscanf(fp, "&d: %s", buff, 1);
   printf("1 : %s\n", buff );

   fgets(buff, 255, (FILE*)fp);
   printf("2: %s\n", buff );

   fgets(buff, 255, (FILE*)fp);
   printf("3: %s\n", buff );
   fclose(fp);
}
```

- `fflush`-> This is all OS dependent and many oem's doesn't implemented fflush. This is the reason many people don't recommend using fflush as this restrict platform independece and also introduce undefined behaviour. We can clear input stream with `fflush(stdin)`

The below program will tell everything about reading and writing text files

```c
// File Handling
#include <stdio.h>
#include <stdlib.h>

int textWrite(FILE *file){
    char *array[] = {"Dennis", "Ritchie"};
    // fputc, fputs, fprintf
    // int fputc(int char, FILE *file)
    fputc('1', file);    // will write 1 in the file's starting.
    fputc(':', file);    // will write : to the file
    // int fputs(char *str, FILE *file)
    fputs("\n This is a string\n", file); // will write whole string to the file.
    // int fprintf(FILE* file, char* frmted, ....)
    fprintf(file, "1st Name: %s, last name: %s", array[0], array[1]); // will write formatted string.
    // none of the above functions adds a newline after writing to the file.

    return 0;
}

int readgetc(FILE *file){ // this function read all chars until it founds EOF.
    char ch;
    while(1){
        ch = fgetc(file);
        if (ch == EOF) break;
        putc(ch, stdout);   // putc(ch, stdout) is same as printf("%c", ch);
    }
    return 0;
}

int readgets(FILE *file){
    char str[10];
    while (fgets(str, 10, file)){
//        puts(str);      // puts put new line after writing to stdout so this will screw output.
        printf("%s", str);
    }
    return 0;
}

int readfscanf(FILE *file){ // fscanf returns int value, that value will be equal to number of values
    char str[100];          // scanf is reading from file.
    while (fscanf(file, "%s%s", str, &str[50])>0){ // here fscanf is taking 2 values str and &str[50]
        printf("%s %s\n", str, &str[50]);           // so it will be returning 2 and when there is only
    }                                               // 1 value left or 0 value left it will stop running
    return 0;
    // this was just for demonstrantion otherwise one should take only one string value as input
    // not anything above it. fscanf(file, "%s", str)
}

int main(){
    // opening a file
    FILE *file = fopen("asus", "r");
    if (file == NULL){
        perror("Error: ");
        exit(EXIT_FAILURE);
    }
//    textWrite(file);
//    read(file);
//    readgets(file);
    readfscanf(file);

    return 0;
}
```

Above code doesn't mention reading and writing binary files.

**I/O Functions**

- fscanf
- fgets
- fopen
- freopen
- fdopen
- fclose
- fcloseall
- fgetpos
- fseek
- ftell
- rewind
- fprintf
- fwrite
- fread

## Variable Argument Functions

In C one can create a function taking variable number of inputs. This can be done with the help of a **stdarg.h** file.

To make a function of variable arguments, one can use `int function(fmt, ...)`. The very 1st parameter is called named argument and ... is called **ellipses**.

```c
int sum(int total, ...){
    va_list list;
    int sum = 0;
    va_start(list, total);
    for (int i = 0; i < total; i++){
        sum += va_arg(list, int);
    }
    va_end(list);
    return sum;
}
```

In above code the first value passed is independent of variable argument function. This value can be anything as per our need. Here to calculate the sum total number of arguments are passed.

1. we need to include `stdarg.h` header file to use variable function arguments.
2. `va_list list` will declare the variable argument list
3. `va_start(list, total)` will initialize the variable argument list.
4. `va_arg(list, int)`. This is used to use a value from the variable argument list. 1st argument is the declared list and 2nd argument is the type of element we are processing. There can be more than 1 types of element in the same list i.e. `va_arg(list, int)` and `va_arg(list, char*)`, both are possible to used in a single function
5. `va_end(list)` is used to free the memory for the `va_list list `.

Another program which is using variable argument function is below

```c
int function(char *fmt, ...){
    if (fmt == NULL) return -1;
    va_list list;
    va_start(list, fmt);
    char *name = malloc(15*sizeof(char));
    if (name == NULL) return -1;
    char *father_name = malloc(15*sizeof(char));
    if (father_name == NULL) return -1;
    int age, father_age;
    int i = 0;
    while (fmt[i] != '\0'){
        switch (fmt[i]){
            case 'n':
                name = va_arg(list, char *);
                break;
            case 'f':
                father_name = va_arg(list, char *);
                break;
            case 'q':
                age = va_arg(list, int);
               	break;
            case 'a':
                father_age = va_arg(list, int);
                break;
            default:
                puts("Your format is nonsense");
        }
    }
    return 0;
}
```

As in above code we are using `va_arg(list, int)` and `va_arg(list, char *)`, we can use any possible value in one function at the same time.

**We can put any number of arguments before ellipses(...) but none after ...**.

## Logfind

Create a prgram which will search for specific words in given files.

## Creative and defensive Programming

```tex
You will frequently run into programmers who feel intense pride
about what they’ve created, which is natural, but this pride gets in the way of
their ability to objectively improve their craft. Because of this pride and
attachment to what they’ve written, they can continue to believe that what they
write is perfect. As long as they ignore other people’s criticism of their code,
they can protect their fragile egos and never improve.
```

Defensive mindset according to author is:

1. Software has errors.
2. You aren't your software, yet you're responsible for the errors.
3. You can never reduce the errors, only reduce the probability

**8 defensive programming Strategies** (Always think like someone who wants to break the code)

1.  **Never trust the input.** (Always validates what you are inputting)
2.  **Prevent errors** : If errors are possible, no matter how probable try to remove prevent them.
3.  **Fail Early and Openly** : Fail early, cleanly and openly stating what happened, where and how to fix it
4.  **Documentation Assumptions**: Clearly state pre-conditions, post-conditions and invariants
5.  **Prevention over Documentation**: Don't do with documentation which can be done with code or avoided completely.
6.  **Automate Everything**: Automate everything, including testing
7.  **Simplify and Clarify**: Always simplify the code to smallest, cleanest form that works without sacrificing safety.
8.  **Question Authority**: Don't blindly follow or reject rules

**Never Trust Input**, simply means confirm the input before taking any actions on it. given and example below

```c
int bad_copy(char *from, char *to)
{
    int i = 0;
    while (( to[i] = from[i]) != '\0') i++;
}

int safe_copy(char *from, char *to, int size)
{
    assert(from != NULL && to != NULL && "this is just a NULL check");

    // don't use while (from[i] != '\0') as if string isn't ending with \0 it will go
    // past the required value to be copied and can give seg fault

    if (size <= 0) return -1;

    for (int i = 0; i < size-1; i++)
    {
        to[i] = from[i];
    }
    to[size - 1] = '\0';		// end a string with NUL always.
}
```

_bad_copy_ is source of huge number of buffer overflows. It is flawed because it thinks that all the strings are going to end with '\0'. This can cause while loop to run infinitly.

In above code there are few things which one can observe

1. If you are going to copy a string you should check if the length passed is valid
2. If you don't want input to be NULL than also add a check for that.
3. If sizes should be in a level, also add a check for that.

This is a list of things to prevent input errors:

1. For each parameter, identify preconditions, and check if these conditions are returning error or failure. Always favour errors over failures.
2. Add `assert` call at the beginning that check for each failure preconditions using `assert(test && "message").` When it fails the system will print assert error with message. This message is very helpful as it will help why assert failed.
3. For other preconditions print the error message or return error code
4. Document why preconditions are necessary. So that another developer can figure out, if they are important anymore or not.
5. If you are modifying the inputs always check they are formed correctly in the same function, if something went wrong abort the operation.
6. Always check the error codes of the function you use, those funtions maybe _fopen, fclose, fread etc._
7. Always try to return the consistent return codes

**Prevent Errors**

1. List all possible errors, no matter how probable
2. Give each error a probability that's percentage of operations that can be vulnerable. For a function call error percentage is the percentage or function calls to that particular function
3. Calculate effort to put in to prevent them or simply give simple and hard tag.
4. Rank them according to effort or probability
5. Try to prevent all errors, if not possible than try to reduce the possibility
6. If there are errors which you can't fix then document them so that someone else working on code fix them.

This can help to work on errors which are more important.

**Fail Early and Openly**

In C when you encounter any error you have 2 choices to select from:

- Return the error code
- abort the process

Fail Early means it is better to fail early than to fail catastrophically. Simply if you are giving a wrong pointer in `safe_copy` function, you should check if pointer is invalid and return an error code early than let it run and fail your program.

Fail Openly means, when you encounter an error you should print proper message or give proper error code. To make it more better also return line number where it happened with function name as authors macros. And always try to give different error message for different possible errors.

**Document Assumptions**

After everything we are left with adding invariants and postconditions. An Invariant is a condition that holds true for some state in function.

In most functions return value is 0, -1 but sometimes one may return something else like a resourced value one should confirm that it is returning something not NULL value.

**Prevention over Documentation **

If you know a bug and have ability to fix it then fix it instead of writing in documentation that there is a bug. You should prevent a bug if you know how to fix it and only case you should document it, when you don't know how to fix it.

**Automate things**

Few things a programmer can automate

- Testing and validation
- Build Process
- Deployment of Software
- System administrations
- Error Reporting

For these things one can write automate tests or unit tests.

## Intermediate Makefiles

The file structure of a project

- LICENSE : Contain the licensing for the copyright
- README.md : A Readme file which contains instructions on how to use the program
- Makefile : Main build file of the project
- bin/ : Where binaries of the user code goes. Content in it is usually written by Makefile.
- build/ : Where libraries and other build artifacts go. Also empty mostly, Makefile will create it if it's not there
- src/ : Where source code goes. like .c and .h files.
- tests/: Where automated tests go.

**MAKEFILE**:

```makefile
CFLAGS=-g -O2 -Wall -Wextra -Isrc -rdynamic $(OPTFLAGS)

LIBS=-1dl $(OPTLIBS)
PREFIX?=/usr/local

SOURCES=$(wildcard src/**/*.c src/*.c)
OBJECTS=$(patsubst %.c,%.o,$(SOURCES))

TEST_SRC=$(wildcard tests/*_tests.c)
TESTS=$(patsubst %.c,%,$(TEST_SRC))

TARGET=build/libYOUR_LIBRARY.a	# replace YOUR_LIBRARY with appropriate value
SO_TARGET=$(patsubst %.a,%.so,$(TARGET))

# The Target Build
all: $(TARGET) $(SO_TARGET) tests

dev: CFLAGS=-g -Wall- Isrc -Wall -Wextra $(OPTFLAGS)

dev: all

$(TARGET): CLAGS += -fPIC
$(TARGET): build $(OBJECTS)
	ar rcs $@ $(OBJECTS)	# to add Object files to TARGET Archive.
	ranlib $@				# to index the TARGET archive
$(SO_TARGET): $(TARGET) $(OBJECTS)
	$(CC) -shared -o $@ $(OBJECTS)

build:
	@mkdir -p build
	@mkdir -p bin


# The Unit Tests
.PHONY: tests
tests: CFLAGS += $(TARGET)
tests: $(TESTS)
	sh ./tests/runtests.sh

# The Cleaner
clean:
	rm -rf build $(OBJECTS) $(TESTS)
	rm -f tests/tests.log
	find . -name "*.gc*" -exec rm {} \;
	rm -rf `find . -name "*.dSYM" -print`

# The Install
install: all
	install -d $(DESTDIR)/$(PREFIX)/lib/	# create the install dir if it doesn't exist
	install $(TARGET) $(DESTDIR)/$(PREFIX)/lib/

# The Checker
check:
	@echo Files with potentially dangerous functions.
	@egrep '[^_.>a-zA-Z0-9](str(n?cpy|n?cat|xfrm|n?dup|str|pbrk|tok|_)\
		|stpn?cpy|a?sn?printf|byte_)' $(SOURCES) || true

```

**The Headers**

1. CFLAGS: These are the usual C Flags which are set in whole of your project. One may have to adjust those according to **platform**. One can augment other CFLAGS options with the help of **OPTFLAGS**.

2. LIBS: These options are used when linking a library. Someone else can augment the linking options with **OPTLIBS**.

   1. ```bash
      make OPTFLAGS=-pthread
      # or
      make PREFIX=/tmp install
      # or
      make OPTFLAGS=-pthread PREFIX=/tmp install
      ```

3. PREFIX: This is just a value which is set to /usr/lib if PREFIX is not already defined. PREFIX?=/usr/lib. Here ?= means that if value is not defined before than set it to assigned value otherwise don't do anything.

4. SOURCES: We are using this as we want to list out dependencies for a specific target. We can write dependecies one by one or by this awesome feature of **gnu make** which uses **wildcard** to list out all the dependencies. Here we are listing all the dependenicies which are in src folder and another subfolder which ends with ".c" extension. This is handy when we want to list all the dependencies for a target and in reality one can't do that for a very big project but with wildcard one can simply save all values in a variable and set variable as dependency.

5. OBJECTS: This is set by **patsubst** which will take 3 arguments. 1st will be the filter for files needed to be processed and how to substitute those files and 3rd argument will be the list of files. So here all the files from SOURCES ending with _.c extension will be renamed to _.o extension and than those files will be assigned to OBJECTS. which again is a trick to list dependencies for some targets as there will be some targets which will need OBJECTS as dependencies.

6. TEST_SRC: This is again using wildcard and listing all the test files present (notice all the test files are ending with \_tests.c)

7. TESTS: Here we are removing the .c extension from all the test files, simply means we are listing object files for all the TESTS so that if any test depends upon the object file of test we can simply list that in dependencies. **EVERYTHING ABOVE WE HAVE DONE JUST TO LIST OUT DEPENDENCIES FOR A TARGET WHICH IS SOMEWHAT IMPORTANT DURING BUILD PROCESS**.

8. TARGET: This tells that ultimate target is build/libYOURLIBRARY.a where YOURLIBRARY need to be changed to whatever library we are trying to build. `.a` files are archives or we can say static libraries, which are linked with executable. So they are copied to the executables.

9. SO_TARGET: Here we are again using **patsubst** to define shared object files. `.so` files are files which are not linked to executable but are required to run a program which depends on them. So `.so` files are probably will be available on someone's system and they aren't copied to executable, reducing the size of the executable and resources. But these should be present in the system.

10. **THE TARGET BUILDS:- all:** As makefile always runs the very 1st target it will always run _all_ when called without specifying any target and all is specifying \$(TARGET) \$(SO_TARGET) tests. On searching TARGET one will find that it is a library So make will build libraries 1st, than make unit tests

11. **dev:** There are 2 targets named dev. 1st is simply setting CFLAGS for a developer build. and another one is calling all, nothing exceptional

12. **$(TARGET):** `$(TARGET): CFLAGS += -fPIC` This simply adds a value for CFLAGS for library build.

13. **$(TARGET): build \$(OBJECTS)**: Here we are calling build first which is creating build directories and than compile all of the objects.

14. **ar rcs \$@ \$(OBJECTS)**: This command is gnu specific command and will create archive with the name specified by **$@** and archive all the files specified by **$(OBJECTS)**.

15. **ranlib $@:** This is again gnu specific tool which index archives. after running this on target we have build the library.

16. **build**: is just creating build and bin directories if they are not available. _@ prefix prevent the given command to be printed on stdout_.

17. **tests: CFLAGS += $(TARGET):** This will link `build/libYOUR_LIBRARY.a` library to each of the test.

18. **tests:$(TESTS):** This says that tests depends on all the tests we have saved in (TESTS) variable, than action on this target is to run a simple shell script (a script which will run them all and report their output)

19. **The Cleaning Part:** This just cleans the project. Removes the build, objects, tests and log files which were created during building or testing, also it removes some other junk files like _dSYM_ files created by Xcode on Mac Systems.

20. **Installer Part:** This will be used to install the project on any system. People may want to install on any other directory like /usr/local/lib. So PREFIX is used here to give flexibility to installer (which is actually common). It has dependency on _all_, so everything will be built before taking any action. Than the 1st action is creating a directory where the install location is specified (-d means create the dir if it doesn't exist). 2nd action just install the $(TARGETS) to the specified location in 2nd argument given to install.

21. **The Checker** Just checks for the bad functions used in the program and will report if found any. `|| true` is a way to prevent make thinks that egrep failed when it didn't find any matches.

## Libraries and Linking

This is the libraries C links for the program. There are basically 2 types of libraries:

1. **Static**: These libraries ends with `.a` extension and we also created these libraries with `ar`(a unix archive tool) in previous exercise. This is just a collection of some object files _ending with .o extension_.
2. **Dynamic**: These libraries ends with `.so, .dll or many others formats in OS X`. When a Program which is dependent on these libraries are executed, OS loads these libraries dynamically and link them with your program.

**Static Libraries** are good to use with small or medium sized projects, because these are easier to deal with **Author also likes to put all the code in Static library so the one can link that code to unit tests and to the files program needs**

**Dynamic Libraries** are good to use with larger systems, when space is tight or if you have large number of programs that use same functionality. This is more useful because these are not copied to program executable, eventually decreasing the size of the program.

The shared library function (libex29.c)

```c
#include <stdio.h>
#include <ctype.h>
#include "dbg.h"

int print_a_message (const char *msg){
    if (!msg) return 1;
    printf("A String: %s\n", msg);
    return 0;
}

int uppercase(const char *msg) {
    if (!msg) return 1;
    for (int i=0; msg[i] != '\0'; i++){
        printf("%c", toupper(msg[i]));
    }
    puts("");

    return 0;
}

int lowercase(const char *msg){
    if (!msg) return 1;

    for (int i = 0; msg[i] != '\0'; i++){
        printf("%c", tolower(msg[i]));
    }
    puts("");
    return 0;
}

int fail_on_purpose(const char *msg){
    return 1;
}

```

It is doing nothing special just some simple functions.

**Compiling this to be a shared library**

> gcc -c libex29.c -o libex29.o
>
> gcc -shared -o libex29.so libex29.o

**The process to create a shared library vary system to system or OS to OS, but above process works with linux fine for now**

Another program which is using this library (ex29.c)

```c
#include <stdio.h>
#include "dbg.h"
#include <dlfcn.h>

typedef int (*lib_function) (const char *data);

int main(int argc, char *argv[]) {
    int rc = 0;
    check(argc == 4, "USAGE: ex29 libex29.so\
            <function> data");
    char *lib_file = argv[1];
    char *func_to_run = argv[2];
    char *data = argv[3];

    void *lib = dlopen(lib_file, RTLD_NOW);
    check(lib != NULL, "Failed to open the library\
            %s: %s", lib_file, dlerror());
    lib_function func = dlsym(lib, func_to_run);
    check(func != NULL, "Didn't find %s function in\
            the library %s: %s", func_to_run,\
            lib_file, dlerror());
    rc = func(data);
    check(rc == 0, "Function %s rturn %d for data:\
            %s", func_to_run, rc, data);
    rc = dlclose(lib);
    check (rc == 0, "Failed to close %s", lib_file);

    return 0;
error:
    return 1;
}
```

compiling the program with dynamic linking `gcc ex29.c -ldl -o ex29`

Here the program needs 4 arguments to run properly, And nothing is special in here to except a way to access shared libraries using **`dlopen, dlsym, dlerror, dlopen`**.

- `void \*dlopen(const char *library_name, int Flags)`. On success it will return a non-NULL handle and on failure it will return NULL.
- `int dlclose(void *handle)` return 0 on success and non-zero value on failure.
- `void *dlsym(void *restrict handle, const char *restrict symbol)`. It will take a handle opened with dlopen and a symbol. It will return the address of the symbol in the handle passed.
- `char *dlerror(void)`. It will return any error related with the shared library handle like opening reading or anything else.

**To list all the function in shared object :-** `nm libex29.o` whose output will look something like this.

> 0000000000000120 T fail_on_purpose
> 00000000000000ae T lowercase
> 0000000000000000 T print_a_message
> U printf
> U putchar
> U puts
> U tolower
> U toupper
> 000000000000003c T uppercase

## Automated Testing

Below is a header file which will contain the unit testing macros.

```c
#undef NDEBUG
#ifndef _minunit_h
#define _minunit_h

#include <stdio.h>
#include <dbg.h>
#include <stdlib.h>

#define mu_suite_start() char* message = NULL

#define mu_assert(test, message) if (!(test)) {\
    log_err(message); return message;}
#define mu_run_test(test) debug("\n-------%s", " " #test); \
    message = test(); tests_run++; if (message) return message;

#define RUN_TESTS(name) int main(int argc, char *argv[]) {\
    argc=1; \
    debug("----- RUNNING: %s", argv[0]);\
    printf("----\nRUNNING: %s\n", argv[0]);\
    char *result = name();\
    if (result != 0) {\
        printf("FAILED: %s\n", result);\
    }\
    else {\
        printf("ALL TESTS PASSED\n");\
    }\
    printf("Tests run: %d\n", tests_run());\
    exit(result != 0);\
}

int tests_run;

#endif
```

# Miscellaneous

## Pointers in C

Pointers are very important topic in C. A pointer holds the address of any variable. To declare a variable as a pointer we need to use \* (astrick) sign. There are 2 very important signs we use, \* and other is &.

This is how we can use pointers

```c
int value = 23;        // addr 1000
int *ptr;              // pointer declaration
p = &value;            // value of p is 1000
// &value represents --> address of value which is 1000 in this case
printf("%p\n", ptr);       // output --> 1000
printf("%p\n", &value);    // output --> 1000
```

#### Pointer Arithmetic

Addition and Subtraction can be done on pointers. This will not be similar to normal addition or subtraction. For an integer pointer incrementing one will increment the address by number of bytes it will take for OS to save.

Suppose the OS has int size of 4 bytes and value of address pointer ptr is 1000 than `ptr+1` will give 1004 as it is searching for next int.

```c
int a = 72;
int *ptr = &a;                // suppose 1000
char *ptr0 = (char*)ptr       // now ptr0 = 1000
// we can also do char *ptr0 = &a;
// casting is not important but this will avoid any warnings
printf("%d\n", ptr + 1);      // 1004;
printf("%d\n", ptr0 + 1);     // 1001;
```

#### Memory Structure of a C Program

When a C program is called OS will reserve some memory to the program. This memory will be divided in 4 parts **(Code instructions, Static/Global variables, Stack, Heap)**.

- Code Instruction part will contain the code file under operation

- Static/Global variable part will store all the static and global variables so that every function in the program can use those.

- Stack (Call Stack) Part is the part where code will save its all function and local variables

  - Every function will create its stack frame where every function will store its locally defined variables, including main function.

  - If main function calls another function it will go to the stack top above main functions stack frame and will create its own stack frame.

Stack memory is allocated for program run, but if there are so many function calls in the program so that stack will fail to handle those in given amount of memory than it will overflow and this process is called **stack overflow**.

**Heap** is another type of memory which has no set limit like stack. It can grow till system has free memory available. If heap size goes too far it will crash the whole system, this can be counted as a disadvantage of heap. To use **heap** in C we need to know about these 4 functions

1. malloc    // to assign memory in heap

2. calloc

3. realloc

4. free // to free the space in heap

```c
#include<stdio.h>
#include<stdlib.h>

int main(int argc; char *argv[]){
    int a = 23;    // stored in stack
    int *p;        // stored in stack
    p = (int*) malloc(sizeof(int));
    *p = 73;
    free(p);
}
```

Malloc will save given amount of memory in the heap for the program and will return a void pointer. Breaking down above prog in steps:

1. `int a = 23` will save var a in stack

2. `int *p;` will be saved in stack as a pointer

3. p will hold the starting address of the variable in heap.

4. `(int*) malloc(sizeof(int))` this line will call malloc function and save passed amount of memory in heap, here 4 bytes for int will be saved as `sizeof(int)` will return 4.

5. Type casting is imp. when using malloc as it will return value as a void pointer.

6. `free(p)` will remove the variable p from the heap memory.

7. In case if malloc couldn't find any free block of memory it will return NULL.

##### **Memory allocation functions with their declaration**

```c
// 1. malloc
void * malloc(size_t size);    // size_t is the size of variable  passed
// 2. calloc
void * calloc(size_t num, size_t size); // num is the number of elements
// size is the size of a single element.
void * realloc(void* ptr, size_t size); // ptr is the pointer for old
// pointer and size is the size for the new pointer.
```

**Calloc is similar to malloc** but you don't need to calculate number of elements and size separatly and multiply them. You can simply pass both values to the calloc function and Calloc will initialize all values to 0 unlike malloc which will return garbage values from the memory block.

**Realloc** is used to reallocated already allocated pointer to somewhere else with a new size.

**Using malloc/calloc in a program**:

```c
int main(int argc, char* argv[]){
    int n;
    scanf("%i", &n);
    int array[n]; // this will give error as array size should be
                  // known before compilation of program.
}
```

In compilors following ANSI 89C, array size should be known at compile time, but this limiltation is removed in ANSI 99. So above code will give error for ANSI 89C. So one should use dynamic memory allocation using malloc or calloc

```c
int main(){
    int n;
    int *array;
    scanf("%i", &n);
    array = (int*) malloc(n*sizeof(int)); // dynamically allocated array
//  array = (int*) calloc(n, sizeof(int)); allocation with calloc.
}
```

if we try to reallocate a memory freed by free there will be an error on some compilors

```c
int main(){
    int size;
    int *array;
    scanf("%i", &size);
    array = (int*) calloc(size, sizeof(int));
    for (int i = 0; i < size; i++){
        scanf("%i", &array[i]);
    }
    free(array);
}
```

**Realloc as malloc and free**

- free -> realloc(ptr, 0). send 0 as size argument

- malloc -> realloc(NULL,size \* sizeof(int)). send pointer addr as NULL.

#### Pointers as function arguments (call by reference)

C supports call by value. But it can be forced to use call by reference with the help of pointers. Take below program in consideration

```c
int inc(int a){
    a += 1;
    printf("%i\n", a);
}
int main(int argc, char *argv[]){
    int value = 12;
    inc(value);
    printf("%i\n", value);
}
```

- This program will be assigned some memory than function main will be saved in call stack's top.

- main function will execute and create a stack frame for itself and save variable `value` as a local variable in its stack frame.

- than main will encounter inc function and it will create another stack frame for itself, and than copy actual argument and pass them as formal arguments.

- **Each variable will be saved in a lookup table to be accessed later**

- In this particular case it will map value to a `value --> a` than will execute inc. `a` will be stored as local variable in this function, so as function execution is over its stack frame will be removed and all the local variables will also be removed.

- so value modified in inc will be removed as that was a formal argument (call by value).

**To pass argument with call by reference**

```c
int inc(int *a){
    *a += 1;
    printf("%i\n", *a);
}
int main(int argc, char *argv[]){
    int value = 12;
    inc(&value);
    printf("%i\n", value);
}
```

This will pass address of value and inc function will increment the value at address of a. This is how one can achieve call by reference in C.

**Pointers and Arrays**

When an array is declared than consecutive memory blocks are saved for that particular array. Memory saved will depend on array size. For example if we declared and int array of size 4 and system treats int as 4 byte value. Than each int will take 4 memory blocks as one will represent 1 byte.

So to save array of 4 ints it will use 4 \* 4 = 16 bytes. Suppose very 1st element is at an address 2000. so the next element will be at 2004. That is the reason why we have pointer arithmetic in the 1st place.

```c
int array[] = {1,2,3,4};
int *ptr = &array;
printf("ptr = %p, array = %p\n", ptr, array);
```

By running above code one will observe that value of ptr and array variable is same, this means that variable `array` saves the address of the very 1st element of `array`. So `array == &array[0]`. Pointer arithmetic works the same way `array + 1 = &array[1]`. Dereferencing works the same way in arrays. `array[0] == *array`. Similarly for next index `array[1] == *(array + 1)`. Although variable array stores an address we can not increment it with `array++` but we can increment a pointer if that points to an address something like below code.

```c
int array[] = {42, 15, 73};
int *ptr = array;
array++;    // will give compilation error
ptr++;      // this is valid
```

**Passing Arrays as function arguments**

C uses call by value, but it is biased for arrays. If compilor sees an array as an argument to a function it will convert that array to a pointer with the same name storing the value of address of 0th index `int value(int array[]) --> int value(int *array)`. So when you test below code you will see that size of array is not as expected, the reason is that function inc is giving the size of a pointer not array itself.

```c
int inc();    // forward declaration
int main(){
    int array[] = {2,3,5}
    printf("%ld\n", sizeof(array));    // will return 12 bytes for x64
    inc(array);
}

int inc(int array[]){
    printf("%ld\n", sizeof(array));    // will return 8 bytes for x64
    array[0] += 1;
}
```

The print statement in main will return 12 with a system using 4 bytes for int, but print statement of inc function will not return 12 because the argument passed isn't interpreted as an array for that function. Compilor has changed it to an pointer, that's why it will return the size of the pointer not sizeof array. For x64 systems pointer size is 8 bytes. So it will return 8 bytes (size of pointer not sizeof array itself).

**So arrays uses call by refrence**, this is helpful as if we use call by value than we need to copy the whole array to function frame of inc but that will consume alot of memory. So it is better to use call by refrence for arrays.

##### Pointer as function return value.

```c
int* sum(int* a, int *b){
    int c = (*a) + (*b);
    return &c;
}

int main(){
    int *value;
    int *cover;
    *value = 4;
    *cover = 5;
    int *ptr;
    ptr = sum(value, cover);
    printf("%i\n", *ptr);
}
```

here ptr will hold the address of variable c returned by sum. But after execution of sum is over it will deallocate all the memory used by sum in stack so if someone is lucky the \*p will not be overwritten but if it is overwritten than the print statement will return some garbage value. So better approach is to use heap rather than stack for these type of tasks.

```c
int* sum(int* a, int* b){
    int *c = (int*) malloc(sizeof(int));
    *c = (*a) + (*b);
    return c;
}

int main(){
    int a = 23, b = 2;
    printf("%i", *(sum(&a, &b)));
}
```

##### Function Pointers

A function pointer for function `int add(int a, int b)` will be `int (*p)(int, int)`

and to execute this function using pointer. `(*p)(2,4)`

```c
int sum(int a, int b){
    return a + b;
}

int main(){
//  int *ptr(int , int);//this is declaration of function ret int pointer
    int (*ptr)(int ,int);
    ptr = &sum ; // ptr = sum; is same
    int result = (*ptr)(2,4) // ptr(2,4) will be same
    printf("%i\n", result)   // this will print 6
}
```

This is important when passing function as an argument to another function.

```c
int foo(){
    printf("calling foo");
}
int bar(int (*function)()){
    *(function)(); // function() is exactly similar
}
int main(){
    bar(foo);
}
```

This is used with qsort function in the standard c library.

```c
#include<stdlib.h>
#include<stdio.h>
// qsort(void *base,size_t lenArray,size_t ele_size,int (*comp)(const void*,const void*))
int compare(const void *p1,const void *p2){
    int a = *((int*)p1);
    int b = *((int*)p2);
    return (a - b);
}
```

##### Memory leak in C

If memory is being allocated on heap again and again without clearing unused memory than program will go on allocating more and more memory, which will result in higher memory usage for the program and ultimately can crash the system. So it is important to use free with malloc or calloc function.

#### Arrays and Strings

A string can be defined in following ways:

```c
char string[] = "some";    // in this case NUL is appended implicitly
char string[] = {'s', 'o', 'm', 'e', '/0'}; // NUL needed explicitly

char *string = "some";    // constant string
```

When a string is defined as a char pointer it will be stored as a constant in the memory. Any attempt to modify them will give runtime error as they are stored in memory inaccesible for a user and will raise sefmentation fault.

But if a string is defined as char array it can be modified easily even with a external function like below

```c
void inc(char *array);

int main(){
    char array[] = "some";
    inc(array);
}

// void inc(const char *array)
void inc(char *array){
    array[0] = 'k';
}
```

The above code can be used to modify the string array but if we don't want any external function to modify strings than we can use **const** keyword in function declaration before char like `void inc(const char *array)`. It will avoid any modification in string using function inc

## Various Functions used in C:

#### Strings <string.h>

- long unsigned int strlen(string) --> will return size of string.

- int strncmp(string1, string2, n) --> it will compare 2 strings until it found one difference upto n chars

- char \*strncpy(string1, string2, n)

#### Input Output <stdio.h>

- printf("%d", value)    --> write in terminal

- scanf("%d", value) --> take input from terminal

- sizeof(var) --> will return size of var in bytes

- strdup(string) --> will return a copy of string

#### Limits <limits.h>

- INT_MAX / INT_MIN / UINT_MIN --> will return the max size of int

#### C- type <ctype.h>

- isalpha(var) / isblank(var) --> will give wether the var is alpha or blank

- isupper / islower / isalnum / isgraph / iscntrl / isdigit

- ispunct (for any punctuation character like {} # ; < > \~ () )

- toupper / tolower --> will return corresponding upper or lower case letter if any exist

#### Booleans <stdbool.h>

- true / false --> to use true false values in program

