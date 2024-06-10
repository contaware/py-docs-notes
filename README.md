# Python Docs & Notes <!-- omit from toc -->

This document is a reference guide for Python programming. It is a bit more than a simple cheat sheet, but it is not a learning book, you need to have some knowledge and experience with Python programming to understand these notes.


## Table of contents <!-- omit from toc -->

- [Install](#install)
  - [Check](#check)
  - [Linux](#linux)
  - [macOS](#macos)
  - [Windows](#windows)
- [Help](#help)
- [Shebang line](#shebang-line)
- [Syntax](#syntax)
- [Variables, immutability and mutability](#variables-immutability-and-mutability)
- [Operators](#operators)
  - [Overview](#overview)
  - [Division](#division)
  - [Chaining](#chaining)
  - [Membership](#membership)
  - [Slicing](#slicing)
  - [For mutable types `a += b` is not the same as `a = a + b`](#for-mutable-types-a--b-is-not-the-same-as-a--a--b)
- [Length](#length)
- [Print and input](#print-and-input)
- [Basic types](#basic-types)
  - [NoneType](#nonetype)
  - [Integers](#integers)
  - [Floating-point numbers](#floating-point-numbers)
  - [Complex numbers](#complex-numbers)
  - [Booleans](#booleans)
  - [Strings](#strings)
  - [Bytes and bytearray](#bytes-and-bytearray)
- [Collections](#collections)
  - [List (ordered sequence of objects)](#list-ordered-sequence-of-objects)
  - [Tuple (immutable list)](#tuple-immutable-list)
  - [Set (unordered collection of unique items)](#set-unordered-collection-of-unique-items)
  - [Dictionary (collection of key-value pairs)](#dictionary-collection-of-key-value-pairs)
- [Conditions](#conditions)
  - [if](#if)
  - [match-case](#match-case)
  - [try-except](#try-except)
- [Loops](#loops)
  - [while](#while)
  - [for](#for)
- [Functions](#functions)
- [Classes](#classes)
- [Modules](#modules)
  - [Import](#import)
  - [Custom](#custom)
  - [Direct run check](#direct-run-check)
- [Packages](#packages)
  - [Use](#use)
  - [Manage](#manage)
  - [Virtual environment](#virtual-environment)
    - [Linux/macOS](#linuxmacos)
    - [Windows](#windows-1)
  - [Examples of packages](#examples-of-packages)
- [Date/Time](#datetime)
- [JSON](#json)
- [I/O and processes](#io-and-processes)
  - [Read file](#read-file)
  - [Write file](#write-file)
  - [stdin](#stdin)
  - [stdout](#stdout)
  - [stderr](#stderr)
  - [Arguments](#arguments)
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

To list the installed versions:

```
py --list
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


## Help

Help in Python is always available right in the interpreter:

- `help(obj)` describes an object, without an argument the interactive help starts.
- `dir(obj)` shows all the object's methods, without an argument it returns the list of names in the current local scope.


## Shebang line

Under Linux/macOS to be able to execute a Python script without invoking it through `python3`, make it executable and add a shebang line at the top of the file:

```py
#!/usr/bin/env python3
```


## Syntax

Python is **case-sensitive**. Comments start at `#` and end with the line, there are no multi-line comments. Python programmers usually use snake case (lowercase with words separated by underscores). Semicolons can be used to separate statements on the same line, but are not required to terminate statements.

Code blocks are always preceded by a colon on the previous line. You should be aware that the amount of spaces (or tabs) used for indenting code blocks is up to the user, as long as it is consistent throughout the script. Most style guides recommend to indent code blocks by four spaces instead of a tab (never mix tabs with spaces).

It's possible to continue expressions on the next line if within parentheses. Also the initializations of the collection types can span multiple lines.

Python uses zero based indexing; negative indexing is allowed, `-1` is the index of the last element, `-2` the second-to-last and so on.


## Variables, immutability and mutability

In Python all variables are objects, and the variable names are **labels** assigned to objects. Assigning variables is just like giving a new nickname, in `foo = bar = baz = 3` the variable names are just labels for the same object.

Thanks to **destructuring** it's possible to multiple assign variables on one line: `x, y = 5, 11`. The expressions on the right side of the equal sign are all evaluated before any of the assignments take place: `a, b = b, a`. The assignment are then applied to the left side of the equal sign in left-to-right order: `i, x[i] = 1, 2`.

By convention the `_` variable is used as a placeholder/throwaway variable in for-loops or when destructuring, and in the interactive shell it stores the last expression value.

The variable types are **dynamic** and determined at runtime. The objects are subdivide into two categories:

1. **Immutable** are objects whose internal state/values can't be changed or altered:

   ```
   NoneType, int, float, bool, str, tuple, complex, bytes
   ```

2. **Mutable** are objects whose internal state/values can be changed after creation:

   ```
   list, set, dict, bytearray
   ```

   - Shallow copies (with `.copy` or [slicing](#slicing)) only make a copy of the first level objects, sublists remain referenced. To copy everything use the `copy` module with its `copy.deepcopy()` function.


To check whether two objects point to the same object us the identity operators `is` (not identity operator: `is not`). Get the object id with `id(obj)` and the object type with `type(obj)`.

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


## Length

```py
len(obj)
```

- It calls the obj's `__len__()` method.


## Print and input

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

### Strings

Strings can either be single-quoted or double-quoted and can contain backslash escapes like `\\`  `\'`  `\"`  `\r` `\n` `\t`. Use `\ooo` for the octal character `ooo` and `\xhh` for the hex character `hh`. Unicode characters are represented with `\uxxxx` or `\Uxxxxxxxx`. Strings prefixed with `r` or `R` are raw strings which treat backslashes as literal characters. Strings prefixed by `f` or `F` are f-strings which allow embedding expressions inside string literals. It's also possible to combine the prefixes to get raw f-strings.

To have a string span multiple lines, place a backslash at the end of the lines or to have the newlines in the string, surround in triple-quotes.

Making strings:

```py
converted = str(12)
repetition = 4 * "string"
concatenation = "str1" + "str2"
span_lines = "str1\
str2"
multiline = """line1
line2"""
```

String formatting:

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

String manipulation:

```py
text.upper()
text.lower()
text.count(sub)
text.strip()  # trim whitespaces
text.lstrip() # trim left whitespaces
text.rstrip() # trim right whitespaces
text.find(sub)
text.rfind(sub)
"1,2,3".split(",") # ['1', '2', '3']
text.replace(old, new)
```

Regular expression operations, use raw strings to avoid interpreting backslashes:

```py
import re
re.split(r'[^a-zA-Z]', text)
re.sub(r'pattern', r'replacement', text)
```

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

a, *mid, b = cities     # destructuring (mid is a list)
cities[-1] = "New York" # replace last one
cities.insert(i, "Bern")
cities.pop(i)           # return & remove ith element
cities.pop(-1)          # pop last one, same as pop()
cities.append("Locarno")
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
zip(list1, list2)       # return a new tuple list
```

- It's not possible to append elements by assigning past the last index.

**Comprehensions**

```py
# newlist = [expression for item in iterable if condition == True]
even_numbers = [x for x in range(10) if x % 2 == 0]
```

### Tuple (immutable list)

```py
t = ("tuples", "are", "immutable", "and", "are", "fast")
t2 = "hello", 12
one = "tuple", # without the comma you get a string

a, *mid, b = t # destructuring (mid is a list)
x = t[0]
t.count("are") # return the count of "are"
t.index("are")
t3 = t + t2    # combine tuples
```

- Python defines a tuple using commas, the parentheses can often be left out.
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

# Merges keys and values of person1 into person
# (overwrites values with the same key)
person1 = {"first_name" : "Jim", "country" : "USA"}
person.update(person1)
```

- Since Python 3.7 dictionaries are ordered.
- Duplicate keys are not supported.
- Keys can be any immutable data type (for that reason we cannot do destructuring like in javascript).
- Values can be any data type.
- Python doesn't support dot notation to access a value like `dict.key`, you must use `dict[key]`

**Comprehensions**

```py
# newdict = {k: v for k in iterable if condition == True}
power2 = {k: 2**k for k in range(4)}

# newdict = {k: v for k, v in iterable if condition == True}
mydict = {'a': 0, 'b': 1, 'c': 2, 'd': 3}
newdict = {k: 2**v for k, v in mydict.items()}
```


## Conditions

### if

```py
if condition1:
    print("One or more statements")
elif condition2:
    pass # use pass statement to do nothing
else:
    print("One or more statements")
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

The **range type** represents an immutable sequence of numbers used to loop:

```py
range(count)                      # 0..(count - 1)
range(start, end_exclusive)       # start..(end_exclusive - 1)
range(start, end_exclusive, step) # range with step increments

nums = [1, 2, 3, 4, 5]
for i in range(len(nums)):
    print(nums[i])
```

Loop through dictionaries (keys are placed in the loop variable by default):

```py
for k in mydict:
    print(k, '->', mydict[k])

for v in mydict.values():
    print(v)

for k, v in mydict.items():
    print(k, '->', v)
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

Varargs implemented with tuples:

```py
def my_min(*args): # args is a name of your choice
    result = args[0]
    for num in args:
        if num < result:
            result = num
    return result
my_min(4, 5, 6, 7, 2)
```

Key-worded varargs implemented with dictionaries:

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

The Python Package Manager (pip) should be called through the correct Python version:

- Linux/macOS: `python3 -m pip ...`
- Windows: `py -m pip ...`

Typical commands (invoke `pip` like shown above):

```
pip list -v
pip show <pkg>
pip install <pkgs>
pip install --upgrade <pkgs>
pip uninstall <pkgs>
```

- Instead of installing globally, it's possible to use the `--user` option to install packages for the current user only. Note that on many Linux/macOS systems, installing without `sudo` implies the `--user` option.
- To protect packages which may also be installed by your system package manager (like `apt` for example), since Python 3.11 some systems require the `--break-system-packages` option, and that also with the `--user` option.

### Virtual environment

It's possible to create an isolated Python installation with separate packages for a each of your projects. To do that, go to your project's directory and run the following commands:

#### Linux/macOS

1. Create a virtual environment in `.venv` directory:

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

1. Create a virtual environment in `.venv` directory:

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

### Examples of packages

- App frameworks: `kivy` `pyqt` `tkinter`
- Parsers: `configparser` `csv` `xml` `json`
- Multimedia: `audioflux` `pillow` `moviepy` `opencv` `simplecv`
- Games: `pygame` `pyglet`
- Network: `requests` `scrapy` `twisted`
- Math: `pandas` `numpy` `scipy` `sympy` `matplotlib` `plotly`


## Date/Time

```py
import datetime
now = datetime.datetime.now()
past = datetime.datetime(2018, 8, 18, 10, 5, 56, 518515)
print(now)
print(past)
print(now.strftime('%d.%m.%Y %H:%M:%S'))
```


## JSON

```py
import json
json_str = '{"first_name": "John", "last_name": "Doe", "age": 30}'
print(type(json_str), json_str)
user = json.loads(json_str)  # str -> dict
print(type(user), user)
json_str2 = json.dumps(user) # dict -> str
print(type(json_str2), json_str2)
```


## I/O and processes

### Read file

```py
my_file = open('my_file.txt')    # default mode is 'r'
for line in my_file:
    do_stuff_with(line.rstrip()) # trim trailing newline
text = my_file.read()            # slurp file in one go
lines = text.splitlines()        # newline characters removed
print(my_file.name)              # file name
print(my_file.mode)              # file mode
print(my_file.closed)            # is file closed?
my_file.close()
```

### Write file

```py
my_file = open('my_file.txt', 'w')  # use 'a' to append
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
