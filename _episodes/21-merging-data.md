---
title: Combining DataFrames with Pandas
teaching: 20
exercises: 25
questions:
- " Can I work with data from multiple sources? "
- " How can I combine data from different data sets? "
objectives:
    - Combine data from multiple files into a single DataFrame using merge and concat.
    - Combine two DataFrames using a unique ID found in both DataFrames.
    - Employ `to_csv` to export a DataFrame in CSV format.
    - Join DataFrames using common fields (join keys).
---

In many "real world" situations, the data that we want to use come in multiple
files. We often need to combine these files into a single DataFrame to analyze
the data. The pandas package provides [various methods for combining
DataFrames](http://pandas.pydata.org/pandas-docs/stable/merging.html) including
`merge` and `concat`.

To work through the examples below, we first need to load the species and
surveys files into pandas DataFrames. In iPython:

```python
import pandas as pd
sep_data = pd.read_csv("pore_info.tsv", delimiter='\t')
sep_data['Run'] = "Not great run"

[395 rows x 12 columns]

good_data = pd.read_csv("good_data.tsv",
delimiter='\t')
good_data['Run'] = "Great run"


[395 rows x 12 columns]
```

# Concatenating DataFrames

We can use the `concat` function in Pandas to append either columns or rows from
one DataFrame to another.  Let's grab two subsets of our data to see how this
works.
To stack the data vertically, we need to make sure we have the
same columns and associated column format in both datasets. When we stack
horizonally, we want to make sure what we are doing makes sense (ie the data are
related in some way).

```python
# Stack the DataFrames on top of each other
vertical_stack = pd.concat([sep_data, good_data], axis=0)

# Place the DataFrames side by side
horizontal_stack = pd.concat([sep_data, good_data], axis=1)
```

### Row Index Values and Concat
Have a look at the `vertical_stack` dataframe? Notice anything unusual?
The row indexes for the two data frames `sep_head` and `sep_head_last10`
have been repeated. We can reindex the new dataframe using the `reset_index()` method.

## Writing Out Data to CSV

We can use the `to_csv` command to do export a DataFrame in CSV format. Note that the code
below will by default save the data into the current working directory. We can
save it to a different folder by adding the foldername and a slash to the file
`vertical_stack.to_csv('foldername/out.tsv')`. We use the 'index=False' so that
pandas doesn't include the index number for each line.

```python
# Write DataFrame to CSV
vertical_stack.to_csv('out.tsv', sep='\t', index=False)
```

Check out your working directory to make sure the CSV wrote out properly, and
that you can open it! If you want, try to bring it back into python to make sure
it imports properly.

```python
# For kicks read our output back into python and make sure all looks good
new_output = pd.read_csv('out.csv', delimiter='\t')
```

# Joining DataFrames

When we concatenated our DataFrames we simply added them to each other -
stacking them either vertically or side by side. Another way to combine
DataFrames is to use columns in each dataset that contain common values (a
common unique id). Combining DataFrames using a common field is called
"joining". The columns containing the common values are called "join key(s)".
Joining DataFrames in this way is often useful when one DataFrame is a "lookup
table" containing additional data that we want to include in the other.

NOTE: This process of joining tables is similar to what we do with tables in an
SQL database.

What we will do now is look at an example of joining together two data frames based on an overlapping column. Our two dataframes will be our vertically stacked output, and a second data frame with organism information.

## Joining Two DataFrames

To better understand joins, let's grab the first 10 lines of our data as a
subset to work with. We'll use the `.head` method to do this. We'll also read
in a subset of the sep_data table.

```python
# Read in first 10 lines of sep_data table
sep_head = sep_data.head(10)

# Make a small subset of the good_data frame
organism_info = pd.read_csv('organism_info', delimiter='\t')
subset = organism_info.head(10)
```

In this example, `sep_data` is the lookup table containing the master list of columns.

## Identifying join keys

To identify appropriate join keys we first need to know which field(s) are
shared between the files (DataFrames). We might inspect both DataFrames to
identify these columns. If we are lucky, both DataFrames will have columns with
the same name that also contain the same data. If we are less lucky, we need to
identify a (differently-named) column in each DataFrame that contains the same
information.

```python
>>> sep_head.columns

Index(['Unnamed: 0', 'length', 'sequence', 'quals', 'readName', 'runID',
       'readNum', 'channel', 'lane', 'date', 'hours'],
      dtype='object')

>>> subset.columns

Index(['Unnamed: 0','readName', 'runID',
       'organism'], dtype='object')
```

In our example, the join key is the column containing the readName which is called `readName`.

Now that we know the fields with the common species ID attributes in each
DataFrame, we are almost ready to join our data. However, since there are
[different types of joins](http://blog.codinghorror.com/a-visual-explanation-of-sql-joins/), we
also need to decide which type of join makes sense for our analysis.

## Inner joins

The most common type of join is called an _inner join_. An inner join combines
two DataFrames based on a join key and returns a new DataFrame that contains
**only** those rows that have matching values in *both* of the original
DataFrames.

Inner joins yield a DataFrame that contains only rows where the value being
joins exists in BOTH tables. An example of an inner join, adapted from [this
page](http://blog.codinghorror.com/a-visual-explanation-of-sql-joins/) is below:

![Inner join -- courtesy of codinghorror.com](http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702708970c-pi.png)

The pandas function for performing joins is called `merge` and an Inner join is
the default option:  

```python
merged_inner = pd.merge(left=sep_head,right=subset, left_on='readName', right_on='readName')
# In this case `readName` is the only column name in  both dataframes, so if we skippd `left_on`
# And `right_on` arguments we would still get the same result

# What's the size of the output data?
merged_inner.shape
merged_inner
```

**OUTPUT:**

```
   record_id  month  day  year  plot_id readName sex  hindfoot_length  \
0          1      7   16  1977        2         NL   M               32   
1          2      7   16  1977        3         NL   M               33   
2          3      7   16  1977        2         DM   F               37   
3          4      7   16  1977        7         DM   M               36   
4          5      7   16  1977        3         DM   M               35   
5          8      7   16  1977        1         DM   M               37   
6          9      7   16  1977        1         DM   F               34   
7          7      7   16  1977        2         PE   F              NaN   

   weight       genus   species    taxa  
0     NaN     Neotoma  albigula  Rodent  
1     NaN     Neotoma  albigula  Rodent  
2     NaN   Dipodomys  merriami  Rodent  
3     NaN   Dipodomys  merriami  Rodent  
4     NaN   Dipodomys  merriami  Rodent  
5     NaN   Dipodomys  merriami  Rodent  
6     NaN   Dipodomys  merriami  Rodent  
7     NaN  Peromyscus  eremicus  Rodent  
```

The result of an inner join of `sep_head` and `subset` is a new DataFrame
that contains the combined set of columns from `sep_head` and `subset`. It
*only* contains rows that have two-letter species codes that are the same in
both the `sep_head` and `subset` DataFrames. In other words, if a row in
`sep_head` has a value of `readName` that does *not* appear in the `readName`
column of `species`, it will not be included in the DataFrame returned by an
inner join.  Similarly, if a row in `subset` has a value of `readName`
that does *not* appear in the `readName` column of `sep_head`, that row will not
be included in the DataFrame returned by an inner join.

The two DataFrames that we want to join are passed to the `merge` function using
the `left` and `right` argument. The `left_on='readName'` argument tells `merge`
to use the `readName` column as the join key from `sep_head` (the `left`
DataFrame). Similarly , the `right_on='readName'` argument tells `merge` to
use the `readName` column as the join key from `subset` (the `right`
DataFrame). For inner joins, the order of the `left` and `right` arguments does
not matter.

The result `merged_inner` DataFrame contains all of the columns from `sep_head`
(record id, month, day, etc.) as well as all the columns from `subset`
(readName, genus, species, and taxa).

Notice that `merged_inner` has fewer rows than `sep_head`. This is an
indication that there were rows in `sep_data` with value(s) for `readName` that
do not exist as value(s) for `readName` in `organism_data`.

## Left joins

What if we want to add information from `subset` to `sep_head` without
losing any of the information from `sep_head`? In this case, we use a different
type of join called a "left outer join", or a "left join".

Like an inner join, a left join uses join keys to combine two DataFrames. Unlike
an inner join, a left join will return *all* of the rows from the `left`
DataFrame, even those rows whose join key(s) do not have values in the `right`
DataFrame.  Rows in the `left` DataFrame that are missing values for the join
key(s) in the `right` DataFrame will simply have null (i.e., NaN or None) values
for those columns in the resulting joined DataFrame.

Note: a left join will still discard rows from the `right` DataFrame that do not
have values for the join key(s) in the `left` DataFrame.

![Left Join](http://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b01287770273e970c-pi.png)

A left join is performed in pandas by calling the same `merge` function used for
inner join, but using the `how='left'` argument:

```python
merged_left = pd.merge(left=sep_head,right=subset, how='left', left_on='readName', right_on='readName')

merged_left

**OUTPUT:**

   record_id  month  day  year  plot_id readName sex  hindfoot_length  \
0          1      7   16  1977        2         NL   M               32   
1          2      7   16  1977        3         NL   M               33   
2          3      7   16  1977        2         DM   F               37   
3          4      7   16  1977        7         DM   M               36   
4          5      7   16  1977        3         DM   M               35   
5          6      7   16  1977        1         PF   M               14   
6          7      7   16  1977        2         PE   F              NaN   
7          8      7   16  1977        1         DM   M               37   
8          9      7   16  1977        1         DM   F               34   
9         10      7   16  1977        6         PF   F               20   

   weight       genus   species    taxa  
0     NaN     Neotoma  albigula  Rodent  
1     NaN     Neotoma  albigula  Rodent  
2     NaN   Dipodomys  merriami  Rodent  
3     NaN   Dipodomys  merriami  Rodent  
4     NaN   Dipodomys  merriami  Rodent  
5     NaN         NaN       NaN     NaN  
6     NaN  Peromyscus  eremicus  Rodent  
7     NaN   Dipodomys  merriami  Rodent  
8     NaN   Dipodomys  merriami  Rodent  
9     NaN         NaN       NaN     NaN  
```

The result DataFrame from a left join (`merged_left`) looks very much like the
result DataFrame from an inner join (`merged_inner`) in terms of the columns it
contains. However, unlike `merged_inner`, `merged_left` contains the **same
number of rows** as the original `sep_head` DataFrame. When we inspect
`merged_left`, we find there are rows where the information that should have
come from `subset` (i.e., `readName`, `genus`, and `taxa`) is
missing (they contain NaN values):

```python
merged_left[ pd.isnull(merged_left.genus) ]
**OUTPUT:**
   record_id  month  day  year  plot_id readName sex  hindfoot_length  \
5          6      7   16  1977        1         PF   M               14   
9         10      7   16  1977        6         PF   F               20   

   weight genus species taxa  
5     NaN   NaN     NaN  NaN  
9     NaN   NaN     NaN  NaN
```

These rows are the ones where the value of `readName` from `sep_head` (in this
case, `PF`) does not occur in `subset`.


## Other join types

The pandas `merge` function supports two other join types:

* Right (outer) join: Invoked by passing `how='right'` as an argument. Similar
  to a left join, except *all* rows from the `right` DataFrame are kept, while
  rows from the `left` DataFrame without matching join key(s) values are
  discarded.
* Full (outer) join: Invoked by passing `how='outer'` as an argument. This join
  type returns the all pairwise combinations of rows from both DataFrames; i.e.,
  the result DataFrame will `NaN` where data is missing in one of the dataframes. This join type is
  very rarely used.

