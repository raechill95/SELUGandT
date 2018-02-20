---
title: Reading and Writing Genomic Data
teaching: 120
exercises: 0
questions:
- "How can I safely move large data files?"
- "How can compression algorithms be used to increase data integrity?"
- "What is in my genomic data anyway?"
---

## Downloading and Checking Big Data

For today, we will briefly practice obtaining, unzipping, and checking large data files. Large data files pose unique challenges. They take a long time to copy, which leaves them vulnerable to corruption. Corruption simply means some part of the file has been written incorrectly. If you are, for example, moving a file between two different computers, and you lose internet, the file may terminate early, or have disjunct parts where copying resumed. Today, we will cover transferring files safely, and checking that they have arrived whole.

## Copy today's files

First, in your /work/ directory, make a folder called "data_transfer_practice". Enter it. Now, we will copy the sample data.

```UNIX
cp /work/amwright/sample_output.tar.gz .
```

These commands are unusual: you're being allowed to access my directories, which is not normally the case. Because the data files are large, it's a pain to host them via git or my personal website. So what I did is use the command

```
chmod g+rx /work/amwright
chmod g+rx sample_output/
```

to make this my directory accessible to you. You may find this command useful later. 

## Zipped Files

Now you have two files, both tarred and gzipped. What does that really mean? 'Tar' means _tape archive_, and a file that has been preserved in this way is called a tarball. When writing large amounts of data, it is actually more efficient to write them in one large block than many small. Think of this like a carton of eggs - it's easier to carry 12 in a dozen package than 12 loose. So a tarball collects the whole directory that you want to preserve together. Any subsequent movements of the tarball are then conducted on the whole tarball, rather than recursively on all subdirectories. 

Tarballs are often combined with a compression algorithm. .gz refers to the file as having been _gzipped_, an open-source file compression format. Compression makes a file smaller. It belongs to a family of algorithms called _lossless_ algorithms, which means the compression is entirely reversible. Losseless algorithms work by minimizing statistical redundancy. So 500 repititions of the character A in a row might become encoded as "500 A characters", saving 486 bytes of storage. Zipping typically only works on one file at a time, hence, we tar, then gzip. We will see how to do this later.

Together, these procedures turn many files into one, and make that one smaller. To illustrate:

```unix

ls -lth
```

You should see something like this: 

```UNIX
-rw-r--r-- 1 amwright loniusers 483M Sep  4  2014 sample_output.tar.gz
```

Now, we will untar and unzip one of the files:

```
tar xvf sample_output.tgz
```

Now, enter:

```UNIX
du -h
```

This is a command that recursively gets the size of every subdirectory in a directory and prints it to the screen. We can see that the sample_ouput directory has a size of about 1.2 GB. So the original directory was compressed by more than a factor of two-fold!

The utility of this is obvious to anyone who has ever been on our building's WiFi. But it's also a data integrity and hygeine issue: the larger a file is, the longer it will take to copy between devices. That means if you are uploading data to work with on LONI, or downloading data from a sequencing core facility, or copying data from me, data can be damaged in transit. An internet hiccup during file copying could lead to the file terminating early or having other interruptions.

These problems are not always obvious immediately. So we'll go over how to check your files for transit problems. 

Tarballs and gzip files have both headers and footers. If either of these are damaged, then the whole file has not been copied. The following command will check that the entirety of the gzip has been copied:

```UNIX
gzip -t sample_output.tgz
```

## What's in a Genomic Data File? 

The Minion produces data in the FAST5 format. This is based on the HDF5 format for storage of complex data. It is effectively a directory structure stored as a single flat file. This allows us to avoid having to make a tarball, and we can still use compression on the file. 

The best way to illustrate what a FAST5 file is is to look at one. In your sample data folder, go into the sample_data/reads/20180216_2054_lambda/fast5/pass/0 folder. Type:

```UNIX
module load hdf5/1.8.12/INTEL-140-MVAPICH2-2.0
h5ls -r h5ls -d 20180216_FAH50339_MN24138_sequencing_run_lambda_92236_read_9899_ch_442_strand.fast5/Analyses/Basecall_1D_000/BaseCalled_template/Fastq 
```

This looks very much like a directory structure. But it's not - it's a single flat file, in which every bit of data has been tagged with what it is. There's a lot of data in here, for example:

```UNIX
h5ls -d 20180216_FAH50339_MN24138_sequencing_run_lambda_92236_read_9899_ch_442_strand.fast5/Analyses/Basecall_1D_000/BaseCalled_template/Fastq
```
Shows a compressed version of the alignment. Overall, this type of file is data rich, but hard to look at. We will use software to break it down next week.

One of the chief things we will do is get FASTQ files out of the raw data. FASTQ files are a little more human readable, containing sequence reads and quality information. Move into the fastq folder and use head to look at the first few lines:

```UNIX
head fastq_runid_0adce96f0fe4a9964393668c94df10a3f88b9d25_0.fastq
```

You will note that we have a line identifying the sample, followed by a line of data and a line of gobbledeegook. 

 !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~
 
 Are the total character encodings for quality. Left to right is increasing quality. How good is this sequence?
 
 
 
 
 
 
 