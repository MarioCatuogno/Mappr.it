# Python 101 : Introduction
*An introduction to Python.*

## Table of content

- [1. Introduction](#introduction)
    - [1.1 Python setup](#python-setup)
- [2. Variables and Types](#variables-and-types)
    - [2.1 Python numbers](python-numbers)
    - [2.2 Python strings](#python-strings)
    - [2.3 Python lists](#python-lists)
    - [2.4 Python tuples](#python-tuples)
    - [2.5 Python dictionary](#python-dictionary)
    - [2.6 Change type of variable](#change-type-of-variable)

## Introduction

**Python** was created by **Guido Van Rossum** in the early 90s and is nowadays one of the most used high-level programming language. Its design philosophy emphasizes code readability, and its syntax allows programmers to express concepts in fewer lines of code than possible.

Python supports multiple programming paradigms, including **object-oriented**, **imperative** and **functional programming** or **procedural styles**. It is a very versatile language and can be used either in data science or programming projects.

The syntax is very easy, in the following example there are some lines of code suited to do basic calculations.

``` python
# Addition and subtraction
print(1 + 2)
print(3 - 2)

# Multiplication and division
print(2 * 3)
print(6 / 2)

# Exponentiation
print(2 ** 3)

# Complex operation
print(100 * (1.1 ** 7) / (3.5 * 2))
```

#### Python setup

## Variables and types

**Variables** are specific, case-sensitive name which can be used to call up values through variable name making the code *reproducible*. For example:

``` python
# Create height and weight Variables
height = 170
weight = 80

# Calculate the BMI and assign the value to a variable
bmi = weight / height ** 2
```

There are different **types** of variable in Python. To print the type of a variable it is possible to use the `type()` function. Python has five standard data types:

* Numbers
* Strings
* Lists
* Tuples
* Dictionary

#### Python numbers

Number data types store numeric values. Number objects are created when you assign a value to them.

Type | Description | Example
--- | ---
**float** | Floating point real values | 15.3
**int** | Signed integers | -387
**long** | Long integers, they can also be represented in octal and hexadecimal | 51924361L
**complex** | Complex number | -.6545+0J

#### Python strings

**Strings** in Python are identified as a contiguous set of characters represented in the quotation marks. Subsets of strings can be taken using the slice operator (`[]` and `[:]`) with indexes starting at 0 in the beginning of the string and working their way from -1 at the end. The plus (`+`) sign is the string concatenation operator and the asterisk (`*`) is the repetition operator. For example:

``` python
# Create a string called str
str = 'Hello World!'

print(str)          # Prints complete string
print(str[0])       # Prints first character of the string
print(str[2:5])     # Prints characters starting from 3rd to 5th
print(str[2:])      # Prints string starting from 3rd character
print(str * 2)      # Prints string two times
print(str + "TEST") # Prints concatenated string
```

There is a special kind of variable called **boolean**, which is a type to represent logical values. For example:

``` python
# Create a boolean called bool
bool = True
```

#### Python lists

**Lists** are the most versatile of Python's compound data types. A list contains items separated by commas and enclosed within square brackets (`[]`). All the items belonging to a list can be of different data type The values stored in a list can be accessed using the slice operator (`[]` and `[:]`) with indexes starting at 0 in the beginning of the list and working their way to end -1. The plus (`+`) sign is the list concatenation operator, and the asterisk (`*`) is the repetition operator. For example:

``` python
# Create a list called list and one called tinylist
list = [ 'abcd', 786 , 2.23, 'john', 70.2 ]
tinylist = [123, 'john']

print(list)            # Prints complete list
print(list[0])         # Prints first element of the list
print(list[1:3])       # Prints elements starting from 2nd till 3rd
print(list[2:])        # Prints elements starting from 3rd element
print(tinylist * 2)    # Prints list two times
print(list + tinylist) # Prints concatenated lists
```

#### Python tuples

A **tuple** is another sequence data type that is similar to the list. A tuple consists of a number of values separated by commas. Unlike lists, however, tuples are enclosed within parentheses. The main differences between lists and tuples are: Lists are enclosed in brackets (`[]`) and their elements and size can be changed, while tuples are enclosed in parentheses (`()`) and cannot be updated. Tuples can be thought of as read-only lists. For example:

``` python
# Create a tuple called tuple and one called tinytuple
tuple = ( 'abcd', 786 , 2.23, 'john', 70.2  )
tinytuple = (123, 'john')

print(tuple)             # Prints complete tuple
print(tuple[0])          # Prints first element of the tuple
print(tuple[1:3])        # Prints elements starting from 2nd till 3rd
print(tuple[2:])         # Prints elements starting from 3rd element
print(tinytuple * 2)     # Prints tuple two times
print(tuple + tinytuple) # Prints concatenated tuples
```

#### Python dictionary

Python's **dictionaries** are kind of hash table type. They work like associative arrays and consist of key-value pairs. A dictionary key can be almost any Python type, but are usually numbers or strings. Values, on the other hand, can be any arbitrary Python object. Dictionaries are enclosed by curly braces (`{}`) and values can be assigned and accessed using square braces (`[]`). Dictionaries have no concept of order among elements. It is incorrect to say that the elements are "*out of order*"; they are simply unordered. For example:

``` python
# Create a dictionary called dict and one called tinydict
dict = {}
dict['one'] = "This is one"
dict[2]     = "This is two"
tinydict = {'name': 'john','code':6734, 'dept': 'sales'}

print(dict['one'])       # Prints value for 'one' key
print(dict[2])           # Prints value for 2 key
print(tinydict)          # Prints complete dictionary
print(tinydict.keys())   # Prints all the keys
print(tinydict.values()) # Prints all the values
```

#### Change type of variable

It is possible to change the type of variable into another kind using the following functions:

* `str()`: transform the variable into a type **string**
* `int()`: transform the variable into a type **integer**
* `float()`: transform the variable into a type **float**
* `bool()`: transform the variable into a type **bool**
