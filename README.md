Python is case-sensitive.

Help in Python is always available right in the interpreter. If you want 
to know how an object works, all you have to do is call help(obj).
Also useful are dir(obj), which shows you all the object's methods, and 
obj.__doc__, which shows you its documentation string.


py.exe launcher
---------------

In Windows the official python installer uses the py.exe launcher which 
selects the correct version for you. But if you have also non-py aware 
installations in your PATH then you end up with problem using pip. To
execute the correct used by the py.exe launcher issue: 
py -m pip -V
py -m pip install ...

Hint: use where python or where pip to see the installation path and 
      execution order 


Blocks, lines and spaces
------------------------

Code blocks are always preceded by a colon (:) on the previous line. You 
should be aware that the amount of whitespace (or tabs) used for 
indenting code blocks is up to the user, as long as it is consistent 
throughout the script. Most style guides recommend to indent code blocks 
by four spaces instead of a tab (never mix tabs with spaces). 

Whitespaces within lines do not matter and it's possible to continue 
expressions on the next line within parentheses. Also lists can span 
multiple lines. Semicolons separate statements on the same line. 


Comments
--------

From # to the end of the line
(there are no multiple line comments)


(Im)mutability
--------------

Variable names are labels assigned to objects. Assigning variables is 
just like giving a new nickname. 
foo = bar = baz = 3 
those three are just labels for the same object, no new object is created.

