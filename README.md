### Simple Python refresher for data science/general needs

(Notes and Jupyter Notebooks taken from Prof. Galen Egan's Foundations of Data Science course, Seattle University 2024)


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


Lists


Tuples


Dictionaries

Flow control
- Logical operators
- For loops
- While loops


Functions and classes
- Functions
- Classes
