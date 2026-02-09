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

* The Python `else` statement allows us to define what code should run when the condition in an `if` statement evaluates to `False`.
* An `else` statement must always appear together with an `if` statement and cannot exist on its own.
* Using `else` helps keep code clean and readable by handling all cases where the `if` condition is not met without writing multiple separate `if` statements.
* Only one block is executed: either the `if` block (when the condition is `True`) or the `else` block (when it is `False`).

```python
# else Statement

weekday = False

if weekday:
  print("wake up at 6:30")
else:
  print("sleep in")
```

* In this example, the `if` block runs only if `weekday` is `True`.

* If `weekday` is `False`, the code inside the `else` block is executed instead.

* `else` statements are especially useful when we want to define a default action for all situations where a condition is not satisfied.

```python
age = 12

if age >= 13:
  print("Access granted.")
else:
  print("Sorry, you must be 13 or older to watch this movie.")
```

### Else If Statements (`elif`)

* In Python, an `elif` statement is short for **“else if”**.
* An `elif` statement checks an additional condition **only if** the previous `if` (or `elif`) condition evaluated to `False`.
* `elif` statements allow us to control the **order** in which conditions are evaluated.
* Python evaluates conditional statements **from top to bottom**:
  1. The `if` condition is checked first.
  2. Each `elif` condition is checked in order.
  3. The `else` block runs only if none of the previous conditions are met.
* As soon as one condition evaluates to `True`, its block is executed and the rest are skipped.

```python
# if / elif / else structure

print("Thank you for the donation!")

if donation >= 1000:
  print("You've achieved platinum status")
elif donation >= 500:
  print("You've achieved gold donor status")
elif donation >= 100:
  print("You've achieved silver donor status")
else:
  print("You've achieved bronze donor status")
```

* Even if multiple conditions are technically `True`, **only the first matching block** is executed.
* This prevents multiple messages from being printed for a single input value.
* Using `elif` is preferable to multiple independent `if` statements when conditions are mutually exclusive.

```python
# Example: letter grades

grade = 85

if grade >= 90:
  print("A")
elif grade >= 80:
  print("B")
elif grade >= 70:
  print("C")
elif grade >= 60:
  print("D")
else:
  print("F")
```

* This pattern is commonly used for categorization tasks such as grading, pricing tiers, access levels, or status labels.
* The order of conditions matters: more restrictive conditions should appear before broader ones.

### Review

* In this lesson, we expanded our understanding of **control flow** and how Python makes decisions based on conditions.

* A **Boolean expression** is an expression that evaluates to either `True` or `False`.

* A **Boolean variable** stores one of these two values: `True` or `False`.

* Boolean expressions are commonly built using **comparison (relational) operators**:

  * `==` equals
  * `!=` not equals
  * `>` greater than
  * `>=` greater than or equal to
  * `<` less than
  * `<=` less than or equal to

* `if` statements allow us to control which blocks of code are executed based on whether a condition evaluates to `True`.

* `else` statements define what code should run when the corresponding `if` condition is not met.

* `elif` statements allow us to check multiple conditions in sequence, stopping as soon as one condition evaluates to `True`.

* Together, `if`, `elif`, and `else` statements let us write clear, readable programs that respond differently depending on input, state, or user behavior.

* These tools form the foundation for building logic in Python programs, such as decision-making, categorization, and validation.

## Match Statements in Python (match case)

### Overview

* **Match statements** (introduced in **Python 3.10**) provide a structured way to control program flow based on the value or structure of an expression.
* They are Python’s answer to **switch statements** found in other languages, but with **extra power** through *structural pattern matching*.
* Conceptually, they often replace long `if`–`elif`–`else` chains when logic depends on many discrete cases.

---

### What Is a Match (Switch) Statement?

* A match statement evaluates **one expression** and compares it against multiple possible **patterns**.
* When a matching case is found:

  * Its block executes.
  * Control immediately exits the `match` block.
* Only **one case** runs per match statement.

Equivalent logic using `if`–`elif`:

```python
user_name = "Dave"

if user_name == "Dave":
    print("Get off my computer Dave!")
elif user_name == "angela_catlady_87":
    print("I know it is you, Dave! Go away!")
elif user_name == "Codecademy":
    print("Access Granted.")
else:
    print("Username not recognized.")
```

The same logic using `match`:

```python
user_name = "Dave"

match user_name:
    case "Dave":
        print("Get off my computer Dave!")
    case "angela_catlady_87":
        print("I know it is you, Dave! Go away!")
    case "Codecademy":
        print("Access Granted.")
    case _:
        print("Username not recognized.")
```

---

#### Syntax of a Match Statement

General form:

```python
match expression:
    case pattern_1:
        # code for pattern_1
    case pattern_2:
        # code for pattern_2
    case pattern_N:
        # code for pattern_N
    case _:
        # default case
```

Key components:

* **`match`** starts the statement.
* **`expression`** is evaluated once.
* **`case`** defines a pattern to compare against the expression.
* **`_`** (underscore) is the default case (equivalent to `else`).

---

#### Important Rules and Behavior

* `expression` can be:

  * A variable
  * A literal
  * A Boolean expression
  * A Python object (string, tuple, list, etc.)
* Matching is **top-down**: cases are checked in order.
* Once a case matches, no other cases are evaluated.
* There is **no fall-through** behavior (unlike C-style switch).
* The default case (`case _:`) is optional but strongly recommended.

---

#### Match vs If–Elif–Else

**Use `if`–`elif` when:**

* Conditions involve ranges (`x > 10`, `x < 0`)
* Logic depends on complex Boolean expressions
* Only a few branches exist

**Use `match` when:**

* One variable or expression has **many discrete values**
* Logic reads better as “value → action”
* You want to leverage **structural pattern matching**

