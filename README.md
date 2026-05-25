### Simple Python refresher for data science/general needs

(Jupyter Notebooks taken from Prof. Galen Egan's Foundations of Data Science course, Seattle University 2024)


Python fundamentals
- Variables
- Types
- Arithmetic
- Ctrl flow


Basics
- What is Python?
- - High-level language; interpreted (don’t need to compile code before running it) and dynamically-typed (type errors not checked before runtime -> runtime failures can occur)
- What is Jupyter Notebook?
- - Nice for combining text (supports both markdown and latex) with code
- - - Either text or code can go into a cell
- - Code gets executed via Python interpreter (or kernel) on current machine
- - Execute a current code cell via Shift + Enter


Var assignment + data types
- Integers, floating point numbers
- F-strings: ```print(f”string string2 {var substitution}”)```
- Check data type of variable: type(<variable>)
- Simple math
  - Specialized arithmetic
    - ```//``` : “floor” of a floating point value result
    - ```%```: integer remainder of a floating point value result
    - ```X ** y```: x to the power of y (exponent)
- The Py “math” module
  - What is a module? A collection of functionality that can be imported/used in script
    - ```import <module>```, so import math at top of script
  - ```math.xxx``` (```xxx``` being the name of the variable, function, or other object)
  - ```math.pi```
  - ```math.sqrt(<num>)```
  - ```math.sin(x), math.cos(x), math.exp(x)``` (i.e. e^x), ```math.log(x), math.log(x, y)```
  - ```math.expm1(x)``` - avoids loss of precision involved specifically in ```e^x - 1``` calculations, when ```x``` is very small (e.g. ```x = 10-20```)
  - E.g. sin^2(2) + cos^2(2) -> ```(math.sin(2) ** 2) + (math.cos(2) ** 2)```
  - E.g. sin(2 * pi) -> ```math.sin(2 * math.pi)``` = -2.4492935982947064e-16)
    - Why? We expected 0; due to *Floating-point precision limitations*
      - pi is an irrational number with infinite decimal places, and Python’s math.pi is 64-bit floating-point approximation accurate to 15-17 decimals -> when multiplying with something else, ultimately multiplying by approximation, leading to a tiny rounding error
      - What if we need to check if a result is 0 (when dealing with floating-point prec. limitations? 
        - ```math.isclose(result, 0, abs_tol=1e-9)```
        - We can also round the result (```round(math.sin(2*math.pi), 10) # returns 0.0```)
        - What if we need to deal with exact values throughout a whole calculation set? ```sympy``` library
          - Install and import sympy (don’t use math as pi’s library)
            - E.g. ```sympy.sin(2 * sympi.pi)``` -> 0
- What if we need help with a function (i.e. don’t understand what it does)?
  - ```math.<func name>?``` -> execute cell, returns a documentation string # This didn’t work…but just hover over mystery fn next time?


Strings
- Python has different "flavors" of strings to hold characters
- Concatenation: ```<string1> + <string2>``` -> <string1string2>
- Replace: ```.replace(<part of str to be replaced in original str>, <replacement>)```
- Capitalize full string: ```.upper(str)```
- Get index (0-indexed) of a char or start of substring in a string: ```.index("substr")```
- How do we split a string? Use desired char to split on, then ```str.split("<splitter>")```
  - Should return a ```List``` of the substrings that were split up
  - Does NOT mutate the original string
- How do we get the last element of a List? ```list[-1]```


Lists
- Lists (in Python) are:
  - Mutable
  - Ordered
  - Container objects (can store other 'objects' (of different types))
  - 0-indexed 
- Initialized with ```[ ]``` square brackets
- Length of list: ```len(my_list)```
- As mentioned: get the last element of a List via ```list[-1]```
- How do we access _multiple elements_ at a time? *Slicing* via ```:``` operator
  - E.g. ```my_list[:2]``` will return everything from 0 _up to_ (and not including) 2
  - E.g. ```my_list[2:]``` will return everything from 2 up to the end of the list (included)
- How do we _extract_ elements N at a time? Via ```::``` operator
  - E.g. ```my_list[::N]```
  - For every extraction, start from current index + 1, move forward N times, then extract
    - ![List extraction example](img/list_extr.png)
- Copy existing list into new list (shallow copy)
  - Slicing: ```list_b = list_a[:]```
  - ```list()``` constructor: ```list_b = list(list_a)```
  - ```.copy()``` (Python 3.3+): ```list_b = list_a.copy()```
  - List comprehension (i.e. using one-line for loop): ```list_b = [item for item in list_a]```
- Deep copy list: need ```deepcopy``` from ```copy``` module
  - For nested objects (e.g. nested lists)
- List examples:
  - What does this return? ```all(item in list_a for item in list_b)```
    - True
    - Grabs an item from list_b and checks if it exists in list_a
    - all() only returns True if every comparison returns True
    - Nested loop (bad complexity)

Sets
- A list but restricted to unique elements only
- ```set(list_x)```: returns a set of unique values
- ```set_1.union(set_2)```: returns a unified set
- ```set_1.intersection(set_2)```: returns intersection
- ```set_1.symmetric_difference(set_2)```: returns the opposite of intersection

Tuples
- An _immutable_ list
Initialized with ```( )``` parentheses
- Trying to update an element will throw an error
- Slicing works the same as for a list


Dictionaries
- A list but each element being a key-value pair
- Access values by passing in the corresponding key
- Initialized with ```{ }``` curly brackets
  - e.g. for element in sample_dict of "key_1: value_1", ```sample_dict[key_1]``` should return ```value_1```
- Keys can be integers or strings
- Keys can be of different types within same dict, but ideally should be same type
  - ![Dictionary diff type e.g.](img/dict.png)
- Particularly useful for storing numerical values associated with easily-remembered descriptions for the values as the keys
- Nested dictionaries: the nested dict can be stored with ```{ }``` as value of k-v pair 
  - Note: Python by default passes only the top-level keys into the loop. Won't have access to key-value pairs by 
    iterating over the dictionary, just the keys (triggering ValueError: too many values to unpack). To loop through
    both keys and values at the same time, you must use the .items() method
  - Correct: ![Correct nested access](img/dict_nested.png)

Flow control
- Logical operators
  - This is how we handle 'Boolean logic', i.e. handling when things are True or False (reserved keywords)
  - operators:
    - ```a and b```
    - ```a or b```
    - ```not a```
    - ```not a and b```
  - Typically used into control flow within context of ```if/elif/else```
    - ```if a:``` (use colon to start/continue control flow for if/elif)
  - Comparing magnitudes: ```<, >, ==, <=, >=```
  - *Zero or empty collections = false, non-zero = true*
- For loops: for repeating code a *known* number of times
  - ```range(x)```: generates an iterable of integers between 0 and x (non-inclusive)
    - e.g. ```for i in range(10):```
  - Loop through collections (lists, tuples, dicts)
    - Note: must wrap the collection in ```enumerate()``` to make it return both the index AND element
      - e.g. ```a = [10, 9, 8, 7]```
      - ```for index, elem in enumerate(a):```
  - ___ comprehension: shortcut of the for-loop construct
    - List comprehension: create a list
      - E.g. ```b = [i * 2 for i in a]```
    - Dictionary comprehension: create a dict
      - E.g. ```dict_comp = {str(i) + " times 2":i * 2 for i in range(10)}```
        - ![dict_comp](img/dict_comp.png)
- While loops: for repeating code an *unknown* number of times until condition met
  - ```while <condition>:```


Functions and classes
- Functions
- Classes
