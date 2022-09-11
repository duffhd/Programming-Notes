# Python Cheatsheet

##### Table of Contents

- [Variables](#variables)  
- [Types](#types)
- [Operators](#operators)
- [If, elif, else](#if-elif-else)
- [Data Structures](#data-structures)
- [Loops](#loops)
- [Functions](#functions)
- [Classes](#classes)
- [Indentation](#indentation)
- [Naming Conventions](#naming-conventions)
- [Comments](#comments)
- [Error handling](#error-handling)
- [Definition skipping](#definition-skipping)
- [Type casting](#type-casting)

## Variables
Python can infer variable types, but you can optionally static type anything on the language.
```python
name = "Your Name"
number = 0

# Static typing
name: str = "Your Name"
number: int = 0
```

### Constants

Constants work the same as normal variables and are only syntactical sugar.
UPPER_CASE IS ALWAYS USED.

```python
NAME = "Duck"
LONG_NAME = "Quack Duck"
```

## Types

| Name | Static typing | Example |
| --- | --- | --- |
| String | str | "alex" |
| Int | int | 10 |
| Float | float | 10.0 |
| Boolean | bool | True, False |
| Tuple | tuple[] | ("hello", 10) |
| List | list[] | [1, 2, 3, 4] |
| Dictionary | dict[] | {name: "alex", hobby: "programming"} |

### Strings

```python
# Strings can be declared with '', or ""
name = "alex"
name = 'alex'
```

#### String concat

```python
first_name = "alex "
last_name = "parra"

full_name = first_name + last_name
```

#### f string
This is the most used and recommended method to put variables inside strings.
```python
guest = "alex"
print(f"Hello {guest}!")
```

## Operators

### Comparison

| Symbol | Reading |
| --- | --- |
| ==  | equals |
| > | greater then |
| < | lesser then |
| >= | greater or equal then |
| <= | lesser or equal then |

### Logical

| and | or |
| --- | --- |

## If, elif, else

```python
number = 5

if number == 0:
    print("zero")
elif number > 0 and number <= 10:
    print("between 1 and 10")
else:
    print("bigger than 10")
```

## Data structures

### Tuple

Tuples are fixed size and can have multiple-types.

```python
my_tuple = (1, "name")

# Static typing
my_tuple: tuple[int, str] = (1, "name")
```

### List
List’s are collections of values from the same type.

```python
empty_list = []
int_list = [1, 2, 3, 4]

# Access by index
# Lists always start at index 0
int_list[0] # 1

# Access by index range
int_list[0:2] # 1, 2, 3

# Access the last element
int_list[-1] # 4

# Add value to the end of the list
int_list.append(5)

# Static typing
int_list: list[int] = [1, 2, 3, 4]
```

### Sets
Sets are collections unordered collection of unique items (there is no duplicates), values inside the set are immutable, but you can remove
or add new values.
```python
# Create a set from values
my_set = {1, 2, 3}

# Create a set from a list
set_from_list = set([1, 2, 3, 3, 4])
# This set will only contain the values 1, 2, 3, 4 because a set
# cannot contain duplicate values.
```

### Dictionary

Dictionaries are a `key: value` pair data type.

```python
person = {"one": 1, "two": 2} 

# Access value by key
person["one"] # 1

# Add new key to the dict
person["three"] = 3

# Reassign the value of an existing key
person["one"] = 11

# Static typing
person: dict[int, str]
```

## Loops

### For

```python
name_list = ["jean", "alice", "herbert"]

for name in name_list:
    print(f"hello {name}!")

# "jean"
# "alice"
# "herbert"
```

### While

```python
flag = True

while flag:
    flag = False
```

### List/Dictionary comprehension
Comprehensions are concise expressions to generate lists or dictionaries.
```python
# Multiply every number in a given range and put the result inside the array
lc = [x * 2 for x in range(10)]

dc = {num: num*num for num in range(1, 11)}

# lc = [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
# dc = {1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81, 10: 100}
```

## Functions
Python always needs indentation to be 4 spaces between function declarations 
and its body (the same apply for loops, classes and other constructs),
not doing so will result in errors. See [Indentation](#indentation).
```python
def add(n1, n2):
    return n1 + n2

# Static typing
# parameter types come after the name with a :, the function return type comes after the parameters
# and after the arrow ->
def add(n1: int, n2: int) -> int:
    return n1 + n2
```

## Classes
In Python classes methods need to receive the self parameter, meaning that they can be accessed on instantiated classes.
Classes parameters are also accessed via self.
```python
class Person:
    def __init__(self) -> None:
        # The self keyword denotes that the variable 
        # can be accessed by the entire class methods.
        self.global_value = 10

	# Methods always need to receive self
    def method(self, value: int) -> None:
        print("i'm a method")

me = Person()
me.method()

# Output: i'm a method
				
```

### Class parameters

```python
class Dog:
    # Receive parameters after self
    def __init__(self, age: int, name: str) -> None:
		 self.age = age
		 self.name = name

    def bark(self) -> None:
        print(f"{self.name} is barking")

my_dog = Dog(10, "doug")
my_dog.bark()

# Output: doug is barking
```
## Indentation
Python uses levels of indentation to differ statements, as seen above, the level of indentation is denoted by a `:`
Always indent using 4 spaces.
```python
# Not putting the correct indentation can leave messy problems
def no_indentation:
print("fail")

# Or

for i in 1..10:
print(i) # i is not present in this scope because is not in the correct indentation
```

## Naming conventions

```python
# Variables are always snake_case
name = "alex"
middle_name = "parra"
int_list = [1, 2, 3, 4]

# Constants are always UPPER_SNAKE_CASE
FIRST_NAME = "alex"

# Functions follow the same principal for varibles
def slice_name(name: str) -> str:
    ...

# Classes follow the UpperCamelCase
class Person
class TallPerson
```

## Comments

```python
# Comments start with # and are not stopped if # appears
# Multiple lines always require #
```

### Docstring

```python
# Docstrings are explanations of what classes and functions do
def say_hi(name: str) -> None:
    """Receive a string and greet it!"""
    print(f"hello {name}!")

class Person:
    """Describes a real person"""
    # They are often used by IDE's to provide fast feedback on their usage
```

## Error handling

```python
# When you try to do something that might fail on run-time
try:
    crash = 10 / 0

except ZeroDivisionError:
    # handle the error here
    print("You can't devide by 0")

finally:
    # Only executed after the try/except
    # finally is totally optional
```

### Chain except

```python
try:
    crash = 10 / 0

except ZeroDivisionError:
    # handle the error here
    print("You can't devide by 0")

except TypeError:
		...
except ValueError:
		...
```

### raise

```python
# if for some reason you want to throw an exception yourself:
if number > 10:
    raise Exception("Number cannot be bigger than 10")
```

## Definition skipping
Sometimes it can be useful to skip a method or function definition.
```python
def to_implement(n1: int, name: str) -> bool:
    ...

class Skip:
    ...
```

You can change the use of three dots to the pass keyword.
```python
def to_implement(n1: int, name: str) -> bool:
    pass
```

## Type casting

```python
# Transform a value to another, be careful, it may lead to runtime exceptions.
# Just wrap the value you want with the name of the type, float(), int(), str()
integer = 10
string_int = str(integer)

calculation = int(10.0 + 5.5 / 15)
```
