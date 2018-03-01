---
title: Indexing, Slicing and Subsetting DataFrames in Python
teaching: 30
exercises: 30
questions:
    - " How can I access specific data within my data set? "
    - " How  can Python and Pandas help me to analyse my data?"
objectives:
    - Describe what 0-based indexing is.
    - Manipulate and extract data using column headings and index locations.
    - Employ slicing to select sets of data from a DataFrame.
    - Employ label and integer-based indexing to select ranges of data in a dataframe.
    - Reassign values within subsets of a DataFrame.
    - Create a copy of a DataFrame.
    - "Query /select a subset of data using a set of criteria using the following operators: =, !=, >, <, >=, <=."
    - Locate subsets of data using masks.
    - Describe BOOLEAN objects in Python and manipulate data using BOOLEANs.
---

In lesson 17, 18 and 19, we read a CSV into a Python pandas DataFrame.  We learned:

- how to save the DataFrame to a named object,
- how to perform basic math on the data,
- how to calculate summary statistics, and
- how to clean up messy data output

In this lesson, we will explore **ways to access different parts of the data**
using:

- indexing,
- slicing, and
- subsetting.

## Loading our data

First, we'll restart a Python environment with all our packages loaded. 

```UNIX

source activate py36

```

We will continue to use the poretools output dataset that we worked with in the last
lesson. Let's reopen and read in the data again:

```python
# Make sure pandas is loaded
import pandas as pd

# Read in the survey CSV
sep_data = pd.read_csv("column_separated.tsv", delimiter = "\t", index_col=0)
```

## Indexing and Slicing in Python

We often want to work with subsets of a **DataFrame** object. There are
different ways to accomplish this including: using labels (column headings),
numeric ranges, or specific x,y index locations.


## Selecting data using Labels (Column Headings)

We use square brackets `[]` to select a subset of an Python object. For example,
we can select all data from a column named `time` from the `sep_data`
DataFrame by name. There are two ways to do this:

```python
# TIP: use the .head() method we saw earlier to make output shorter
# Method 1: select a 'subset' of the data using the column name
sep_data['time']

# Method 2: use the column name as an 'attribute'; gives the same output
sep_data.time
```

We can also create a new object that contains only the data within the
`time` column as follows:

```python
# Creates an object, surveys_species, that only contains the `time` column
time_stamps= sep_data['time']
```

We can pass a list of column names too, as an index to select columns in that
order. This is useful when we need to reorganize our data.

**NOTE:** If a column name is not contained in the DataFrame, an exception
(error) will be raised.

```python
# Select the species and plot columns from the DataFrame
time_chan= sep_data[['time','channel']]

# What happens if you ask for a column that doesn't exist?
time_chan= sep_data[['time','channels']]
```

Python tells us what type of error it is in the traceback, at the bottom it says `KeyError: 'channels'` which means that `channels` is not a column name (or Key in the related python data type dictionary).

## Extracting Range based Subsets: Slicing

**REMINDER**: Python Uses 0-based Indexing

Let's remind ourselves that Python uses 0-based
indexing. This means that the first element in an object is located at position
0. This is different from other tools like R and Matlab that index elements
within objects starting at 1.

```python
# Create a list of numbers:
a = [1, 2, 3, 4, 5]
```

![indexing diagram](../fig/slicing-indexing.svg)
![slicing diagram](../fig/slicing-slicing.svg)


> ## Challenge - Extracting data
>
> 1. What value does the code below return?
>
>    ```python
>    a[0]
>    ```
>
> 2. How about this:
>
>    ```python
>    a[5]
>    ```
>
> 3. In the example above, calling `a[5]` returns an error. Why is that?
>
> 4. What about?
>
>    ```python
>    a[len(a)]
>    ```
{: .challenge}


## Slicing Subsets of Rows in Python

Slicing using the `[]` operator selects a set of rows and/or columns from a
DataFrame. To slice out a set of rows, you use the following syntax:
`data[start:stop]`. When slicing in pandas the start bound is included in the
output. The stop bound is one step BEYOND the row you want to select. So if you
want to select rows 0, 1 and 2 your code would look like this:

```python
# Select rows 0, 1, 2 (row 3 is not selected)
sep_data[0:3]
```

