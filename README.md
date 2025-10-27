# Python Docs & Notes <!-- omit from toc -->

This document is a reference guide for Python programming. It is a bit more than a simple cheat sheet, but it is not a learning book, you need to have some knowledge and experience with Python programming to understand these notes.


## Table of contents <!-- omit from toc -->

- [Install](#install)
  - [Check](#check)
  - [Linux](#linux)
  - [macOS](#macos)
  - [Windows](#windows)
- [Shebang line](#shebang-line)
- [Syntax](#syntax)
- [Variables, immutability and mutability](#variables-immutability-and-mutability)
- [Operators](#operators)
  - [Overview](#overview)
  - [Division](#division)
  - [Chaining](#chaining)
  - [Membership](#membership)
  - [Unpacking/packing](#unpackingpacking)
  - [Slicing](#slicing)
  - [For mutable types `a += b` is not the same as `a = a + b`](#for-mutable-types-a--b-is-not-the-same-as-a--a--b)
- [len, min, max](#len-min-max)
- [print, input](#print-input)
- [Basic types](#basic-types)
  - [NoneType](#nonetype)
  - [Integers](#integers)
  - [Floating-point numbers](#floating-point-numbers)
  - [Complex numbers](#complex-numbers)
  - [Booleans](#booleans)
  - [Strings](#strings)
    - [Defining](#defining)
    - [Formatting](#formatting)
    - [Manipulation](#manipulation)
    - [Regular expression](#regular-expression)
  - [Bytes and bytearray](#bytes-and-bytearray)
- [Collections](#collections)
  - [List (ordered sequence of objects)](#list-ordered-sequence-of-objects)
  - [Tuple (immutable list)](#tuple-immutable-list)
  - [Set (unordered collection of unique items)](#set-unordered-collection-of-unique-items)
  - [Dictionary (collection of key-value pairs)](#dictionary-collection-of-key-value-pairs)
- [Help and docstring](#help-and-docstring)
- [Type hints](#type-hints)
- [Conditions](#conditions)
  - [Truthy and Falsy](#truthy-and-falsy)
  - [if](#if)
  - [Conditional expression](#conditional-expression)
  - [match-case](#match-case)
  - [try-except](#try-except)
- [Loops](#loops)
  - [while](#while)
  - [for](#for)
  - [iter](#iter)
- [Functions](#functions)
- [Classes](#classes)
- [Modules](#modules)
  - [Import](#import)
  - [Conditional import](#conditional-import)
  - [Custom](#custom)
  - [Direct run check](#direct-run-check)
- [Packages](#packages)
  - [Use](#use)
  - [Manage](#manage)
  - [Virtual environment](#virtual-environment)
    - [Purpose](#purpose)
    - [Linux/macOS](#linuxmacos)
    - [Windows](#windows-1)
    - [Check whether in virtual environment](#check-whether-in-virtual-environment)
  - [Examples of packages](#examples-of-packages)
- [Math](#math)
- [Random](#random)
- [Time](#time)
- [Date/Time](#datetime)
- [JSON](#json)
- [Jupyter](#jupyter)
- [I/O and processes](#io-and-processes)
  - [Read file](#read-file)
  - [Write file](#write-file)
  - [stdin](#stdin)
  - [stdout](#stdout)
  - [stderr](#stderr)
  - [Arguments](#arguments)
  - [Exit process](#exit-process)
  - [Environment variables](#environment-variables)
  - [Path](#path)
  - [Terminal](#terminal)
  - [Processes](#processes)


## Install

### Check

To see whether Python is already installed, in your terminal run:

```
python3 --version  # on Linux and macOS
py --version       # on Windows with py-launcher
python --version   # on Windows
```

To verify that your script is running with the wanted version:

```py
import sys
print(sys.version)
print(sys.executable)
```

### Linux

To install on Debian/Ubuntu:

```
sudo apt install python3
```

### macOS

To install using [Homebrew](https://brew.sh/):

```
brew install python
```

Close and reopen your terminal so that it finds the new version.

### Windows

For Windows [download](https://www.python.org/downloads/) the installer and run it. If you only have a single Python installation you should be done and you can skip the remaining of this section.

For Windows the official Python installer provides the `py.exe` launcher which can be used to choose the wanted Python version.

To list the installed versions (**\*** indicates the active version):

```
py --list
py --list-paths
```

To use a specific version:

```
py -<version> [python-args] [script]
```

- To set a default version add an environment variable like `PY_PYTHON=3.12`

If you also have **non py-launcher aware** installations in your `PATH` (use `where python` or `where pip` to check that), then you may have problems using the correct `pip`. To execute the `pip` corresponding to the py-launcher:

```
py -<version> -m pip --version
```


## Shebang line

Under Linux/macOS to be able to execute a Python script without invoking it through `python3`, make it executable and add a shebang line at the top of the file:

```py
#!/usr/bin/env python3
```


## Syntax

Python is **case-sensitive**. Comments start at `#` and end with the line, there are no multi-line comments. But when several lines of code have to be comment out for testing, it's more practical to make them an unassigned [triple-quoted string literal](#strings) by surrounding them with **triple-quotes**. Python programmers usually use **snake case** (lowercase with words separated by underscores). Semicolons can be used to separate statements on the same line, but are not required to terminate statements.

**Code blocks are always preceded by a colon**. A code block typically begins on a new line, with all statements having the same indentation amount. Note that the amount of spaces (or tabs) used to indent code blocks is up to the user, as long as it is consistent throughout the script. Most style guides recommend **indenting** code blocks by **four spaces** rather than using a tab (never mix tabs with spaces). Alternatively, a code block can be placed immediately after the colon on the same line, with semicolons separating the statements.

When a single line contains `(` or `[` or `{` or `'''` or `"""`, it can be **split after these symbols** to extend across multiple lines. However, if a single line does not contain those symbols, it's necessary to add final **backslashes** to divide into multiple lines.

Python uses **zero based indexing**; negative indexing is allowed, `-1` is the index of the last element, `-2` the second-to-last and so on.


## Variables, immutability and mutability

In Python all variables are objects, and the variable names are **labels** assigned to objects. Variables names are composed of alphanumeric characters or underscores and are not permitted to commence with a number. Assigning variables is just like giving a new nickname, in `foo = bar = baz = 3` the variable names are just labels for the same object.

In Python assignments are statements, use the **assignment expression** or **Walrus** operator `:=` to have the right side value returned. This construct is also called a named expression.

By convention the `_` variable is used as a placeholder/throwaway variable in for-loops or when unpacking, and in the interactive shell it stores the last expression value.

The variable types are **dynamic** and determined at runtime. The objects are subdivide into two categories:

1. **Immutable** are objects whose internal state/values can't be changed or altered:

   ```py
   NoneType, int, float, bool, str, tuple, complex, bytes
   ```

2. **Mutable** are objects whose internal state/values can be changed after creation:

   ```py
   list, set, dict, bytearray
   ```

   - Shallow copies (with `.copy` or [slicing](#slicing)) only make a copy of the first level objects, sublists remain referenced. To copy everything use the `copy` module with its `copy.deepcopy()` function.


To check whether two variable names point to the same object us the **identity operators** `is` (not identity operator: `is not`). Get the object id with `id(obj)` and the object type with `type(obj)`.

In Python functions are using **pass-by-assignment**. When we call a function, each parameter becomes a new nickname to the given object:

1. If we pass-in immutable arguments, then we cannot modify the arguments themselves, the resulting behavior is **pass-by-value**.

2. If we pass-in mutable arguments, then we can change them, the resulting behavior is **pass-by-reference**.


## Operators

### Overview

```
Assign:                          = += -= *= /= **= %= //=
Inc/dec:                         x += 1  x -= 1
Math:                            + - * / %
Exponentiation:                  **
Truncation division
(truncates to neg. infinity):    //
Quotient and remainder (→tuple): divmod(num1, num2)
Comparison:                      < <= > >= == !=
Boolean:                         and or not
Bitwise:                         ~ | & ^
Shift:                           << >>
```

### Division

The `/` operator treats the operands as floats (even if they are int) and performs a floating-point division.

### Chaining

```py
1 < a < 3
```

### Membership

```py
abc = ["a","b","c","d","e"]
"a" in abc       # True
"a" not in abc   # False
```

### Unpacking/packing

Unpacking is the operation of extracting data from a structured type into distinct variables. The opposite operation is termed packing. In some programming languages instead of unpacking we speak of destructuring. Unpacking happens automatically when **assigning an iterable to a tuple/list of variables**.

The most common application of unpacking is to multiple assign variables on one line: `x, y = 5, 11`. The expressions on the right side of the equal sign are all evaluated before any of the assignments take place: `a, b = b, a`. The assignment are then applied to the left side of the equal sign in left-to-right order: `i, x[i] = 1, 2`.

There are situations where we need to explicitly trigger unpacking/packing:

- The `*` operator in front of an iterable performs unpacking when reading the iterable and packing when writing into it.
- The `**` operator in front of a dictionary performs unpacking when reading the dictionary and packing when writing into it.

The following example illustrates where unpacking and packing happen:

```py
list1 = [1, 2, 3]
c = 4
d = 5
[a, b, *list2] = [*list1, c, d]
```

1. The `[*list1, c, d]` list is created from the unpacked `list1`, `c` and `d`.
2. The assignment operation unpacks the right-hand side list into elements.
3. The first two elements get assigned to `a` and `b`.
4. The remaining elements are packed into `list2`.

### Slicing

Slicing does return a new sliced object (start and end_exclusive can be negative): 

```py
s[start:end_exclusive]      # extract from start till end_exclusive
s[start:]                   # extract from start till last
s[:end_exclusive]           # extract from begin till end_exclusive
s[start:end_exclusive:step] # extract from start till end_exclusive
                            # with increments of step
```

### For mutable types `a += b` is not the same as `a = a + b`

When the `+=` operator is used on an object which has `__iadd__` (in-place addition) defined, the object is modified in place. Otherwise it will instead use `__add__` and return a new object. Mutable types have `__iadd__`, whereas immutable ones only have `__add__`.


## len, min, max

```py
# It calls obj's __len__() method
len(obj)
```

```py
# Order numerically/alphabetically
smallest = min(iterable)

# Order by len()
shortest_word = min(iterable, key=len)
```

```py
# Order numerically/alphabetically
largest = max(iterable)

# Order by len()
longest_word = max(iterable, key=len)
```


## print, input

```py
print("No newline yet/", end="")
print("line", "continuation", sep="-")
```

```py
n = input("Enter a number: ") # returns a string
print("Your number plus 1 is", int(n) + 1)
```


## Basic types

### NoneType

```py
x = None
x is None     # True
x is not None # False
```

### Integers

```py
cost = 1_000  # thousands separator
converted = int("12")
from_hex = int("3f", 16) # base 16
truncate = int(12.92)
absolute_value = abs(-1)
```

### Floating-point numbers

```py
absolute_value = abs(-1.23)
converted = float("12.3")

# The second argument is the number
# of decimals which defaults to 0
rounded = round(3.1415, 2)
```

### Complex numbers

```py
# We always need a number before j
num = 12 + 1j
```

### Booleans

```py
is_red = True
is_blue = False
converted = bool(1) # True
converted = bool(0) # False
```

```py
# - True if all are True
# - True for an empty iterable
all(iterable)
```

```py
# - True if at least one is True
# - False for an empty iterable
any(iterable)
```

### Strings

#### Defining

Strings can either be single-quoted or double-quoted and can contain backslash escapes like `\\`  `\'`  `\"`  `\r` `\n` `\t`. Use `\ooo` for the octal character `ooo` and `\xhh` for the hex character `hh`. Unicode characters are represented with `\uxxxx` or `\Uxxxxxxxx`. Strings prefixed with `r` or `R` are raw strings which treat backslashes as literal characters. Strings prefixed by `f` or `F` are f-strings which allow embedding expressions inside string literals. It's also possible to combine the prefixes to get raw f-strings.

To have the newlines in the string, surround in **triple-quotes**.

```py
converted = str(12)
repetition = 4 * "string"
concat1 = "str1" + "str2"
concat2 = "str1" "str2"
concat3 = "str1\
str2"
multiline = """line1
line2"""
```

#### Formatting

```py
val1, num1, num2 = "one", 123, 59.058
width, precision = 6, 1
print(f"_{val1:5}_")     # left-aligned
print(f"_{val1:>5}_")    # do right-align
print(f"_{val1:^5}_")    # do center-align
print(f"_{num1:5d}_")    # right-aligned
print(f"_{num1:<5d}_")   # do left-align
print(f"{num1:05d}")     # zero-fill
print(f"0x{num1:04x}")   # zero-fill hex
print(f"_{num2:7.2f}_")  # right-aligned
print(f"_{num2:<7.2f}_") # do left-align
print(f"_{num2:{width}.{precision}f}_")
print(f"_{num1+num2:.0f}_")

# Double the braces to escape them
print(f"{{foo}}")
```

#### Manipulation

```py
text.upper()
text.lower()
text.count(sub)
text.strip()    # trim whitespaces
text.lstrip()   # trim left whitespaces
text.rstrip()   # trim right whitespaces
sub in text     # return True or False
text.index(sub) # raises ValueError if not found
text.find(sub)  # return index, -1 if not found
text.rfind(sub) # return index of last one
"1,2,3".split(",")     # return a list
text.replace(old, new) # replace all
```

#### Regular expression

Always use raw string patterns to avoid interpreting backslashes.

```py
import re
m = re.search(r'pattern', text)   # return first as Match or None
print(text[m.start():m.end()])    # m.end() is exclusive 
l1 = re.findall(r'pattern', text) # return all found as a list
l2 = re.split(r'[^a-zA-Z]', text) # return splits as a list
res = re.sub(r'pattern', r'replacement', text) # replace all
```

The regex (regular expression) functions have a `flags` parameter to alter the expression's behavior. The most common flags are `re.IGNORECASE`, `re.MULTILINE`, `re.DOTALL` and `re.ASCII` which can be combined using the bitwise OR (the `|` operator). By default `\w` matches also unicode characters, with `re.ASCII` only ascii characters are matched.


### Bytes and bytearray

```py
# String to Bytes
s = "Hello, World!"
print(s.encode()) # utf8 bytes type
print(bytearray(s, 'utf-8'))

# Bytes to String
utf8 = b'\x48\x65\x6c\x6c\x6f' # b'Hello'
utf8_arr = bytearray(utf8)
print(utf8.decode())
utf8_arr[0] = 104
print(str(utf8_arr, 'utf-8'))
```


## Collections

### List (ordered sequence of objects)

```py
cities = ["Vienna", "London", "Paris", 
          "Berlin", "Zurich", "Hamburg"]

cities[-1] = "New York" # replace last one
cities.insert(i, "Bern")
cities.pop(i)           # return & remove ith element
cities.pop(-1)          # pop last one, same as pop()
cities.append("Locarno")
print(cities + other)   # combine lists
cities.extend(other)    # append other list
cities.remove("Berlin") # remove first occurrence
cities.clear()          # clear all
cities.index("Berlin")  # find position of element
cities.index("Berlin", start)
cities.index("Berlin", start, end_exclusive)
cities.reverse()        # reverse in-place
list(reversed(cities))  # return a new reversed list
cities.sort()           # sort in-place
cities.sort(reverse=True)
sorted(cities)          # return a new sorted list
list(zip(list1, list2)) # return a new tuple list

# Unpack/pack
a, *mid, b = cities     # unpack + pack to mid
cities = [a, *mid, b]   # unpack mid to elements

# Constructor
l1 = list(iterable)
```

- It's not possible to append elements by assigning past the last index.

**Comprehensions**

```py
# newlist = [expression for item in iterable if condition == True]
even_numbers = [x for x in range(10) if x % 2 == 0]
```

### Tuple (immutable list)

Python defines a tuple with commas, the parentheses can usually be left out (see the exceptions below):

```py
t = ("tuples", "are", "immutable", "and", "are", "fast")
t2 = "hello", 12
one = "solo",  # without the comma you get a string

x = t[0]
t.count("are") # return the count of "are"
t.index("are")
t3 = t + t2    # combine tuples

# Unpack/pack
a, *mid, b = t # unpack + pack to mid
t = a, *mid, b # unpack mid to elements

# Constructor
t4 = tuple(iterable)
```

- Use parentheses to avoid ambiguity with precedences.
- Parentheses are required when passing a tuple as a function argument.
- Parentheses are necessary to define the empty tuple: `()`

### Set (unordered collection of unique items)

```py
x = {"a","b","c","d","e"}
y = {"b","c"}

x.add("a")        # does nothing
y.add("a")
y.remove("a")     # raises error if not present
y.discard("a")    # remove if present
x.difference(y)   # return difference as new set
x.union(y)        # return union as new set
x.intersection(y) # return intersection as new set

# Constructor
z = set(iterable)
```

**Comprehensions**

```py
# newset = {expression for item in iterable if condition == True}
letters = {x for x in 'Hello World'} # no duplicates
```

### Dictionary (collection of key-value pairs)

```py
person = {"first_name" : "John",
          "last_name" : "Doe",
          "age" : 33}

x = person["first_name"]              # read
person["first_name"] = "Jimmy"        # change
person["email"] = "jim.doe@gmail.com" # add
y = person.pop("email")               # return & remove
del person["age"]                     # remove

# Unpack and merge
# (values with the same key are overwritten)
person1 = {**person,
           "first_name" : "Jim",
           "country" : "USA"}

# Constructor
d1 = dict(a=1, b=2, c=3)              # named args
d2 = dict([('a',1),('b',2),('c',3)])  # iterable
d3 = dict(iterable, d=4, e=5)         # named args at end
```

- Since Python 3.7 dictionaries are ordered.
- Duplicate keys are not supported.
- Keys can be any immutable data type.
- Values can be any data type.
- Python doesn't support dot notation to access a value like `dict.key`, you must use `dict[key]`

**Comprehensions**

```py
# newdict = {k: v for k in iterable if condition == True}
power2 = {k: 2**k for k in range(4)}

# newdict = {k: v for k, v in iterable if condition == True}
mydict = {'a': 0, 'b': 1, 'c': 2, 'd': 3}
power2 = {k: 2**v for k, v in mydict.items()}

# newlist = [(k, v) for k, v in iterable if condition == True]
letter_pairs = {'a': 'A', 'b': 'B', 'c': 'C'}
tuple_list = [(k, v) for k, v in letter_pairs.items()]
```


## Help and docstring

Help in Python is always available right in the interpreter:

- `help(obj)` describes an object, without an argument the interactive help starts.
- `dir(obj)` shows all the object's methods, without an argument it returns the list of names in the current local scope.

If the first statement in a function, class or module is an unassigned string literal, then it's called a **docstring**. By convention [multiline strings](#strings) are used. The docstring becomes the `__doc__` special attribute of that object which can be printed with `help(obj)`:

```py
def sum_nums(a, b):
    """Sum two numbers.

    Args:
        a: first number.
        b: second number.

    Returns:
        The sum of the two numbers.
    """
    return a + b

help(sum_nums) # uses: sum_nums.__doc__
```


## Type hints

Python does not enforce variable type and function annotations. They can be used by third party tools such as type checkers, IDEs and linters.

```py
# Use the name of the type in the annotation
x: int = 1
x: float = 1.0
x: bool = True
x: str = "test"

# The item type is in brackets
x: list[int] = [1, 2, 4]
x: set[int] = {6, 7}
x: dict[str, float] = {"field1": 1.0, "field2": 3.0}
x: tuple[int, str, float] = (3, "yes", 7.5)

# Annotate a function definition like
def stringify(num: int) -> str:
    return str(num)

# - If a function does not return a value, use None as the return type
# - Default value for an argument goes after the type annotation
def show(value: str, excitement: int = 10) -> None:
    print(value + "!" * excitement)
```


## Conditions

### Truthy and Falsy

The terms **truthy** and **falsy** refer to values that are not booleans but, when evaluated, assume the values of `True` and `False`.

- **Falsy**: `None`, **zero** of any numeric type, **empty** strings, **empty** sequences or **empty** collections.

- **Truthy**: all the others.
 
### if

```py
if condition1:
    print("One or more statements")
elif condition2:
    pass # use pass statement to do nothing
else:
    print("One or more statements")
```

### Conditional expression

If `condition` is `True`, the left-side of `if` is evaluated and its value is returned; otherwise, the right-side of `else` is evaluated and its value is returned.

```py
# Return value assigned
ret = x if condition else y
print(ret)

# Return value ignored 
print("OK") if condition else print("Not OK")
```

### match-case

```py
match var:
    case pattern1:
        print("One or more statements")
    case pattern2a | pattern2b:  # or
        print("One or more statements")
    case _:  # default
        print("One or more statements")
```

### try-except

```py
try:
    raise Exception("Bad")
except ZeroDivisionError:
    print("You are dividing by zero")
except Exception as e:
    print(e)
else:
    print("Runs if no exception")
finally:
    print("Runs always")
```


## Loops

### while

```py
while condition:
    print("One or more statements")
else:
    print("Completed without break being called")

i = 0
while i < 10:
    print(i)
    i += 1
```

- Use `break` to break out of the loop and `continue` to jump to the start.

### for

We can loop over `str`, `list`, `tuple`, `set` and `dict`:

```py
for var in iterable:
    print("One or more statements")
else:
    print("Completed without break being called")

nums = [1, 2, 3, 4, 5]
for i in nums:
    print(i)
```

- Use `break` to break out of the loop and `continue` to jump to the start.

The **range type** represents an immutable iterable sequence of numbers which has a length and can be indexed/sliced:

```py
range(count)                      # 0..(count - 1)
range(start, end_exclusive)       # start..(end_exclusive - 1)
range(start, end_exclusive, step) # range with step increments

nums = [1, 2, 3, 4, 5]
for i in range(len(nums)):
    print(nums[i])
```

Loop through dictionaries:

```py
# Iterating over keys is the default
for k in mydict:
    print(k, '->', mydict[k])
for k in mydict.keys():
    print(k, '->', mydict[k])

for v in mydict.values():
    print(v)

for k, v in mydict.items():
    print(k, '->', v)
```


### iter

An **iterator** object gets created when calling `iter()` on an iterable object (list, tuple, dict, set, str, range). The iterator has the `next()` method for iteration. Iterators are stateful, once an item is consumed it's gone. The `next()` method raises a `StopIteration` to signal the end of the iteration. An iterator that has been fully consumed is termed **exhausted**.

Iterables are able to be iterated over and iterators are the agents that perform the iteration. All iterators are also iterables because calling `iter()` on an iterator will give itself back.

```py
animals = ["dog", "cat", "snake"]

# That works because iterators are also iterables
iter_animals = iter(animals)
for item in iter_animals:
    print(item)

# That works if None is not in array
iter_animals = iter(animals)
while (item := next(iter_animals, None)) is not None:
    print(item)

# That's what a for-loop internally does
iter_animals = iter(animals)
while True:
    try:
        item = next(iter_animals)
        print(item)
    except StopIteration:
        break
```

**Generator functions** have `yield` statement(s) and automatically return an iterator:

```py
def my_gen_func(x):
    yield 1
    yield 2
    yield x * x

iter_nums = my_gen_func(2)
while (num := next(iter_nums, None)) is not None:
    print(num)
```

**Generator expressions** are in parentheses and evaluate to an iterator:

```py
iter_range = (n for n in range(3, 5))
while (num := next(iter_range, None)) is not None:
    print(num)
```


## Functions

The passed number of arguments must much the function definition, except for the default arguments which are optional:

```py
def my_calc(x, y):
    # Make sure the passed args are numbers
    assert isinstance(x, (int, float))
    assert isinstance(y, (int, float))
    global num        # make num global
    num = 3
    z = 2 * x + y     # z is local to function
    return z
my_calc(1, 2)         # positional args
my_calc(y = 2, x = 1) # named args

def default_return():
    a = 1 + 1
print(default_return()) # returns None

def default_arg(name="everybody"):
    return "Hello " + name + "!"
print(default_arg())
```

Varargs implemented with tuple packing:

```py
def my_min(*args): # args is a name of your choice
    result = args[0]
    for num in args:
        if num < result:
            result = num
    return result
my_min(4, 5, 6, 7, 2)
```

Key-worded varargs implemented with dictionary packing:

```py
def my_func(**kwargs): # kwargs is a name of your choice
    for kw in kwargs:
        print(kw, ":", kwargs[kw])
my_func(shopkeeper = "M. Palin", client = "J. Cleese")
```

**Lambda** functions (anonymous functions):

```py
lambda [args] : expression   # args comma separated

greet = lambda : print('Hi') # zero args
greet()                      # call like usual
greet_name = lambda name : print('Hi', name)
greet_name('John')
x = lambda a, b : a * b
print(x(3, 4))
```

A **Closure** is a function that references variables from the enclosing scope, even after the outer functions are done:

```py
def my_func(n):
    return lambda a : a * n

my_doubler = my_func(2)
my_tripler = my_func(3)
print(my_doubler(3))
print(my_tripler(3))
```


## Classes

```py
import math

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        
    def get_x(self):
        return self.x
        
    def get_y(self):
        return self.y
        
class Asteroid(Point):
    def __init__(self, x, y):
        super().__init__(x, y)
        
    def compute_distance(self, other):
        dist_x = self.x - other.get_x()
        dist_y = self.y - other.get_y()
        return math.sqrt(dist_x*dist_x + dist_y*dist_y)
        
a1 = Asteroid(0, 0)
a2 = Asteroid(3, 4)
print(a1.compute_distance(a2))
```


## Modules

### Import

- `import math, random`: every attribute or function are accessed by putting `math.` and `random.` in front of the name like `print(math.pi)` or `print(random.randint(0,9))`

- `import numpy as np`: this renames `numpy` to `np` within the script. Note that the name `numpy` itself is no longer valid.

- `from fibo import fib, fib2`: this imports names from a module directly into the importing module's symbol table. This does not introduce the module name from which the imports are taken in the local symbol table (so in the example, `fibo` is not defined).

- `from fibo import *`: this imports all names that a module defines. Do not use this facility since it introduces an unknown set of names into the interpreter, possibly hiding some things you have already defined.

### Conditional import

```py
try:
    import cowsay
except ImportError:
    cowsay = None
if cowsay:
    cowsay.cow("Good Morning!")
else:
    print("Good Morning! (cowsay not installed)")
```

### Custom

To make your own module just write a normal Python file and then import it without the `.py` extension:

```py
import mymodule
mymodule.myfunc("Hi")
```

### Direct run check

To check whether a module is execute directly (without import):

```py
if __name__ == "__main__":
    print("Running this module")
```


## Packages

### Use

Packages are a way of structuring the module namespace by using "dotted module names". For example, the module name `pkgA.modB` designates a submodule named `modB` in a package named `pkgA`.

- `import sound.effects.echo`: this imports an individual module from the package. A function is referenced like `sound.effects.echo.echofilter()`.

- `from sound.effects import echo`: a function is referenced like `echo.echofilter()`.

- `from sound.effects.echo import echofilter`: the function is referenced like `echofilter()`.

### Manage

To search for a package use [pypi.org/search](https://pypi.org/search/).

The Python Package Manager (pip) should be called through the correct Python version:

- Linux/macOS: `python3 -m pip ...`
- Windows: `py -m pip ...`

Typical commands (invoke `pip` like shown above):

```
pip list -v
pip show <pkg>
pip install <pkgs>
pip install --dry-run <pkgs>
pip install --upgrade <pkgs>
pip uninstall <pkgs>
```

- The `pip` package names aren't case sensitive.
- Instead of installing globally, it's possible to use the `--user` option to install packages for the current user only. Note that on many Linux/macOS systems, installing without `sudo` implies the `--user` option. Listing with `--user` only shows packages installed with `--user`, while not using that option lists all packages.
- To protect packages which may also be installed by your system package manager (like `apt` for example), since Python 3.11 some systems require the `--break-system-packages` option, and that also with the `--user` option.

### Virtual environment

#### Purpose

It's possible to create an **isolated** Python installation with separate packages for each of your projects. By default only the packages explicitly installed in the virtual environment are accessible.

#### Linux/macOS

1. Go to your project's directory and the first time create a virtual environment in `.venv` directory:

   ```
   python3 -m venv .venv
   ```

2. Activate the virtual environment:

   ```
   . .venv/bin/activate
   ```

3. Install/uninstall packages with `pip` and execute your programs with `python`

4. Deactivate the virtual environment (or just close the terminal):

   ```
   deactivate
   ```

#### Windows

1. Go to your project's directory and the first time create a virtual environment in `.venv` directory:

   ```
   py -m venv .venv
   ```

2. Activate the virtual environment:

   ```
   .venv\Scripts\activate
   ```

3. Install/uninstall packages with `pip` and execute your programs with `python`

4. Deactivate the virtual environment (or just close the terminal):

   ```
   deactivate
   ```

#### Check whether in virtual environment

- From terminal it shows the path of `.venv`:

  ```
  pip -V
  ```

- From script:
  
  ```py
  import sys
  if sys.prefix != sys.base_prefix:
      print("Inside virtual environment")
  else:
      print("Not inside virtual environment")
  ```

### Examples of packages

- App frameworks: `kivy` `tkinter` `pyqt` / `pyside`
- Web frameworks: `django` `flask`
- CLI frameworks: `click` `rich` `pyfiglet`
- Config: `configparser` `xml` `json`
- Text: `csv` `pypdf`
- Develop: `loguru` `python-dotenv` `pytest`
- Multimedia: `audioflux` `pillow` `moviepy` `opencv` `simplecv` `scikit-image`
- Games: `pygame` `pyglet`
- Network: `requests` `httpx` `scrapy` `beautifulsoup` `twisted`
- Math: `pandas` `numpy` `scipy` `sympy` `matplotlib` `plotly` `seaborn`
- Machine learning: `scikit-learn` `tensorflow` `keras` `pytorch`


## Math

```py
import math
import sys

# Math functions
print(math.sin(math.pi/2))
print(math.sqrt(81))
print(math.log(math.e))       # default base is e
print(math.log(10, 10))       # 2nd arg is base

# Not a number
x = float('nan')              # or: math.nan
print(math.isnan(x))
print(math.isfinite(x))       # True if not inf and not nan
print(math.inf/math.inf)      # nan

# Infinity
pos_inf = float('inf')        # or: math.inf
neg_inf = float('-inf')       # or: -math.inf
print(math.isinf(pos_inf))    # True if positive or negative inf
print(math.isfinite(neg_inf)) # True if not inf and not nan
print(sys.float_info.max*10)  # inf
```


## Random

There is no need to call `random.seed()` as it is called for us with the OS's randomness sources.

```py
import random

# Floating-point
x = random.random() # 0.0 <= x < 1.0

# Integer
# Note: random.randint(a, b)
#                 =
#       random.randrange(a, b+1)
y = random.randint(a, b) # a <= y <= b

# Randomly select an element from:
# range(start, stop, step)
z = random.randrange(start, stop, step)
```


## Time

```py
import time

# Pause execution for the given seconds
time.sleep(1.5);

# Measuring execution time
start_time = time.time() # seconds since the Unix epoch
# some code...
end_time = time.time()   # seconds since the Unix epoch
print("Execution Time:", end_time - start_time, "sec")
```


## Date/Time

```py
import datetime

# Now
now = datetime.datetime.now() # local time
now_utc = datetime.datetime.now(datetime.UTC)

# Construct local time
print(datetime.datetime(2018, 8, 18, 10, 5, 56, 518515))

# Format
print(now.strftime("%d.%m.%Y %H:%M:%S"))
print(now_utc.isoformat(timespec='seconds'))

# Time delta (uses wall clock time semantics)
# days seconds microseconds milliseconds minutes hours weeks
print(now + datetime.timedelta(hours=1, seconds=1))

# Unix/POSIX timestamp
ts = now.timestamp()
print(ts, datetime.datetime.fromtimestamp(ts))
```


## JSON

```py
import json
json_str = '{"first_name": "John", "last_name": "Doe", "age": 30}'

# json str -> dict
user = json.loads(json_str)
print(user)

# dict -> json str
json_str2 = json.dumps(user, indent=2)
print(json_str2)
```


## Jupyter

Jupyter is a web based interactive IDE for writing code in cells. Each cell's output is stored in source file with its code.

Install:

```py
pip install jupyter
```

Launch notebook web server:

```py
jupyter notebook
```

- To browser another location use the `--notebook-dir` option or change the terminal directory before launching the notebook.

Note: Google provides a Jupyter cloud version called Colab.


## I/O and processes

### Read file

By default a file is opened in read and text mode. Opening fails if the file does not exist. Open with the `encoding='utf-8'` argument to make sure utf-8 is used on all systems.

To open a file in binary mode use `'rb'`.

```py
my_file = open('my_file.txt')    # default mode is 'r'
for line in my_file:
    print(line.rstrip())         # trim trailing newline
my_file.seek(0)	                 # move to the beginning
lines1 = my_file.readlines()     # newline characters not removed
print(lines1)                    # print lines list
my_file.seek(0)	                 # move to the beginning
text = my_file.read()            # slurp file in one go
lines2 = text.splitlines()       # newline characters removed
print(lines2)                    # print lines list                    
print(my_file.name)              # file name
print(my_file.mode)              # file mode
print(my_file.closed)            # is file closed?
my_file.close()
```

### Write file

The write mode is `'w'`, and the append mode is `'a'`; for both cases the file is written in text mode and created if not existing. By default the line ending is automatically converted depending from the system. To always use a specific line ending open with the `newline='\n'` argument. Open with the `encoding='utf-8'` argument to make sure utf-8 is used on all systems.

To open a file in binary mode use `'wb'` or `'ab'`.

```py
my_file = open('my_file.txt', 'w')
my_file.write('some text\n')
print('another line', file=my_file) # print adds a newline
my_file.close()
```

### stdin

```py
import sys
for line in sys.stdin:
    do_stuff_with(line.rstrip()) # trim trailing newline
text = sys.stdin.read()          # slurp stdin in one go

# Hint: interrupt by sending EOF on its own line
#       (Ctrl-D on Linux/macOS or CTRL-Z on Windows)
```

### stdout

```py
import sys
print('Hello, stdout.')
sys.stdout.write('Hello, stdout.\n')
```

### stderr

```py
import sys
print('a logging message.', file=sys.stderr)
sys.stderr.write('a logging message.\n')
```

### Arguments

```py
import sys
for arg in sys.argv[1:]: # skip argv[0]
    print(arg)
```

### Exit process

The `sys.exit` function raises a SystemExit exception, signaling an intention to exit the interpreter.

```py
import sys
sys.exit()  # no error, same as sys.exit(0)
sys.exit(1) # error range: 1-127
```

### Environment variables

```py
import os
os.environ['VAR']                # error if VAR not defined
os.environ.get('VAR', 'default') # default if VAR not defined
```

### Path

```py
from pathlib import Path
p = Path('C:\\Windows')
p = Path()     # rel. path of current dir
p = Path.cwd() # abs. path of current dir
p.resolve()    # abs. physical path
str(p)         # convert to string

# Script dir & path join operator /
p = Path(__file__).resolve().parent / 'test.txt'

# Iterate
for i in sorted(p.iterdir(), reverse = True):
    print(i)
for i in sorted(p.glob('*.py'), reverse = True):
    print(i)

p.name
p.parent
p.parts
p.stat().st_size
p.stat().st_mtime
p.is_dir()
p.is_file()
p.unlink()
p.rename(target)
p.mkdir()
p.rmdir()

import shutil
shutil.copytree('src', 'dest') # recursive copy
shutil.rmtree('a_dir')         # recursive delete
```

### Terminal

```py
# ANSI escapes
print('\033[31mRED\033[0m')
line, col = 15, 10
print(f'\033[{line};{col}HFind Me!')

import os
term_size = os.get_terminal_size()
print(term_size.columns, term_size.lines)
```

### Processes

```py
import subprocess as sp
proc = sp.run(['ls', '-lh']) # blocks until done
if proc.returncode != 0:
    do_something_else()

# with statement closes file after nested block
with open('./foo', 'w') as foofile:
    sp.run(['ls'], stdout=foofile)
with open('foo') as foofile:
    sp.run(['tr', 'a-z', 'A-Z'], stdin=foofile)
with open('foo.log', 'w') as logfile:
    sp.run(['ls', 'foo', 'bar'], stderr=logfile)

import psutil
p = psutil.Process(pid)
p.kill()
for proc in psutil.process_iter():
    if proc.name() == 'my_proc':
        proc.kill()
```
