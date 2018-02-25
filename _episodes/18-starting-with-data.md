---
title: Starting With Data
teaching: 30
exercises: 30
questions:
    - " How can I import data in Python?"
    - " What is Pandas?"
    - " Why should I use Pandas to work with data?"
objectives:
    - "Navigate the workshop directory and download a dataset."
    - "Explain what a library is and what libraries are used for."
    - "Describe what the Python Data Analysis Library (Pandas) is."
    - "Load the Python Data Analysis Library (Pandas)."
    - "Use `read_csv` to read tabular data into Python."
    - "Describe what a DataFrame is in Python."
    - "Access and summarize data stored in a DataFrame."
    - "Define indexing as it relates to data structures."
    - "Perform basic mathematical operations and summary statistics on data in a Pandas DataFrame."
    - "Create simple plots."
---

# Working With Pandas DataFrames in Python

We can automate the process of performing data manipulations in Python. It's efficient to spend time
building the code to perform these tasks because once it's built, we can use it
over and over on different datasets that use a similar format. This makes our
methods easily reproducible. We can also easily share our code with colleagues
and they can replicate the same analysis.

### Starting in the same spot

To help the lesson run smoothly, let's ensure everyone is in the same directory.
This should help us avoid path and file name issues. At this time please
navigate to the workshop directory. If you working in IPython Notebook be sure
that you start your notebook in the workshop directory.

