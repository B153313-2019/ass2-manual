---
description: Manual for normal operators
---

# Manual - Original User Version

* **This application is user - frendly.** 
* **Firstly, you need enter "python3 BPSMassignment2.py" in the server terminal.**

```text
python3 BPSMassignment2.py
```

* **Then the script will show you some basic information to you, and ask you whether start or not.**
* If you enter yes, the task 1 will start. If you enter no, the program will be closed.

```text
==============================
# Assignment 2 of BPSM       #
# Version:2.0.0              #
# Updated:2019.11.14         #
# Author:LittleTrain         #
==============================

=================This application is really useful!!!=================
# Give the protein family and the taxonomic group
# Then the relevant protein sequence will be retrived
# Alignment of these sequences will be completed
# Similarity graph of aligned sequences will be drawn
# Motifs of protein sequences will be scanned
# And other 3 functions wait you to find and use......
======================================================================

Do you want to start task 1 now?(yes or no)

```

* **At the beginning of task 1, you need to enter a protein family name and a taxonomy group name.**
* And then choose a mode to filter sequences. Once you determined the names and mode, the program will search and print the number of sequences that found. The program will ask you whether download these sequences. If you enter yes, then sequences will be downloaded, if no, the program will exit.
* **If the number of sequence is more than 10000,  there will have a warning to you. It is really too much to download more than 10000 sequences,  and the clustalo limit the multiple alignment sequences  number is 10000. You can choose to continue with such huge number of sequences, But I highly recommend you not to do this.**

```text
=======================Task 1 starts executing========================

Please input a protein family name:glucose-6-phosphatase
Please input a taxonomic group name:aves

There are 8 modes to filter sequences:
1."null"
2.[TI]
3.NOT PARTIAL
4.NOT PREDICTED
5.[TI] NOT PARTIAL
6.[TI] NOT PREDICTED
7.NOT PARTIALNOT PREDICTED
8.[TI] NOT PARTIAL NOT PREDICTED

Choose one mode that you want
Please enter the index(1-8):

The number of sequences is 729
Do you want to download them(yes or no)?
```

```text
*******************************Warning********************************
The number of sequences is 15664.
You can continue or exit, but you are highly recommended to change mode or names:
1.still continue
2.change to a more precise mode
3.reassign a protein family and a taxonomy group
4.exit
Please make your choice(enter the index of choices):
```

* **After waiting for a short while, download will be finished, and the process of task 1 done.**
* The file name of those sequences is "ProteinSequences.fasta".

```text
----------------------Download starts executing-----------------------

Downloading, please wait.
It may take you ten seconds to a few minutes.
This is determined by the amount of data.

--------------------------Download finished---------------------------

=======================Task 1 has been finished=======================

```

* **Then, the program whether continue or exit. If you enter yes, task 2 will start executing.**

```text
Do you want to continue to process task 2 (yes or no)?yes

=======================Task 2 starts executing========================

Data processing, please wait...

```

* **When task 2 begin, the program will process data. Then it will show you the number of sequences.** Now, we want to plotting to sequences. But before this begin, the program will ask you to choose a mode. There are two mode for you, first is create alignment and conservation graph of all sequences, second is to find 250 most similar sequences and use them to make alignment and plotting. 
* **You should remember, the mode you choose will also applied in the followed tasks.**

```text
=======================Task 2 starts executing========================

Data processing, please wait...

The number of sequences is 729
You can create a conservation graph of all of sequences
You can also plotting to no more than the 250 most similar sequences.
Now, make your choice.
Remember, the number you choose will also applied in task 3

1.plotting to all of sequences
2.plotting to no more than the 250 most similar sequences
3.exit

Pleas enter the index of you want(1-3):2
Create a consensus sequence from a multiple alignment

Building a new DB, current time: 11/16/2019 01:40:56
New DB name:   /localdisk/home/s1920138/Assignment2/nem
New DB title:  ProteinSequences.fasta
Sequence type: Protein
Keep MBits: T
Maximum file size: 1000000000B
Adding sequences from FASTA; added 729 sequences in 0.043052 seconds.
CFastaReader: Hyphens are invalid and will be ignored around line 2
Plot conservation of a sequence alignment
Created plotcon.svg

=======================Task 2 has been finished=======================

```

* **Next task is to scan protein sequences of interest with motifs from the PROSITE database, to determine whether any known motifs \(domains\) are associated with this subset of sequences.**
* Once you enter "yes" to start this task, the program will run automatically. And the progress bar will tell you how much has the program completed.
* And the result stored in "MotifTotalResult.txt". After the application finished, you can enter _head MotifTotalResult.txt_ in the unix terminal to check the result.

```text
Do you want to continue to process task 3 (yes or no)?yes

=======================Task 3 starts executing========================

Task 3: |██████████████████████████████████████████████████| 100.0% Complete

Process completed. Result stored in "MotifTotalResult.txt".

=======================Task 3 has been finished=======================
```

* **After the end of the three main programs,  there are 3 extra functions in this application.**
* You can choose one at will. And once the program completed, you can choose another program until all of 3 programs completed.
* The result stored in relative files \(pepwindowall.svg, charge.svg, tmap.svg\)

```text
======================Now, it's "wildcard" time!======================

There are 3 extra functions in this application.

1.pepwindowall:Draw Kyte-Doolittle hydropathy plot for a protein alignment

2.pscan:Scan protein sequence(s) with fingerprints from the PRINTS database

3.tmap:Predict and plot transmembrane segments in protein sequences

4.exit

Please enter your command:1

--------------The program pepwindowall starts executing---------------

Draw Kyte-Doolittle hydropathy plot for a protein alignment
Created pepwindowall.svg

--------------------Program pepwindowall completed--------------------

2.pscan:Scan protein sequence(s) with fingerprints from the PRINTS database

3.tmap:Predict and plot transmembrane segments in protein sequences

4.exit

Please enter your command:2

-----------------The program charge starts executing------------------

Draw a protein charge plot
Created charge.svg

-----------------------Program charge completed-----------------------

3.tmap:Predict and plot transmembrane segments in protein sequences

4.exit

Please enter your command:3

------------------The program tmap starts executing-------------------

Predict and plot transmembrane segments in protein sequences
Created tmap.svg

------------------------Program tmap completed------------------------

Congratulations! All the projects have been completed.

************The program will quit, thank you for your use*************
```

* That's all of my application, thank you for your use.