Immutable are objects whose internal state/values can't be changed or
altered (the object's methods return a new object):
int, float, bool, str, tuple, complex

Mutable are objects whose internal state/values can be changed after 
creation:
list, set, dict
Shallow copy (with .copy or slicing) only make a copy of the first level 
objects, sublists remain referenced. To copy everything use the module 
copy.deepcopy. 
 
Identity operators:               is      # checks whether two objects point to the same object
Not identity operators:           is not
Object id to check identity:      id(x)
Object type:                      type(x)

In Python functions are using pass-by-assignment, when we call a 
function each parameter is assigned to the object they were passed in, 
each parameter becomes a new nickname to the object that was given in: 

- if we pass in immutable arguments, then we have no way of modifying 
the arguments themselves, it can look like Python uses the pass-by-value. 

- if we pass in mutable arguments, then we can change them, it can look 
like Python uses a pass-by-reference. 


Operators
---------

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

int(x)
float(x)
str(x)
list(x)
round(number, num_digits)
abs(number)
divmod(num1, num2) # returns quotient and remainder

Conditions can be chained:        1 < a < 3

Complex number:                   4.21j

Integer with thousands separator: 1_000

For mutable types a += b is not the same as a = a + b:

When the += operator is used on an object which has an __iadd__ 
(in-place addition) defined the object is modified in place. Otherwise 
it will instead use the plain __add__ and return a new object. 
That is why for mutable types like lists += changes the object's value, 
whereas for immutable types like tuples, strings and integers a new 
object is returned instead (a += b becomes equivalent to a = a + b). 


Conditions
----------

if <condition>:
    <one or more indented statements> 
elif <condition>:
    <one or more indented statements> 
else:
    <one or more indented statements>  
    
    
Loops
-----

for <variable> in <list>:
     <one or more indented statements>
     
nums = [1, 2, 3, 4, 5]
for i in range(len(nums)):
    print(nums[i])
    
range(count)                      # produces a range object from 0, ..., count - 1
range(start, end_exclusive)       # produces a range object from start, ..., end_exclusive - 1
range(start, end_exclusive, step) # produces a range object with step increments
     
while <condition>:
    <one or more indented statements>
    
Special statements: break and continue
     

Strings
-------

"String"
'String'
escape chars like in C with the backslash

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


Bytes
-----

utf8 = "Hello, World!".encode()
print(utf8) # b'Hello, World!'
print(utf8.decode()) # Hello, World!

r = [0x48, 0x65, 0x6C, 0x6C, 0x6F]
Hello = bytes(r)
print(Hello.decode()) # Hello

mybytes = bytes.fromhex("f0f1 f2") # white spaces are ignored
b'\xf0\xf1\xf2'.hex() # "f0f1f2"


Lists (ordered sequence of objects)
-----------------------------------

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


Sets (unordered collection of unique and immutable objects)
-----------------------------------------------------------

x = {"a","b","c","d","e"}
y = {"b","c"}
z = {"c","d"}

z.add("y")
z.discard("y") # or remove("y")
x.difference(y)
x.union(y)
x.intersection(y)


Dictionaries (unordered and accessed by a key)
----------------------------------------------

person = {"first_name" : "John",
          "last_name" : "Doe",
          "age" : 33}

x = person["first_name"] # read
person["first_name"] = "Oliver" # change
person["email"] = "oliver.pfister@gmx.ch" # add
person.pop("email") # or del person["email"]
person.update(person_new) # merges the keys and values of one dictionary into another, overwriting values of the same key


Tuples (immutable lists)
------------------------

t = ("tuples", "are", "immutable", "and", "are", "fast")

x = t[0] # read
t.count("are") # returns the count of the given element
t.index("are") # see list
t3 = t + t1 # combine tuples


Len
---

Did you expect to have something like list.len() or tuple.len()? 
Actually there is something like that, but it is called list.__len__() 
or tuple.__len__(). And what len() really does is, it takes the object 
and tries to call the objects's __len__() method. So essentially, len() 
works only on objects that has a __len__() method. To get the last element
you usually use my_list[-1]. 


Slicing
-------

Slicing does not change the original object.

Extract from start till end_exclusive:   s[start:end_exclusive]
Extract from start till last:            s[start:]
Extract from begin till end_exclusive:   s[:end_exclusive]
Extract from start till end_exclusive
with increment of step:                  s[start:end_exclusive:step]:           

Note: start and end_exclusive can also be negative


Checking if an element is contained or not
------------------------------------------

abc = ["a","b","c","d","e"]
"a" in abc # True
"a" not in abc # False


Functions
---------

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

better to use varargs implemented with tuples and indicated by an *:

def my_min(*args): # args is just a name, you can name that vararg anything
    result = args[0]
    for num in args:
        if num < result:
            result = num
    return result
my_min(4, 5, 6, 7, 2)

** lets you pass a keyworded variable-length of arguments to your function:
   
def my_func(**kwargs):
    for kw in kwargs:
        print(kw, ":", kwargs[kw])
my_func(shopkeeper = "Michael Palin", client = "John Cleese")

		
Date/Time
---------

import datetime
now = datetime.datetime.now()
past = datetime.datetime(2018, 8, 18, 10, 5, 56, 518515)
print(now)
print(past)
print(now.strftime('%d.%m.%Y %H:%M:%S'))

		
Input
-----

n = input("Enter a number: ")
n = int(n) + 1
print("Your number plus 1 is ", n)
          

Import modules
--------------

- import math, random

  Every attribute or function are accessed by putting math. and random. 
  in front of the name: 
  print(math.pi)
  print(random.randint(0,9))

- import numpy as np

  This renames numpy to np within the script. Note that the name numpy 
  itself is no longer valid. 

- from fibo import fib, fib2

  This imports names from a module directly into the importing module's 
  symbol table. This does not introduce the module name from which the 
  imports are taken in the local symbol table (so in the example, fibo is 
  not defined). 

- from fibo import *

  This imports all names that a module defines. Do not use this facility 
  since it introduces an unknown set of names into the interpreter, 
  possibly hiding some things you have already defined. 


Packages
--------

Packages are a way of structuring Python’s module namespace by using 
"dotted module names". For example, the module name A.B designates a 
submodule named B in a package named A. 

- import sound.effects.echo

  This imports an individual module from the package. A function must be 
  referenced like: 
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


class
-----

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


Kivy
----

Install:
https://kivy.org/doc/stable/gettingstarted/installation.html#install-pip

Windows:
py -m pip install --upgrade pip wheel setuptools
py -m pip install kivy[base] kivy_examples

Linux:
python3 -m pip install --upgrade pip wheel setuptools
python3 -m pip install kivy[base] kivy_examples


Python instead of Bash
----------------------

#!/bin/bash             -->         #!/usr/bin/env python

read:
my_file = open('my_file.txt') # the default mode is 'r'
for line in my_file: # we could also explicitly calling lines = list(my_file)
    do_stuff_with(line.rstrip()) # python always returns the trailing newline
file_text = my_file.read()
lines = file_text.splitlines() # with newline characters removed
my_file.close()

write:
my_file = open('my_file.txt', 'w') # there is also 'a' to append
my_file.write('some text\n')
print('another line', file=my_file) # print adds a newline
my_file.close()
	
stdin:
import sys
for line in sys.stdin:
    do_stuff_with(line.rstrip()) # python always returns the trailing newline
text = sys.stdin.read() # slurp stdin in one go

stdout:
print("Hello, stdout.") # functionally same as sys.stdout.write('Hello, stdout.\n')

stderr:
print('a logging message.', file=sys.stderr) # functionally same as sys.stderr.write('a logging message.\n')

args:
for arg in sys.argv[1:]: # argv[0] is the script name, skip that
    do_stuff_with(arg)

env vars:
import os
os.environ['VAR'] # error is raised if the environment variable is not defined
print(os.environ.get('VAR', 'default')) # returns default value if VAR not defined

paths:
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

proc:
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
