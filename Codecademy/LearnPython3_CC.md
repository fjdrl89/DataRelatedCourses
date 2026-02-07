# Learn Python 3

## Hello World

### Comments
- A comment is a piece of text within a program that is not executed. It can be used to provide additional information to aid in understanding the code.
- The # character is used to start a comment and it continues until the end of the line.

```python
# Comment on a single line

user = "JDoe" # Comment after code
```

### Arithmetic Operations

- Python supports different types of arithmetic operations that can be performed on literal numbers, variables, or some combination. The primary arithmetic operators are:

  * `+` for addition
  * `-` for subtraction
  * `*` for multiplication
  * `/` for division
  * `%` for modulus (returns the remainder)
  * `**` for exponentiation

```python
# Arithmetic operations

result = 10 + 30
result = 40 - 10
result = 50 * 5
result = 16 / 4
result = 25 % 2
result = 5 ** 3
```

- Python offers a companion to the division operator called the modulo. Returns the remainder after division of one number by another.
 operator.
- The modulo operator is indicated by `%` and gives the remainder of a division calculation.
- If the two numbers are divisible, then the result of the modulo operation will be `0`.

### Plus-Equals Operator `+=`

- The plus-equals operator += provides a convenient way to add a value to an existing variable and assign the new value back to the same variable. In the case where the variable and the value are strings, this operator performs string concatenation instead of addition.
- The operation is performed in-place, meaning that any other variable which points to the variable being updated will also be updated.

```python
# Plus-Equal Operator

counter = 0
counter += 10

# This is equivalent to

counter = 0
counter = counter + 10

# The operator will also perform string concatenation

message = "Part 1 of message "
message += "Part 2 of message"
```

### Variables

- A variable is used to store data that will be used by the program. This data can be a number, a string, a Boolean, a list or some other data type. Every variable has a name which can consist of letters, numbers, and the underscore character `_`.
- The equal sign `=` is used to assign a value to a variable. After the initial assignment is made, the value of a variable can be updated to new values as needed.

```python
# These are all valid variable names and assignment

user_name = "codey"
user_id = 100
verified = False

# A variable's value can be changed after assignment

points = 100
points = 120
``` 

### Modulo Operator `%`

- A modulo calculation returns the remainder of a division between the first and second number. For example:
  * The result of the expression `4 % 2` would result in the value 0, because 4 is evenly divisible by 2 leaving no remainder.
  * The result of the expression `7 % 3` would return 1, because 7 is not evenly divisible by 3, leaving a remainder of 1.

```python
# Modulo operations

zero = 8 % 4

nonzero = 12 % 5
```

### Integers

- An integer is a number that can be written without a fractional part (no decimal). An integer can be a positive number, a negative number or the number 0 so long as there is no decimal portion.
- The number 0 represents an integer value but the same number written as 0.0 would represent a floating point number.

```python
# Example integer numbers

chairs = 4
tables = 1
broken_chairs = -2
sofas = 0

# Non-integer numbers

lights = 2.5
left_overs = 0.0
```

### String Concatenation

- Python supports the joining (concatenation) of strings together using the `+` operator. The `+` operator is also used for mathematical addition operations. If the parameters passed to the `+` operator are strings, then concatenation will be performed. If the parameter passed to `+` have different types, then Python will report an error condition. Multiple variables or literal strings can be joined together using the + operator.

```python
# String concatenation

first = "Hello "
second = "World"

result = first + second

long_result = first + second + "!"
```

### Errors

- The Python interpreter will report errors present in your code. For most error cases, the interpreter will display the line of code where the error was detected and place a caret character ^ under the portion of the code where the error was detected.

```python
if False ISNOTEQUAL True:
                  ^
SyntaxError: invalid syntax
```

### ZeroDivisionError

- A `ZeroDivisionError` is reported by the Python interpreter when it detects a division operation is being performed and the denominator (bottom number) is 0.
- In mathematics, dividing a number by zero has no defined value, so Python treats this as an error condition and will report a `ZeroDivisionError` and display the line of code where the division occurred.
- This can also happen if a variable is used as the denominator and its value has been set to or changed to 0.

```python
numerator = 100
denominator = 0
bad_results = numerator / denominator

ZeroDivisionError: division by zero
```

### Strings

- A string is a sequence of characters (letters, numbers, whitespace or punctuation) enclosed by quotation marks. It can be enclosed using either the double quotation mark " or the single quotation mark '.
- If a string has to be broken into multiple lines, the backslash character \ can be used to indicate that the string continues on the next line.

```python
user = "User Full Name"
game = 'Monopoly'

longer = "This string is broken up \
over multiple lines"
```

