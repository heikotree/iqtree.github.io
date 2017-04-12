---
layout: userdoc
title: "Frequently Asked Questions"
author: Jana Trifinopoulos, Minh Bui
date:    2017-04-12
docid: 9
icon: question-circle
doctype: manual
tags:
- manual
description: For common questions and answers.
sections:
- name: How do I get help?
  url: how-do-i-get-help
- name: How do I report bug?
  url: how-do-i-report-bug
- name: How to interpret ultrafast bootstrap (UFBoot) supports?
  url: how-do-i-interpret-ultrafast-bootstrap-ufboot-support-values
- name: How does IQ-TREE treat gap/missing/ambiguous characters?
  url: how-does-iq-tree-treat-gapmissingambiguous-characters
- name: Can I mix DNA and protein data in a partitioned analysis?
  url: can-i-mix-dna-and-protein-data-in-a-partitioned-analysis
- name: What is the interpretation of branch lengths when mixing codon and DNA data?
  url: what-is-the-interpretation-of-branch-lengths-when-mixing-codon-and-dna-data
- name: What is the purpose of composition test?
  url: what-is-the-purpose-of-composition-test
- name: What is the good number of CPU cores to use
  url: what-is-the-good-number-of-cpu-cores-to-use
- name: How do I save time for standard bootstrap?
  url: how-do-i-save-time-for-standard-bootstrap
- name: Why does IQ-TREE complain about the use of +ASC model?
  url: why-does-iq-tree-complain-about-the-use-of-asc-model
---

Frequently asked questions
==========================

For common questions and answers.
<!--more-->


How do I get help?
------------------
<div class="hline"></div>

If you have questions please follow the steps below:

