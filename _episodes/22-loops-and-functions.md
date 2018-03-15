---
title: Data Workflows and Automation
teaching: 40
exercises: 50
questions:
  - " Can I automate operations in Python? "
  - " What are functions and why should I use them? "
objectives:
    - Describe why for loops are used in Python.
    - Employ for loops to automate data analysis.
    - Write unique filenames in Python.
    - Build reusable code in Python.
    - Write functions using conditional statements (if, then, else).
---

So far, we've used Python and the pandas library to explore and manipulate
individual datasets by hand, much like we would do in a spreadsheet. The beauty
of using a programming language like Python, though, comes from the ability to
automate data processing through the use of loops and functions.

## For loops

Loops allow us to repeat a workflow (or series of actions) a given number of
times or while some condition is true. We would use a loop to automatically
process data that's stored in multiple files (daily values with one file per
channel, for example). Loops lighten our work load by performing repeated tasks
without our direct involvement and make it less likely that we'll introduce
errors by making mistakes while processing each file by hand.

Let's write a simple for loop that simulates what a kid might see during a
visit to the zoo:

```python
>>> animals = ['lion', 'tiger', 'crocodile', 'vulture', 'hippo']
>>> print(animals)
['lion', 'tiger', 'crocodile', 'vulture', 'hippo']

>>> for creature in animals:
...    print(creature)
lion
tiger
crocodile
vulture
hippo
```

The line defining the loop must start with `for` and end with a colon, and the
body of the loop must be indented.

In this example, `creature` is the loop variable that takes the value of the next
entry in `animals` every time the loop goes around. We can call the loop variable
anything we like. After the loop finishes, the loop variable will still exist
and will have the value of the last entry in the collection:

```python
>>> animals = ['lion', 'tiger', 'crocodile', 'vulture', 'hippo']
>>> for creature in animals:
...    pass

>>> print('The loop variable is now: ' + creature)
The loop variable is now: hippo
```

We are not asking python to print the value of the loop variable anymore, but
the for loop still runs and the value of `creature` changes on each pass through
the loop. The statement `pass` in the body of the loop just means "do nothing".

> ## Challenge - Loops
>
> 1. What happens if we don't include the `pass` statement?
>
> 2. Rewrite the loop so that the animals are separated by commas, not new lines
> (Hint: You can concatenate strings using a plus sign. For example,
> `print(string1 + string2)` outputs 'string1string2').
{: .challenge}

## Automating data processing using For Loops

The file we've been using so far, `pore_info.tsv`, a bunch of run data for our nanopore run. I'm going to have us separate it out by channel.

Let's start by making a new directory inside the folder `data` to store all of
these files using the module `os`:

```python
import os

os.mkdir('data/channel_files')
```

The command `os.mkdir` is equivalent to `mkdir` in the shell. Just so we are
sure, we can check that the new directory was created within the `data` folder:

```python
>>> os.listdir('data')
['']
```

The command `os.listdir` is equivalent to `ls` in the shell.

In previous lessons, we saw how to use the library pandas to load the species
data into memory as a DataFrame, how to select a subset of the data using some
criteria, and how to write the DataFrame into a CSV file. Let's write a script
that performs those three steps in sequence for the channel 66:

```python
import pandas as pd

# Load the data into a DataFrame
sep_data = pd.read_csv("pore_info.tsv", delimiter='\t')

# Select only data for the channel 22
data22 = sep_data[sep_data.channel == 22]

```
Why didn't this work?

```unix
sep_data['channel'] = sep_data['channel'].str.replace('ch=', '')

sep_data.channel = sep_data.channel.astype("int")

# Write the new DataFrame to a CSV file
data22.to_csv('data/channel_files/channel22.tsv', sep='\t')
```

To create channel data files, we could repeat the last two commands over and
over, once for each channel of data. Repeating code is neither elegant nor
practical, and is very likely to introduce errors into your code. We want to
turn what we've just written into a loop that repeats the last two commands for each
channel in the dataset.

Let's start by writing a loop that simply prints the names of the files we want
to create - the dataset we are using covers channel 1 through 512, and we'll create
a separate file for each of those channels. Listing the filenames is a good way to
confirm that the loop is behaving as we expect.

