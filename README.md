# Python refresher for data science/general needs

(Jupyter Notebooks taken from Prof. Galen Egan's Foundations of Data Science course, Seattle University 2024)

(exercises completed by me)

## Table of Contents
1. [Python Fundamentals](#python-fundamentals)
2. [Data Manipulation](#data-manipulation)
3. [Data Visualization](#data-visualization)
4. [Data Preparation](#data-preparation)


## Python Fundamentals
1. [Basics](#basics)
2. [Var assignment + data types](#var-assignment--data-types)
3. [Strings](#strings) 
4. [Lists](#lists)
5. [Sets](#sets)
6. [Tuples](#tuples)
7. [Dictionaries](#dictionaries)
8. [Flow control](#flow-control)
9. [Functions and classes](#functions-and-classes)


### Basics
- What is Python?
  - High-level language; interpreted (don’t need to compile code before running it) and dynamically-typed (type errors not checked before runtime -> runtime failures can occur)
- What is Jupyter Notebook?
  - Nice for combining text (supports both Markdown and latex) with code
    - Either text or code can go into a cell
  - Code gets executed via Python interpreter (or kernel) on current machine
  - Execute a current code cell via Shift + Enter


### Var assignment + data types
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


### Strings
- Python has different "flavors" of strings to hold characters
- Concatenation: ```<string1> + <string2>``` -> <string1string2>
- Replace: ```.replace(<part of str to be replaced in original str>, <replacement>)```
- Capitalize full string: ```.upper(str)```
- Get index (0-indexed) of a char or start of substring in a string: ```.index("substr")```
- How do we split a string? Use desired char to split on, then ```str.split("<splitter>")```
  - Should return a ```List``` of the substrings that were split up
  - Does NOT mutate the original string
- How do we get the last element of a List? ```list[-1]```


### Lists
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

### Sets
- A list but restricted to unique elements only
- ```set(list_x)```: returns a set of unique values
- ```set_1.union(set_2)```: returns a unified set
- ```set_1.intersection(set_2)```: returns intersection
- ```set_1.symmetric_difference(set_2)```: returns the opposite of intersection

### Tuples
- An _immutable_ list
Initialized with ```( )``` parentheses
- Trying to update an element will throw an error
- Slicing works the same as for a list


### Dictionaries
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

### Flow control
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


### Functions and classes
- Functions: code reusability (running a section of code more than once whenever function called)
  - ```def <function name>(arguments):```
- Classes
  - An example of OOP; a paradigm where related code bits and functionality are grouped in 'objects'
    - Must be initialized in memory via instantiation for use
    - Declaration
      - ```class <class name>:```
    - Constructor definition: the ```__init__``` function; defines how you initialize an instance of the class
      - By convention, 1st argument of ```__init__``` is always ```self```; refers to the instance of the class
      - e.g. ```def __init__(self, <arg1name=defaultarg1value>, <arg2...>):```
    - Class methods: can be accessed from any class instance
      - Required to pass in ```self``` as argument every time
      - e.g. ```def <method_name>(self):```

[Back to top](#table-of-contents)

## Data Manipulation
1. [Specific package imports](#specific-package-imports)
2. [Pandas: Basics](#pandas-basics)
3. [Pandas: Accessing & slicing DataFrames](#pandas-accessing-and-slicing-dataframes)
4. [Pandas: Merging DataFrames](#pandas-merging-dataframes)
5. [Pandas: Saving DataFrames](#pandas-saving-dataframes)
6. [Pandas: Parsing Data](#parsing-data)
7. [Pandas: Alternatives](#pandas-alternatives)

### Specific package imports
- Pandas: ```import pandas as pd```
- Numpy: ```import numpy as np```
- Scikit-learn: import one class or function at a time as needed (LinearRegression, etc.)

### Pandas: Basics
- Read in a .csv file: ```pd.read_csv(<file path>)```
  - This returns a *DataFrame*; ```df = pd.read_csv(<file path>)```
- What's a DataFrame (df)?
  - Kind of like an object/class that contains useful methods for working with its data
- Create a new df: ```df = pd.DataFrame({})``` (effectively passed in a dictionary)
  - E.g. ```df = pd.DataFrame({'day': [1,2,3,4,5], 'meas': [0.1,0.2,0.3,0.4,0.5]})```
- Printing the df -> gives the rows of the .csv like an Excel sheet
  - The rows could be numbered starting from 0, or in general an array of integers called the *DataFrame index*
  - The index can be ints, strings, times, or anything
  - ```print(df.index)``` -> RangeIndex(start=0, stop=<stop>, step=<increment>)
- How to get the columns of the df?
  - ```print(df.columns)``` -> ```Index(['<col1name>', '<col2name>', ...])``` (array)
- The indices and column names help arrange and label data
- .csv's data can be viewed without its labels: ```df.values``` -> just gives an array of rows
  - Data type of ```df.values``` is ```numpy.ndarray```
    - Under the hood, pandas stores data in Numpy "ndarrays" (short for N-dimensional arrays) that store 
      any # of values; Numpy arrays are a standard data type for storing/performing calculations on data in Python
- Append a row
  - `df = df.loc[<get last row + 1>, <get all rows>] = [new row]`
    - `df = df.loc[df.index.max() + 1, :] = ['john', 20, 1000]`
- Other df helpers
  - ```df.head()```: first 5 rows
  - ```df.tail()```: last 5 rows
  - ```df.shape```: returns (# rows, # cols) of the data
  - How do we find the data types of each col? ```df.dtypes```
  - ...find general info of the df? ```df.info()```
  - ...find general statistical info? ```df.describe()``` - returns count/mean/std/min for columns
    - We can also group by a column and then get statistics on that grouping
      - E.g. ```df.groupby("column_name").describe()```
  - Sort values: ```df.sort_values(by=<col name>, ascending=<false/true>)```
    - E.g. ```iris.sort_values(by="sepal_length", ascending=False).head()```

### Pandas: Accessing and slicing DataFrames
Why? Because much of the time we extract the interesting portion of the data, get rid of the rest, use part of the 
  data for training a model, another part for testing the model, and so on. Pandas can help with this

Get a single column's data: ```col = df["column_name"]```
- This data type is ```type(col)``` or a ```Series```, like a 1D DataFrame
- Many methods apply only to Series but not to dfs (e.g. ```.unique()```)
- If no spaces in column name, can directly get column like a property of the DataFrame
  - E.g. `iris.SepalLengthCm`, `iris.Species`

Get multiple columns of data: ```cols_data = df[["col_1_name", "col_2_name"]]```
- Pass in *list* of columns
- Then can run ```.head()```, etc.
- What is the ```type(cols_data)```? df (>1 col)

Get only x number of indices from a df: ```df_x = df.loc[:x]```
- Contrary to list slicing, ```x``` is included in the index (inclusive indexing)
- If indices are NOT integers, would have to specify the indices
  - e.g. for indices "a" and "b", ```df2.loc[["a", "b"]]```
- `.loc`: 1st argument is accessing rows, 2nd argument is accessing columns *by name*
- `.iloc`: both arguments access rows and columns by index

Get certain indices and certain columns (akin to a "sub" df)
- E.g. for indices "a" and "b" and columns "green" and blue": ```df2_2gb = df2.loc[["a", "b"], ["green", "blue"]]```
  - Pass in a list of the indices and a separate list of the cols
  - ![DF Slice Row](img/df_slice_row_col.png)

If needing to treat a df like an N-dim. array or list and access contents via int row/col numbers, need ```.iloc()```
- E.g. ```df2_i = df2.iloc[:2, 1:]```, i.e. get all rows up to 2nd, and all cols starting at 1st
- ```.loc[]```: tell pandas to return specific index and column *names*
- ```.iloc[]```: tell pandas to return specific row and column *numbers*

What if we want to get the last (or final) x # of rows of a df? Negative index
- ```df.iloc[-x:]```

What if we wanted to *select* a subset of data *based on a logical condition*? -> Logical slicing
- Format: ```df_subset = df.loc[logical_condition, column_names]```, ```logical_condition``` is array of same length
  as the df index except all elements either ```True``` or ```False```
  - To create ```logical_condition``` array, relate dataframe part of interest to some value with a logical operator
    - E.g. ```logical_condition = df["species"] == "setosa"```
      - ```df["species"] == "setosa"```: using the species column, compares each of its rows' species value to 
        "setosa", effectively returning a column of bools (with indices)
      - ![Logical slicing](img/slicing_logical.png)
      - Then use this bool column as argument to ```.loc[]``` to slice
      - (seems like 2nd argument of column/s is optional)
      - ![Logical slicing 2](img/slicing_logical_2.png)
- Format for multiple comparisons:
  - ```df_subset = df.loc[logical_cond_1 & logical_cond_2, columns_to_show]```
  - Why ```&``` operator instead of ```and```? (using the latter throws ambiguity error)
    - Because pandas overloads the bitwise operator in order to do element-wise filtering, row-by-row
      across the array
  - ![Pandas bitwise overload](img/pandas_overload_bitwise.png)

What if we want to slice, column-wise, by the column's name (instead of index)?
- ```df.columns.get_loc("<column_name>")``` -> returns index of column_name, L->R
- E.g. get the final 50 rows of data with only the column named "petal_length" 
  (reminder: index column counts as 0th)
  ```col_idx = df.columns.get_loc("petal_length") ... df_final = df.iloc[-50:, col_idx]```

What if we want to access the value at a particular row/col? 
- `df.iat[<row #>, <col #>]`
  - E.g. `iris.iat[82, 1]` -> np.float64(5.8) (the value at the 82nd row in first column = 5.8)
- `df.at[<row #>, <col name>]` (same as above but column name instead of #)

What if we want to get rows of a known column by passing a list of desired column *values*? 
- `df.<col_name>.isin([<col_category_1>, <col_category_2>])`
  - E.g. `df.Species.isin(["virginica", "versicolor"]).tail()` -> last 5 rows of df that returns rows only containing 
    "veriscolor" and "virginica" as column values in "species" column

What if we want to try to get column values starting with a known string? 
- `df.<col_name>.str.startswith(<substring>)`
  - E.g. `df.Species.str.startswith("seto")` -> only column values in "Species" column that starts with "seto"

### Pandas: Merging DataFrames
Pandas methods for merging rely on relational db-based languages (e.g. SQL)
- ```df_left.merge(df2_right, on=<join key>, how=<join method>)```
- Join key: what are we joining on (shared column, index name or list of names)?
  - Is often the *independent variable* (e.g. time in a time series dataset to match measurements to same time basis)
  - Join key arg: ```on=```, or if key named something different in L or R dfs, ```left_on``` or ```right_on```
  - Join method arg: ```how=```; specifies which dfs we use in order to create new merged key (equiv. to left join, 
    right join, outer join, etc.)
    - Default if not specified: ```inner```
- E.g. ```df_out = df1.merge(df2, on = 'day', how = 'left')
  - Would expect to get "NaN" for any values in the right df that don't exist for the key
  - ![Join left](img/merge_joinleft.png)
- Can also merge on multiple keys
  - E.g. ![Left right](img/left_right_twokeys.png)
  - ![Merge two keys right join](img/merge_twokeys_joinright.png)

### Pandas: Saving DataFrames
```df.to_csv(<file_path>)```

If don't want to save the df index as a column:
```df.to_csv(<file_path>, index=False)```

### Pandas: Parsing data
If we need multiple datasets from different places for a common project, what are the issues we could run into?
- Datasets often have inconsistencies between each other and in and of themselves
  - Labels
  - Spacing
  - Separators
  - E.g. ![Inconsistent](img/inconsistent_data_1.png)

What are solutions?
- `.read_csv()` is a very complicated and flexible function to handle multiple edge cases
  - [Documentation](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html)
  - Misplaced or missing header (e.g. header/column names could try to show up as a data value): 
    - Skip first row or first couple rows
    - E.g. ```pd.read_csv("<file_path>", skiprows=2)```
  - Non-comma delimiter:
    - `pd.read_csv("<file_path>", skiprows=x, sep="<separator type, regex>")`
    - E.g. `...sep="\t"`
  - Bad rows or bad lines (e.g. extra fields):
    - `.read_csv(...on_bad_lines="skip")`
  - Inconsistent delimiter (e.g. tabs, then spaces, then tabs, etc.):
    - Handle more general case via regex
      - E.g. a general separator: `.read_csv(...sep=r"\s+", ...)`
        - `r` instructs interpreting backslash as part of regex rather than as an escape character
  - Non-standard representations of null
    - Standard: a numpy `NaN`
    - Example of non-standard that could show up in a field: "NotANumber"
    - Must explicitly pass in a list of expected representations of null found in the dataset
      - E.g. `.read_csv(...na_values=["NotANumber"], ...)`

Datasets also may need to be loaded from either local or web-based sources. Different formats of each dataset are 
technical obstacle when combining data
- Not all data comes from `.csv`
  - Data from public Internet, or API calls to internal endpoints can come as `json`

Solutions?
- Python's "requests" module: allows *loading json data into Python dictionary* -> can use as-is or convert into df
  1. `import requests`
  2. Set URL that will request data from an API
  3. `response = requests.get(url)`
  - See the response:
    - Just `response` will only display response code
    - `response.json()`: shows json data
    - To get a more compact view: `response.json().keys()`
    - Once a dictionary list within the response is identified as possible extraction, we can convert to df:
      - `pd.DataFrame(data=<list>)` (argument name: data)
      - E.g. ![List from API](img/list_from_api.png)
        - List contains rows as dictionary format `{}`
        - After df: ![List from API to df](img/list_api_to_df.png)


### Pandas: Alternatives
- R: basically Python's answer to the R `data.frame` data type
  - Has all the functionality of pandas (and more)
  - Best for statistics-focused work
    - Some functionality doesn't exist in Python, or if it does, requires tedious Python package mgmt
  - Not a good choice for building full software stack (not what it was designed for)
    - Probably don't want Python for your full stack, either
- polars: similar to pandas, becoming more popular
  - Significant efficiency gains, best for large amounts of data

[Back to top](#table-of-contents)

## Data Visualization
1. [The Basics](#the-basics)
2. [Scatterplots, Correlation](#scatterplots-correlation)
3. [Histograms](#histograms)
4. [Box Plots](#box-plots)
5. [Time Series Data](#time-series-data)
6. [More Advanced Plotting](#more-advanced-plotting)
7. [Plot Params](#plot-params)
8. [Multi-Panel Plots](#multi-panel-plots)
9. [Heatmaps](#heatmaps)
10. [Surface Plots](#surface-plots)
11. [Animations](#animations)
12. [Geospatial Visualization](#geospatial-visualization)

### The Basics:
### Common statistical libraries/techniques
#### Recap: generate 1D data
- Generate a linearly spaced array over a specified length:
  - `import numpy as np`
  - `np.linspace(<lowest>, <highest>, <# of values>)`
- Generate an array of random noise (normal distribution) over a specified length: 
  - `np.random.randn(<# of values>)`
  - Values drawn from a standard normal distribution, i.e. mean = 0, std = 1
    - mean = 0: data "centered" around 0, since it's drawn from standard normal distribution
    - std = 1: ~68% values fall in [-1, 1], ~95 values fall in [-2, 2] and ~99.7% values fall in [-3, 3]

#### Seaborn: a more compatible plotting package (for pandas)
- Better integration with pandas than matplotlib
- Enriched visualizations

#### Scipy: contains statistical libraries for working with distributions
- `from scipy.stats import norm, skewnorm, uniform`
- Allows working closer with *random variates* (`.rvs()`)
  - Random variates: specific numerical values generated by a random process (i.e. random variable) according to a 
  - specified probability distribution
  - E.g. if random variable X is normal distribution with mean=0, std=1, then random variate norm.rvs(size=3) outputs 
    random variates [-0.53, 1.21, 0.14]
- Create set of standard normal distribution values:
  - `values = norm.rvs(loc=<mean>, scale=<std dev>, size=<# of samples>)`
    - shape/skewness: when = 0, distribution identical to standard normal distribution
      - When > 0, right-skewed, when < 0, left-skewed
    - loc: shift's distribution's mean
    - scale: adjusts standard deviation
    - size: defines the shape of output array of random variates
- Create set of skewed distribution values:
  - `values = skewnorm.rvs(<shape/skewness>, loc=...)`
    - shape/skewness: when = 0, distribution identical to standard normal distribution
      - When > 0, right-skewed, when < 0, left-skewed
- Create set of uniform values:
  - `values = uniform.rvs(loc=0, scale=1, size=1000)`
  - Any value within the range is equally likely

#### LaTeX
High-quality typesetting system designed for production of technical and scientific documentation. Created by Leslie 
Lamport (this guy again?) as an extension of Donald Knuth's \TeX typesetting system. Intended to separate document 
content from visual design. Standard for STEM fields to programmatically typeset.

Installation:
- Typically need to install a LaTeX distribution, specific to OS
  - Windows: install MiKTeX or TeX Live
- Find installation folder of LaTeX and locate binary folder
  - Windows: `C:\texlive\2026\bin\windows`
- Update (create new) PATH variable with the path of the binaries
- Restart terminal or IDE
- Verify installation: `latex --version`

Core concepts:
- Write plain text interspersed with commands

Key benefits:
- Superior mathematical notation (complex equations, fractions, matrix layouts, Greek letters)
- Stability; can process large documents well

Comparison with standard word processor (using buttons to format):
```
\documentclass{article}
\title{My First Document}
\author{Jane Doe}
\begin{document}
\maketitle

\section{Introduction}
Here is a complex mathematical equation rendered clearly:
\begin{equation}
  f(x) = \int_{-\infty}^{infty} e^{x^2} dx
\end{equation}

\end{document}
```

Sometimes Matplotlib may be configured to use external LaTeX installation for text rendering (`useTex=true`), but can't 
find the necessary executables on system's PATH. 
- Quick fixes:
  - If matplotlib imported as `mpl`, turn off high-quality LaTeX directly from mpl: `mpl.rcParams['text.usetex] = False`
  - Reset matplotlib's pyplot instance (e.g. `import matplotlib.pylot as plt`) to defaults: `plt.rcdefaults()`

### Scatterplots, Correlation
The best choice for comparing two variables

Matplotlib: standard plotting package
- ```import matplotlib.pyplot as plt```
- Generate a plot:
  - By default, a line graph:`plt.plot(<array1>, <array2>)` (arrays ideally same length)
  - To change into a scatterplot: `plt.plot(<array1>, <array2>, 'o')` ("'o" argument)
    - Or equivalently: `plt.scatter(<array1>, <array2>)`
- Add plot labels and title to x-y graph:
  - `plt.xlabel("<label name>")`
    - `plt.ylabel("<label name>")`
    - `plt.title("<title name>")`

E.g. Plot 100 random values linearly between -10 and 10 and label them
```
len = 100
x_vals = np.linspace(-10, 10, len)
noise = np.random.randn(len)
y = x + noise # (uses the 100 x values, [-10, 10] to create a similar linear signal but with noise
plt.scatter(x_vals, y_vals)
plt.xlabel("x")
plt.ylabel("y")
plt.title("x and y graph")
```
- Gaussian noise, prior to adding it to x-vals (non-linear):
  - ![Gaussian noise](img/gaussian_noise.png)
- After adding noise to x-vals (i.e. linear adjustment):
  - ![After noise](img/after_noise.png)

Quantify level of correlation between two axes:
- `corr = np.corrcoef(<axis1>, <axis2>)`
  - Technically produces a 2x2 numpy array, so have to extract via simple array indexing (e.g. corr[0, 1])
    - After extraction, still is a numpy data type (likely `np.float64()`)

### Histograms
The best choice for visualizing one variable

Qualities:
- Splits variable into intervals, ranges, bins of values, and count within each bin
  - X-axis holds bins
  - Y-axis holds bin counts

Create matplotlib histogram with noise:
- `plt.hist(noise, bins="auto")` (+ labeling)
- Manually specify bin # by substituting "auto" with a value
  - Can also provide a range with custom intervals via `np.arange()`:
    - `plt.hist(...bins=np.arange(<low_bound>, <high_bound>, <interval size>))`

Create seaborn histogram:
  - `ax = sns.histplot()`
    - Allows using a variable to hold the plot
- Show approximation of underlying distribution via smooth line overlay:
  - `sns.histplot(..., kde=True)`
  - Calculated using *kernel density estimation*

Ideal histogram qualities:
- Provides accurate sense of distribution
  - E.g. clearly displays unimodal, bimodal, skewed, symmetric, etc.
  - If manually adjusting, find optimal # of bins (not too few, not too many bars)

Normalize histogram area:
- Allows for estimating probability of finding a sampling within a value range
- Update y-axis from bin counts to density, i.e. area of all the bars (i.e. area under the curve) sums to 1
  - Probability of finding a sampling now can be *directly estimated* via area under the curve between the values 
- ```
  plt.hist(noise, bins="auto", density=True)
  plt.xlabel("Noise magnitude")
  plt.ylabel("Density")
  ```
- Find total area under the curve after conversion: need density and # bins
  - ```
    density, bins, _ = plt.hist(..., density=True)
    y = density
    dx = np.diff(bins) # calculates n-th discrete difference between consecutive elements on an axis
    np.sum(y * dx) # np.float64()
    ```

E.g. 
```
values = skewnorm.rvs(5, loc=0, scale=1, size=1000)
ax = sns.histplot(x=values, stat="density", kde=True)
ax.set_xlabel("Actively-managed portfolio returns versus S&P 500 (%)")
```

```
values = uniform.rvs(loc=0, scale=1, size=1000)
ax = sns.histplot(x=values, stat="density", kde=True)
```

### Box Plots
Best for comparing multiple variables' distributions to each other
- Also known as box-and-whisker
- Alternative: make multiple histograms and stack them, but can get cluttered

Can pass in a DataFrame as argument
- `sns.boxplot(data=<df>, x=<dependent variable>, hue=<independent/control variable>)`
  - x: responding variable to map box plot over (e.g. sepal length)
  - hue: comparison variable (e.g. species) (hue decided by the library, changes between control variables)
  - ![Boxplot species](img/boxplot_species.png)

Properties
- Median
- Q1: 25th percentile
- Q3: 75th percentile
- IQR: Q3 - Q1
- Max: Q3 + 1.5*IQR
- Min: Q1 - 1.5*IQR
- Outliers: any values below min or above max

### Time Series Data
 Any data which considers the sampling time relevant. Requires special handling in Python. Once desired dataset is 
filtered and extracted into DataFrame, ensure DateTime data is converted to *pandas DateTime type*:
- e.g. `pd.to_datetime(<dataframe variable>["<column in df>"])`
- If running `df.info()`, column should now appear under `Dtype` of `datetime64[...]`
- Direct update on the df column: e.g. `df_full["date_utc"] = pd.to_datetime(df_full["date.utc"])"`

Initialize and plot a figure object:
```
fig = plt.figure() # initialize
plt.plot(<df_col1>, <df_col2>) # make the plot
```

DateTime ticks in axis may be obscured:
- Create date range object (or pass in directly) with pandas: 
  `dr = pd.date_range(start=<str_date1>, end=<str_date2>, freq=<interval>)`
  - freq: a number followed by D, M, or Y (e.g. `freq=5D`)`
- Set x-axis tick positions with date range object: `plt.xticks(dr)`
  - Rotate/offset the labels: `fig.autofmt_xdate()`

### More Advanced Plotting:
Best for more professional and formal presentation of data
### Plot Params
Default matplotlib labels are hard to reach on bigger screens; they stay the same size as a default computer screen, 
even between screens. *Pass in a parameter dictionary (i.e. params = { }) with label/font sizes*
- Param objects with nested params to modify:
  - `axes` (for label size)
  - `font` (for size and type) 
  - `legend` (for font size)
  - `xtick`, `ytick` (for label size)
  - `text` (use tex) (?)
- e.g. ```
  params = {
  "axes.labelsize": 18,
  "font.size": 18,
  "font.family": "serif",
  "legend.fontsize": 16,
  "xtick.labelsize": 18,
  "ytick.labelsize": 18,
  "text.usetext": True, # for LaTeX / TeX; set to False if running slow/not working
}
  ```

Then pass the object into `plt.rcParams.update()`, e.g. `plt.rcParams.update(params)`

Can also use a mpl package `matplotlib.dates as mdate` for improved date formatting (below).

Can also pass in the same parameter dictionary to Seaborn and even combine matplotlib with it to use its figure methods:
```
sns.set_theme(rc=params) # seaborn
fig = plt.figure() # mpl
ax = sns.lineplot(df, axes, ...) # seaborn figure object
ax.set_xticks(<date range obj>)
ax.set_ylabel(r"NO$_2$ concentration ($\mu$g\L)") # formatting with latex
ax.set_xlabel(...)
ax.set_title(...)
ax.xaxis.set_major_formatter(mdate.DateFormatter("%m/%d")) # custom x-tick with latex
fig.autofmt_xdate()
fig.set_size_inches(10, 5) # custom size
```


### Multi-Panel Plots
Best for multiple subplots in a single figure.

Algorithm:
- Read data into a df (e.g. read_csv)
- Convert time series into appropriate DateTime format
- Get list of unique axes along same dimension (e.g. `cities = df["city"].unique()`)) 
- Initialize plot and subplot vars: `fig, ax = plt.subplots (<# subplots in rows>, <# subplots in cols>, sharex=<bool>`
  - Returns the figure containing each template subplot `fig` and an array of the template subplots `ax`
- Iterate through unique list (e.g. `for i, city in enumerate(cities):`)
  - Extract column from data for each item to create a subplot for
  - Set labels
- Autoformat date and set sizes

### Heatmaps
Necessary to visualize how one variable changes as a *function of two other variables*

May need to pivot the data to create a 2D grid to plot (e.g. `flights = flights.pivot(index="month", columns="year", 
values="passengers")`) (i.e. put x axis on y, y on x)
- index: value to now index by
- columns: self-explanatory
- values: the cell values (probably was another column in the old grid)

`ax = sns.heatmap(<dataset>, cbar_kws={'label': '<the value of 'values'>`})`

### Surface Plots (3D)
Usually for optimization settings. See notebook

### Animations
See notebook

### Geospatial Visualization
See notebook

[Back to top](#table-of-contents)

## Data Preparation
1. [Numpy Basics](#numpy-basics)
2. [Stats Review](#stats-review)
3. [Visual Tools for Outlier Deteection](#visual-tools-for-outlier-detection)
4. [NaN Handling and Imputation](#nan-handling-and-imputation)
5. [Smoothing Techniques](#smoothing-techniques)

### Numpy Basics
As seen before, values stored in a pandas DataFrame are just numpy arrays

#### Basics of Numpy Arrays
Import: `import numpy as np`

Base numpy data type is the *n-dimensional array*; behaves like a Python list but better with time and space complexity

Create a numpy array from a Python list:
- `numpy_arr = np.array([py_list])`

Slice/extract:
- E.g. `numpy_arr[0]`, `numpy_arr[-1:]`

Apply a comparison operator to return a boolean array containing comparison results of each array value:
- E.g. `numpy_arr < 3` -> array([True, True, False, ...])
- This allows *extraction* of the array based on what returned True:
  - E.g. `numpy_arr[numpy_arr < 3]` -> returns only portion of array that returned True in logical condition

NaN values: handled with `np.nan` type
- This is a float data type, so if trying to replace values in array with np.nan, need to type the whole array to hold
  floats (or throws ValueError: cannot convert float NaN to <data type that's in there now>)
  - E.g. `numpy_arr = np.array(py_list, dtype=np.float64)`
  - E.g. `numpy_arr = np.array(py_list.astype(float)`

#### Initialization Shortcuts

np.linspace(min, max, # values)

np.arange(min, max, spacing) (e.g. `np.arange(1, 10, 0.5`))

Zeroes: np.zeros((3,3))

Ones: np.ones((5,)) -> 1 row of 1's

Array of NaN: np.zeros((5,2)) * np.nan -> 5 rows and 2 cols of None

np.empty() creates array of uninitialized array elements from available mem space and may be faster to execute. 
np.zeros() outputs an initialized array of value 0. 


#### Math with Arrays
Broadcasting: multiplying a vector with a scalar
- E.g. (arr + x -> [arr[0] + x, arr[1] + x, ...])

np.argsort(a) -> sorts by index

a.sort(axis optional)

#### Image-based processing
```
from skimage import io
photo = io.imread('image')
type(photo)
photo.shape -> width, height, # of channels (3 if RGB)
```

#### Math with Matrices
- Axis: axis=0 -> looking at columns, axis=1 -> looking at rows

Initialize random mxn matrix: `matr = np.random.rand(3,3)`

col_1 = example_matrix[:, 0] : extracts all rows within the 0th column

last_element = example_matrix[2,2]

For matrices a and b,

Element-wise multiplication: `a * b` (multiplies elements in identical positions across two matrices of same size)

Matrix-multiplication: `a.dot(b)` (combines rows of 1st matrix with columns of second)

Element-wise comparison: `a == b`, `a > b`

Array-wise comparison: `np.array_equal(a, b)`

Logical or: `np.logical_or(a, b)`

Logical and: `np.logical_and(a, b)`

Transpose (rows become columns, columns become rows): `a.T`

Combine arrays: `np.ravel(a, b)`

Reshape: `arr = arr.reshape(desired # rows, desired # cols)`

Slicing
- Syntax: two_dim[row_start:row_end]
- For array [[1,2,3],[4,5,6][7,8,9]]:
  - array[0:2] -> get first two rows
  - array[1:3] -> last two rows
  - array[:,1] -> sliced 2D columns (2,5,8)

#### Transcendental functions
How we work with exponential/sinusoidal/logarithmic functions

E.g. Sin: `np.sin(np.arange(x))` -> [np.sin(0), np.sin(1), np.sin(2), ...]

E.g. Log: `np.log(...)`

E.g. Exp: `np.exp(...)`

#### Reductions
AKA aggregations; e.g. avg, total, other derived quantities from actual amounts for further analysis

Ultimately creates features for underlying ML processing

E.g. `np.sum(np.array([1,2,3,4])`

E.g. `arr.min()`: minimum of array

E.g. `arr.argmin(arr)`: gets the index of the smallest value in array

### Stats Review

#### Mean, Median, Standard Deviation

Mean: `arr.mean()`

Median: `np.median(arr)` or `np.median(matr, axis=-1) # last axis`

If mean ~ median, then distribution is close to being uniform

Std dev: `arr.std()` -> provides value where 68% of the data is

#### Probability Density Functions

#### Probablity Distributions

### Visual Tools for Outlier Detection

### NaN Handling

#### Proper Labeling

#### Dropping NaN Values

#### Imputation

#### Advanced Imputers

### Smoothing Techniques

#### Moving Average

#### Median Filter




[Back to top](#table-of-contents)