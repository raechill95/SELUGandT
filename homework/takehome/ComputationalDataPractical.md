## Computational Data Practical

### Project Organization practical

1. Create a new directory in your work directory. Title it with your last name and "datapractical". 

2. Within the directory, create scripts, output and data subdirectories. 

3. Initialize a git repository in your directory. Create a README. Copy the text from this document into the README. **You will fill out your answers in the README.**

4. Import the data from the class repository into your project repo. You will need to git pull to get updated files. The files will be in the ```homework/takehome``` folder of the class repo. 

### Pandas practical

The next several steps will involve data cleaning in Python.  I will ask you to make commits at various points, or to paste in the command you used, or to save a copy of a datafile.

5. Load both the "good" and "bad" data files into Pandas. Standardize the columns across the two datafiles. This may involve splitting columns up, or dropping columns from the dataframes. 

6. Save these two dataframes to files with names that mark them as distinct from the raw data. 

7. Now, combine each of these dataframes with the file of the same name with the suffix "\_organism\_info". How will it make sense to add the "organism info" column to the dataframe. Hint: it does not have to be a join. We have covered other syntax for adding a column.

8. Save these two new dataframes to files with names that mark them as distinct from the raw data.

9. Combine the two dataframes. You should have then have one dataframe. Paste below the command that you used to do this join or concatenation. Save the dataframe of the joined or concatenated data with a name indicating what it is. 

Exit Pandas. Make sure all the datafiles generated in steps 1-9 are in your data directory, and commit them. From the project directory in the course repo, copy over the pore_plotting.py into your scripts directory.  

### Script practical

10. In general terms, what is this file doing. What is it's input? What is its output. You can call this script with

```
python pore_plotting.py -h
```

to see the help, but to understand what is going on, you may need to have a look at the code. You will not understand all of it, and that's OK.

11. Call the script on each data frame. Write the plots to the output folder.

12. Make two comments in the script: one comment on a line 12, explaining what the code in lines 8-11 are doing. Make one other comment somewhere indicating something you don't understand.

13. Save and commit your modified script.

For the last bit, we will make a plot from our joined data with organism information (i.e. the data file you output in Pandas practical step 9. 

### Plotting practical

14. Load the joined dataset.

15. Convert the quality scores in this dataframe to numeric values.

16. Plot these make a bar-and-whiskers plot. This plot should show the quality per position in read, across reads. For each position, there should be two boxes: one for the "good" dataset, and one for the bad. Paste your code below.practical

17. Save your plot to the output directory. Embed the plot below.

![Your figure here](../output/yourfigure) 