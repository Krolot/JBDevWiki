# How to use Squirrel

The squirrel language uses .nut files, to create a .nut file you just simply right click on wherever you want you file in, click on create text file and rename it to yourname.nut, then you can use any text editor you like to modify it, notepad, notepad++, vscode and others.

## Start simple

One of the simplest things you can do while coding is a comment, a comment is just text that will be ignored by the game, just to document what you are doing. Comments are written like this:

```squirrel
//Hello World!
```
This is your first line of code, this will help you remember what you did before, like a notebook.

## Variables

Variables are very valuable for coding, they are specially useful when you want to change any value, because you can just change the variable value instead of changing it in every place you need.

### Local Variables

Local variables are variables that only exist where they are defined, and they will no longer exist when the block of code where they are stops being used.

 This is how you define a local variable:
 
```squirrel
local x = 10;
```
This simple variable called "x" has the value of ten.

```squirrel
function func() {
    local x = 10;
}
```
In this case the variable will only work INSIDE the function.

### Global Variables

```squirrel
global_y <- 20;
```
This variable is called global_y, the "<-" operator is used to create or assign a variable in the current scope.

```squirrel
global_y <- 20;

function func1() {
    global_y = global_y + 1;
}

function func2() {
    global_y = global_y + 2;
}
```
Global variables keep their value between function calls, so each function modifies the same shared state. When you call func1 global_y will change to 21, then if you call func2, global_y will change to 23, thats because the value of global_y its maintained through the script.

## Functions

Functions are the main way of executing code, essentially they are sections of the code that can be called at will, once they are called they get executed.

```squirrel
function MyFunction() {
    print("Hello World!");
}
```
This is the syntax of a function, this function does nothing until it is called.

```squirrel
MyFunction();
```
Once you put this somewhere in your code where it can be executed it will work. Then in your console you will get the message "Hello World!". Thats how you call a function.

### Functions with parameters

You can also add parameters to your functions, these are like "inputs".

```squirrel
function Add(a, b) {
    return a + b;
}
```
This is a very basic function that adds the values of both parameters

```squirrel
local result = Add(5, 3);
```
This is how you use it. a equals to 5 and b equals to 3, the final result will be 8.

### Return

This is VERY important, as you saw in both earlier examples theres this special word on the function "return" this is the value that the function will return. Not only that but this helps us to stop the function when something happens.

```squirrel
function Multiply(a, b) {
    return a * b;
}
```
As you can see this returns the multiplication of a and b.

```squirrel
local x = 10;

function Example(a, b) {
	if (x == 10)
		return
	
	return a + b
}
```
This function wont return any value, because we put the condition that if the variable x equals 10 dont return anything, if x doesnt equal ten then continue. If it continues then it will return a plus b.

## Conditionals

This might be the MOST important thing to learn, conditionals are conditions that you tell the script to check, can be used in a lot of ways, from checking values, making sure something is true or false, or simply stopping a function to happen unless the right condition is met, like the last example.

### If

If is the most basic conditional of all, and its the most useful.

```squirrel
if (x == 10) {
    print("x is 10");
}
```
As you can see here, we check if x equals 10, if it does, then it prints "x is 10"

### Else

Else always comes with an if, there cannot be an else without an if. Its usually used so if the condition isnt met, the script does the next thing, whatever is inside else.

```squirrel
if (x == 10) {
    print("x is 10");
}
else {
    print("x is not 10");
}
```
This is very similar to the last example, in this case if x doesnt equal ten, then we go to the else, and print "x is not 10".

### Else-If

As you can imagine, an else-if is just an else with a condition.

```squirrel
if (x == 10) {
    print("ten");
}
else if (x == 20) {
    print("twenty");
}
else {
    print("other");
}
```
This is almost the same as the two last examples, unlike those, in this one the else if works as a second condition. If x equals 10 then print "ten", if x doesnt equal ten, but it equals 20 then print "twenty", if both conditions arent met then print "other".

## "=" VS "=="

You might think that "=" and "==" are the same thing, but thats not true.

We use "=" to tell the script to set a certain value to another.

```squirrel
local x = 10;
```
As you can see in the first example of local variables, this says that x IS ten, if you put "x = 20", then x becomes 20.


