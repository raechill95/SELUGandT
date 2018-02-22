---
title: A More In-Depth Look at Nanopore Data
teaching: 180
exercises: 0
questions:
- "What software is available to look at Nanopore data?"
- "What features do these software packages have?"
- "How can I use Pore to plot some interesting stats about my data?"
---

## Creating Custom Compute Environments on LONI

HPC environments balance between what is good for most users, and what is good for any individual user. So we get nice installs made available to us via modeuls, but we might not have every individual package we ever want to use available. This will be true of almost any compute environment you ever work with. 

Therefore, it is good to know how to create your own environments for loading and using software. Today, we will be creating a unique Python environment in our home directories, so we can work with a suite of Nanopore processing tools called PoreTools. 

We'll use a tool called [Miniconda](https://conda.io/miniconda.html), which is used for setting up Python compute environments, aimed at scientific computation. First, we'll download and install Miniconda. Run the below in your work directory.

```UNIX
wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh

bash Miniconda2-latest-Linux-x86_64.sh

source ~/.bashrc

conda info
```

The above downloads MiniConda. Then, it executes the installation script. 

The .bashrc file contains default settings about how your computer operates. Yours is probably close to empty right now, but the .bashrc will store system settings. For example, you see the way my terminal has different colors than yours. That information is coded in the .bashrc.

Likewise, we can write in information about our software settings. Miniconda makes itself the default Python installation on your system (LONI, by default has some old version). It has written this change to the .bashrc. But it won't read that change until we source the .bashrc file. 

Next, we use conda to install cython. Poretools depends on Cython. To depend, in the context of a software package means that a package is required by another. For example, cython interfaces with C/C++. Because we are working with huge data, C/C++ can dramatically decrease runtime. Some of the functions in poretools make use of the Cython interface to C/C++. Scipy has various numerical and statistical functions (like equation solvers and clustering algorithms).

```UNIX
conda install cython scipy
```