### SyntaxError

- A `SyntaxError` is reported by the Python interpreter when some portion of the code is incorrect.
- This can include misspelled keywords, missing or too many brackets or parentheses, incorrect operators, missing or too many quotation marks, or other conditions.

```python
age = 7 + 5 = 4

# SyntaxError: can't assign to operator
```

### NameError

- A `NameError` is reported by the Python interpreter when it detects a variable that is unknown.
- This can occur when a variable is used before it has been assigned a value or if a variable name is spelled differently than the point at which it was defined.
- The Python interpreter will display the line of code where the `NameError` was detected and indicate which name it found that was not defined.

```python
misspelled_variable_name

NameError: name 'misspelled_variable_name' is not defined
```

### Numbers

- An **integer**, or int, is a whole number. It has no decimal point and contains all counting numbers (1, 2, 3, …) as well as their negative counterparts and the number 0. If you were counting the number of people in a room, the number of jellybeans in a jar, or the number of keys on a keyboard you would likely use an integer.
- A **floating-point number**, or a **float**, is a decimal number. It can be used to represent fractional quantities as well as precise measurements. If you were measuring the length of your bedroom wall, calculating the average test score of a seventh-grade class, or storing a baseball player’s batting average for the 1998 season you would likely use a float.

### Floating Point Numbers

- Python variables can be assigned different types of data.
- One supported data type is the floating point number.
- A floating point number is a value that contains a decimal portion.
- It can be used to represent numbers that have fractional quantities.
- For example, `a = 3/5` can not be represented as an integer, so the variable a is assigned a floating point value of `0.6`.

```python
# Floating point numbers

pi = 3.14159
meal_cost = 12.99
tip_percent = 0.20
```

### `print()` Function

- The `print()` function is used to output text, numbers, or other printable information to the console.
- It takes one or more arguments and will output each of the arguments to the console separated by a space.
- If no arguments are provided, the `print()` function will output a blank line.

```python
print("Hello World!")

print(100)

pi = 3.14159
print(pi)
```

### Plus Equals

- Python offers a shorthand for updating variables. 
- When you have a number saved in a variable and want to add to the current value of the variable, you can use the `+=` 
(plus-equals) operator.

```python
# First we have a variable with a number saved
number_of_miles_hiked = 12

# Then we need to update that variable
# Let's say we hike another two miles today
number_of_miles_hiked += 2

# The new value is the old value
# Plus the number after the plus-equals
print(number_of_miles_hiked)
# Prints 14
```

- The plus-equals operator also can be used for string concatenation, like so:

```python
hike_caption = "What an amazing time to walk through nature!"

# Almost forgot the hashtags!
hike_caption += " #nofilter"
hike_caption += " #blessed"
```

### Multi-line Strings