But you probably noticed that in conditionals like if and else if we use instead "=="
```squirrel
if (x == 10) {
    print("x is 10");
}
```
This is because we tell the script that we are looking if x EQUALS 10, if "x == 10" then we print the message.

Make sure to follow this rule, in vscript (C-Like languages) its essential.

## Optional Braces

You might also seen that we close functions, ifs, elses, and others with "{}" braces. Sometimes they are optional.

This is how our code looks with braces:
```squirrel
if (x == 10) {
    print("x is 10");
}
```
and this is how it looks without them:
```squirrel
if (x == 10)
    print("x is 10");
```
Both of these codes do the same, but we need to know why. When you have a condition without braces ONLY the next line counts.
So this:
```squirrel
if (x == 10)
    print("A");
    print("B");
```
Actually behaves like this:
```squirrel
if (x == 10)
    print("A");

print("B");
```
So B ALWAYS runs, if you want your code to be more readable you can put braces even if the code block only has one line. Or you can just dont use them, if you want a cleaner code, your choice.

## Logical Operators

These are very useful for combining conditions in different ways.

### AND

AND is used to check if condition 1 and condition 2 are both true, if any of them is false then the conditions arent met. It uses the operator "&&".
```squirrel
if (x == 10 && y == 20)
{
    print("both are correct");
}
```
This will print only if x equals ten AND y equals twenty.

### OR

OR is used to check if condition 1 is true OR condition 2 is true, if only one, or both are true then the conditions are met, if none then the conditions arent met. It uses the operator "||".
```squirrel
if (x == 10 || y == 20)
{
    print("at least one is correct");
}
```
This will print only if either x equals 10 or y equals 20.

### Comparison

These are very simple, these check if something is more or less than something else.

```squirrel
if (x < 50)
```
This checks if x is less than 50.

```squirrel
if (x > 50)
```
This checks if x is more than 50.

We also have "<=" less or equal and ">=" more or equal that are used in very similar ways.

```squirrel
if (x >= 50)
if (x <= 50)
```
This checks if x is equal or more than 50 and viceversa.

## Math Operators

Squirrel supports basic math operators.

```squirrel
local a = 10 + 5; // 15
local b = 10 - 5; // 5
local c = 10 * 5; // 50
local d = 10 / 5; // 2
```
The first one is for addition, the second is for subtraction, the third one is for multiplication and the last one is for division.

## Null

Null means "nothing" or "no value".

```squirrel
local x = null;
```

This variable exists, but it does not point to anything yet.

```squirrel
if (x == null)
{
    print("x doesnt have a value");
}
```

## While/For

In JBMod VScript, While and For loops should generally be avoided. They can easily freeze the server, break gameplay and cause severe performance issues if used incorrectly.
JBMod provides safer alternatives for repeating code, which will be covered in the next page.
## Extras

### What is print?

If you are not sure what print means its very simple, when you want to test if something works you can use the print() function, this function reads from whatever you add to it and puts it into console.
```squirrel
print("Hello World!")
```
This will just print the sentence "Hello World!" in the console.

### Data Types

These are some of the most common data types you will encounter while scripting.

```squirrel
local number = 10;
local decimal = 5.5;
local text = "Hello";
local state = true;
```

each one of them corresponds to these

- Integer (10)
- Float (5.5)
- String ("Hello")
- Boolean (true/false) This one ONLY can be true/false.

You must know this if you want to make sure that the code you are making works.

## Final Script

```squirrel
// This is a comment

local x = 10;          // Local variable
global_y <- 20;        // Global variable

function Add(a, b)
{
    return a + b;
}

local result = Add(x, global_y);

if (result > 20)
{
    print("Result is greater than 20");
}
else if (result == 20)
{
    print("Result is exactly 20");
}
else
{
    print("Result is less than 20");
}

if (x == 10 && global_y == 20)
{
    print("Both conditions are true");
}

if (x == 10 || global_y == 0)
{
    print("At least one condition is true");
}
```

This is a very simple script that shows a lot of the concepts that we have learned. Time to get into the game and try to code some cool things!

## Learning Resources

- [Official Squirrel Documentation](http://www.squirrel-lang.org/doc/squirrel3.html)
- [Valve Developer Community: VScript](https://developer.valvesoftware.com/wiki/VScript)
