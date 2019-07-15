# PART Scripting Guide

The scripting language used in PART has been modeled closely after the basic syntax of C# (based in turn on C/C++), though it has been adapted to fit PART better.

## Data Types

There are four datatypes currently implemented in PART scripts.  These are Integers, Doubles, Strings, and Booleans.

**Integers**, or `int` in declarations, are signed 32-bit integers. This means that they cannot hold any fractional values (values after the decimal place are automatically `floor`ed), can hold a value anywhere between −2,147,483,648 and 2,147,483,647.

**Doubles**, or `double` in declarations, are 64-bit floating point numbers.  The computer tracks and thinks of these numbers effectively in scientific notation: reserving 11 of the bits to represent the exponent, 52 bits to represent the fractional component, and one to represent the sign.  The technical limits of the numbers that doubles can represent are about +/- 1.7 \* 10^308, where the smallest non-zero magnitude number that can be represented is about 5 \* 10^-324.

**Booleans**, or `bool` in delcarations, simply represent a value of `true` or `false`.  In principle they can be represented with only 1 bit, but because of how memory is utilized in different programming environments, they are frequently either 8 bits or 32 bits.

**Strings**, or `string` in declarations, represent text.  In PART scripts, text is written surrounded by double quotation marks, like this:  `"This is a string"`.  There are a few ways to write special characters as well.  The character `\` is known as an Escape character, because it lets you enter special characters (based on the one that follows it).  If you need to include a double-quote character in the text, you do so by escaping it like this: `"Then he said, \"I don't believe it.\""`  Similarly, new lines can be inserted with `\n\r`, and if you want a backslash in the text, then simply use `\\`.

## Comments

Comments are helpful statements written in a block of code to help readers understand what you're doing.  They are completely ignored by the program.  Comments are either written with a double slash `//` or with the block style `/* Comment goes in between */`.  When you use the double slash, the comment automatically ends at the end of the current line.  The block style starts at the `/*` continues until it reaches the `*/` marker.

## Semicolons

In PART Script, statements end with semicolons.  This is how the program knows the statement has finished.  With the semicolon omitted, you can continue a particularly long expression on the following line.

```C#
double exampleDuration = 10.0; //in milliseconds
int particularlyLongExample = Floor(3.14159 * 2.0 *
    0.001 * exampleDuration);
```

## Declarations And Assignments

Broadly speaking, you need variables to do anything meaninful in a PART script.  The simplest type of variable is a *local* one, which, as the name suggests, just lives in the script and ceases to exist whenever it is not running.  Local variables can be declared and assigned like this:

```C#
int firstIntegerVariable;
int secondTestInteger = 10;
//Note: the value used in an assignment can be an expression...
int thirdInt = firstIntegerVariable * secondTestInteger;

double anExampleDouble = 20.0;
anExampleDouble = secondTestInteger;
/*Note: The above line is valid because Integers can be implicitly converted to Doubles, as you're guaranteed not to lose any information.  The reverse, however is not true, and requires a Cast*/

string anExampleString = "Here is a simple String";

bool anExampleBoolean = false;
```

If you don't assign it a value, as with `firstIntegerVariable`, then it defaults to 0.

## Scope and Blocks

Scope is an important concept in Programming, and it's no different in PART Scripts.  When a local variable is declared, it is only available to the current Block and every Block inside of it.  Declaring a local variable at the top of a script object makes it available to the entire script, but to nothing outside.

Blocks are created with Curly Braces, `{` and `}`, and are frequently used in some of the structures we'll see soon.

```C#
int localInt;

{
    //This is perfectly valid, because localInt is available in this internal block
    int smallerScopeInt = localInt;
}

//This line would be a syntax error because smallerScopeInt is not in this scope, AND it no longer exists, besides
localInt = smallerScopeInt  //NO, BAD
```

## Global and Extern

Local variables are great, but if you can't do anything with them and they only have the values you give them, then they'll hardly be helpful.  That is where Global and Extern come into play.  When a global variable is declared, it and its value are declared for the whole running battery.  Other scripts can access globals by declaring them as well.  Similarly, when a task makes use of a variable for one of its parameters, or when a task saves one of its outputs to a variable, these live in the same pool of memory as the globals.  This is how scripts and tasks can talk to one another.

```C#
//Script 1
global int testInteger1;
global int testInteger2 = 1;
global int testInteger3 = 1;
```

Extern, on the other hand, works very similarly, but it will not create the global variable.  It merely asserts that it must already exist.

```C#
//Script 2
global int testInteger1 = 2;
extern int testInteger2;
```

If you do not declare a variable as `global` or `extern` in a script, you cannot use it, even if it exists.  This helps protect script writers against accidentally using a global when they didn't intend to, or confusing situations where it's unclear what variable or value is being used.  Thus *Script 2* above would not be able to use `testInteger3` without declaring it with either `extern` or `global`.

### Assignment in Global Declarations

It is very important to note that assignment in a global declaration (see `testInteger2` in *Script 1* above) only runs if the global was not already defined.  This behavior may be somewhat confusing, but it makes a lot of code a lot easier to write.

```C#
//This line gives us access to the global testInteger1, and sets it equal to 0 if it did not already exist
global int testInteger1 = 0;

//The following two lines gives us access to the global testInteger2, and then always sets it to 0.
global int testInteger2;
testInteger2 = 0;
```