---

#### Beyond Simple Equality

Unlike traditional switch statements, `match` supports:

* Tuple and list matching
* Nested structures
* Destructuring (unpacking values)
* Guards (`if` conditions inside cases)

Example (preview only):

```python
match point:
    case (0, 0):
        print("Origin")
    case (x, 0):
        print("On X-axis")
    case (0, y):
        print("On Y-axis")
    case _:
        print("Somewhere else")
```

---

#### When to Use Match Statements

* To replace long and repetitive `if`–`elif` chains.
* When readability matters and logic is value-based.
* When working with structured data (tuples, lists, objects).
* When future extensibility is important.

---

#### Key Takeaways

* `match`–`case` is available **only in Python 3.10+**.
* It improves clarity for multi-branch decision logic.
* It executes exactly **one matching case**.
* It is more powerful than classic switch statements due to pattern matching.
* Prefer `match` for clean, declarative control flow when checking many possible values.

---
--- 

### Project 1 – Magic 8-Ball (Python)

#### Project Goal

Build a simple Python program that simulates a **Magic 8-Ball**: the user asks a Yes/No question, and the program returns a random fortune each time it runs.
This project practices **variables**, **random numbers**, **control flow**, and **basic input/output formatting**.

---

#### Task 1 – Setting up the core variables

We start by defining the basic information needed for the program.

##### What we do

* Store the name of the person asking the question.
* Store the question itself.
* Prepare a variable to hold the Magic 8-Ball’s answer.

##### Code

```python
name = "Joe"
question = "Will I win the lottery?"
answer = ""
```

##### Tips

* `name` and `question` are strings.
* Initializing `answer` as an empty string is common when its value will be assigned later.
* Avoid calling the variable `input`, since that shadows a built-in Python function.

---

#### Task 2 – Importing randomness

To make the answer different every time, we need randomness.

##### What we do

* Import Python’s built-in `random` module.

##### Code

```python
import random
```

##### Reminder

* Imports should always go **at the top of the file**.
* `random` is part of the Python standard library, so no installation is needed.

---

#### Task 3 – Generating a random number

The Magic 8-Ball has 9 possible answers. We represent each one with a number from 1 to 9.

##### What we do

* Generate a random integer between 1 and 9 (inclusive).
* Store it in a variable.

##### Code

```python
random_number = random.randint(1, 9)
```

##### Debugging tip

During development, it helps to check the output:

```python
print(random_number)
```

Once verified, comment it out:

```python
# print(random_number)
```

---

#### Task 4 – Control flow with if / elif / else

Now we map each random number to a specific Magic 8-Ball response.

##### What we do

* Use `if`, `elif`, and `else` statements.
* Assign a different answer for each value from 1 to 9.
* Add a defensive `else` case for unexpected values.

##### Code

```python
if random_number == 1:
    answer = "Yes - definitely"
elif random_number == 2:
    answer = "It is decidedly so"
elif random_number == 3:
    answer = "Without a doubt"
elif random_number == 4:
    answer = "Reply hazy, try again"
elif random_number == 5:
    answer = "Ask again later"
elif random_number == 6:
    answer = "Better not tell you now"
elif random_number == 7:
    answer = "My sources say no"
elif random_number == 8:
    answer = "Outlook not so good"
elif random_number == 9:
    answer = "Very doubtful"
else:
    answer = "Error"
```

##### Key points

* Only **one branch** runs.
* The `else` block is a safety net.
* This is a classic use case for multi-branch control flow.

---

#### Task 5 – Printing the result

Now we display the output in the required format.

##### What we do

* Print the user’s name and question.
* Print the Magic 8-Ball’s answer.

##### Code

```python
print(f"{name} asks: {question}")
print(f"Magic 8-Ball's answer: {answer}")
```

##### Output example

```
Joe asks: Will I win the lottery?
Magic 8-Ball's answer: My sources say no
```

---

#### Optional Challenge 1 – Handling an empty name

If the name is an empty string, the output formatting should change.

##### What we do

* Check whether `name` is empty.
* Adjust the printed message accordingly.

##### Code

```python
if name == "":
    print(f"Question: {question}")
else:
    print(f"{name} asks: {question}")

print(f"Magic 8-Ball's answer: {answer}")
```

##### Why this matters

* Improves user experience.
* Demonstrates conditional logic based on strings.

---

#### Optional Challenge 2 – Handling an empty question

If no question is asked, the Magic 8-Ball should refuse to answer.

##### What we do

* Check if `question` is empty.
* Print a warning instead of generating a fortune.

##### Code

```python
if question == "":
    print("You must ask a question for the Magic 8-Ball to work!")
else:
    if name == "":
        print(f"Question: {question}")
    else:
        print(f"{name} asks: {question}")

    print(f"Magic 8-Ball's answer: {answer}")
```

##### Reminder

* Always validate inputs before producing output.
* Nested `if` statements are fine when logic depends on multiple conditions.

---

#### Optional Challenge 3 – Adding more answers

To extend the Magic 8-Ball:

* Increase the range in `randint()`.
* Add more `elif` branches.
* Keep answers clearly mapped to numbers.

Example:

```python
random_number = random.randint(1, 11)
```

---

#### Key Takeaways

* This project combines **variables**, **randomness**, **conditionals**, and **formatted output**.
* `random.randint()` is ideal for discrete random choices.
* `if` / `elif` chains are clear when mapping numbers to actions.
* Input validation (empty strings) makes programs more robust.
* This logic can later be simplified using lists or `match` statements once those concepts are learned.

This is a strong foundational project and a perfect stepping stone toward more interactive programs.


---

### Project 2: Sal's Shipping



---
---




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





























