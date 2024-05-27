# Python Docs & Notes <!-- omit from toc -->

This document is a reference guide for Python programming. It is a bit more than a simple cheat sheet, but it is not a learning book, you need to have some knowledge and experience about Python programming to understand these notes.


## Table of contents <!-- omit from toc -->

- [Introduction](#introduction)
- [py.exe launcher](#pyexe-launcher)
- [Blocks, lines and spaces](#blocks-lines-and-spaces)
- [Comments](#comments)
- [(Im)mutability](#immutability)
- [Operators](#operators)
- [Conditions](#conditions)
- [Loops](#loops)
- [Strings](#strings)
- [Bytes](#bytes)
- [Lists (ordered sequence of objects)](#lists-ordered-sequence-of-objects)
- [Sets (unordered collection of unique and immutable objects)](#sets-unordered-collection-of-unique-and-immutable-objects)
- [Dictionaries (unordered and accessed by key)](#dictionaries-unordered-and-accessed-by-key)
- [Tuples (immutable lists)](#tuples-immutable-lists)
- [Len](#len)
- [Slicing](#slicing)
- [Checking if an element is contained or not](#checking-if-an-element-is-contained-or-not)
- [Functions](#functions)
- [Date/Time](#datetime)
- [Input](#input)
- [Import modules](#import-modules)
- [Packages](#packages)
- [class](#class)
- [Kivy](#kivy)
- [How to use Python instead of Bash](#how-to-use-python-instead-of-bash)


## Introduction

Python is case-sensitive.

Help in Python is always available right in the interpreter:

- `help(obj)` to know how an object works.
- `dir(obj)` shows you all the object's methods.
- `obj.__doc__` shows the object's documentation string.


## py.exe launcher

In Windows the official python installer uses the py.exe launcher which selects the correct version for you. But if you have also non-py aware installations in your `PATH` then you end up with problem using `pip`. To execute the correct used by the `py.exe` launcher issue:

```
py -m pip -V
py -m pip install ...
```

Hint: use `where python` or `where pip` to see the installation path and execution order.


## Blocks, lines and spaces

Code blocks are always preceded by a colon on the previous line. You should be aware that the amount of whitespace (or tabs) used for indenting code blocks is up to the user, as long as it is consistent throughout the script. Most style guides recommend to indent code blocks by four spaces instead of a tab (never mix tabs with spaces).

Whitespaces within lines do not matter and it's possible to continue expressions on the next line within parentheses. Also lists can span multiple lines. Semicolons separate statements on the same line.


## Comments

From `#` to the end of the line, there are no multiple line comments.


## (Im)mutability

Variable names are labels assigned to objects. Assigning variables is just like giving a new nickname, in `foo = bar = baz = 3` the variable names are just labels for the same object, no new object is created.

1. Immutable are objects whose internal state/values can't be changed or altered (the object's methods return a new object):

   ```
   int, float, bool, str, tuple, complex
   ```

2. Mutable are objects whose internal state/values can be changed after creation:

   ```
   list, set, dict
   ```

   - Shallow copies (with `.copy` or **slicing**) only make a copy of the first level objects, sublists remain referenced. To copy everything use the module `copy.deepcopy`.


To check whether two objects point to the same object us the identity operators `is` (not identity operator `is not`). Get the object id with `id(x)` and the object type with `type(x)`.

In Python functions are using pass-by-assignment, when we call a function each parameter is assigned to the object they were passed in, each parameter becomes a new nickname to the object that was given in:

- If we pass in immutable arguments, then we have no way of modifying 
the arguments themselves, it can look like Python uses the pass-by-value.

- If we pass in mutable arguments, then we can change them, it can look 
like Python uses a pass-by-reference.


## Operators

```
Assign:                           = += -= *= /= **= %= //=
For inc/dec use:                  x += 1  x -= 1
Math operators:                   + - * / %
Exponentiation operator:          **
Truncation or floor division
(truncates to negative infinity): //
Comparison operators:             < <= > >= == !=
Boolean values:                   True False
Boolean operators:                and or not
Bitwise operators:                ~ | & ^
Shift operators:                  << >>
Placeholder do nothing operation: pass
```

```py
int(x)
float(x)
str(x)
list(x)
round(number, num_digits)
abs(number)
divmod(num1, num2) # returns quotient and remainder
```

Conditions can be chained: `1 < a < 3`

Complex number: `4.21j`

Integer with thousands separator: `1_000`

For mutable types `a += b` is not the same as `a = a + b`:

When the `+=` operator is used on an object which has an `__iadd__` (in-place addition) defined, the object is modified in place. Otherwise it will instead use the plain `__add__` and return a new object. That is why for mutable types like lists `+=` changes the object's value, whereas for immutable types like tuples, strings and integers a new object is returned instead (`a += b` becomes equivalent to `a = a + b`).


## Conditions

```py
if <condition>:
    <one or more indented statements>
elif <condition>:
    <one or more indented statements>
else:
    <one or more indented statements>
```


## Loops

```py
for <variable> in <list>:
     <one or more indented statements>
```

```py
nums = [1, 2, 3, 4, 5]
for i in range(len(nums)):
    print(nums[i])
    
range(count)                      # produces a range object from 0, ..., count - 1
range(start, end_exclusive)       # produces a range object from start, ..., end_exclusive - 1
range(start, end_exclusive, step) # produces a range object with step increments
```

```py
while <condition>:
    <one or more indented statements>
```

Special statements: `break` and `continue`


## Strings

Strings can either be single-quoted or double-quoted, the strings can contain backslash escapes like `\\`  `\'`  `\"`  `\r` `\n` `\t`. Use `\ooo` for the octal character `ooo` and `\xhh` for the hex character `hh`. Unicode characters are represented with `\uxxxx` or `\Uxxxxxxxx`. To have a string span multiple lines, place a backslash at the end of the line.

Strings can optionally be prefixed with `r` or `R`, such strings are called raw strings and treat backslashes as literal characters.

```py
concatenation = "string1" + "String2"
repetition = 4 * "string"

len("string")
text.upper()
text.lower()
text.find(sub, start, end_exclusive) # start and end_exclusive are optional
text.rfind(sub, start, end_exclusive) # start and end_exclusive are optional
"1,2,3".split(",") # ['1', '2', '3']
import re # use regular expression to split
re.split(r'[^a-zA-Z]', text)
text.replace(old, new)
import re # use regular expression to replace 
re.sub(r'pattern', r'replacement', text) # always prefix patterns containing \ escapes with raw strings (r in front of the string),
                                         # otherwise the \ is interpreted as an escape sequence
"text with {} placeholders {} it".format("two","in")
"Art {:5d}, Price {:8.2f}".format(453, 59.058) # "Art   453, Price    59.06"
"{:04d} {:04d} {:04d}".format(1, -17, 33) # 0001 -017 0033
```


## Bytes

```py
utf8 = "Hello, World!".encode()
print(utf8) # b'Hello, World!'
print(utf8.decode()) # Hello, World!

r = [0x48, 0x65, 0x6C, 0x6C, 0x6F]
Hello = bytes(r)
print(Hello.decode()) # Hello

mybytes = bytes.fromhex("f0f1 f2") # white spaces are ignored
b'\xf0\xf1\xf2'.hex() # "f0f1f2"
```


## Lists (ordered sequence of objects)

```py
cities = ["Vienna", "London", "Paris", 
          "Berlin", "Zurich", "Hamburg"]
person = [["Marc", "Mayer"], 
          ["17, Oxford Str", "12345", "London"], 
          "07876-7876"]
          
print(person[1][0]) # 17, Oxford Str
cities[-1] # "Hamburg"

cities.insert(i, "Bern")
person.pop(i) # returns and removes the ith element, pop(-1) is equivalent to pop() and pops the last element
cities.append("Locarno")
cities.extend(person) # adds the person list not as a sublist but as a continuation of the list
cities.remove("Berlin") # remove the first occurrence of "Berlin" from the cities list
cities.index("Berlin") # find the position of an element within a list
cities.index("Berlin", start) # search starting at start
cities.index("Berlin", start, end_exclusive) # search limited by start and end_exclusive
```


## Sets (unordered collection of unique and immutable objects)

```py
x = {"a","b","c","d","e"}
y = {"b","c"}
z = {"c","d"}

z.add("y")
z.discard("y") # or remove("y")
x.difference(y)
x.union(y)
x.intersection(y)
```


## Dictionaries (unordered and accessed by key)

```py
person = {"first_name" : "John",
          "last_name" : "Doe",
          "age" : 33}

x = person["first_name"] # read
person["first_name"] = "Oliver" # change
person["email"] = "oliver.pfister@gmx.ch" # add
person.pop("email") # or del person["email"]
person.update(person_new) # merges the keys and values of one dictionary into another, overwriting values of the same key
```


## Tuples (immutable lists)

```py
t = ("tuples", "are", "immutable", "and", "are", "fast")

x = t[0] # read
t.count("are") # returns the count of the given element
t.index("are") # see list
t3 = t + t1 # combine tuples
```


## Len

Did you expect to have something like `list.len()` or `tuple.len()`? Actually there is something like that, but it is called `list.__len__()` or `tuple.__len__()`. And what `len()` really does is, it takes the object and tries to call the objects's `__len__()` method. So essentially, `len()` works only on objects that have a `__len__()` method. To get the last element you usually use `list[-1]`.


## Slicing

Slicing does not change the original object. Zero based indexing is used, start and end_exclusive can be negative, in which case index -1 represents the last element.

```py
s[start:end_exclusive]      # extract from start till end_exclusive
s[start:]                   # extract from start till last
s[:end_exclusive]           # extract from begin till end_exclusive
s[start:end_exclusive:step] # extract from start till end_exclusive
                            # with increment of step
```


## Checking if an element is contained or not

```py
abc = ["a","b","c","d","e"]
"a" in abc       # True
"a" not in abc   # False
```


## Functions

```py
def MyFunction(x, y):
    global number	# global is only needed if you want to write to it
    number = 3
    z = 2 * (x + y) # variable names are by default local to the function
    return z
    
def hello(name="everybody"):
    result = "Hello " + name + "!")
    
def my_min(nums):
    result = nums[0]
    for num in nums:
        if num < result:
            result = num
    return result
my_min([4, 5, 6, 7, 2])
```

Better to use varargs implemented with tuples and indicated by an `*`:

```py
def my_min(*args): # args is just a name, you can name that vararg anything
    result = args[0]
    for num in args:
        if num < result:
            result = num
    return result
my_min(4, 5, 6, 7, 2)
```

With `**` we can pass keyworded variable-length arguments to your function:

```py
def my_func(**kwargs):
    for kw in kwargs:
        print(kw, ":", kwargs[kw])
my_func(shopkeeper = "Michael Palin", client = "John Cleese")
```


## Date/Time

```py
import datetime
now = datetime.datetime.now()
past = datetime.datetime(2018, 8, 18, 10, 5, 56, 518515)
print(now)
print(past)
print(now.strftime('%d.%m.%Y %H:%M:%S'))
```


## Input

```py
n = input("Enter a number: ")
n = int(n) + 1
print("Your number plus 1 is ", n)
```


## Import modules

- import math, random

  Every attribute or function are accessed by putting `math.` and `random.` in front of the name:
  print(math.pi)
  print(random.randint(0,9))

- import numpy as np

  This renames numpy to np within the script. Note that the name numpy itself is no longer valid.

- from fibo import fib, fib2

  This imports names from a module directly into the importing module's symbol table. This does not introduce the module name from which the imports are taken in the local symbol table (so in the example, fibo is not defined).

- from fibo import *

  This imports all names that a module defines. Do not use this facility since it introduces an unknown set of names into the interpreter, possibly hiding some things you have already defined.


## Packages

Packages are a way of structuring Python’s module namespace by using "dotted module names". For example, the module name A.B designates a submodule named B in a package named A.

- import sound.effects.echo

  This imports an individual module from the package. A function must be referenced like:
  sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)

- from sound.effects import echo
  We can call the function like:
  echo.echofilter(input, output, delay=0.7, atten=4)

- from sound.effects.echo import echofilter
  We can call the function directly:
  echofilter(input, output, delay=0.7, atten=4)

System:
configparser # ini files

Multimedia:
pillow
opencv
simplecv

Games:
pygame
pyglet

Network:
requests
scrapy
twisted

Math:
numpy
sciPy
symPy
matplotlib
plotly

Machine learning:
keras
tensorflow


## class

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


## Kivy

[Kivy](https://kivy.org) is an open source software library for the rapid development of applications equipped with novel user interfaces, such as multi-touch apps.

Windows installation:

```bat
py -m pip install --upgrade pip wheel setuptools
py -m pip install kivy[base] kivy_examples
```

Linux installation:

```bash
python3 -m pip install --upgrade pip wheel setuptools
python3 -m pip install kivy[base] kivy_examples
```


## How to use Python instead of Bash

`#!/bin/bash`  →  `#!/usr/bin/env python`

read:

```py
my_file = open('my_file.txt') # the default mode is 'r'
for line in my_file: # we could also explicitly calling lines = list(my_file)
    do_stuff_with(line.rstrip()) # python always returns the trailing newline
file_text = my_file.read()
lines = file_text.splitlines() # with newline characters removed
my_file.close()
```

write:

```py
my_file = open('my_file.txt', 'w') # there is also 'a' to append
my_file.write('some text\n')
print('another line', file=my_file) # print adds a newline
my_file.close()
```

stdin:

```py
import sys
for line in sys.stdin:
    do_stuff_with(line.rstrip()) # python always returns the trailing newline
text = sys.stdin.read() # slurp stdin in one go
```

stdout:

```py
print("Hello, stdout.") # functionally same as sys.stdout.write('Hello, stdout.\n')
```

stderr:

```py
print('a logging message.', file=sys.stderr) # functionally same as sys.stderr.write('a logging message.\n')
```

args:

```py
for arg in sys.argv[1:]: # argv[0] is the script name, skip that
    do_stuff_with(arg)
```

env vars:

```py
import os
os.environ['VAR'] # error is raised if the environment variable is not defined
print(os.environ.get('VAR', 'default')) # returns default value if VAR not defined
```

paths:

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

proc:

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