The reason for this is that globals are generally used to make information more persistent, so without this behavior, code would have to be absolutely filled with tests of whether a global variable exists, prior to its declaration, in order to capture desired behaviors.

## If and Operators

Anyone who has done any programming is familiar with the concept of an If statement.  If some condition is true, the following block of code runs.  It is highly encouraged to always use Curly Braces for each block of an if statement, but you do not have to.

PART script offers all the comparison operators for values you might expect.

* Greater Than: `>`
* Less Than: `<`
* Greater Than Or Equal To: `>=`
* Less Than Or Equal To: `<=`
* Is Equal To: `==`
* Is Not Equal To: `!=`

Additionally, PART script offers the boolean operators you expect as well.

* And: `&&`
* Or: `||`
* Not: `!`
* Is Equal To: `==`
* Is Not Equal To: `!=`

```C#
int finalValue;
bool firstBool = false;
bool secondBool = (2 <= 3);

if (firstBool)
{
    //This code will not run
    finalValue = 1;
}
else if (3 == 5)
{
    //This code also will not run
    finalValue = 2;
}
else if (secondBool)
{
    //This code WILL run
    finalValue = 3;
}
else
{
    //This would run if all the above tests failed, but alas they did not
    finalValue = 4;
}

//The final value of finalValue is 3.
```

## Math Operators and Functions

PART Script has many common mathematical operators, which work on Doubles and Integers.

* Addition: `+`
* Subtraction: `-`
* Multiplication: `*`
* Division: `/`
* Exponentiation: `^`
* Modulo: `%`

Some further functions were included to round out the list.  Functions are invoked by writing the function name, then surrounding the argument(s) with parantheses.  As of yet, you cannot define your own functions.

* Natural Logarithm: `Ln(x)`
* Base-10 Logarithm: `Log10(x)`
* Round: `Round(x)`
* Floor: `Floor(x)`
* Ceiling: `Ceiling(x)`
* IsNaN: `IsNaN(x)`

Additionally, a few more functions simplify some statements.

* Max: `Max(x,y)`
* Min: `Min(x,y)`
* Clamp: `Clamp(value,min,max)`

There are also a few inplace operators.  These require a variable, but can make some common statements simpler.

* PlusEquals: `variable += x`
* MinusEquals: `variable -= x`
* TimesEquals: `variable *= x`
* DivideEquals: `variable /= x`
* PowerEquals: `variable ^= x`
* ModuloEquals: `variable %= x`
* PostIncrement: `variable++`
* PostDecrement: `variable--`
* PreIncrement: `++variable`
* PreDecrement: `--variable`
* AndEquals: `variable &= x`
* OrEquals: `variable |= x`

Writing `x++` is effectively the same as `x = x + 1`, or `x += 1`.

## Loops

Both **For** and **While** loops see a lot of use in programming, and here they are implemented in the C# Style.

### While Loops

While loops are the simpler case.  They consist of a Condition, an a body.  Let's begin with an example to dissect

```C#
int testInteger = 0;

while (testInteger < 20)
{
    testInteger = 2 * (testInteger + 1);
}
```

Every time the loop runs (including the first) it checks the condition (`testInteger < 20`, in this case).  If this condition evaluates to `true`, then the loop runs again.  If it evaluates to `false`, the loop ends.

The statement following the loop, a block in this case, is the loop's Body and that's what is executed on each loop.

When this loop terminates, testInteger will have a value of 30.

### For Loops

Let's begin with an example to dissect

```C#
int testInteger = 0;

for (int i = 0; i < 100; i++)
{
    testInteger = i;
    if (i == 10)
    {
        break;
    }
}
```

The for loop contains 3 components separated by semicolons.

The first statement is the *Initialization* code.  This runs once before the loop starts, and is frequently used to declare and initialize the iterating variable, `i` in this case.  Any variable declared in this initialization statement is scoped to just the for loop, so once the loop ends the variable goes out of scope and is destroyed.

The second expression is the *Condition*.  Every time the loop runs (including the first) it checks this condition.  If this condition evaluates to `true`, then the loop runs again.  If it evaluates to `false`, the loop ends.

The third statement is the Incrementer.  It runs once after every successful execution of the loop.  In simple loops it is frequently used to increment the iterating variable.

The statement following the loop, a block in this case, is the loop's Body and that's what is executed on each loop.

All together, the loop does the following: Initialization -> Condition -> Body -> Incrementer -> Condition -> Body -> Incrementer -> Condition...

Additionally, there are special `break` and `continue` keywords.

Break ends the loop immediately.  In the above case, even though the Condition would run until `i` was 100, it will end after i is equal to 10 because of the `break`.

Continue ends the current pass of the loop, jumping straight to the Incrementer step and continuing onward.

## Persistent Data

There exist a few functions to allow you to store persistent data for a user.  For the functions used to retrieve data, `key` refers to the string that the values are stored under, and the functions return a value of the named type.

* `GetInt(key)`
* `GetDouble(key)`
* `GetBool(key)`
* `GetString(key)`

And the functions used to set the data are as follows

* `SetInt(key,value)`
* `SetDouble(key,value)`
* `SetBool(key,value)`
* `SetString(key,value)`