1. Continue to read the FAQ below, which may answer your questions already.
2. If not,  read the documentation <http://www.iqtree.org/doc>.
3. If you still could not find the answer, search the [IQ-TREE Google group](https://groups.google.com/d/forum/iqtree). There is a "Search for topics" box at the top of the Google group web page.
4. Finally, if no answer is found, post a question to the IQ-TREE group. The average response time is one to two working days.


> For other feedback and feature request, please post a topic to the [IQ-TREE Google group](https://groups.google.com/d/forum/iqtree). We welcome all suggestions to further improve IQ-TREE! For feature request, please also explain why you think such a new feature would be useful or how can it help for your work.

How do I report bug?
--------------------
<div class="hline"></div>

For bug report, please send the following information to the [IQ-TREE Google group](https://groups.google.com/d/forum/iqtree):

1. A description of the behaviour, which you think might be unexpected or caused by a bug. 
2. The first 10 lines and last 10 lines of the `.log` file.
3. (If possible) the assertion message printed on the screen, which may look like this:

        iqtree-omp: ....cpp:140: ...: Assertion '...' failed.
 
The development team will get back to you and may ask for the full `.log` file and input data files for debugging purpose, if necessary. In such case please **only send your data files directly to the developers for confidential reason**! Keep in mind that everyone can see all emails sent to the group!



How do I interpret ultrafast bootstrap (UFBoot) support values?
---------------------------------------------------------------
<div class="hline"></div>

The ultrafast bootstrap (UFBoot) feature (`-bb` option) was published in  ([Minh et al., 2013]). One of the main conclusions is, that UFBoot support values are more unbiased: 95% support correspond roughly to a probability of 95% that a clade is true. So this has a different meaning than the normal bootstrap supports (where you start to believe in the clade if it has >80% BS support). For UFBoot, you should only start to believe in a clade if its support is >= 95%. Thus, the interpretations are different and you should not compare BS% with UFBoot% directly. 

Moreover, it is recommended to also perform the SH-aLRT test ([Guindon et al., 2010]) by adding `-alrt 1000` into the IQ-TREE command line. Each branch will then be assigned with SH-aLRT and UFBoot supports. One would typically start to rely on the clade if its SH-aLRT >= 80% and UFboot >= 95%. 


How does IQ-TREE treat gap/missing/ambiguous characters?
---------------------------------------------------------
<div class="hline"></div>

Gaps (`-`) and missing characters (`?` or `N` for DNA alignments) are treated in the same way as `unknown` characters, which represent no information. The same treatment holds for many other ML software (e.g., RAxML, PhyML). More explicitly,
for a site (column) of an alignment containing `AC-AG-A` (i.e. A for sequence 1, C for sequence 2, `-` for sequence 3, and so on), the site-likelihood
of a tree T is equal to the site-likelihood of the subtree of T restricted to those sequences containing non-gap characters (`ACAGA`).

Ambiguous characters that represent more than one character are also supported: each represented character will have equal likelihood. For DNA the following ambigous nucleotides are supported according to [IUPAC nomenclature](https://en.wikipedia.org/wiki/Nucleic_acid_notation):

| Nucleotide | Meaning |
|------------|---------------------------------------------------------------|
| R    | A or G (purine)  |
| Y    | C or T (pyrimidine) |
| W    | A or T (weak) |
| S    | G or C (strong) |
| M    | A or C (amino)|
| K    | G or T (keto)|
| B    | C, G or T (next letter after A) |
| H    | A, C or T (next letter after G) |
| D    | A, G or T (next letter after C) |
| V    | A, G or C (next letter after T) |
| ?, -, ., ~, O, N, X | A, G, C or T (unknown; all 4 nucleotides are equally likely) |

For protein the following ambiguous amino-acids are supported:

| Amino-acid | Meaning |
|------------|---------------------------------------------------------------|
| B          | N or D |
| Z          | Q or E |
| J          | I or L |
| U          | unknown AA (although it is the 21st AA) |
| ?, -, ., ~, * or X | unknown AA (all 20 AAs are equally likely) |


Can I mix DNA and protein data in a partitioned analysis?
---------------------------------------------------------
<div class="hline"></div>

Yes! You can specify this via a NEXUS partition file. In fact, you can mix any data types supported in IQ-TREE, including also codon, binary and morphological data. To do so, each data type should be stored in a separate alignment file (see also [Partitioned analysis with mixed data](Advanced-Tutorial#partitioned-analysis-with-mixed-data)). As an example, assuming `dna.phy` is a DNA alignment and and `prot.phy` is a protein alignment. Then a partition file mixing two types of data can be specified as follows:

    #nexus
    begin sets;
        charset part1 = dna.phy: 1-100 201-300;
        charset part2 = dna.phy: 101-200;
        charset part3 = prot.phy: 1-150;
        charset part4 = prot.phy: 151-400;
        charpartition mine = HKY:part1, GTR+G:part2, WAG+I+G:part3, LG+G:part4;
    end;
  
>**NOTE**: The site count for each alignment should start from 1, and **not** continue from the last position of a previous alignment (e.g., see `part3` and `part4` declared above).

What is the interpretation of branch lengths when mixing codon and DNA data?
----------------------------------------------------------------------------
<div class="hline"></div>

When mixing codon and DNA data in a partitioned analysis, the branch lengths are interpreted as the number of nucleotide substitutions per nucleotide site! This is different from having only codon data, where branch lengths are the number of nucleotide substitutions per codon site (thus typically 3 times longer than under DNA models).

Note that if you mix codon, DNA and protein data, the branch lengths are then the number of character substitutions per site, where character is either nucleotide or amino-acid.


What is the purpose of composition test?
--------------------------------------------
<div class="hline"></div>

At the beginning of each run, IQ-TREE performs a composition chi-square test for every sequence in the alignment.  The purpose is to test for homogeneity of character composition (e.g., nucleotide for DNA, amino-acid for protein sequences). A sequence is denoted `failed` if its character composition significantly deviates from the average composition of the alignment.    

More specifically, for each sequence, compute: 

    chi2 = \sum_{i=1}^k (O_i - E_i)^2 / E_i

where k is the size of the alphabet (e.g. 4 for DNA, 20 for amino acids) and the values 1 to k correspond uniquely to one of the characters. 
O_i is the character frequency in the sequence tested. 
E_i is the overall character frequency from the entire alignment.

Whether the character composition deviates significantly from the overall composition is done by testing the chi2 value using the chi2-distribution with k-1 degrees of freedom (df=3 for DNA or df=19 for amino acids). By and large it is a normal Chi^2 test. 

This test should be regarded as an *explorative tool* which might help to nail down problems in a dataset. One would typically not remove failing sequences by default. But if the tree shows unexpected topology the test might point in direction of the origin of the problem. 

Furthermore, please keep in mind, this test is performed at the very beginning, where IQ-TREE does not know anything about the models yet. That means:

* If you have partitioned (multi-gene) data, it might be more reasonable to test this separately for each partition in a partition analysis. Here, one might want to be able to decide whether some partitions should better be discarded if it is hard to find a composition representing the sequences in the partition. Or on the other hand if a sequence fails for many partitions and show very unexpected phylogenetic topologies, try without it.
* If you have (phylogenomic) protein data, you can also try several [protein mixture models](Substitution-Models#protein-mixture-models), which account for different amino-acid compositions along the sequences, for example, the `C10` to `C60` profile mixture models.
* Finally, it is recommended to always check the alignment (something one should always do anyway), especially if they have been collected and produced automatically.


What is the good number of CPU cores to use?
--------------------------------------------
<div class="hline"></div>

Starting with version 1.5.1, you can use option `-nt AUTO` to automatically determine the best number of threads for your current data and computer.

If you want to know more details: IQ-TREE can utilize multicore machines to speed up the analysis via `-nt` option. However, it does not mean that using more cores will always result in less running time: if your alignment is short, using too many cores may even slow down the analysis. This is because IQ-TREE parallelizes the likelihood computation along the alignment. Thus, the parallel efficiency is only increased with longer alignments. 

How do I save time for standard bootstrap?
--------------------------------------------
<div class="hline"></div>

The standard bootstrap is rather slow and may take weeks/months for large data sets. One way to speed up is to use the multicore version. However, this only works well for long alignments (see [What is the good number of CPU cores to use?](#what-is-the-good-number-of-cpu-cores-to-use)). Another way is to use many machines or a computing cluster and split the computation among the machines. To illustrate, you want to perform 100 bootstrap replicates and have 5 PCs, each has 4 CPU cores. Then you can:

1. Perform 5 independent bootstrap runs (each with 20 replicates) on the 5 machines with 5 prefix outputs (such that output files are not overwritten). For example: 

        iqtree-omp -nt 4 -s input_alignment -bo 20 ... -pre boot1
        iqtree-omp -nt 4 -s input_alignment -bo 20 ... -pre boot2
        iqtree-omp -nt 4 -s input_alignment -bo 20 ... -pre boot3
        iqtree-omp -nt 4 -s input_alignment -bo 20 ... -pre boot4
        iqtree-omp -nt 4 -s input_alignment -bo 20 ... -pre boot5

    Note that if you have access to a computing cluster, you may want to submit these jobs onto the cluster queue in parallel and with even more fined grained parallelization (e.g. one replicate per job).
        
2. Once all 5 runs finished, combine the 5 `.boottrees` file into one file (e.g. by `cat` command under Linux):

        cat boot*.boottrees > alltrees
     
3. Construct a consensus tree from the combined bootstrap trees:

        iqtree -con -t alltrees
        
    The consensus tree is then written to `.contree` file.
     
4. You can also perform the analysis on the original alignment:

        iqtree-omp -nt 4 -s input_alignment ...

    and map the support values onto the obtained ML tree:

        iqtree -sup input_alignment.treefile -t alltrees 

    The ML tree with assigned bootstrap supports is written to `.suptree` file.

Why does IQ-TREE complain about the use of +ASC model?
--------------------------------------------------------
<div class="hline"></div>

When using ascertainment bias correction (ASC) model, sometimes you may get an error message:

`ERROR: Invaid use of +ASC because of ... invariant sites in the alignment`

or when performing model testing:

`Skipped since +ASC is not applicable`

This is because your alignment contains _invariant_ sites (columns), which violate the mathematical condition of the model. The invariant sites can be:

* Constant sites: containing a single character state over all sequences. For example, all sequences show an `A` (Adenine) at a particular site in a DNA alignment. 
* Partially constant sites: containing a single character, gap or unknown character. For example, at a particular site some sequences show a `G` (Guanine), some sequences have `-` (gap) and the other have `N`.
* Ambiguously constant sites: For example, some sequences show a `C` (Cytosine), some show a `Y` (meaning `C` or `T`) and some show a `-` (gap). 

All these sites must be removed from the alignment before a +ASC model can be applied.

>**TIP**: Starting with IQ-TREE version 1.5.0, an output alignment file with suffix `.varsites` is written in such cases, which contain only variable sites from the input alignment. The `.varsites` alignment can then be used with the +ASC model.
{: .tip}


[Guindon et al., 2010]: http://dx.doi.org/10.1093/sysbio/syq010
[Minh et al., 2013]: http://dx.doi.org/10.1093/molbev/mst024