We have seen that we can loop over a list of items, so we need a list of channels
to loop over. We can get the channels in our DataFrame with:

```python
>>> sep_data['channel']
0      280
1      186
2      199
3      153
4      153
5      330
6      280
7      180
8      186
9      199
10     180

```

but we want only unique channels, which we can get using the `unique` function
which we have already seen.  

```python
>>> sep_data['channel'].unique()
array([280, 186, 199, 153, 330, 180, 269, 504,  22, 398, 390, 482, 358,
       233, 248, 152, 442,  50, 244,  62,  65,  59, 313,  99,  11,  76,
       498, 317, 437, 242, 353, 159, 469])
```

Putting this into our for loop we get

```python
>>> for channel in sep_data['channel'].unique():
...    filename='data/channel_files/channel_data' + str(channel) + '.tsv'
...    print(filename)
...
data/channel_files/channel_data280.tsv
data/channel_files/channel_data186.tsv
data/channel_files/channel_data199.tsv
data/channel_files/channel_data153.tsv
data/channel_files/channel_data330.tsv
data/channel_files/channel_data180.tsv
data/channel_files/channel_data269.tsv
data/channel_files/channel_data504.tsv
data/channel_files/channel_data22.tsv
data/channel_files/channel_data398.tsv
data/channel_files/channel_data390.tsv
data/channel_files/channel_data482.tsv
data/channel_files/channel_data358.tsv
data/channel_files/channel_data233.tsv
data/channel_files/channel_data248.tsv
data/channel_files/channel_data152.tsv
data/channel_files/channel_data442.tsv
data/channel_files/channel_data50.tsv
data/channel_files/channel_data244.tsv
data/channel_files/channel_data62.tsv
data/channel_files/channel_data65.tsv
data/channel_files/channel_data59.tsv
data/channel_files/channel_data313.tsv
data/channel_files/channel_data99.tsv
data/channel_files/channel_data11.tsv
data/channel_files/channel_data76.tsv
data/channel_files/channel_data498.tsv
data/channel_files/channel_data317.tsv
data/channel_files/channel_data437.tsv
data/channel_files/channel_data242.tsv
data/channel_files/channel_data353.tsv
data/channel_files/channel_data159.tsv
data/channel_files/channel_data469.tsv
```

We can now add the rest of the steps we need to create separate text files:

```python
for channel in sep_data['channel'].unique():

    # Select data for the channel
    channel_dat = sep_data[sep_data.channel == channel]

    # Write the new DataFrame to a CSV file
    filename = 'data/channel_files/channel_dat' + str(channel) + '.tsv'
    channel_dat.to_csv(filename, sep='\t')
```

Look inside the `channel_files` directory and check a couple of the files you
just created to confirm that everything worked as expected.

## Writing Unique FileNames

Notice that the code above created a unique filename for each channel.

	filename = 'data/channel_files/channel_dat' + str(channel) + '.csv'

Let's break down the parts of this name:

* The first part is simply some text that specifies the directory to store our
  data file in (data/channel_files/) and the first part of the file name
  (surveys): `'data/channel_files/channel_dat'`
* We can concatenate this with the value of a variable, in this case `channel` by
  using the plus `+` sign and the variable we want to add to the file name: `+
  str(channel)`
* Then we add the file extension as another text string: `+ '.csv'`

Notice that we use single quotes to add text strings. The variable is not
surrounded by quotes. This code produces the string
`data/channel_files/channel_dat32.csv` which contains the path to the new filename
AND the file name itself.

> ## Challenge - Modifying loops

> 1. Let's say you only want to look at data from a given multiple of channels. How would you modify your loop in order to generate a data file for only every 5th channel, starting from 85?
>
> 2. Instead of splitting out the data by channels, a colleague wants to do analyses each lane separately. How would you write a unique CSV file for each lane?
{: .challenge}

## Building reusable and modular code with functions

Suppose that separating large data files into individual channel files is a task
that we frequently have to perform. We could write a **for loop** like the one above
every time we needed to do it but that would be time consuming and error prone.
A more elegant solution would be to create a reusable tool that performs this
task with minimum input from the user. To do this, we are going to turn the code
we've already written into a function.