The stop bound in Python is different from what you might be used to in
languages like Matlab and R.

```python
# Select the first 5 rows (rows 0, 1, 2, 3, 4)
sep_data[:5]

# Select the last element in the list
# (the slice starts at the last element, and ends at the end of the list)
sep_data[-1:]
```

We can also reassign values within subsets of our DataFrame.

But before we do that, let's look at the difference between the concept of
copying objects and the concept of referencing objects in Python.

## Slicing Subsets of Rows and Columns in Python

We can select specific ranges of our data in both the row and column directions
using either label or integer-based indexing.

- `loc` is primarily *label* based indexing. *Integers* may be used but
  they are interpreted as a *label*.
- `iloc` is primarily *integer* based indexing

To select a subset of rows **and** columns from our DataFrame, we can use the
`iloc` method. For example, we can select month, day and year (columns 2, 3
and 4 if we start counting at 1), like this:

```python
# iloc[row slicing, column slicing]
sep_data.iloc[0:3, 1:4]
```

which gives the **output**

```
                                            sequence  \
0  TTATTGTAGTCGGTGGTGTGGCGGGTTGACTGAACTTGCTGCTTTT...   
1  TTGTTGGCACTTCGTTTCAGTTCTGCGGTGCTGGGCGGCGACCTCG...   
2  GGATTTCAGTTGCATGTTACTTATCCAAATTGTGTTTGGTTAGTCG...   

                                               quals  \
0  )""'$#$#&&'+)-&,%%%*&%*+(,*%%)*,-'+*+,.//*($%$...   
1  $&%'*'&$%'&(#(+410&+*,$#$$%')()219622522369343...   
2  +-&7.)*3537&+%+$4'%%%$(*5)+)*-,*.,,.*'**1.19+'...   

                                readName  
0  @bf327144-9003-4b2d-bad5-4cb380f40e8d  
1  @f54a0ac6-3563-432f-a447-60e4c95efb79  
2  @43a565b2-0dcb-4ea6-a32d-3984c3746e47 
```

Notice that we asked for a slice from 0:3. This yielded 3 rows of data. When you
ask for 0:3, you are actually telling Python to start at index 0 and select rows
0, 1, 2 **up to but not including 3**.

Let's explore some other ways to index and select subsets of data:

```python
# Select all columns for rows of index values 0 and 10
sep_data.loc[[0, 10], :]

# What does this do?
sep_data.loc[0, ['channel', 'time', 'lane']]

# What happens when you type the code below?
sep_data.loc[[0, 10, 399], :]
```

**NOTE**: Labels must be found in the DataFrame or you will get a `KeyError`.

Indexing by labels `loc` differs from indexing by integers `iloc`.
With `loc`, the both start bound and the stop bound are **inclusive**. When using
`loc`, integers *can* be used, but the integers refer to the
index label and not the position. For example, using `loc` and select 1:4
will get a different result than using `iloc` to select rows 1:4.

We can also select a specific data value using a row and
column location within the DataFrame and `iloc` indexing:

```python
# Syntax for iloc indexing to finding a specific data element
dat.iloc[row, column]
```

In this `iloc` example,

```python
sep_data.iloc[2, 6]
```

gives the **output**

```
'ch=199'
```

Remember that Python indexing begins at 0. So, the index location [2, 6]
selects the element that is 3 rows down and 7 columns over in the DataFrame.



> ## Challenge - Range
>
> 1. What happens when you execute:
>
>    - `sep_data[0:1]`
>    - `sep_data[:4]`
>    - `sep_data[:-1]`
>
> 2. What happens when you call:
>
>    - `sep_data.iloc[0:4, 1:4]`
>    - `sep_data.loc[0:4, 1:4]`
>
> - How are the two commands different?
{: .challenge}


## Subsetting Data using Criteria

We can also select a subset of our data using criteria. For example, we can
select all rows that have a read length of over 1000:

```python
sep_data[sep_data.length >= 1000]
```

Which produces a lot of output, so I'm not showing it. 

Or we can select all rows that do not contain the year 2002:

```python
sep_data[sep_data.length != 1000]
```

We can define sets of criteria too:

