# Python Docs & Notes <!-- omit from toc -->

This document is a reference guide for Python programming. It is a bit more than a simple cheat sheet, but it is not a learning book, you need to have some knowledge and experience with Python programming to understand these notes.


## Table of contents <!-- omit from toc -->

- [Install](#install)
  - [Linux](#linux)
  - [macOS](#macos)
  - [Windows](#windows)
- [Help](#help)
- [Syntax](#syntax)
- [Variables, immutability and mutability](#variables-immutability-and-mutability)
- [Operators](#operators)
  - [Overview](#overview)
  - [Conditions can be chained](#conditions-can-be-chained)
  - [For mutable types `a += b` is not the same as `a = a + b`](#for-mutable-types-a--b-is-not-the-same-as-a--a--b)
- [Basic types](#basic-types)
  - [Integers](#integers)
  - [Floating-point numbers](#floating-point-numbers)
  - [Complex numbers](#complex-numbers)
  - [Booleans](#booleans)
  - [Strings](#strings)
  - [Bytes](#bytes)
- [Collections](#collections)
  - [List (ordered sequence of objects)](#list-ordered-sequence-of-objects)
  - [Tuple (immutable list)](#tuple-immutable-list)
  - [Set (unordered collection of unique items)](#set-unordered-collection-of-unique-items)
  - [Dictionary (unordered collection accessed by key)](#dictionary-unordered-collection-accessed-by-key)
- [Elements](#elements)
  - [Length](#length)
  - [Slicing](#slicing)
  - [Checking if an element is contained](#checking-if-an-element-is-contained)
- [Conditions](#conditions)
  - [if](#if)
  - [pass statement](#pass-statement)
- [Loops](#loops)
  - [while](#while)
  - [for](#for)
- [Functions](#functions)
- [Classes](#classes)
- [Input](#input)
- [Import modules](#import-modules)
- [Date/Time](#datetime)
- [Packages](#packages)
- [How to use Python instead of Shell](#how-to-use-python-instead-of-shell)
  - [read file](#read-file)
  - [write file](#write-file)
  - [stdin](#stdin)
  - [stdout](#stdout)
  - [stderr](#stderr)
  - [args](#args)
  - [env](#env)
  - [path](#path)
  - [proc](#proc)


## Install

To see whether Python is already installed, in your terminal run:

```
python --version   # on Windows
python3 --version  # on Linux and macOS
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

- `help([obj])` describes an object, without an argument the interactive help starts.
- `dir([obj])` shows all the object's methods, without an argument it returns the list of names in the current local scope.


## Syntax

Python is **case-sensitive**. Comments start at `#` and end with the line, there are no multi-line comments. Python programmers usually use snake case (lowercase with words separated by underscores). Semicolons can be used to separate statements on the same line, but are not required to terminate statements.

Code blocks are always preceded by a colon on the previous line. You should be aware that the amount of spaces (or tabs) used for indenting code blocks is up to the user, as long as it is consistent throughout the script. Most style guides recommend to indent code blocks by four spaces instead of a tab (never mix tabs with spaces). Spaces within lines do not matter.

It's possible to continue expressions on the next line if within parentheses. Also lists can span multiple lines.


## Variables, immutability and mutability

In Python there are only objects. The object type is dynamic and determined at runtime.

Variable names are labels assigned to objects. Assigning variables is just like giving a new nickname, in `foo = bar = baz = 3` the variable names are just labels for the same object.

1. Immutable are objects whose internal state/values can't be changed or altered (the object's methods return a new object):

   ```
   int, float, bool, str, tuple, complex
   ```

2. Mutable are objects whose internal state/values can be changed after creation:

   ```
   list, set, dict
   ```

   - Shallow copies (with `.copy` or [slicing](#slicing)) only make a copy of the first level objects, sublists remain referenced. To copy everything use the module `copy.deepcopy`


To check whether two objects point to the same object us the identity operators `is` (not identity operator `is not`). Get the object id with `id(x)` and the object type with `type(x)`.

In Python functions are using **pass-by-assignment**. When we call a function, each parameter becomes a new nickname to the given object:

- If we pass in immutable arguments, then we have no way of modifying 
the arguments themselves, it can look like Python uses the **pass-by-value**.

- If we pass in mutable arguments, then we can change them, it can look 
like Python uses a **pass-by-reference**.


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

### Conditions can be chained

```py
1 < a < 3
```

### For mutable types `a += b` is not the same as `a = a + b`

When the `+=` operator is used on an object which has an `__iadd__` (in-place addition) defined, the object is modified in place. Otherwise it will instead use `__add__` and return a new object. Mutable types have `__iadd__`, whereas immutable ones only have `__add__`.


## Basic types

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
rounded = round(3.1415, 2) # sec param is num of decimals
                           # (defaults to 0)
```

### Complex numbers

```py
num = 12 + 1j  # need to place 1 before j
```

### Booleans

```py
is_red = True
is_blue = False
converted = bool(0)
converted = bool(1)
```

### Strings

Strings can either be single-quoted or double-quoted, the strings can contain backslash escapes like `\\`  `\'`  `\"`  `\r` `\n` `\t`. Use `\ooo` for the octal character `ooo` and `\xhh` for the hex character `hh`. Unicode characters are represented with `\uxxxx` or `\Uxxxxxxxx`. To have a string span multiple lines, place a backslash at the end of the line.

Strings can optionally be prefixed with `r` or `R`, such strings are called raw strings and treat backslashes as literal characters.

Making strings:

```py
converted = str(12)
concatenation = "string1" + "String2"
repetition = 4 * "string"
```

Strings manipulation:

```py
text.upper()
text.lower()
text.count(sub)
text.find(sub, start, end_exclusive)  # start and end_exclusive are optional
text.rfind(sub, start, end_exclusive) # start and end_exclusive are optional
"1,2,3".split(",")                    # ['1', '2', '3']
text.replace(old, new)
"text with {} placeholders {} it".format("two", "in")
"{:04d},{:4d},{:6.2f}".format(123, 453, 59.058)
```

Regular expression operations, use raw strings to avoid interpreting backslashes:

```py
import re
re.split(r'[^a-zA-Z]', text)
re.sub(r'pattern', r'replacement', text)
```

### Bytes

```py
utf8 = "Hello, World!".encode()
print(utf8)           # b'Hello, World!'
print(utf8.decode())  # Hello, World!

r = [0x48, 0x65, 0x6C, 0x6C, 0x6F]
Hello = bytes(r)
print(Hello.decode()) # Hello

mybytes = bytes.fromhex("f0f1 f2") # whitespaces are ignored
b'\xf0\xf1\xf2'.hex() # "f0f1f2"
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
cities.extend(other)    # append list
cities.remove("Berlin") # remove first occurrence
cities.clear()          # clear all
cities.index("Berlin")  # find position of element
cities.index("Berlin", start)
cities.index("Berlin", start, end_exclusive)
```

### Tuple (immutable list)

```py
t = ("tuples", "are", "immutable", "and", "are", "fast")

x = t[0]
t.count("are") # return the count of "are"
t.index("are")
t3 = t + t1    # combine tuples
```

### Set (unordered collection of unique items)

```py
x = {"a","b","c","d","e"}
y = {"b","c"}

x.add("a")     # does nothing
y.add("a")
y.discard("a") # or remove("a")
x.difference(y)
x.union(y)
x.intersection(y)
```

### Dictionary (unordered collection accessed by key)

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


## Elements

Python uses zero based indexing; negative indexing is allowed (-1 represents the last element).

### Length

```py
len(obj)
```

- It calls the obj's `__len__()` method.

### Slicing

Slicing does return a new sliced object (start and end_exclusive can be negative): 

```py
s[start:end_exclusive]      # extract from start till end_exclusive
s[start:]                   # extract from start till last
s[:end_exclusive]           # extract from begin till end_exclusive
s[start:end_exclusive:step] # extract from start till end_exclusive
                            # with increment of step
```

### Checking if an element is contained

```py
abc = ["a","b","c","d","e"]
"a" in abc       # True
"a" not in abc   # False
```


## Conditions

### if

```py
if <condition>:
    <one or more indented statements>
elif <condition>:
    <one or more indented statements>
else:
    <one or more indented statements>
```

### pass statement

```py
if <condition>:
    pass   # do nothing
else:
    do_something()
```


## Loops

### while

```py
while <condition>:
    <one or more indented statements>

i = 0
while i < 10:
    print(i)
    i += 1
```

### for

```py
for <variable> in <list>:
     <one or more indented statements>

nums = [1, 2, 3, 4, 5]
for i in nums:
    print(i)
```

The **range type** represents an immutable sequence of numbers used for looping:

```py
range(count)                      # 0..(count - 1)
range(start, end_exclusive)       # start..(end_exclusive - 1)
range(start, end_exclusive, step) # range with step increments
```

The above for-loop example can be written like:

```py
nums = [1, 2, 3, 4, 5]
for i in range(len(nums)):
    print(nums[i])
```

The statements to break out of a loop and to jump to the start are: `break` and `continue`


## Functions

The passed number of arguments must much the function definition, except for the default arguments which are optional:

```py
def my_function(x, y):
    global number   # make it global
    number = 3
    z = 2 * (x + y) # local to function
    return z
    
def my_hello(name="everybody"): # default argument
    return "Hello " + name + "!"
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
my_func(shopkeeper = "Michael Palin", client = "John Cleese")
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
        
a1 = Asteroid(0,0)
a2 = Asteroid(3,4)
print (a1.compute_distance(a2))
```


## Input

```py
n = input("Enter a number: ") # returns a string
n = int(n) + 1
print("Your number plus 1 is ", n)
```


## Import modules

- `import math, random`: every attribute or function are accessed by putting `math.` and `random.` in front of the name like `print(math.pi)` or `print(random.randint(0,9))`

- `import numpy as np`: this renames `numpy` to `np` within the script. Note that the name `numpy` itself is no longer valid.

- `from fibo import fib, fib2`: this imports names from a module directly into the importing module's symbol table. This does not introduce the module name from which the imports are taken in the local symbol table (so in the example, `fibo` is not defined).

- `from fibo import *`: this imports all names that a module defines. Do not use this facility since it introduces an unknown set of names into the interpreter, possibly hiding some things you have already defined.


## Date/Time

```py
import datetime
now = datetime.datetime.now()
past = datetime.datetime(2018, 8, 18, 10, 5, 56, 518515)
print(now)
print(past)
print(now.strftime('%d.%m.%Y %H:%M:%S'))
```


## Packages

Packages are a way of structuring the module namespace by using "dotted module names". For example, the module name `pkgA.modB` designates a submodule named `modB` in a package named `pkgA`.

- `import sound.effects.echo`: this imports an individual module from the package. A function is referenced like `sound.effects.echo.echofilter()`.

- `from sound.effects import echo`: a function is referenced like `echo.echofilter()`.

- `from sound.effects.echo import echofilter`: the function is referenced like `echofilter()`.

The **Python Package Manager** should be called through the correct Python version:

```
python3 -m pip ...   # for Linux/macOS
py -m pip ...        # for Windows
```

Typical commands (invoke `pip` like shown above):

```
pip list
pip show <pkg>
pip install <pkgs>
pip install --upgrade <pkgs>
pip uninstall <pkgs>
```

Small list of packages:

- App framework: `kivy`
- System: `configparser`
- Multimedia: `pillow` `opencv` `simplecv`
- Games: `pygame` `pyglet`
- Network: `requests` `scrapy` `twisted`
- Math: `numpy` `sciPy` `symPy` `matplotlib` `plotly`


## How to use Python instead of Shell

`#!/bin/sh`  →  `#!/usr/bin/env python`

### read file

```py
my_file = open('my_file.txt') # the default mode is 'r'
for line in my_file: # we could also explicitly calling lines = list(my_file)
    do_stuff_with(line.rstrip()) # python always returns the trailing newline
file_text = my_file.read()
lines = file_text.splitlines() # with newline characters removed
my_file.close()
```

### write file

```py
my_file = open('my_file.txt', 'w') # there is also 'a' to append
my_file.write('some text\n')
print('another line', file=my_file) # print adds a newline
my_file.close()
```

### stdin

```py
import sys
for line in sys.stdin:
    do_stuff_with(line.rstrip()) # python always returns the trailing newline
text = sys.stdin.read() # slurp stdin in one go
```

### stdout

```py
print("Hello, stdout.") # functionally same as sys.stdout.write('Hello, stdout.\n')
```

### stderr

```py
print('a logging message.', file=sys.stderr) # functionally same as sys.stderr.write('a logging message.\n')
```

### args

```py
for arg in sys.argv[1:]: # argv[0] is the script name, skip that
    do_stuff_with(arg)
```

### env

```py
import os
os.environ['VAR'] # error is raised if the environment variable is not defined
print(os.environ.get('VAR', 'default')) # returns default value if VAR not defined
```

### path

```py
from pathlib import Path
p = Path('C:\\Windows') # makes the given path
p = Path() # makes a relative path from the current directory
p = Path.cwd() # makes an absolute path from the current directory
p = p.resolve() # makes the path absolute, replaces symbolic links with physical paths and makes all shares UNC
for i in sorted(p.iterdir(), reverse = True): # iterate over directory content
    print(i)
for i in sorted(p.glob('*.py'), reverse = True): # use filename globbing
    print(i)
p.name
p.parent
p.parts
p.stat().st_size
p.stat().st_mtime # print(datetime.datetime.fromtimestamp(p.stat().st_mtime))
p.is_dir() # is existing dir?
p.is_file() # is existing file?
p.unlink()
p.rename(target)
p.mkdir()
p.rmdir()
import shutil
shutil.copytree('src', 'dest') # $ cp -r src dest
shutil.rmtree('a_dir') # $ rm -r a_dir
```

### proc

```py
import subprocess as sp
proc = sp.run(['ls', '-lh']) # run always waits for the process to finish
if proc.returncode != 0:
    # do something else
# The following with statement will automatically close the file after the nested block
with open('./foo', 'w') as foofile:
    sp.run(['ls'], stdout=foofile)
with open('foo') as foofile:
    sp.run(['tr', 'a-z', 'A-Z'], stdin=foofile)
with open('foo.log', 'w') as logfile:
    sp.run(['ls', 'foo bar baz'], stderr=logfile)
import psutil
p = psutil.Process(pid)
p.kill()
for proc in psutil.process_iter():
    if proc.name() == 'my_proc':
        proc.kill()
```