Functions are reusable, self-contained pieces of code that are called with a
single command. They can be designed to accept arguments as input and return
values, but they don't need to do either. Variables declared inside functions
only exist while the function is running and if a variable within the function
(a local variable) has the same name as a variable somewhere else in the code,
the local variable hides but doesn't overwrite the other.

Every method used in Python (for example, `print`) is a function, and the
libraries we import (say, `pandas`) are a collection of functions. We will only
use functions that are housed within the same code that uses them, but it's also
easy to write functions that can be used by different programs.

Functions are declared following this general structure:

```python
def this_is_the_function_name(input_argument1, input_argument2):

    # The body of the function is indented
    # This function prints the two arguments to screen
    print('The function arguments are:', input_argument1, input_argument2, '(this is done inside the function!)')

    # And returns their product
    return input_argument1 * input_argument2
```

The function declaration starts with the word `def`, followed by the function
name and any arguments in parenthesis, and ends in a colon. The body of the
function is indented just like loops are. If the function returns something when
it is called, it includes a return statement at the end.

This is how we call the function:

```python
>>> product_of_inputs = this_is_the_function_name(2,5)
The function arguments are: 2 5 (this is done inside the function!)

>>> print('Their product is:', product_of_inputs, '(this is done outside the function!)')
Their product is: 10 (this is done outside the function!)
```

LIVECODE BUILDING THIS FUNCTION HERE. Once it is built, they can procede to challenges.

> ## Challenge - Functions
>
> 1. Change the values of the arguments in the function and check its output
> 2. Try calling the function by giving it the wrong number of arguments (not 2)
>   or not assigning the function call to a variable (no `product_of_inputs =`)
> 3. Declare a variable inside the function and test to see where it exists (Hint:
>   can you print it from outside the function?)
> 4. Explore what happens when a variable both inside and outside the function
>   have the same name. What happens to the global variable when you change the
>   value of the local variable?
{: .challenge}

We can now turn our code for saving channel data files into a function. There are
many different "chunks" of this code that we can turn into functions, and we can
even create functions that call other functions inside them. Let's first write a
function that separates data for just one channel and saves that data to a file:

```python
def one_channel_csv_writer(this_channel, all_data):
    """
    Writes a csv file for data from a given channel.

    this_channel --- channel for which data is extracted
    all_data --- DataFrame with multi-channel data
    """

    # Select data for the channel
    surveys_channel = all_data[all_data.channel == this_channel]

    # Write the new DataFrame to a csv file
    filename = 'data/channel_files/function_surveys' + str(this_channel) + '.tsv'
    surveys_channel.to_csv(filename, sep='\t')
```

The text between the two sets of triple double quotes is called a docstring and
contains the documentation for the function. It does nothing when the function
is running and is therefore not necessary, but it is good practice to include
docstrings as a reminder of what the code does. Docstrings in functions also
become part of their 'official' documentation:

```python
one_channel_csv_writer?
```

```python
one_channel_csv_writer(232,sep_data)
```

We changed the root of the name of the CSV file so we can distinguish it from
the one we wrote before. Check the `channel_files` directory for the file. Did it
do what you expect?

What we really want to do, though, is create files for multiple channels without
having to request them one by one. Let's write another function that replaces
the entire For loop by simply looping through a sequence of channels and repeatedly
calling the function we just wrote, `one_channel_csv_writer`:


```python
def channel_data_csv_writer(start_channel, end_channel, all_data):
    """
    Writes separate CSV files for each channel of data.

    start_channel --- the first channel of data we want
    end_channel --- the last channel of data we want
    all_data --- DataFrame with multi-channel data
    """

    # "end_channel" is the last channel of data we want to pull, so we loop to end_channel+1
    for channel in range(start_channel, end_channel+1):
        one_channel_csv_writer(channel, all_data)
```

Because people will naturally expect that the end channel for the files is the last
channel with data, the for loop inside the function ends at `end_channel + 1`. By
writing the entire loop into a function, we've made a reusable tool for whenever
we need to break a large data file into channel files. Because we can specify the
first and last channel for which we want files, we can even use this function to
create files for a subset of the channels available. This is how we call this
function:

```python
# Load the data into a DataFrame
sep_data = pd.read_csv('out.tsv', delimiter='\t')

# Create CSV files
channel_data_csv_writer(23, 233, sep_data)
```