A quick aside that there are Python libraries like [OS
Library](https://docs.python.org/3/library/os.html) that can work with our
directory structure, however, that is not our focus today.

### Our Data

For this lesson, we will be the pore data we obtained from processing our Nanopore output in the software poretools.

We will be using our pore\_info.tsv file. 

We are studying the species and weight of animals caught in plots in our study
area. The dataset is stored as a `.tsv` file: each row holds information for a
single read:

| Column           | Description                        |
|------------------|------------------------------------|
| length        | Number of nucleotide bases in the read      |
| Name           | Name of the read as assigned by the Nanopore              |
| Sequence             | Nucleotide sequence of the read                 |
| Quality score            | Quality score of the read              |


The first few rows of our first file look like this:

```
length  name    sequence        quals
368     @bf327144-9003-4b2d-bad5-4cb380f40e8d runid=0adce96f0fe4a9964393668c94df10a3f88b9d25 read=14750 ch=280 start_time=2018-02-16T23:12:05Z 0//20180216_FAH50339_MN24138_sequencing_run_lambda_92236_read_14750_ch_280_strand.fast5  TTATTGTAGTCGGTGGTGTGGCGGGTTGACTGAACTTGCTGCTTTTGATGATGATATTATTGAACAGAGGCTCTCCGACGTTCACGGGTGACAAGCCGCGTATTGAAGGCCGATGCTGGCCAAAGTCAAAATCCGTGGCTCCGCCAAAGTGAGAGGCACCTGTCGAATTTGAGGCGTGCAGCCGATGAATCCCGTTATGCGTTTTGCTGTGTTGCCCGCATTGCGGAGAACTGATATCTTAAATTTGGCGACAAAGTGCCGTTTGGCCTCAAATATGGACGCCGGATGACCCCTCCAGCGTGTTTATCTCACGAGCACTCGTACCTGCCGCTCATCCGCCAGCAGGAGCTGGACTTTCTTTGATGCAA        )""'$#$#&&'+)-&,%%%*&%*+(,*%%)*,-'+*+,.//*($%$&(++-,+,*')*)220479,,''),,,'-,)*233:1')),0))*++,&*%&'+-/.4*++'((%&$)#%'&++'.*%'**&*(&(-/**,))'*)&(1,5,+*,'''(%%%,5+-/7&$&,,''')*,(,&''))''$&'%')2/(&*(')..2/5,,'*&&)$('$&--*,'/*(&)'&'%%)#%&&')()(-*0(1++.12225.&)&(*2,74+)%%()*,5.,*)*135//0/351./021-+.*,-233.,)'%))/.&')***.+*($$$(),,0/%%(''(,,.040,-,,)*).1760.10-%###%$'%%"%
```

---

## About Libraries
A library in Python contains a set of tools (called functions) that perform
tasks on our data. Importing a library is like getting a piece of lab equipment
out of a storage locker and setting it up on the bench for use in a project.
Once a library is set up, it can be used or called to perform many tasks.

## Pandas in Python
One of the best options for working with tabular data in Python is to use the
[Python Data Analysis Library](http://pandas.pydata.org/) (a.k.a. Pandas). The
Pandas library provides data structures, produces high quality plots with
[matplotlib](http://matplotlib.org/) and integrates nicely with other libraries
that use [NumPy](http://www.numpy.org/) (which is another Python library) arrays.

Python doesn't load all of the libraries available to it by default. We have to
add an `import` statement to our code in order to use library functions. To import
a library, we use the syntax `import libraryName`. If we want to give the
library a nickname to shorten the command, we can add `as nickNameHere`.  An
example of importing the pandas library using the common nickname `pd` is below.


```python
import pandas as pd
```

Each time we call a function that's in a library, we use the syntax
`LibraryName.FunctionName`. Adding the library name with a `.` before the
function name tells Python where to find the function. In the example above, we
have imported Pandas as `pd`. This means we don't have to type out `pandas` each
time we call a Pandas function.


# Reading CSV Data Using Pandas

We will begin by locating and reading our survey data which are in CSV format. CSV stands for Comma-Separated Values and is a common way store formatted data. Other symbols my also be used, so you might see tab-separated, colon-separated or space separated files. It is quite easy to replace one separator with another, to match your application. The first line in the file often has headers to explain what is in each column. CSV (and other separators) make it easy to share data, and can be imported and exported from many applications, including Microsoft Excel. For more details on CSV files, see the [Data Organisation in Spreadsheets](http://www.datacarpentry.org/spreadsheet-ecology-lesson/05-exporting-data/) lesson.
We can use Pandas' `read_csv` function to pull the file directly into a
[DataFrame](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dataframe).

## So What's a DataFrame?

A DataFrame is a 2-dimensional data structure that can store data of different
types (including characters, integers, floating point values, factors and more)
in columns. It is similar to a spreadsheet or an SQL table or the `data.frame` in
R. A DataFrame always has an index (0-based). An index refers to the position of
an element in the data structure.

```python
# Note that pd.read_csv is used because we imported pandas as pd
pd.read_csv("pore_info.tsv", delimiter="\t")
```

The above command yields the **output** below:

```
     length                                               name  \
0       368  @bf327144-9003-4b2d-bad5-4cb380f40e8d runid=0a...   
1     27977  @f54a0ac6-3563-432f-a447-60e4c95efb79 runid=0a...   
2      3040  @43a565b2-0dcb-4ea6-a32d-3984c3746e47 runid=0a...   
3     13805  @f2f91bb1-2592-4193-b9ac-8d94b0f740b1 runid=0a...   
4     17176  @040557c3-1df7-475b-84e5-4ce2ea532508 runid=0a...   
5      2731  @26ad1236-11c3-40c7-9a6e-13899b65223a runid=0a...   
6      3224  @d666315c-d4e0-4f04-b34c-475670d82571 runid=0a...   
7       652  @0d00671e-d5db-4c2f-acba-7fe9061448f0 runid=0a...   
8      1216  @f55e9dec-43f9-45c4-a79f-b04921bbcd84 runid=0a...   
9     10827  @0bb5a3d8-29c6-4513-b6f5-8ad8a76549e6 runid=0a...   
10    15681  @cdae1bd8-aedc-443e-b399-f2c4f7b9556f runid=0a...   
11     2360  @a7ccd1ff-5dd2-4861-842b-c71526edac78 runid=0a...   
12     1983  @3106ea5c-aee8-4968-8566-6ac100511003 runid=0a...   
13    19988  @5327cf5f-1b9f-425c-b4f6-2ff5a8770409 runid=0a...   
14     7968  @2a6a1473-f2b0-4b70-8775-446ae1a5a01c runid=0a...   
15    18257  @2cdd4bf2-c991-4261-8800-56005ba0b87f runid=0a...   
16     1637  @291047ce-3b57-4ee4-b870-12508ea57426 runid=0a...   
17     2284  @694fa73f-88c5-4947-aa57-2c3af6ce96b6 runid=0a...   
18     3404  @f46d368c-642b-4f29-9d7e-731801ff19ec runid=0a...   
19    23683  @b7054984-f99e-4ab8-a264-16f986e78c04 runid=0a...   
20     2494  @485771d3-74e8-4eec-a63c-f582709065e5 runid=0a...   
21     2654  @814e4259-c8c8-4a6c-a79b-9fe0452b0d95 runid=0a...   
22      346  @eed92119-dec4-4497-823a-1321fc4cd00f runid=0a...   
23     6460  @411f7b31-3680-4f87-803c-308cbef250b5 runid=0a...   
24     1932  @504eed53-7ebe-4dd3-b048-07dd93111f9c runid=0a...   
25     2745  @23729eb3-8c79-4e03-b61f-900b02c6504e runid=0a...   
26     2044  @6ea3e1db-b349-4fc6-8b8e-0858099bbba1 runid=0a...   
27     6086  @dda7c716-75b2-476b-9359-c12c8ba20819 runid=0a...   
28     2614  @92adb215-0584-48c0-bc93-07d76189a2cd runid=0a...   
29      423  @2c2688e2-484d-4d67-9774-c60e739b8673 runid=0a...   
..      ...                                                ...   
365    4023  @a4e0c07b-b2aa-406e-89ba-347a7dd72e32 runid=0a...   
366    6304  @cbef7e75-0e18-4e8c-824a-6d7f664bc225 runid=0a...   
367     728  @02ec6236-bb8b-4ba2-a749-06f7df421ea2 runid=0a...   
368   44774  @9cdeceb3-4fa3-4e3a-ade9-03e4944af8c8 runid=0a...   
369   10466  @69d2f308-87b8-41c9-9fb4-890d6c685ee5 runid=0a...   
370    4977  @b57073c0-1c72-4241-b49d-a60a4455ba72 runid=0a...   
371    2620  @8ed0a330-939f-4645-8b1e-d634167eddf9 runid=0a...   
372   10420  @8e7f35b3-9c7c-4544-b26b-6bd5af0483a9 runid=0a...   
373    4274  @37dccd2a-f92b-4825-899c-cb99503a10ff runid=0a...   
374    6493  @addbe750-7e08-457c-9533-c0333a82edd5 runid=0a...   
375    1056  @70114cd7-1a31-46ff-8e50-a5935a707c16 runid=0a...   
376    3579  @401f7455-a253-4c5f-a47b-99d5c9910868 runid=0a...   
377   11644  @086d1575-5d10-49f6-8529-206a1060f48c runid=0a...   
378    3408  @1b814771-21f3-4930-b9c0-6095950b4947 runid=0a...   
379    2087  @f64d7f3d-550a-4119-b085-e5be8210d3d1 runid=0a...   
380    2185  @5ab3e470-5a6c-4b02-ab11-f008b527a0e1 runid=0a...   
381    1616  @3324120a-2e10-4b72-a6e4-9764e61967aa runid=0a...   
382   42221  @9dcdbd70-98ea-4c48-b3a8-515ffe1fde58 runid=0a...   
383    1748  @ebff0759-67b0-4497-9338-e41b638c3471 runid=0a...   
384   12966  @bbb0f47a-22e5-408c-8300-4a9b4bb18434 runid=0a...   
385   14958  @f9ae9751-bee9-4e99-a49c-9e89530c942c runid=0a...   
386    3938  @934ba96a-7b84-4483-861e-16947454cf3e runid=0a...   
387   41525  @d49f6709-7bfd-4aac-9d07-0b742cfc30a3 runid=0a...   
388    3548  @514a8790-b37d-4595-a94a-fc69396817d1 runid=0a...   
389    3518  @d0b0a814-2458-4439-a675-fc306a3667b1 runid=0a...   
390    3577  @d652a40f-e1e6-4237-a636-97049daa6dec runid=0a...   
391    3420  @448f2a9d-1bf5-4aeb-89e4-46ffb2a36a83 runid=0a...   
392    5223  @c840176a-4a47-4033-99fb-0609a7b5eae2 runid=0a...   
393    4452  @05c1dfe6-5588-48ec-aab7-a8367676e794 runid=0a...   
394    5009  @90dc1afb-233f-4e18-bdba-459d15d17c4b runid=0a...   

                                              sequence  \
0    TTATTGTAGTCGGTGGTGTGGCGGGTTGACTGAACTTGCTGCTTTT...   
1    TTGTTGGCACTTCGTTTCAGTTCTGCGGTGCTGGGCGGCGACCTCG...   
2    GGATTTCAGTTGCATGTTACTTATCCAAATTGTGTTTGGTTAGTCG...   
3    ATGCGTACTTCGTTCATTGTACTTCGTTCCAGTTACGTATTGCTGT...   
4    TTGGTACAGCCACTTCGTTCAGTTACGTATTGCTGGCGGCGACCTC...   
5    ATCACCGTTCGAGAGATTACGTATTGCGGATAGCGCCCGTAACCTG...   
6    CTCATAGGTTTCGTTCGGTTACGGTATTGCTGCCATCAGATTGTGT...   
7    TCGGTACTTCGCGGTTTCGCAGTTACGTATTGCTTGCGTGTGGAAA...   
8    TTGCGTGGTGAATTCATTCTCCTCGTGAATATCGACTTCAGGACGA...   
9    TTTTTTTGTGTATTGGGCGGCAGCCTCTTTCGCTATTTATGAAATT...   
10   CTGGCCAAAACTGGTTCCAGTTACGTATTGCTGTTCCAGCACAATC...   
11   CTAACCATTGCAGGGTGGCTCAATTACTGGCTGCCTTCCAGGGATG...   
12   TGGACTCCGGCACGATCTCGTCCATCGCCTGTACTTTTCATCCCGC...   
13   TCGTGTACTTCGTTCGAGATTACACGTATTGCGTTCGCCGCCCGTA...   
14   TCCGGTAGTACTTCGTTCCAGTTACGTACTAAGGTCGCCAAGCCCG...   
15   TCGGTGTACTTCGTTTAAGTTACGTATTGCTGGCGGCGACCTCGCC...   
16   CTGATGTACTTCGTTCGGTTTACATTGCTCAAGTGCCGTCCACCAT...   
17   CGTACACATTGCTACGTTCAGTTGTTGTGTGCTCATTGAAGAAAGT...   
18   GAACGGGTTCCCGGATTACGTATTATGCTTATCCAGTTGTGTGTTT...   
19   TTAAAAAATTCGAGTTACGTATTGCTGGAGTGCCGCCCCTTTAACC...   
20   TTGAAGGGCAATCCTTGCGTTTTGCAATGGCGTACGCTTCGCGGAA...   
21   ATCAGTATGCCTTGTATACGGAATTTCACAGTTACGTATTGCTGGG...   
22   TTGGTGCCGCTTCGGCGTTTCGATTACATATTGCGGCGCAAATTTC...   
23   ACGATACCCGTAGCACCGTTCAGAAGCCGCATTATTGACCGCCGGA...   
24   TTGGTATACTTCGTTCAGTTACGTATTGCTGTCAGATAGAGTTAAG...   
25   TGGTGGCGTTCGTTGCCGTATTTTAGCTGCCATTTGGCTCTGAATG...   
26   TTGTTGTACTTCGTTCCAGTTACGTATTGCTCTACAGCTTCCAGCA...   
27   TTGTTGTGCTTCCGGTTCATTTGCTGGGTCGCCGCCCCGTAACCTG...   
28   TTCATCCGGGTCATGCGGTGCCGCGCGAGGCGGCAGGATTCTTCCT...   
29   CATTGTGCTGGATTCCGGTTCACCGTATTGCTTAGCACGGTTCACT...   
..                                                 ...   
365  TTAAACGCTAACATTATCCAGTGATGATAACCAGACGACGGGCGCA...   
366  CATTGCTTCGTTCCAGTTACGTATTGCTTGACGATGATAAATTCAC...   
367  TTGGTGCTTCGTTCAAGTTACGTATTGCTGCTTACCATCAGATTGT...   
368  TGGGATGAATTTGGGTGGCGGAGGACGAATCGCTACTTTGGTACAT...   
369  TTGGTACTTCGTTCCAGTTACGTATTGCTGTTTAATGCATTGATGC...   
370  TTCAAAAGCTGGATTCAGTTGCAGTATTGCTGGAGTTTATAGAGGA...   
371  TTGAAGTGATGGCAGAGCGGAAAAGAGGCATTATTCAGCGCCGTTC...   
372  TCGGTGGCCCGCTTCGTTCAGTTGCATTGCTGAATTACTTCGCCCG...   
373  TTATTGTGCTACGTTTCAGTTACATTAACAGTTTATCCAAAAGGAA...   
374  CTATGAGATTTTCCACTCGTTTTGAAGAAGTCACCATTCTGAAGAT...   
375  TTGGTAATACTACGTTCAGTTACGTATTGCTGGTCGCGCCCGTAAC...   
376  TTCTGCGTGTACTTTGATTCAGTTGCATTGCTGCTTACGGTTCACT...   
377  TTGGTATTACTTCGTTCAGTTACATGACTGCTTGGTCGGCGAAAAC...   
378  CTCAGCAAAGCGCAGTTCAGTTACGTATTGCTGCTTACGGTTCACT...   
379  GAAGCGCTGATTTCAGTTGCGCCGCTATATTGTATCGTGAGGATGC...   
380  CTATGTTGCTTCGTTCAGTTACGTATTGCTGCTTACGGTTCACTAC...   
381  TGGTATACTTCGTTCAGTTACGTATTGCTAGGTCGCCGCCCGTAAC...   
382  ATCAGTGGCATTCACGTTCAGTTACGTATTGCTGGGCGGCATTTCA...   
383  TTGTTGTGCTTCCGTTCAGTTACGTATTGCGCTGCTTACGGTTCAC...   
384  TTGAGCGCAGCAACATCGCTGACGCATCTGCATGTCAGTAATTGCC...   
385  TTGTTGTACTTCGTTCAGTTACGTATTGCTAGGTCGCCGCCCGTAA...   
386  TTGTTGTGCTTCGTTCAGTTGCGTATTGCTCTCTGCTCATACGAGA...   
387  TGGGTATGCGCTGGTTGGAATTACGCGGCCATTGCTAGATTGCGCC...   
388  TGCGATACTTCGTTCCGTTACATTGCTGCCATCAGATTGTGTTTGT...   
389  TTGGTATGCTCGTTCAGTTACGTATTGCTCTTTTTTTTTTGGAATT...   
390  TCACCATTTGCTTCGTTCGGTTACGTATTGCTGCCAAATAGTTGTG...   
391  CTAAACGCTGGATTTCAGTTACGTATTTGCTGGCGGCGACCTCGTT...   
392  TTGTTGTACTTCGTTCAGTTACGTATTGCTAGGTCGCCGCCCAACC...   
393  TATTGTTGCTTTTCAGCGGCCCGCTTTCCTTCCGGCGTACCTTTGT...   
394  GGTAGCCATGGATTTCAGTTACGTATTGCTCTGTAGAAGGTGACGC...   

                                                 quals  
0    )""'$#$#&&'+)-&,%%%*&%*+(,*%%)*,-'+*+,.//*($%$...  
1    $&%'*'&$%'&(#(+410&+*,$#$$%')()219622522369343...  
2    +-&7.)*3537&+%+$4'%%%$(*5)+)*-,*.,,.*'**1.19+'...  
3    %*&'))*,204&'''#'''*'),+,*,11&%(+7+2.0,3-*)*'(...  
4    $$*'+'&&'&%&).412183012;1118/9//.;6;6587466:2+...  
5    %$&*)6:0-+*)+$-'%(**.10'%)'%&''+*16-.76716,)(*...  
6    ##((+%'$&&0+.103)3//&,'$%%.'*+233++69+.&4**-/4...  
7    ##&#*&*+.+'&+,*,,&%&%,0/./88,8,)****)**,''&))*...  
8    )"%%&$'%#%&%++--4;0.,&'&&)&%&$$%&$)'&'%$&.&-$%...  
9    $$$$&('(*(-&&('*'323,),,)&''+.+%'))0++''%.--*0...  
10   #"$$#&())%%&&&-*,'&(5,+,0+-/1..25824,+***0/80*...  
11   $(01.**/3./-0/*).((%&&%/(+-*-))')++(*-&#43),*(...  
12   $#&-..--/-*++++/.23&22+(--+-)(*&./3:;646+..*(0...  
13   $2,.+,.03884597&&)$.'''(*+0.21..*)-++-9-/4,%'+...  
14   $,,5-,)%''+1.214++&&%/*'%$)$&&%(0,64))'()&*+&(...  
15   '&.')%+).295306,'(&0+(),+/,4-)()&$)*'))))%)$$%...  
16   %#&$($%$('*1-15++(*('%().***+,3+).,.2/403260.,...  
17   $'''('(&(%%'*%*(+-,&*/-%,,(+&,%()-/9/-.)$()'$&...  
18   $##$$/-..*)')*%.($*+-)*%%$#&&&()02&,+*(*(*+*63...  
19   %$(4%%$$$&'&&,,.&*)-)0&$&%&(&%(%%%$..,%%'%)&++...  
20   $&(('(-/*3,+10/0--+,//-./+2+).,)--+((($%$$%)('...  
21   $.'$')(.+,.((#%%&&&)*0)30)*%)&--0,+(*%*&%-((,*...  
22   $(-*(%$$##-''&%%&'(&$$$,'&'&&&))&#%$#$%)-&)1(0...  
23   #')+,'+.('%$%%#'((,<6$&&$&+)')(4*%)('&/)$%%',&...  
24   ##$$$%'',-6201:8597<6659,9+**124+3+.50-%').)'(...  
25   )%%*,+.050*'*$&%%'$%%&%%#%)$(*+-7*%$'(-,,+./)&...  
26   %''(*),/44840246''+28.//31843399731/**''*))%+$...  
27   11(/*&*&%%)%'*('%#$)*(',-240...67//27,-1987743...  
28   #####&%&)*++%('*'++%./((%''%&%%&*)*+0&30-24)**...  
29   $/'&''),.-&+%)(,40-,$'&&)&+(%$&$%#&%()/+/3')*%...  
..                                                 ...  
365  ####$,)*)%%/+//.*.)((**,+(++--+(()1,+&%&)'&%$#...  
366  %*(&)9,7-118&&(11,-,.206+''()%+++-,2359.5*/%')...  
367  #$##$#(.,83720-*)9-++.+(+)&),&$&&$,4004,,-7,.+...  
368  #&)%#$&$&)/029412&#('$)).)&''$%%),%'*.2*&%#%%(...  
369  %#)$&$)%(#%10,((*9.%,.1)1+&+,'*'$'$,+/0.8/).*+...  
370  )#%/-,'%%'&)#/)*''/)%&'&)),.)*.00-+//+)*,-&'('...  
371  #"%(()')(()/-.,,-../58.((&(')',,-465++,./:5440...  
372  '$&()%&(*%%(/-6,/74')-*'('((%&%)#$(%&)0.3)-5-+...  
373  $"):+++%+,)-'-.(('+),)*$%&')./--1%&'2'042)+--0...  
374  $$####%&/081)+$)(3.6::++0,'&()(($))55-)$$&-*.1...  
375  )1((*(&(%()%((-,8(-5-%,((+01,,.355634/.;4-/957...  
376  %&%#$$&,((*-,1((#)+-)-.&#$#&'(+10.--.0.79<:356...  
377  &%0'+(,)'+3:45648.005.0++&$#&$(,,.4023,,(,20&2...  
378  %"$$&'++&$%#%$))4.(-/3)(%%$,')++*)*%%'('3,5/12...  
379  $$$%%(()(%)2)+-+2*%%%%$$%'$((*+(*),3.7.+.6-,),...  
380  &/-0-+,&$')3,09+)*7,*+12'&**0))(.&,-9:9;956711...  
381  #(%%%'%())(**3-)1.700.4,4,,-7%,,*)*:/62;3/0715...  
382  $#&&'''$((&&(*15;5343))))'-.,+--.-()%&$).*+&&(...  
383  &.%/,%*+,,21(*331+4/8./.5-7*%''')((.)*+49784*+...  
384  (#**--16/01.--,)+*-,+'*(*,/2/-...08:).+-/2//48...  
385  %(.)'%*&-17346:7(0.8./.401812375:12*-*'*16*,76...  
386  (0005,/+0-/34788,-6-(,24*9***20/.+)+--/..0)&)$...  
387  $&/&%$$%&&&%((**'+'&(%$%'*%+(**+301.,('**&(),2...  
388  #"%'$''++95,2+*%(*.*.')'),7245515+,)8*,-/478/2...  
389  %-122,($$')+,641/610098171&++*,28588<;<7,-),.0...  
390  #%#&%%'-*&*,3*+-64+281+.//.13/0/+,),20-',6,,,/...  
391  $'&&$$##'%''..+1&.)80003-.2-(()+3.-7222553-*,)...  
392  .1+00(&&'()/,249(*-,*+./--3-//+(()+-5*''&&$'%'...  
393  $&5*%,($&&)-+1,'-')+5-),/350&('')+20/*+0/,11--...  
394  '1('',)(('+$.)),.0/900,.,-/'/1--+*&&((**/125.)...  

[395 rows x 4 columns]
```

We can see that there were 33,549 rows parsed. Each row has 9
columns. The first column is the index of the DataFrame. The index is used to
identify the position of the data, but it is not an actual column of the DataFrame.
It looks like  the `read_csv` function in Pandas  read our file properly. However,
we haven't saved any data to memory so we can work with it.We need to assign the
DataFrame to a variable. Remember that a variable is a name for a value, such as `x`,
or  `data`. We can create a new  object with a variable name by assigning a value to it using `=`.

Let's call the imported survey data `surveys_df`:

```python
nano_dat = pd.read_csv("pore_info.tsv", delimiter="\t")
```

Notice when you assign the imported DataFrame to a variable, Python does not
produce any output on the screen. We can view the value of the `surveys_df`
object by typing its name into the Python command prompt.

```python
nano_dat
```

which prints contents like above.

Note: if the output is too wide to print on your narrow terminal window, you may see something 
slightly different as the large set of data scrolls past. You may see simply the last column
of data:
```python
17        NaN  
18        NaN  
19        NaN  
20        NaN  
21        NaN  
22        NaN  
23        NaN  
24        NaN  
25        NaN  
26        NaN  
27        NaN  
28        NaN  
29        NaN  
...       ...  
35519    36.0  
35520    48.0  
35521    45.0  
35522    44.0  
35523    27.0  
35524    26.0  
35525    24.0  
35526    43.0  
35527     NaN  
35528    25.0  
35529     NaN  
35530     NaN  
35531    43.0  
35532    48.0  
35533    56.0  
35534    53.0  
35535    42.0  
35536    46.0  
35537    31.0  
35538    68.0  
35539    23.0  
35540    31.0  
35541    29.0  
35542    34.0  
35543     NaN  
35544     NaN  
35545     NaN  
35546    14.0  
35547    51.0  
35548     NaN  

[395 rows x 3 columns]
```
Never fear, all the data is there, if you scroll up. Selecting just a few rows, so it is
easier to fit on one window, you can see that pandas has neatly formatted the data to fit
our screen:
```python

>>> nano_dat.head() # The head() function displays the first several lines of a file. It
   length                                               name  \
0     368  @bf327144-9003-4b2d-bad5-4cb380f40e8d runid=0a...   
1   27977  @f54a0ac6-3563-432f-a447-60e4c95efb79 runid=0a...   
2    3040  @43a565b2-0dcb-4ea6-a32d-3984c3746e47 runid=0a...   
3   13805  @f2f91bb1-2592-4193-b9ac-8d94b0f740b1 runid=0a...   
4   17176  @040557c3-1df7-475b-84e5-4ce2ea532508 runid=0a...   

                                            sequence  \
0  TTATTGTAGTCGGTGGTGTGGCGGGTTGACTGAACTTGCTGCTTTT...   
1  TTGTTGGCACTTCGTTTCAGTTCTGCGGTGCTGGGCGGCGACCTCG...   
2  GGATTTCAGTTGCATGTTACTTATCCAAATTGTGTTTGGTTAGTCG...   
3  ATGCGTACTTCGTTCATTGTACTTCGTTCCAGTTACGTATTGCTGT...   
4  TTGGTACAGCCACTTCGTTCAGTTACGTATTGCTGGCGGCGACCTC...   

                                               quals  
0  )""'$#$#&&'+)-&,%%%*&%*+(,*%%)*,-'+*+,.//*($%$...  
1  $&%'*'&$%'&(#(+410&+*,$#$$%')()219622522369343...  
2  +-&7.)*3537&+%+$4'%%%$(*5)+)*-,*.,,.*'**1.19+'...  
3  %*&'))*,204&'''#'''*'),+,*,11&%(+7+2.0,3-*)*'(...  
4  $$*'+'&&'&%&).412183012;1118/9//.;6;6587466:2+...  
```

## Exploring Our Species Survey Data

Again, we can use the `type` function to see what kind of thing `surveys_df` is:

```python
>>> type(nano_dat)
<class 'pandas.core.frame.DataFrame'>
```

As expected, it's a DataFrame (or, to use the full name that Python uses to refer
to it internally, a `pandas.core.frame.DataFrame`).

What kind of things does `type(nano_dat)` contain? DataFrames have an attribute
called `dtypes` that answers this:

```python
>>> nano_dat.dtypes
length       int64
name        object
sequence    object
quals       object
dtype: object
```

All the values in a column have the same type. For example, months have type
`int64`, which is a kind of integer. Cells in the month column cannot have
fractional values, but the weight and hindfoot_length columns can, because they
have type `float64`. The `object` type doesn't have a very helpful name, but in
this case it represents strings (such as 'M' and 'F' in the case of sex).

We'll talk a bit more about what the different formats mean in a different lesson.

### Useful Ways to View DataFrame objects in Python

There are many ways to summarize and access the data stored in DataFrames,
using attributes and methods provided by the DataFrame object.

To access an attribute, use the DataFrame object name followed by the attribute
name `df_object.attribute`. Using the DataFrame `nano_dat` and attribute
`columns`, an index of all the column names in the DataFrame can be accessed
with `nano_dat.columns`.

Methods are called in a similar fashion using the syntax `df_object.method()`.
As an example, `nano_dat.head()` gets the first few rows in the DataFrame
`surveys_df` using **the `head()` method**. With a method, we can supply extra
information in the parens to control behaviour.

Let's look at the data using these.

> ## Challenge - DataFrames
>
> Using our DataFrame `nano_dat`, try out the attributes & methods below to see
> what they return.
>
> 1. `nano_dat.columns`
> 2. `nano_dat.shape` Take note of the output of `shape` - what format does it
>    return the shape of the DataFrame in?
>    
>    HINT: [More on tuples, here](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences).
> 3. `nano_dat.head()` Also, what does `nano_dat.head(15)` do?
> 4. `nano_dat.tail()`
{: .challenge}


## Calculating Statistics From Data In A Pandas DataFrame

We've read our data into Python. Next, let's perform some quick summary
statistics to learn more about the data that we're working with. We might want
to know how many animals were collected in each plot, or how many of each
species were caught. We can perform summary stats quickly using groups. But
first we need to figure out what we want to group by.

Let's begin by exploring our data:

```python
# Look at the column names
nano_dat.columns
```

which **returns**:

```
Index(['length', 'name', 'sequence', 'quals'], dtype='object')

```

Let's get a list of all the species. The `pd.unique` function tells us all of
the unique values in the `length` column.

```python
pd.unique(nano_dat['length'])
```

which **returns**:

```python
array([  368, 27977,  3040, 13805, 17176,  2731,  3224,   652,  1216,
       10827, 15681,  2360,  1983, 19988,  7968, 18257,  1637,  2284,
        3404, 23683,  2494,  2654,   346,  6460,  1932,  2745,  2044,
        6086,  2614,   423, 27692,  8942,  3528,  2030, 15737,  9634,
        9914,  7736, 10075, 28767, 16351,  1525,  1109,  3488, 10675,
        3855,   882, 10872,  8197, 14996, 12241,  2468, 13344,  1816,
        6303,  3583, 31585,   809,  4534,  8594, 17934,  1564,  2847,
        1497,  2818,  7162,  1991,   286,  8767,  1643, 14838,  3513,
       11508,  1535,   417, 15252, 11870,   791,  9240,  2648,  1226,
        1999, 16944,  2394, 12437,  3789,   786,  7857, 10022, 12435,
        4417,  4806,  1307,  6517,  8696,  3647, 11071, 16022, 32781,
        1855,  7560, 21484,  1041,  1726,  4943,   871,  2809,  2518,
       13767,  3550,  4098,  2942, 17302,   550,  2060,  5007,  2741,
        3579,  7416,  5642,  3233,  3433, 34511, 18445,  8699, 20247,
        3422,  3181,  1115, 39025,  1240,  3631, 10163,   689, 18853,
        3612,  7027,  5830,  1982,  1949, 23722,  2506,  2744,  3494,
       16210,  3402,  8451, 12900,  3955,   943, 20329, 24091, 48110,
        1624, 32505,  9828, 34871,  3114,   661, 11578, 11659,  2250,
        3450, 33330,  3735,  2248,  8156,  2076,  3970,  2124, 19993,
       32861,  2417, 11687, 12670,  1806,  5712, 11850, 22255, 16853,
        3208,  6491,   597, 10791,  3484,  3206, 47935,  1321, 13077,
       47758,  6018,  7089, 22382,  3225, 37712, 20988,  2874,  4342,
       28579,  2844,  3580,  4471,  3055,  3201,  3623,  1032,  4560,
        2002,   390,  5436,  9972,  2521,  1943,   730,   432, 12153,
        6494,  3428,  1224, 11969,  4799,  9337, 48151,  7577, 16162,
       25392,  3350, 20525, 10086, 40052,  3288,   714,  7011,  3476,
        3622, 24917,  3001, 11509,  1791, 30501,  3441,  3473,  3836,
        3602,  3708, 25477,   250, 10132,  1349,  5125,  3327,  1330,
        3088,  6760,  8456,  1398,  4316, 37324,  2069,  1323,  2153,
       22875, 11054,  3449,   468, 33005,  9783,  3337,  9568, 40327,
        2048,  1047,   249,  5095, 19178, 44995,  3616,   300,  2176,
        1741,   336,  5709, 48664, 25020, 39541,  5987, 36971, 29577,
        4401,  2425, 46697, 41595, 19300, 23516,  1419,  7296,  5136,
        4503,  1553,  8815, 26529,   935,  8609,  2946, 41114,  1034,
       10130,  1746, 10404, 13519,   860, 42912, 14965, 10037,  5589,
        2682,  9266,  2723,  3813,  3157,   609,  3545,  3863,  6120,
        4299,  3672, 19862,  8048,  4231, 46710,   541,  3158,   564,
        8077, 13193,  8037,  6110,  4012,  1095, 24437,  3570,  4105,
        4852,  4468,  3362, 18568, 25883,  3576,  3399, 19949,  1885,
        6637,  3342,  1693,  3308,   982,  4023,  6304,   728, 44774,
       10466,  4977,  2620, 10420,  4274,  6493,  1056, 11644,  3408,
        2087,  2185,  1616, 42221,  1748, 12966, 14958,  3938, 41525,
        3548,  3518,  3577,  3420,  5223,  4452,  5009])

```

> ## Challenge - Statistics
>
> 1. Create a list of unique lengths found in the surveys data. Call it
>   `size_range`. How many unique sizes are there in the data? 
>
> 2. What is the difference between `len(length)` and `nano_dat['length'].nunique()`?
{: .challenge}

# Groups in Pandas

We often want to calculate summary statistics grouped by subsets or attributes
within fields of our data. For example, we might want to calculate the average
weight of all individuals per plot.

We can calculate basic statistics for all records in a single column using the
syntax below:

```python
nano_dat['length'].describe()
```
gives **output**

```python
count      395.000000
mean      9280.949367
std      10988.108415
min        249.000000
25%       2481.000000
50%       4098.000000
75%      11611.000000
max      48664.000000
Name: length, dtype: float64
```

Does this look familiar? Maybe like some output from another software? 

We can also extract one specific metric if we wish:

```python
nano_dat['length'].min()
nano_dat['length'].max()
nano_dat['length'].mean()
nano_dat['length'].std()
nano_dat['length'].count()
```

But if we want to summarize by one or more variables, for example runid, we can
use **Pandas' `.groupby` method**. Once we've created a groupby DataFrame, we
can quickly calculate summary statistics by a group of our choice.

First, let's break up the name column. It has way too much information in it. First, we will create a column that is split on the delimiter within the column:

```UNIX
nano_dat['name_split'] = df['name'].str.split(' ')
```

If you now type nano_dat, you'll see that there is an additional column in our data frame, which contains a list of all the information formerly read as a single entry in the name column.

But how do we get them to each be a column, not a big list column? As it turns out, we can unpack this natively:

```
nano_dat['name'].str.split(' ', 5, expand=True)
```

But this creates a whole new data frame. That's not really what we want, either ... 

The below combines elements of both of these answers:

```
nano_dat = nano_dat.join(nano_dat['name'].str.split(' ', 5, expand=True).rename(columns={0:'readName', 1: 'runID', 2:'readNum', 3:'channel',4:'time',5:'lane'}))
```

Do it. Then look at the output. Take five and figure out what the command is doing.

Is anything still amiss?

```
nano_dat = nano_dat.drop(["name"], axis=1)
```

OK, so now we have a dataframe that has each bit of data in a column, as opposed to all the data mashed up in multiple columns. This is more useful, and allows us to do things like generate groupings by variable:

```python
# Group data by channel
grouped_data = nano_dat.groupby("channel")
```

The **pandas function `describe`** will return descriptive stats including: mean,
median, max, min, std and count for a particular column in the data. Pandas'
`describe` function will only return summary values for columns containing
numeric data.

```python
# Summary statistics for all numeric columns by sex
grouped_data.describe()
# Provide the mean for each numeric column by sex
grouped_data.mean()
```

`grouped_data.mean()` **OUTPUT:**

```python

>>> grouped_data.mean()
               length
channel              
ch=11    10044.375000
ch=152    4635.600000
ch=153   12996.396226
ch=159   24437.000000
ch=180    7342.166667
ch=186    9852.867647
ch=199    3917.916667
ch=22    10406.352941
ch=233    7881.700000
ch=242   12720.428571
ch=244    3488.000000
ch=248    4498.200000
ch=269    3634.000000
ch=280    4038.050000
ch=313   12287.250000
ch=317    9569.000000
ch=330    5013.875000
ch=353     661.000000
ch=358   10940.363636
ch=390    3460.333333
ch=398   11042.333333
ch=437   21275.833333
ch=442    7712.857143
ch=469    3342.000000
ch=482    2284.000000
ch=498   15831.000000
ch=50    12297.363636
ch=504    6129.300000
ch=59     3205.600000
ch=62      882.000000
ch=65    18424.900000
ch=76    16944.000000
ch=99    12296.500000 

```

The `groupby` command is powerful in that it allows us to quickly generate
summary stats.


## Quickly Creating Summary Counts in Pandas

Let's next count the number of samples for each channel (i.e., how many of our reads came from that channel). We can do this in a few
ways, but we'll use `groupby` combined with **a `count()` method**.


```python
# Count the number of samples by channel
nano_dat.groupby('channel').count()
```

Or, we can also count just the rows that read length > 5000:

```python
grouped = nano_dat.groupby('lane')
grouped.filter(lambda x: x['length'] > 5000)
```

## Basic Math Functions

If we wanted to, we could perform math on an entire column of our data. For
example let's multiply all length values by 2. A more practical use of this might
be to normalize the data according to a mean, area, or some other value
calculated from our data.

	# Multiply all length values by 2
	nano_dat['length']*2