```python
sep_data[(sep_data.length >= 1000) & (sep_data.length <= 4000)]
```

### Python Syntax Cheat Sheet

Use can use the syntax below when querying data by criteria from a DataFrame.
Experiment with selecting various subsets of the "surveys" data.

* Equals: `==`
* Not equals: `!=`
* Greater than, less than: `>` or `<`
* Greater than or equal to `>=`
* Less than or equal to `<=`


> ## Challenge - Queries
>
> 1. Select a subset of rows in the `sep_data` DataFrame that contain reads of length less than 1000, and more than 5000. How
>   many rows did you end up with? What did your neighbor get?
>
> 2. You can use the `isin` command in Python to query a DataFrame based upon a
>   list of values as follows:
>
>    ```python
>    sep_data[sep_data['channel'].isin(['ch=233', 'ch=234'])]
>    ```
>
>   Use the `isin` function to find all reads that contain particular lengths
>   in the "sep_data" DataFrame. How many records contain these values?
>
> 3. Experiment with other queries. Create a query that finds all rows with a
>   length value > or equal to 5000.
>
> 4. The `~` symbol in Python can be used to return the OPPOSITE of the
>   selection that you specify in Python. It is equivalent to **is not in**.
>   Write a query that selects all rows that did not come from channel 233 in the "sep_data" data.
{: .challenge}


# Using masks to identify a specific condition

A **mask** can be useful to locate where a particular subset of values exist or
don't exist - for example,  NaN, or "Not a Number" values. To understand masks,
we also need to understand `BOOLEAN` objects in Python.

Boolean values include `True` or `False`. For example,

```python
# Set x to 5
x = 5

# What does the code below return?
x > 5

# How about this?
x == 5
```

When we ask Python what the value of `x > 5` is, we get `False`. This is
because the condition,`x` is not greater than 5, is not met since `x` is equal
to 5.

To create a boolean mask:

- Set the True / False criteria (e.g. `values > 5 = True`)
- Python will then assess each value in the object to determine whether the
  value meets the criteria (True) or not (False).
- Python creates an output object that is the same shape as the original
  object, but with a `True` or `False` value for each index location.

Let's try this out. Let's identify all locations in the survey data that have
null (missing or NaN) data values. We can use the `isnull` method to do this.
The `isnull` method will compare each cell with a null value. If an element
has a null value, it will be assigned a value of  `True` in the output object.

```python
pd.isnull(sep_data)
```

A snippet of the output is below:

```python
     length  sequence  quals  readName  runID  readNum  channel   time   lane
0     False     False  False     False  False    False    False  False  False
1     False     False  False     False  False    False    False  False  False
2     False     False  False     False  False    False    False  False  False
3     False     False  False     False  False    False    False  False  False
4     False     False  False     False  False    False    False  False  False
5     False     False  False     False  False    False    False  False  False
6     False     False  False     False  False    False    False  False  False
7     False     False  False     False  False    False    False  False  False
8     False     False  False     False  False    False    False  False  False
9     False     False  False     False  False    False    False  False  False
10    False     False  False     False  False    False    False  False  False
11    False     False  False     False  False    False    False  False  False
12    False     False  False     False  False    False    False  False  False
13    False     False  False     False  False    False    False  False  False
14    False     False  False     False  False    False    False  False  False
15    False     False  False     False  False    False    False  False  False
16    False     False  False     False  False    False    False  False  False

[395 rows x 9 columns]
```

To select the rows where there are null values, we can use
the mask as an index to subset our data as follows:

```python
# To select just the rows with NaN values, we can use the 'any()' method
sep_data[pd.isnull(sep_data).any(axis=1)]
```

Note that the `weight` column of our DataFrame contains many `null` or `NaN`
values. We will explore ways of dealing with this in Lesson 20.

We can run `isnull` on a particular column too. What does the code below do?

```python
# What does this do?
empty_lengths = sep_data[pd.isnull(sep_data['length'])]['length']
print(empty_lengths)
```

Let's take a minute to look at the statement above. We are using the Boolean
object `pd.isnull(sep_data['length'])` as an index to `sep_data`. We are
asking Python to select rows that have a `NaN` value of weight.