BEWARE! If you are using IPython Notebooks and you modify a function, you MUST
re-run that cell in order for the changed function to be available to the rest
of the code. Nothing will visibly happen when you do this, though, because
simply defining a function without *calling* it doesn't produce an output. Any
cells that use the now-changed functions will also have to be re-run for their
output to change.

> ## Challenge- More functions
>
> 1. Add two arguments to the functions we wrote that take the path of the
>    directory where the files will be written and the root of the file name.
>    Create a new set of files with a different name in a different directory.
> 2. How could you use the function `channel_data_csv_writer` to create a CSV file
>    for only one channel? (Hint: think about the syntax for `range`)
> 3. Make the functions return a list of the files they have written. There are
>    many ways you can do this (and you should try them all!): either of the
>    functions can print to screen, either can use a return statement to give back
>    numbers or strings to their function call, or you can use some combination of
>    the two. You could also try using the `os` library to list the contents of
>    directories.
> 4. Explore what happens when variables are declared inside each of the functions
>    versus in the main (non-indented) body of your code. What is the scope of the
>    variables (where are they visible)? What happens when they have the same name
>   but are given different values?
{: .challenge}

The functions we wrote demand that we give them a value for every argument.
Ideally, we would like these functions to be as flexible and independent as
possible. Let's modify the function `channel_data_csv_writer` so that the
`start_channel` and `end_channel` default to the full range of the data if they are
not supplied by the user. Arguments can be given default values with an equal
sign in the function declaration. Any arguments in the function without default
values (here, `all_data`) is a required argument and MUST come before the
argument with default values (which are optional in the function call).

```python
    def channel_data_arg_test(all_data, start_channel = 1977, end_channel = 2002):
        """
        Modified from channel_data_csv_writer to test default argument values!

        start_channel --- the first channel of data we want --- default: 23
        end_channel --- the last channel of data we want --- default: 233
        all_data --- DataFrame with multi-channel data
        """

        return start_channel, end_channel


    start,end = channel_data_arg_test (sep_data, 185, 267)
    print('Both optional arguments:\t', start, end)

    start,end = channel_data_arg_test (sep_data)
    print('Default values:\t\t\t', start, end)
```

```
    Both optional arguments:	185 267
    Default values:			23 233
```

The "\t" in the `print` statements are tabs, used to make the text align and be
easier to read.

But what if our dataset doesn't start in 23 and end in 233? We can modify the
function so that it looks for the start and end channels in the dataset if those
dates are not provided:

```python
    def channel_data_arg_test(all_data, start_channel = None, end_channel = None):
        """
        Modified from channel_data_csv_writer to test default argument values!

        start_channel --- the first channel of data we want --- default: None - check all_data
        end_channel --- the last channel of data we want --- default: None - check all_data
        all_data --- DataFrame with multi-channel data
        """

        if not start_channel:
            start_channel = min(all_data.channel)
        if not end_channel:
            end_channel = max(all_data.channel)

        return start_channel, end_channel


    start,end = channel_data_arg_test (sep_data, 185, 267)
    print('Both optional arguments:\t', start, end)

    start,end = channel_data_arg_test (sep_data)
    print('Default values:\t\t\t', start, end)
```
```
    Both optional arguments:	185 267
    Default values:			23 233
```

The default values of the `start_channel` and `end_channel` arguments in the function
`channel_data_arg_test` are now `None`. This is a build-it constant in Python
that indicates the absence of a value - essentially, that the variable exists in
the namespace of the function (the directory of variable names) but that it
doesn't correspond to any existing object.

> ## Challenge - Variables
>
> 1. What type of object corresponds to a variable declared as `None`? (Hint:
> create a variable set to `None` and use the function `type()`)
>
> 2. Compare the behavior of the function `channel_data_arg_test` when the
> arguments have `None` as a default and when they do not have default values.
>
> 3. What happens if you only include a value for `start_channel` in the function
> call? Can you write the function call with only a value for `end_channel`? (Hint:
> think about how the function must be assigning values to each of the arguments -
> this is related to the need to put the arguments without default values before
> those with default values in the function definition!)
{: .challenge}

## If Statements

