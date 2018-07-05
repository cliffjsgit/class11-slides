---
title: 'Class 11: Chapter 19: The Goodies'
separator: '\-\-\-\-\-'
verticalSeparator: '\+\+\+\+\+'
theme: 'moon'
revealOptions:
    transition: 'fade'
---

### ITSE-1402 Intermediate Python
<span style="font-family:Helvetica Neue; font-weight:bold; color:#e49436">Class 11: Chapter 19: The Goodies</span>
<br /><br />
##### [https://coder.run/1402-class11](https://coder.run/1402-class11)

-----

##### Chapter 19: The Goodies

+++++

[https://coder.run/1402-chap19](https://coder.run/1402-chap19)

+++++

The material we have covered in this class so far is fundamentals you need to know to get coding just about anything you wish to in Python.

Note:
Obviously this is said a little tongue in cheek, but it's a rather true statement.  We simply can't talk about every module.. even those included in the built-in library. We can't discuss every exception. We can't hit every single concept that you might use in python. Not only because you'll get buried in information, but also because you won't learn it.

+++++

Now we will talk about some alternative ways to do things and in some cases some more pythonic.

Note:
The author of our book realizes this and has saved some material toward the end to avoid overwhelming you. Obviously even with this chapter, you still won't know everything. Practice, Google, and reading documentation makes perfect. We still have 2 more classes to go, but it's a good time to talk about it. When you leave this class don't stop coding. If python is something you are serious in learning, don't stop learning when you leave the class. If you just don't do anything with the knowledge you've gotten when you leave the class, you will lose it and it will happen more quickly than you might think. This is, in part, why I encourage you to do codefights. This simple site gives you an exercise you can do nearly every day and you should do them, if nothing else, to keep sharp on the concepts. A man smarter than I that does more coding than I recommended this site to me and told me to make sure I code at least once a day even if it's just for a few minutes. I'll be the first to admit I don't do this, but it doesn't make it any less true.

+++++

##### 19.1 Conditional expressions

+++++

We saw conditional statements in Section 5.4. Conditional statements are often used to choose one of two values:

```python
if x > 0:
    y = math.log(x)
else:
    y = float('nan')
```

Note:
This statement checks whether x is positive. If so, it computes math.log. If not, math.log would raise a ValueError. To avoid stopping the program, we generate a “NaN”, which is a special floating-point value that represents “Not a Number”.

+++++

We can write this statement more concisely using a conditional expression:

```python
y = math.log(x) if x > 0 else float('nan')
```

Note:
You can almost read this line like English: “y gets log-x if x is greater than 0; otherwise it gets NaN”.

+++++

Recursive functions can sometimes be rewritten using conditional expressions. Here is a recursive version of factorial:

```python
def factorial(n):
    if n == 0:
        return 1
    else:
        return n * factorial(n-1)
```

+++++

We can rewrite it like this:

```python
def factorial(n):
    return 1 if n == 0 else n * factorial(n-1)
```

+++++

Another use of conditional expressions is handling optional arguments. For example, here is the init method from GoodKangaroo:

```python
def __init__(self, name, contents=None):
  	self.name = name
  	if contents == None:
    		contents = []
  	self.pouch_contents = contents
```

+++++

We can rewrite this one like this:

```python
def __init__(self, name, contents=None):
    self.name = name
    self.pouch_contents = [] if contents == None else contents 
```

Note:
In general, you can replace a conditional statement with a conditional expression if both branches contain simple expressions that are either returned or assigned to the same variable.

+++++

##### 19.2 List comprehensions

+++++

In Section 10.7 we saw the map and filter patterns. 

```python
def capitalize_all(t):
    res = []
    for s in t:
        res.append(s.capitalize())
    return res
```

Note:
For example, this function takes a list of strings, maps the string method capitalize to the elements, and returns a new list of strings.

+++++

We can write this more concisely using a list comprehension:

```python
def capitalize_all(t):
    return [s.capitalize() for s in t]
```

Note:
The bracket operators indicate that we are constructing a new list. The expression inside the brackets specifies the elements of the list, and the for clause indicates what sequence we are traversing.
The syntax of a list comprehension is a little awkward because the loop variable, s in this example, appears in the expression before we get to the definition.

+++++

List comprehensions can also be used for filtering.

```python
def only_upper(t):
    res = []
    for s in t:
        if s.isupper():
            res.append(s)
    return res
```

Note:
For example, this function selects only the elements of t that are upper case, and returns a new list.

+++++

```python
def only_upper(t):
    return [s for s in t if s.isupper()]
```

Note:
List comprehensions are concise and easy to read, at least for simple expressions. And they are usually faster than the equivalent for loops, sometimes much faster.
List comprehensions are harder to debug because you can’t put a print statement inside the loop. You should only use them if the computation is simple enough that you are likely to get it right the first time.

+++++

##### 19.3 Generator expressions

+++++    
    
We can rewrite it using a list comprehension

```python
g = (x**2 for x in range(5))
# <generator object <genexpr> at 0x7f4c45a786c0>
next(g)    # 0
next(g)    # 1

for val in g:
	print(val)

# 4
# 9
# 16
```

Note:
Generator expressions are similar to list comprehensions, but with parentheses instead of square brackets.
The result is a generator object that knows how to iterate through a sequence of values. But unlike a list comprehension, it does not compute the values all at once; it waits to be asked. The built-in function next gets the next value from the generator.
When you get to the end of the sequence, next raises a StopIteration exception. You can also use a for loop to iterate through the values.
The generator object keeps track of where it is in the sequence, so the for loop picks up where next left off.

+++++

Generator expressions are often used with functions like sum, max, and min:

```python
sum(x**2 for x in range(5))
30
```

+++++

##### 19.4 any and all

+++++

Python provides a built-in function, any, that takes a sequence of boolean values and returns True if any of the values are True. It works on lists:

```python
any([False, False, True])   # True
```

But it is often used with generator expressions:
```python
any(letter == 't' for letter in 'monty')  # True
```

Note:
That example isn’t very useful because it does the same thing as the in operator. 

+++++

We could use any to rewrite some of the search functions we wrote in Section 9.3. For example, we could write avoids like this:

```python
def avoids(word, forbidden):
    return not any(letter in forbidden for letter in word)
```

Note:
The function almost reads like English, “word avoids forbidden if there are not any forbidden letters in word.”
Using any with a generator expression is efficient because it stops immediately if it finds a True value, so it doesn’t have to evaluate the whole sequence.

+++++

Python provides another built-in function that works just like any, all, and returns True if every element of the sequence is True.

+++++

##### 19.5 Sets

+++++

Previously we used dictionaries to find the words that appear in a document but not in a word list.

```python
def subtract(d1, d2):
    res = dict()
    for key in d1:
        if key not in d2:
            res[key] = None
    return res
```

Note:
The function written takes d1, which contains the words from the document as keys, and d2, which contains the list of words. It returns a dictionary that contains the keys from d1 that are not in d2.
In all of these dictionaries, the values are None because we never use them. As a result, we waste some storage space.

+++++

Python provides another built-in type, called a set, that behaves like a collection of dictionary keys with no values. Adding elements to a set is fast; so is checking membership. And sets provide methods and operators to compute common set operations.

```python
def subtract(d1, d2):
    return set(d1) - set(d2)
```

Note:
For example, set subtraction is available as a method called difference or as an operator, -. So we can rewrite subtract like this. 
The result is a set instead of a dictionary, but for operations like iteration, the behavior is the same.

+++++

Some of the exercises in this book can be done concisely and efficiently with sets. For example, here is a solution to has_duplicates, from Exercise 7, that uses a dictionary:

```python
def has_duplicates(t):
    d = {}
    for x in t:
        if x in d:
            return True
        d[x] = True
    return False
```

Note:
When an element appears for the first time, it is added to the dictionary. If the same element appears again, the function returns True.

+++++

Using sets, we can write the same function like this:

```python
def has_duplicates(t):
    return len(set(t)) < len(t)
```

Note:
An element can only appear in a set once, so if an element in t appears more than once, the set will be smaller than t. If there are no duplicates, the set will be the same size as t.

+++++

We can also use sets to do some of the exercises in Chapter 9. For example, here’s a version of uses_only with a loop:

```python
def uses_only(word, available):
    for letter in word: 
        if letter not in available:
            return False
    return True
```

+++++

uses_only checks whether all letters in word are in available. We can rewrite it like this:

```python
def uses_only(word, available):
    return set(word) <= set(available)
```

Note:
The <= operator checks whether one set is a subset or another, including the possibility that they are equal, which is true if all the letters in word appear in available.

+++++

##### 19.6 Counters

+++++

A Counter is like a set, except that if an element appears more than once, the Counter keeps track of how many times it appears.

+++++

Counter is defined in a standard module called collections, so you have to import it. You can initialize a Counter with a string, list, or anything else that supports iteration:

```python
from collections import Counter
count = Counter('parrot')
# Counter({'r': 2, 't': 1, 'o': 1, 'p': 1, 'a': 1})
count['d']
# 0
```

Note:
Counters behave like dictionaries in many ways; they map from each key to the number of times it appears. As in dictionaries, the keys have to be hashable.
Unlike dictionaries, Counters don’t raise an exception if you access an element that doesn’t appear. Instead, they return 0:

+++++

We can use Counters to rewrite is_anagram from Exercise 6:

```python
def is_anagram(word1, word2):
    return Counter(word1) == Counter(word2)
```

Note:
If two words are anagrams, they contain the same letters with the same counts, so their Counters are equivalent.

+++++

Counters provide methods and operators to perform set-like operations, including addition, subtraction, union and intersection.

```python`
count = Counter('parrot')
for val, freq in count.most_common(3):
    print(val, freq)
# r 2
# p 1
# a 1
```

Note:
As seen here, they also provide an often-useful method, most_common, which returns a list of value-frequency pairs, sorted from most common to least.

+++++

##### 19.7 defaultdict

The collections module also provides defaultdict, which is like a dictionary except that if you access a key that doesn’t exist, it can generate a new value on the fly.

+++++

When you create a defaultdict, you provide a function that’s used to create new values. The built-in functions that create lists, sets, and other types can be used as factories:

```python
from collections import defaultdict
d = defaultdict(list)
t = d['new key']
print(t)
[]
t.append('new value')
print(d)
defaultdict(<class 'list'>, {'new key': ['new value']})
```

Note:
A factory is a function that produces objects.
Notice that the argument is list, which is a class object, not list(), which is a new list. The function you provide doesn’t get called unless you access a key that doesn’t exist.
If you are making a dictionary of lists, you can often write simpler code using defaultdict.

+++++

This is an example of code that can be made simpler with defaultdict:

```python
def all_anagrams(filename):
    d = {}
    for line in open(filename):
        word = line.strip().lower()
        t = signature(word)
        if t not in d:
            d[t] = [word]
        else:
            d[t].append(word)
    return d
```

+++++

This can be simplified using setdefault, which you might have used before:

```python
def all_anagrams(filename):
    d = {}
    for line in open(filename):
        word = line.strip().lower()
        t = signature(word)
        d.setdefault(t, []).append(word)
    return d
```

Note:
This solution has the drawback that it makes a new list every time, regardless of whether it is needed. For lists, that’s no big deal, but if the factory function is complicated, it might be.

+++++

We can avoid this problem and simplify the code using a defaultdict:

```python
def all_anagrams(filename):
    d = defaultdict(list)
    for line in open(filename):
        word = line.strip().lower()
        t = signature(word)
        d[t].append(word)
    return d
```

+++++

##### 19.8 Named tuples

+++++

Many simple objects are basically collections of related values. For example, the Point object defined in Chapter 15 contains two numbers, x and y. 

+++++

When you define a class like this, you usually start with an init method and a str method:

```python
class Point:
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

    def __str__(self):
        return '(%g, %g)' % (self.x, self.y)
```

Note:
This is a lot of code to convey a small amount of information. 

+++++

Python provides a more concise way to say the same thing:

```python
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
# <class '__main__.Point'>
```

Note:
The first argument is the name of the class you want to create. The second is a list of the attributes Point objects should have, as strings. The return value from namedtuple is a class object.
Point automatically provides methods like \_\_init\_\_ and \_\_str\_\_ so you don’t have to write them.

+++++ 

To create a Point object, you use the Point class as a function:

```python
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
print(p)
# Point(x=1, y=2)
```

Note:
The init method assigns the arguments to attributes using the names you provided. The str method prints a representation of the Point object and its attributes.

+++++

You can access the elements of the named tuple by name or as a normal tuple.

```python
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
print((p.x, p.y))
# (1, 2)
print((p[0], p[1]))
# (1, 2)
x, y = p
print((x, y))
# (1, 2)
```

+++++

Named tuples provide a quick way to define simple classes. The drawback is that simple classes don’t always stay simple. You might decide later that you want to add methods to a named tuple. In that case, you could define a new class that inherits from the named tuple:

```python
class Pointier(Point):
    # add more methods here
```

Note:
Or you could switch to a conventional class definition.

+++++

##### 19.9 Gathering keyword args

+++++

In Section 12.4, we saw how to write a function that gathers its arguments into a tuple:

```python
def printall(*args):
    print(args)
printall(1, 2.0, '3')
# (1, 2.0, '3')
printall(1, 2.0, third='3')
# TypeError: printall() got an unexpected keyword argument 'third'
```

Note:
You can call this function with any number of positional arguments (that is, arguments that don’t have keywords):
But the * operator doesn’t gather keyword arguments.

+++++

To gather keyword arguments, you can use the ** operator:

```python
def printall(*args, **kwargs):
    print(args, kwargs)
printall(1, 2.0, third='3')
# (1, 2.0) {'third': '3'}
```

You can call the keyword gathering parameter anything you want, but kwargs is a common choice. The result is a dictionary that maps keywords to values

+++++

If you have a dictionary of keywords and values, you can use the scatter operator, ** to call a function:

```python
d = dict(x=1, y=2)
Point(**d)
# Point(x=1, y=2)
d = dict(x=1, y=2)
Point(d)
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# TypeError: __new__() missing 1 required positional argument: 'y'
```

Note:
Without the scatter operator, the function would treat d as a single positional argument, so it would assign d to x and complain because there’s nothing to assign to y.

+++++

When you are working with functions that have a large number of parameters, it is often useful to create and pass around dictionaries that specify frequently used options.

+++++

[https://www.youtube.com/watch?v=OSGv2VnC0go](https://www.youtube.com/watch?v=OSGv2VnC0go)

-----

Homework is 19.1