Now, we will install poretools. This software is actually made by [Nick Loman and Aaron Quinlan](https://github.com/arq5x/poretools). But there are some graphics functions that don't work on LONI. So I patched the software for us to use, and stored these changes on my personal fork.

```UNIX
git clone https://github.com/wrightaprilm/poretools

python setup.py install --user

```

We have now installed poretools locally. We will tell the .bashrc how to find it. Add the following line (substitute your username) to the end of the .bashrc. 


```UNIX

export PATH="$PATH:/home/amwright/.local/bin:$PATH"
```

Source the .bashrc again. Now, from wherever you are, on LONI, if you type

```
poretools -h
```

Poretools should start and display its usage options.

## Running Poretools

We saw that Nanopore provides us with some visualizations. Poretools enables a wider set of analyses about the quality and amount of data that we have obtained from our read.

First, grab a node. We'll need some memory to run this:

```
qsub -I -V -A loni_selu_gt -q single -l nodes=1:ppn=1,walltime=3:00:00

```
Recall that this will put you back into your home directory, and you will need to move back into your work.

Go into the reads folder, 2054, fast5, pass, 0. 

First, we will try to extract reads from our fast5 files. The poretools command fastq provides various ways to do that:

```unix

poretools fastq -h

```

This should produce the manual for this command:

```UNIX
poretools fastq test_data/
poretools fastq --min-length 5000 test_data/
poretools fastq --max-length 5000 test_data/
poretools fastq --type all test_data/
poretools fastq --type fwd test_data/
poretools fastq --type rev test_data/
poretools fastq --type 2D test_data/
poretools fastq --type fwd,rev test_data/
```

Let's try just getting the biggest reads:

```UNIX
poretools fastq --min-length 5000 0/ > big_reads

```
Poretools will automatically find all FAST5 files in a directory when using the fastq command. The last bit of this command is called a redirect. It will write our output to a file. By default, poretools writes to the standard output (i.e., the screen). How many long reads do we have? (Hint: The [pipes and filters lesson](https://paleantology.github.io/SELUGandT/05-Pipes/) from two weeks ago contains a command that will be useful to you in finding this info). 

What happens if you combine two commands? Take five, pick another command that interests you, and see if you can come up with an interesting output.

We can get a quick look at summary stats like so:

```UNIX
poretools stats 0/
```


## Graphical Outputs

Getting graphical outputs on servers is hard - because servers have a text interface. Poretools is really slick, and backended with several powerful graphics libraries in Python, so we can make some nice looking graphical outputs of our data with little effort.

We can plot the distribution of read lengths like so:

```UNIX
poretools hist --saveas hist.pdf 0/
```

Now, lets look at the number of reads and basepairs as a function of time. 

```UNIX
poretools yield_plot --plot-type reads --saveas reads.pdf 0
poretools yield_plot --plot-type basepairs --saveas bp.pdf 0
```

Tansfer these two pdfs to your machine using scp. Think about this hard, we'll come back together in a moment to see if we've been sucessful. Look at the distribution of read lengths - does it make sense per the summary stats? 

Now, let's have a look at quality:

```UNIX
poretools qualpos --saveas quality.pdf 0/
```

Not at all amazing. 

## Outputs for further visualizations

We have some plots that we've made. But we might want to have custom visuals. Resultantly, we can store the data in a tabular format to download and visualize in a medium of your choosing:

```UNIX
poretools tabular 0/ > pore_info.tsv
```

Have a look at what is in this file:

```UNIX
head -n2 pore_info.tsv 
```

Should look something like this: 

```UNIX
length	name	sequence	quals
368	@bf327144-9003-4b2d-bad5-4cb380f40e8d runid=0adce96f0fe4a9964393668c94df10a3f88b9d25 read=14750 ch=280 start_time=2018-02-16T23:12:05Z 0//20180216_FAH50339_MN24138_sequencing_run_lambda_92236_read_14750_ch_280_strand.fast5TTATTGTAGTCGGTGGTGTGGCGGGTTGACTGAACTTGCTGCTTTTGATGATGATATTATTGAACAGAGGCTCTCCGACGTTCACGGGTGACAAGCCGCGTATTGAAGGCCGATGCTGGCCAAAGTCAAAATCCGTGGCTCCGCCAAAGTGAGAGGCACCTGTCGAATTTGAGGCGTGCAGCCGATGAATCCCGTTATGCGTTTTGCTGTGTTGCCCGCATTGCGGAGAACTGATATCTTAAATTTGGCGACAAAGTGCCGTTTGGCCTCAAATATGGACGCCGGATGACCCCTCCAGCGTGTTTATCTCACGAGCACTCGTACCTGCCGCTCATCCGCCAGCAGGAGCTGGACTTTCTTTGATGCAA	)""'$#$#&&'+)-&,%%%*&%*+(,*%%)*,-'+*+,.//*($%$&(++-,+,*')*)220479,,''),,,'-,)*233:1')),0))*++,&*%&'+-/.4*++'((%&$)#%'&++'.*%'**&*(&(-/**,))'*)&(1,5,+*,'''(%%%,5+-/7&$&,,''')*,(,&''))''$&'%')2/(&*(')..2/5,,'*&&)$('$&--*,'/*(&)'&'%%)#%&&')()(-*0(1++.12225.&)&(*2,74+)%%()*,5.,*)*135//0/351./021-+.*,-233.,)'%))/.&')***.+*($$$(),,0/%%(''(,,.040,-,,)*).1760.10-%###%$'%%"%
```

which is a tabular format that can be read by most Office suites, R, Python, etc. 


## So obviously this run didn't go great. Homework! 

Embed the PDFs you downloaded below. Write a brief sentence about what the plot means and why it might have failed (or how we can improve). Add, commit and push this file, with your changes, to your copy of this repository by the start of class Tuesday.

![Embed your histogram](../fig/histogram) 

![Embed your quality ](../fig/qulity) 

![Embed your basepairs ](../fig/bp) 

![Embed your read length chart ](../fig/reads) 