The body of the test function now has two conditionals (if statements) that
check the values of `start_channel` and `end_channel`. If statements execute a segment
of code when some condition is met. They commonly look something like this:

```python
    a = 5

    if a<0:  # Meets first condition?

        # if a IS less than zero
        print('a is a negative number')

    elif a>0:  # Did not meet first condition. meets second condition?

        # if a ISN'T less than zero and IS more than zero
        print('a is a positive number')

    else:  # Met neither condition

        # if a ISN'T less than zero and ISN'T more than zero
        print('a must be zero!')
```

Which would return:

```
    a is a positive number
```

Change the value of `a` to see how this function works. The statement `elif`
means "else if", and all of the conditional statements must end in a colon.

The if statements in the function `channel_data_arg_test` check whether there is an
object associated with the variable names `start_channel` and `end_channel`. If those
variables are `None`, the if statements return the boolean `True` and execute whatever
is in their body. On the other hand, if the variable names are associated with
some value (they got a number in the function call), the if statements return `False`
and do not execute. The opposite conditional statements, which would return
`True` if the variables were associated with objects (if they had received value
in the function call), would be `if start_channel` and `if end_channel`.

As we've written it so far, the function `channel_data_arg_test` associates
values in the function call with arguments in the function definition just based
in their order. If the function gets only two values in the function call, the
first one will be associated with `all_data` and the second with `start_channel`,
regardless of what we intended them to be. We can get around this problem by
calling the function using keyword arguments, where each of the arguments in the
function definition is associated with a keyword and the function call passes
values to the function using these keywords:

```python
    def channel_data_arg_test(all_data, start_channel = None, end_channel = None):
        """
        Modified from channel_data_csv_writer to test default argument values!

        start_channel --- the first channel of data we want --- default: None - check all_data
        end_channel --- the last channel of data we want --- default: None - check all_data
        all_data --- DataFrame with multi-channel data
        """

        if not start_channel:
            start_channel = min(all_data.channel)
        if not end_channel:
            end_channel = max(all_data.channel)

        return start_channel, end_channel


    start,end = channel_data_arg_test (sep_data)
    print('Default values:\t\t\t', start, end)

    start,end = channel_data_arg_test (sep_data, 23, 223)
    print('No keywords:\t\t\t', start, end)

    start,end = channel_data_arg_test (sep_data, start_channel = 185, end_channel = 265)
    print('Both keywords, in order:\t', start, end)

    start,end = channel_data_arg_test (sep_data, end_channel = 265, start_channel = 185)
    print('Both keywords, flipped:\t\t', start, end)

    start,end = channel_data_arg_test (sep_data, start_channel = 185)
    print('One keyword, default end:\t', start, end)

    start,end = channel_data_arg_test (sep_data, end_channel = 265)
    print('One keyword, default start:\t', start, end)
```
```
    Default values:			23 233
    No keywords:			185 265
    Both keywords, in order:	185 265
    Both keywords, flipped:		185 265
    One keyword, default end:	185 265
    One keyword, default start:	23 265
```

> ## Challenge - Modifying functions
>
> 1. Rewrite the `one_channel_csv_writer` and `channel_data_csv_writer` functions to
> have keyword arguments with default values
>
> 2. Modify the functions so that they don't create channel files if there is no
> data for a given channel and display an alert to the user (Hint: use conditional
> statements to do this. For an extra challenge, use `try`
> statements!)
>
> 3. The code below checks to see whether a directory exists and creates one if it
> doesn't. Add some code to your function that writes out the CSV files, to check
> for a directory to write to.
>
> ```Python
>	if 'dir_name_here' in os.listdir('.'):
>	    print('Processed directory exists')
>	else:
>	    os.mkdir('dir_name_here')
>	    print('Processed directory created')
> ```
>
> 4. The code that you have written so far to loop through the channels is good,
> however it is not necessarily reproducible with different datasets.
> For instance, what happens to the code if we have additional channels of data
> in our CSV files? Using the tools that you learned in the previous activities,
> make a list of all channels represented in the data. Then create a loop to process
> your data, that begins at the earliest channel and ends at the latest channel using
> that list.
>
> HINT: you can create a loop with a list as follows: `for channels in channel_list:`
{: .challenge}