- Python strings are very flexible, but if we try to create a string that occupies multiple lines we find ourselves face-to-face with a `SyntaxError`.
- Python offers a solution: **multi-line strings**.
- By using three quote-marks (""" or ''') instead of one, we tell the program that the string doesn’t end until the next triple-quote.
- This method is useful if the string being defined contains a lot of quotation marks and we want to be sure we don’t close it prematurely.

```python
leaves_of_grass = """
Poets to come! orators, singers, musicians to come!
Not to-day is to justify me and answer what I am for,
But you, a new brood, native, athletic, continental, greater than
  before known,
Arouse! for you must justify me.
"""
```

### User Input

- While we output a variable’s value using `print()`, we assign information to a variable using `input()`.
- The `input()` function requires a prompt message, which it will print out for the user before they enter the new information.
- For example:
```python
likes_snakes = input("Do you like snakes? ")
```


### If Statement

- The Python **if statement** is used to determine the execution of code based on the evaluation of a Boolean expression.
  * If the `if` statement expression evaluates to `True`, then the indented code following the statement is executed.
  * If the expression evaluates to `False` then the indented code following the if statement is skipped and the program executes the next line of code which is indented at the same level as the if statement.

```python
test_value = 100

if test_value > 1:
  print("This code is executed!")

if test_value > 1000:
  print("This code is NOT executed!")

print("Program continues at this point.")
```

### Else Statement

- The Python `else` statement provides alternate code to execute if the expression in an `if` statement evaluates to `False`.
- The indented code for the `if` statement is executed if the expression evaluates to `True`. The indented code immediately following the else is executed if and only if the expression evaluates to `False`.
- To mark the end of the else block, the code must be unindented to the same level as the starting if line.

```python
test_grade = 61

if test_grade > 60:
  print("You passed.")
else:
  print("You failed.")

# Output: You passed.
```

### Elif Statement

- The Python `elif` statement allows for continued checks to be performed after an initial `if` statement. An `elif` statement differs from the else statement because another expression is provided to be checked, just as with the initial `if` statement.
- If the expression is `True`, the indented code following the `elif` is executed. If the expression evaluates to `False`, the code can continue to an optional `else` statement.
- Multiple `elif` statements can be used following an initial `if` to perform a series of checks. Once an `elif` expression evaluates to `True`, no further `elif` statements are executed.

```python
pet_type = "fish"

if pet_type == "dog":
  print("You have a dog.")
elif pet_type == "cat":
  print("You have a cat.")
elif pet_type == "fish":
  # This is performed
  print("You have a fish")
else:
  print("Not sure!")
```

## Control Flow 

- A program will run (wake up) and start moving through its checklists, is this condition met, is that condition met, okay let’s execute this code and return that value.
- This is the control flow of your program. In Python, your script will execute from the top down, until there is nothing left to run.
- It is your job to include gateways, known as **conditional statements**, to tell the computer when it should execute certain blocks of code. If these conditions are met, then run this function.

### Syntax Error

- A `SyntaxError` is reported by the Python interpreter when some portion of the code is incorrect.
- This can include misspelled keywords, missing or too many brackets or parentheses, incorrect operators, missing or too many quotation marks, or other conditions.

```python
age = 7 + 5 = 4

# SyntaxError: can't assign to operator
```

### TypeError

- A `TypeError` in Python occurs when an operation is attempted on a data type that does not support it.
- This typically happens when functions or operators are used incorrectly with incompatible types.

```python
result = "hello" + 5

# TypeError: can only concatenate str (not "int") to str
```

### NameError 

- In Python, a `NameError` is an exception that is raised when referencing a variable that is not found in the current namespace.

```python
str = "Hello World"

print(x)

# This will throw a NameError since x is not defined.
```

### Errors

- Errors are mistakes within a Python program.
- When programs throw errors that we didn’t expect to encounter, we call those errors **bugs**.
- Programmers call the process of updating the program so that it no longer produces bugs **debugging**.

### `elif` Statement

- The Python `elif` statement allows for continued checks to be performed after an initial if statement.
- An `elif` statement differs from the `else` statement because another expression is provided to be checked, just as with the initial if statement.
  * If the expression is `True`, the indented code following the `elif` is executed.
  * If the expression evaluates to `False`, the code can continue to an optional `else` statement.
- Multiple `elif` statements can be used following an initial if to perform a series of checks.
- Once an `elif` expression evaluates to `True`, no further `elif` statements are executed.

```python
# elif Statement

pet_type = "fish"

if pet_type == "dog":
  print("You have a dog.")
elif pet_type == "cat":
  print("You have a cat.")
elif pet_type == "fish":
  # this is performed
  print("You have a fish")
else:
  print("Not sure!")
```

### `or` Operator

- The Python `or` operator combines two Boolean expressions and evaluates to `True` if at least one of the expressions returns `True`.
- Otherwise, if both expressions are `False`, then the entire expression evaluates to `False`.

```python
True or True      # Evaluates to True
True or False     # Evaluates to True
False or False    # Evaluates to False
1 < 2 or 3 < 1    # Evaluates to True
3 < 1 or 1 > 6    # Evaluates to False
1 == 1 or 1 < 2   # Evaluates to True
```

### Equal Operator `==`

- The equal operator, `==`, is used to compare two values, variables or expressions to determine if they are the same.
- If the values being compared are the same, the operator returns `True`, otherwise it returns `False`.
- The operator takes the data type into account when making the comparison, so a string value of "2" is not considered the same as a numeric value of 2.

```python
# Equal operator

if 'Yes' == 'Yes':
  # evaluates to True
  print('They are equal')

if (2 > 1) == (5 < 10):
  # evaluates to True
  print('Both expressions give the same result')

c = '2'
d = 2

if c == d:
  print('They are equal')
else:
  print('They are not equal')
```

### Not Equals Operator `!=`

- The Python `not equals` operator, `!=`, is used to compare two values, variables or expressions to determine if they are NOT the same.
- If they are NOT the same, the operator returns `True`. If they are the same, then it returns `False`.
- The operator takes the data type into account when making the comparison so a value of 10 would NOT be equal to the string value "10" and the operator would return `True`.
- If expressions are used, then they are evaluated to a value of `True` or `False` before the comparison is made by the operator.

```python
# Not Equals Operator

if "Yes" != "No":
  # evaluates to True
  print("They are NOT equal")

val1 = 10
val2 = 20

if val1 != val2:
  print("They are NOT equal")

if (10 > 1) != (10 > 1000):
  # True != False
  print("They are NOT equal")
```

### Comparison Operators

- In Python, relational operators compare two values or expressions. The most common ones are:

  * `<` less than
  * `>` greater than
  * `<=` less than or equal to
  * `>=` greater than or equal too

- If the relation is sound, then the entire expression will evaluate to `True`. If not, the expression evaluates to `False`.

```python
a = 2
b = 3
a < b  # evaluates to True
a > b  # evaluates to False
a >= b # evaluates to False
a <= b # evaluates to True
a <= a # evaluates to True
```

### `if` Statement

- The Python `if` statement is used to determine the execution of code based on the evaluation of a Boolean expression.
  * If the `if` statement expression evaluates to `True`, then the indented code following the statement is executed.
  * If the expression evaluates to `False` then the indented code following the `if` statement is skipped and the program executes the next line of code which is indented at the same level as the if statement.

```python
# if Statement

test_value = 100

if test_value > 1:
  # Expression evaluates to True
  print("This code is executed!")

if test_value > 1000:
  # Expression evaluates to False
  print("This code is NOT executed!")

print("Program continues at this point.")
```

### `else` Statement

- The Python `else` statement provides alternate code to execute if the expression in an `if` statement evaluates to `False`.
- The indented code for the `if` statement is executed if the expression evaluates to `True`.
- The indented code immediately following the `else` is executed only if the expression evaluates to `False`.
- To mark the end of the `else` block, the code must be unindented to the same level as the starting `if` line.

```python
# else Statement

test_value = 50

if test_value < 1:
  print("Value is < 1")
else:
  print("Value is >= 1")

test_string = "VALID"

if test_string == "NOT_VALID":
  print("String equals NOT_VALID")
else:
  print("String equals something else!")
```

### `and` Operator

- The Python `and` operator performs a Boolean comparison between two Boolean values, variables, or expressions.
- If both sides of the operator evaluate to `True` then the `and` operator returns `True`.
- If either side (or both sides) evaluates to `False`, then the `and` operator returns `False`.
- A non-Boolean value (or variable that stores a value) will always evaluate to `True` when used with the and operator.

```python
True and True     # Evaluates to True
True and False    # Evaluates to False
False and False   # Evaluates to False
1 == 1 and 1 < 2  # Evaluates to True
1 < 2 and 3 < 1   # Evaluates to False
"Yes" and 100     # Evaluates to True
```

### Boolean Values

- Booleans are a data type in Python, much like integers, floats, and strings. However, booleans only have two values:
  * `True`
  * `False`
 
- Specifically, these two values are of the bool type. Since booleans are a data type, creating a variable that holds a boolean value is the same as with other data types.

```python
is_true = True
is_false = False

print(type(is_true)) 
# will output: <class 'bool'>
```

### `not` Operator

- The Python Boolean `not` operator is used in a Boolean expression in order to evaluate the expression to its inverse value.
- If the original expression was `True`, including the `not` operator would make the expression `False`, and vice versa.

```python
not True     # Evaluates to False
not False    # Evaluates to True
1 > 2        # Evaluates to False
not 1 > 2    # Evaluates to True
1 == 1       # Evaluates to True
not 1 == 1   # Evaluates to False
```

### Else Statements

* An `else` block lets you define what should happen when an `if` condition evaluates to `False`.
* `else` must come immediately after an `if` (or after the last `elif`) and uses the same indentation level as the matching `if`.
* Using `else` avoids writing multiple separate `if` statements for “all other cases” and keeps control flow cleaner.
* Only one of the blocks runs: either the `if` block (when the condition is `True`) or the `else` block (when it is `False`).

Generic pattern

```python
if condition:
  # runs when condition is True
  ...
else:
  # runs when condition is False
  ...
```

Example

```python
age = 12

if age >= 13:
  msg = "Access granted."
else:
  msg = "Sorry, you must be 13 or older to watch this movie."

msg
#> 'Sorry, you must be 13 or older to watch this movie.'
```

Gotcha

* `else` cannot stand alone: it always needs a matching `if` (and correct indentation), otherwise you’ll get a `SyntaxError`.

Micro-exercise: set `age = 13` and confirm the message becomes `"Access granted."`

<details><summary>Solution</summary>  

```python
age = 13

if age >= 13:
  msg = "Access granted."
else:
  msg = "Sorry, you must be 13 or older to watch this movie."

msg
#> 'Access granted.'
```





## Lists



## Loops



## Functions



## Python: Code Challenges




## Strings



## Modules



## Dictionaries 




## Files



## Classes 




## Python: Code Challenges 2




## Next Steps





























