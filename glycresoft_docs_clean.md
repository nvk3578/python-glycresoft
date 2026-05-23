# GlycReSoft Documentation (Cleaned)

## Mục lục

- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [404](#404)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)
- [glycresoft  documentation](#glycresoft--documentation)

---

# glycresoft  documentation

«  
Database Building Tools

   ::  

Contents

   ::  

Building a Glycan Hypothesis from a Text File
  »

Table Of Contents

Building a Combinatorial Glycan Hypothesis

glycresoft build-hypothesis glycan-combinatorial

Example Usage

Building a Combinatorial Glycan Hypothesis
¶

When you do not have a complete list of glycan compositions you
know are reasonable and present, a first pass can be done with a
combinatorial glycan composition hypothesis.

A combinatorial glycan hypothesis is defined by a set of 
component

ranges
 and a set of 
algebraic

constraints
.

A 
component

range
 is defined by a 
component
 residue such as a
monossaccharide (or any entity which can be encoded with 
IUPAClite Monosaccharide and Glycan Notation
)
with a 
minimum
 count and a 
maximum
 count. Candidate glycan compositions will
contain the 
component
 with every possible value in the range between 
minimum

and 
maximum

An 
algebraic

constraint
 is defined by two expressions containing numbers
binary arithmetic operators and variables corresponding to 
components
 and
a comparator operator relating the two expressions. No candidate glycan composition
will be included in the hypothesis will fail to satisfy all applied constraints.

HexNAc > (NeuAc + NeuGc + 1)

This constraint requires that the total number of sialylic acids always be one less
than the number of HexNAc, which is common for canonical N-glycans in humans.

To pass these instructions to the build tool, they must be written in a text file
following the format

; comments are allowed on lines starting with a semi-colon
<component> <minimum> <maximum>
<component> <minimum> <maximum>
...
; a blank line separates the component range rules from the constraints

<expression> <operator> <expression>
<expression> <operator> <expression>
...

Warning

Due to the average size of a combinatorial search space and limitations of composition
motifs, glycan compositions produced by this program are only checked for being N-glycans,
and as such you cannot tag compositions O-glycans directly with this tool at this time. This
restriction is wholly artificial and may be removed in the future. For more information
on glycan composition classifiers, please see 
Glycan Composition Classes
.

glycresoft build-hypothesis glycan-combinatorial
¶

glycresoft

build-hypothesis

glycan-combinatorial

[
OPTIONS
]

RULE_FILE

DATABASE_CONNECTION

Options

-r
,

--reduction

<string>
¶

Reducing end modification

-d
,

--derivatization

<string>
¶

Chemical derivatization to apply

-n
,

--name

<string>
¶

The name for the hypothesis to be created

-c
,

--glycan-type

<choice>
¶

The glycan classification to apply to all generated glycan compositions. If not set, a heuristic will be used to guess if a composition in an N-glycan. This is only relevant when building a glycopeptide hypothesis.

Choices: [

n-glycan; o-glycan; gag-linker]

Arguments

RULE_FILE
¶

Required argument
<path>

DATABASE_CONNECTION
¶

Required argument
<string> A connection URI for a database, or a path on the file system

For more information on reductions and derivatizations, please see 
Glycan Modifications

Example Usage
¶

$

cat

rules-file.txt
Hex

3

10

HexNAc

2

9

Fuc

0

5

Neu5Ac

0

4

Fuc

<

HexNAc
HexNAc

>

NeuAc

+

1

$

glycresoft

build-hypothesis

glycan-combinatorial

rules-file.txt

combinatorial-database

-n

"Combinatorial Human N-Glycans"

Building

Glycan

Hypothesis

Combinatorial

Human

N-Glycans

13
:12:06

-

glycresoft:log:175

-

INFO

-

Begin

Combinatorial

Glycan

Hypothesis

Serializer

{
'derivatization'
:

None,

'engine'
:

Engine
(
sqlite:///combinatorial-database
)
,

'glycan_file'
:

u
'rules-file.txt'
,

'loader'
:

None,

'reduction'
:

None,

'start_time'
:

datetime.datetime
(
2017
,

8
,

31
,

13
,

12
,

6
,

105000
)
,

'status'
:

'started'
,

'transformer'
:

None,

'uuid'
:

'459bffe3d9fd42019eb202c7afb3f72f'
}

13
:12:06

-

glycresoft:log:175

-

INFO

-

Generating

Glycan

Compositions

from

Symbolic

Rules

for

GlycanHypothesis
(
id
=
1
,

name
=
Combinatorial

Human

N-Glycans
)

13
:12:08

-

glycresoft:log:175

-

INFO

-

1000

glycan

compositions

created

13
:12:09

-

glycresoft:log:175

-

INFO

-

Generated

1280

glycan

compositions

13
:12:09

-

glycresoft:log:175

-

INFO

-

Hypothesis

Completed

13
:12:09

-

glycresoft:log:175

-

INFO

-

End

Combinatorial

Glycan

Hypothesis

Serializer

13
:12:09

-

glycresoft:log:175

-

INFO

-

Started

at

2017
-08-31

13
:12:06.105000.
Ended

at

2017
-08-31

13
:12:09.047000.
Total

time

elapsed:

0
:00:02.942000
CombinatorialGlycanHypothesisSerializer

completed

successfully.

 «  
Database Building Tools

   ::  

Contents

   ::  

Building a Glycan Hypothesis from a Text File
  »


# glycresoft  documentation

«  
Export Glycopeptide Search Results

   ::  

Contents

   ::  

Export Hypotheses
  »

Table Of Contents

Export Glycan Search Results

glycresoft export glycan-identification

Export Glycan Search Results
¶

glycresoft
 stores its search results
in a SQLite3 database like the one it writes hypotheses
into. This lets it search for results quickly, but the
format is not as easy to read through as a text file, and
adds extra layers of complexity to find information requiring
complex SQL queries. It’s possible to export search results in
several other formats which can be read by other tools.

glycresoft export glycan-identification
¶

Write each glycan chromatogram in CSV format

glycresoft

export

glycan-identification

[
OPTIONS
]

DATABASE_CONNECTION

ANALYSIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

-r
,

--report
¶

Export an HTML report instead of a CSV [default: False]

-t
,

--threshold

<float>
¶

Minimum score threshold to include [default: 0]

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

ANALYSIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycan analysis to use

 «  
Export Glycopeptide Search Results

   ::  

Contents

   ::  

Export Hypotheses
  »


# glycresoft  documentation

«  
Output Files and Exporting

   ::  

Contents

   ::  

Export Glycan Search Results
  »

Table Of Contents

Export Glycopeptide Search Results

glycresoft export glycopeptide-identification

glycresoft export glycopeptide-spectrum-matches

glycresoft export glycopeptide-training-mgf

glycresoft export identified-glycans-from-glycopeptides

glycresoft export annotate-matched-spectra

glycresoft export write-csv-spectrum-library

Export Glycopeptide Search Results
¶

glycresoft
 stores its search results
in a SQLite3 database like the one it writes hypotheses
into. This lets it search for results quickly, but the
format is not as easy to read through as a text file, and
adds extra layers of complexity to find information requiring
complex SQL queries. It’s possible to export search results in
several other formats which can be read by other tools.

glycresoft export glycopeptide-identification
¶

Write each distinct identified glycopeptide in CSV format

glycresoft

export

glycopeptide-identification

[
OPTIONS
]

DATABASE_CONNECTION

ANALYSIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

-r
,

--report
¶

Export an HTML report instead of a CSV [default: False]

-m
,

--mzml-path

<path>
¶

Path to read processed spectra from instead of the path embedded in the analysis metadata

-t
,

--threshold

<float>
¶

The minimum MS2 score threshold (larger is better) [default: 0]

-f
,

--fdr-threshold

<float>
¶

The maximum FDR threshold (smaller is better) [default: 1]

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

ANALYSIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycopeptide analysis to use

glycresoft export glycopeptide-spectrum-matches
¶

Write each matched glycopeptide spectrum in CSV format

glycresoft

export

glycopeptide-spectrum-matches

[
OPTIONS
]

DATABASE_CONNECTION

ANALYSIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

-t
,

--threshold

<float>
¶

The minimum MS2 score threshold (larger is better) [default: 0]

-f
,

--fdr-threshold

<float>
¶

The maximum FDR threshold (smaller is better) [default: 1]

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

ANALYSIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycopeptide analysis to use

glycresoft export glycopeptide-training-mgf
¶

Export identified glycopeptide spectrum matches for model training as
an annotated MGF file.

glycresoft

export

glycopeptide-training-mgf

[
OPTIONS
]

DATABASE_CONNECTION

ANALYSIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

-m
,

--mzml-path

<path>
¶

Alternative path to find the source mzML file

-t
,

--threshold

<float>
¶

Minimum MS2 score to export

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

ANALYSIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycopeptide analysis to use

glycresoft export identified-glycans-from-glycopeptides
¶

Export the glycans attached to any identified glycopeptides.

The files are written in an importable format to be used to create a new glycan hypothesis.

glycresoft

export

identified-glycans-from-glycopeptides

[
OPTIONS
]

DATABASE_CONNECTION

ANALYSIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

ANALYSIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycopeptide analysis to use

glycresoft export annotate-matched-spectra
¶

Export graphical renderings of individual MS/MS assignments of glycopeptides to PDF.

The output file will contain one spectrum per PDF page.

glycresoft

export

annotate-matched-spectra

[
OPTIONS
]

DATABASE_CONNECTION

ANALYSIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

-m
,

--mzml-path

<path>
¶

Alternative path to find the source mzML file

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

ANALYSIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycopeptide analysis to use

glycresoft export write-csv-spectrum-library
¶

Export individual MS/MS assignments of glycopeptides to CSV.

Each row corresponds to a single product ion match, partitioned by
scan ID. Unmatched peaks are ignored.

glycresoft

export

write-csv-spectrum-library

[
OPTIONS
]

DATABASE_CONNECTION

ANALYSIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

-m
,

--mzml-path

<path>
¶

Alternative path to find the source mzML file

-t
,

--fdr-threshold

<float>
¶

[default: 0.05]

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

ANALYSIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycopeptide analysis to use

 «  
Output Files and Exporting

   ::  

Contents

   ::  

Export Glycan Search Results
  »


# glycresoft  documentation

«  
Export Glycan Search Results

   ::  

Contents

   ::  

Tutorials
  »

Table Of Contents

Export Hypotheses

glycresoft export glycan-hypothesis

glycresoft export glycopeptide-hypothesis

Export Hypotheses
¶

glycresoft
 stores its search spaces
in a SQLite3 database like the one it writes hypotheses
into. This lets it search for entries quickly, but the
format is not as easy to read through as a text file, and
adds extra layers of complexity to find information requiring
complex SQL queries. It’s possible to export hypotheses in
several other formats.

glycresoft export glycan-hypothesis
¶

Write each theoretical glycan composition in CSV format

glycresoft

export

glycan-hypothesis

[
OPTIONS
]

DATABASE_CONNECTION

HYPOTHESIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

-i
,

--importable
¶

Make the file importable for later re-use, with less information. [default: False]

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

HYPOTHESIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycan hypothesis to use

glycresoft export glycopeptide-hypothesis
¶

Write each theoretical glycopeptide in CSV format

glycresoft

export

glycopeptide-hypothesis

[
OPTIONS
]

DATABASE_CONNECTION

HYPOTHESIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

HYPOTHESIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycopeptide hypothesis to use

 «  
Export Glycan Search Results

   ::  

Contents

   ::  

Tutorials
  »


# glycresoft  documentation

«  
Glycome Network Definitions

   ::  

Contents

   ::  

Search and Analysis Tools
  »

Table Of Contents

Building a Glycopeptide Hypothesis from a FASTA File

glycresoft build-hypothesis glycopeptide-fa

Usage Example

Supported Proteases

Supported Modification Rules

UniProt Integration

Building a Glycopeptide Hypothesis from a FASTA File
¶

The simplest way to build a glycopeptide database is to start with a
list of theoretical glycoproteins in a FASTA file and perform a simple

in-silico
 digest with one or more proteases, apply a set of 
modification

rules
,
and combine the resulting peptides with a glycan hypothesis to produce
glycopeptides.

Warning

Glycan compositions read from the associated glycan hypothesis must be
classified as N-glycans, O-glycans, or GAG-Linkers in order for the algorithm
to select the appropriate glycosite assignment algorithm. Glycan compositions
lacking a classification 
will not
 be considered.

The build process is computationally intensive, as such this tool will spawn
several worker processes to share the load.

glycresoft build-hypothesis glycopeptide-fa
¶

Constructs a glycopeptide hypothesis from a FASTA file of proteins and a
collection of glycans.

glycresoft

build-hypothesis

glycopeptide-fa

[
OPTIONS
]

FASTA_FILE

DATABASE_CONNECTION

Options

-z
,

--peptide-length-range

<tuple>
¶

The minimum and maximum peptide length to consider [default: 5, 60]

-g
,

--glycan-source

<string>
¶

The path, identity, or other specifier for the glycan source [required]

-s
,

--glycan-source-type

<choice>
¶

The type of glycan information source to use [default: text]

Choices: [

combinatorial; text; hypothesis;

analysis]

-G
,

--glycan-source-identifier

<string>
¶

The name or id number of the hypothesis or analysis to be used when using those glycan source types.

-p
,

--processes

<int>
¶

Number of worker processes to use. Defaults to 4 or the number of CPUs, whichever is lower [default: 4]

-n
,

--name

<string>
¶

The name for the hypothesis to be created

-u
,

--occupied-glycosites

<int>
¶

The number of occupied glycosylation sites permitted. [default: 1]

-e
,

--enzyme

<string>
¶

The proteolytic enzyme to use during digestion. May be specified multiple times, generating a co-digestion. May specify an enzyme name or a regular expression describing the cleavage pattern. Recognized enzyme names are: 2-iodobenzoate, alpha-lytic protease, arg-c, asp-n, asp-n_ambic, bnps-skatole, caspase 1, caspase 10, caspase 2, caspase 3, caspase 4, caspase 5, caspase 6, caspase 7, caspase 8, caspase 9, chymotrypsin, chymotrypsin high specificity, chymotrypsin low specificity, clostripain, cnbr, enterokinase, factor xa, formic acid, glu-c, glutamyl endopeptidase, granzyme b, hydroxylamine, iodosobenzoic acid, leukocyte elastase, lys-c, lys-c/p, none, ntcb, pepsin ph1.3, pepsin ph2.0, pepsina, proline endopeptidase, proteinase k, staphylococcal peptidase i, thermolysin, thrombin, trypchymo, trypsin, trypsin/p, v8-de, v8-e [default: trypsin] (May specify more than once)

-m
,

--missed-cleavages

<int>
¶

The number of missed proteolytic cleavage sites permitted [default: 1]

-c
,

--constant-modification

<string>
¶

Peptide modification rule which will be applied constantly (May specify more than once)

-v
,

--variable-modification

<string>
¶

Peptide modification rule which will be applied variablely (May specify more than once)

-V
,

--max-variable-modifications

<int>
¶

The maximum number of variable modifications that can be applied to a single peptide [default: 4]

-y
,

--semispecific-digest
¶

Apply a semispecific enzyme digest permitting one peptide terminal to be non-specific [default: False]

-R
,

--reverse
¶

Reverse protein sequences [default: False]

-F
,

--not-full-crossproduct
¶

Do not produce full crossproduct. For when the search space is too large to enumerate, store, and load. [default: False]

--retain-all-peptides
¶

Do not require a glycosylation site when saving base peptides [default: False]

-C
,

--include-n-x-c-glycosylation
¶

Whether to support N-x-C in the N-glycosylation sequon [default: False]

-a
,

--annotation-path

<path>
¶

The path to an annotation XML file from UniProt, or ‘-’ to suppress annotations.

Arguments

FASTA_FILE
¶

Required argument
<path> A file containing protein sequences in FASTA format

DATABASE_CONNECTION
¶

Required argument
<string> A connection URI for a database, or a path on the file system

Usage Example
¶

Below is a basic example of how to use this tool

# We'll build a simple glycopeptide hypothesis from just two glycoproteins, the

# isoforms of Alpha-1-acid glycoprotein.

$

cat

agp.fa
>sp
|
P02763
|
A1AG1_HUMAN

Alpha-1-acid

glycoprotein

1

OS
=
Homo

sapiens

GN
=
ORM1

PE
=
1

SV
=
1

MALSWVLTVLSLLPLLEAQIPLCANLVPVPITNATLDQITGKWFYIASAFRNEEYNKSVQ
EIQATFFYFTPNKTEDTIFLREYQTRQDQCIYNTTYLNVQRENGTISRYVGGQEHFAHLL
ILRDTKTYMLAFDVNDEKNWGLSVYADKPETTKEQLGEFYEALDCLRIPKSDVVYTDWKK
DKCEPLEKQHEKERKQEEGES

>sp
|
P19652
|
A1AG2_HUMAN

Alpha-1-acid

glycoprotein

2

OS
=
Homo

sapiens

GN
=
ORM2

PE
=
1

SV
=
2

MALSWVLTVLSLLPLLEAQIPLCANLVPVPITNATLDRITGKWFYIASAFRNEEYNKSVQ
EIQATFFYFTPNKTEDTIFLREYQTRQNQCFYNSSYLNVQRENGTVSRYEGGREHVAHLL
FLRDTKTLMFGSYLDDEKNWGLSFYADKPETTKEQLGEFYEALDCLCIPRSDVMYTDWKK
DKCEPLEKQHEKERKQEEGES

# This hypothesis will include a combinatorial glycan composition hypothesis

# defined by these rules

$

cat

combinatorial-rules.txt
Hex

3

12

HexNAc

2

10

Fuc

0

5

Neu5Ac

0

4

Fuc

<

HexNAc
HexNAc

>

NeuAc

+

1

# We'll permit the following additional options:

# -u 1 means that only one glycosylation site will ever be occupied

# -e trypsin means that the proteins will be *in-silico* digested with trypsin

# -m 1 means that we will only permit one missed cleavage by the protease

# -c "Carbamidomethyl (C)" means we will have a constant modification "Carbamidomethyl"

# on all Cysteine residues

# -v "Deamidation (N)" means we will have a variable modification "Deamidation" on any

# Asparagine residue

# -v "Pyro-glu from Q (Q@N-term)" means that we will have a variable modification "Pyro-glu from Q"

# on any Glutamine residue on the N-terminus of the peptide sequence

# -p 4 means that this task will spawn four worker processes to share the work between

$

glycresoft

build-hypothesis

glycopeptide-fa

-g

combinatorial-rules.txt

-s

combinatorial
\

-u

1

-e

trypsin

-m

1

-c

"Carbamidomethyl (C)"

-v

"Deamidation (N)"
\

-v

"Pyro-glu from Q (Q@N-term)"

-p

4

-n

"Alpha-1-acid Glycopeptide Hypothesis"
\

agp.fa

fasta-glycopeptides.db

2017
-08-31

02
:50:08.610441

Begin

Combinatorial

Glycan

Hypothesis

Serializer

{
'derivatization'
:

None,

'engine'
:

Engine
(
sqlite:///fasta-glycopeptides.db
)
,

'glycan_file'
:

'combinatorial-rules.txt'
,

'loader'
:

None,

'reduction'
:

None,

'start_time'
:

datetime.datetime
(
2017
,

8
,

31
,

2
,

50
,

8
,

608881
)
,

'status'
:

'started'
,

'transformer'
:

None,

'uuid'
:

'93604f0929794e2bab9df23e29118b09'
}

2017
-08-31

02
:50:08.628096

Generating

Glycan

Compositions

from

Symbolic

Rules

for

GlycanHypothesis
(
id
=
1
,

name
=
GlycanHypothesis-93604f0929794e2bab9df23e29118b09
)

2017
-08-31

02
:50:15.188556

1000

glycan

compositions

created

2017
-08-31

02
:50:20.824145

Generated

1900

glycan

compositions

2017
-08-31

02
:50:20.835196

Hypothesis

Completed

2017
-08-31

02
:50:20.835577

End

Combinatorial

Glycan

Hypothesis

Serializer

2017
-08-31

02
:50:20.835661

Started

at

2017
-08-31

02
:50:08.608881.
Ended

at

2017
-08-31

02
:50:20.835265.
Total

time

elapsed:

0
:00:12.226384
CombinatorialGlycanHypothesisSerializer

completed

successfully.

2017
-08-31

02
:50:20.903601

Begin

Multiple

Process

Fasta

Glycopeptide

Hypothesis

Serializer

{
'constant_modifications'
:

[
Carbamidomethyl:57.021464
]
,

'engine'
:

Engine
(
sqlite:///fasta-glycopeptides.db
)
,

'fasta_file'
:

'agp.fa'
,

'max_glycosylation_events'
:

1
,

'max_missed_cleavages'
:

1
,

'n_processes'
:

4
,

'protease'
:

'trypsin'
,

'start_time'
:

datetime.datetime
(
2017
,

8
,

31
,

2
,

50
,

20
,

899793
)
,

'status'
:

'started'
,

'total_glycan_combination_count'
:

-1,

'uuid'
:

'1e6d202801ce4417b3b153ac84732401'
,

'variable_modifications'
:

[
Deamidated:0.984016,

Gln->pyro-Glu:-17.026549
]}

2017
-08-31

02
:50:20.903709

Extracting

Proteins

2017
-08-31

02
:50:20.927056

Digesting

Proteins

2017
-08-31

02
:50:28.965977

205

Base

Peptides

Produced

2017
-08-31

02
:50:28.966163

Begin

Applying

Protein

Annotations

2017
-08-31

02
:50:29.726805

...

Extracting

Best

Peptides

2017
-08-31

02
:50:29.753947

...

Building

Mask

2017
-08-31

02
:50:29.755910

...

Removing

Duplicates

2017
-08-31

02
:50:29.765368

...

Complete

2017
-08-31

02
:50:29.768705

Combinating

Glycans

2017
-08-31

02
:50:30.766838

...

Building

combinations

for

Hypothesis

1

2017
-08-31

02
:50:33.867166

1900

Glycan

Combinations

Constructed.

2017
-08-31

02
:50:33.867282

Building

Glycopeptides

2017
-08-31

02
:50:33.895503

Begin

Creation.

Dropping

Indices

2017
-08-31

02
:50:33.939597

...

Processing

Glycan

Combinations

0
-1900

(
100
.00%
)

2017
-08-31

02
:50:34.021121

...

Dealt

Peptides

0
-11

4
.60%

2017
-08-31

02
:50:34.021361

...

Dealt

Peptides

11
-22

9
.21%

2017
-08-31

02
:50:34.021429

...

Dealt

Peptides

22
-33

13
.81%

2017
-08-31

02
:50:34.021495

...

Dealt

Peptides

33
-44

18
.41%

2017
-08-31

02
:50:34.021560

...

Dealt

Peptides

44
-55

23
.01%

2017
-08-31

02
:50:34.021624

...

Dealt

Peptides

55
-66

27
.62%

2017
-08-31

02
:50:34.021781

...

Dealt

Peptides

66
-77

32
.22%

2017
-08-31

02
:50:34.021890

...

Dealt

Peptides

77
-88

36
.82%

2017
-08-31

02
:50:34.022035

...

Dealt

Peptides

88
-99

41
.42%

2017
-08-31

02
:50:34.022238

...

Dealt

Peptides

99
-110

46
.03%

2017
-08-31

02
:50:34.340675

...

Dealt

Peptides

110
-121

50
.63%

2017
-08-31

02
:50:34.367891

...

Dealt

Peptides

121
-132

55
.23%

2017
-08-31

02
:50:34.376397

...

Dealt

Peptides

132
-143

59
.83%

2017
-08-31

02
:50:34.410928

...

Dealt

Peptides

143
-154

64
.44%

2017
-08-31

02
:50:46.488753

...

Dealt

Peptides

154
-165

69
.04%

2017
-08-31

02
:50:48.904966

...

Dealt

Peptides

165
-176

73
.64%

2017
-08-31

02
:50:52.744931

...

Dealt

Peptides

176
-187

78
.24%

2017
-08-31

02
:50:52.765699

...

Dealt

Peptides

187
-198

82
.85%

2017
-08-31

02
:50:52.806290

...

Dealt

Peptides

198
-209

87
.45%

2017
-08-31

02
:50:59.408814

...

Dealt

Peptides

209
-220

92
.05%

2017
-08-31

02
:51:08.196088

...

Dealt

Peptides

220
-231

96
.65%

2017
-08-31

02
:51:11.241885

...

Dealt

Peptides

231
-239

100
.00%

2017
-08-31

02
:51:11.241991

...

All

Peptides

Dealt

2017
-08-31

02
:51:33.927773

...

130001

Glycopeptides

Created

2017
-08-31

02
:52:10.984681

Process

3560

completed.

(
41

peptides,

57000

glycopeptides
)

2017
-08-31

02
:52:11.928619

Process

3556

completed.

(
66

peptides,

55100

glycopeptides
)

2017
-08-31

02
:52:12.924645

Process

3558

completed.

(
66

peptides,

57000

glycopeptides
)

2017
-08-31

02
:52:17.504664

Process

3562

completed.

(
66

peptides,

62700

glycopeptides
)

2017
-08-31

02
:52:19.507377

Joining

Process

3556

(
False
)

2017
-08-31

02
:52:19.507669

Joining

Process

3558

(
False
)

2017
-08-31

02
:52:19.507839

Joining

Process

3560

(
False
)

2017
-08-31

02
:52:19.507991

Joining

Process

3562

(
False
)

2017
-08-31

02
:52:19.508170

All

Work

Done.

Rebuilding

Indices

2017
-08-31

02
:52:21.342381

Analyzing

Indices

2017
-08-31

02
:52:21.605914

Done

Analyzing

Indices

2017
-08-31

02
:52:21.645477

Generated

231800

glycopeptides

2017
-08-31

02
:52:21.645579

Done

2017
-08-31

02
:52:21.659734

Hypothesis

Completed

2017
-08-31

02
:52:21.660019

End

Multiple

Process

Fasta

Glycopeptide

Hypothesis

Serializer

2017
-08-31

02
:52:21.660143

Started

at

2017
-08-31

02
:50:20.899793.
Ended

at

2017
-08-31

02
:52:21.659825.
Total

time

elapsed:

0
:02:00.760032
MultipleProcessFastaGlycopeptideHypothesisSerializer

completed

successfully.

If you instead wish to use an existing glycan hypothesis instead
of creating a new one, you can modify the instructions above:

$

glycresoft

build-hypothesis

glycan-combinatorial

rules-file.txt

combinatorial-database

-n

"Combinatorial Human N-Glycans"

...

$

glycresoft

build-hypothesis

glycopeptide-fa

-g

combinatorial-database.db

-s

hypothesis

-G

1
\

-u

1

-e

trypsin

-m

1

-c

"Carbamidomethyl (C)"

-v

"Deamidation (N)"
\

-v

"Pyro-glu from Q (Q@N-term)"

-p

4

-n

"Alpha-1-acid Glycopeptide Hypothesis"
\

agp.fa

fasta-glycopeptides.db
...

The primary difference here is that the value of the 
-g
 option is the path to the source
database’s path (or connection URI), 
-s
 indicates that the source is a hypothesis, and
the new 
-G
 option identifies the hypothesis to use, as a single database may contain many
hypotheses. The value of 
-G
 can be the ID of the hypothesis or the hypothesis’s name (in this case
“Combinatorial Human N-Glycans”).

Supported Proteases
¶

Enzyme Name

Recognized Pattern

bnps-skatole

W

caspase 1

(?<=[FWYL]w[HAT])D(?=[^PEDQKR])

caspase 10

(?<=IEA)D

caspase 2

(?<=DVA)D(?=[^PEDQKR])

caspase 3

(?<=DMQ)D(?=[^PEDQKR])

caspase 4

(?<=LEV)D(?=[^PEDQKR])

caspase 5

(?<=[LW]EH)D

caspase 6

(?<=VE[HI])D(?=[^PEDQKR])

caspase 7

(?<=DEV)D(?=[^PEDQKR])

caspase 8

(?<=[IL]ET)D(?=[^PEDQKR])

caspase 9

(?<=LEH)D

chymotrypsin

(?<=[FYWL])(?!P)

chymotrypsin high specificity

([FY](?=[^P]))|(W(?=[^MP]))

chymotrypsin low specificity

([FLY](?=[^P]))|(W(?=[^MP]))|(M(?=[^PY]))|(H(?=[^DMPW]))

clostripain

R

cnbr

M

enterokinase

(?<=[DE]{3})K

factor xa

(?<=[AFGILTVM][DE]G)R

formic acid

D

glutamyl endopeptidase

E

glu-c

E

granzyme b

(?<=IEP)D

hydroxylamine

N(?=G)

iodosobenzoic acid

W

lys-c

(?<=K)(?!P)

ntcb

w(?=C)

pepsin ph1.3

((?<=[^HKR][^P])[^R](?=[FLWY][^P]))|((?<=[^HKR][^P])[FLWY](?=w[^P]))

pepsin ph2.0

((?<=[^HKR][^P])[^R](?=[FL][^P]))|((?<=[^HKR][^P])[FL](?=w[^P]))

proline endopeptidase

(?<=[HKR])P(?=[^P])

proteinase k

[AEFILTVWY]

staphylococcal peptidase i

(?<=[^E])E

thermolysin

[^DE](?=[AFILMV])

thrombin

((?<=G)R(?=G))|((?<=[AFGILTVM][AFGILTVWA]P)R(?=[^DE][^DE]))

trypsin

([KR](?=[^P]))|((?<=W)K(?=P))|((?<=M)R(?=P))

none

^&$

trypchymo

(?<=[FYWLKR])(?!P)

trypsin/p

(?<=[KR])

lys-c/p

(?<=K)

pepsina

(?<=[FL])

v8-de

(?<=[BDEZ])(?!P)

v8-e

(?<=[EZ])(?!P)

2-iodobenzoate

(?<=W)

arg-c

(?<=R)(?!P)

asp-n_ambic

(?=[DE])

asp-n

(?=[BD])

leukocyte elastase

(?<=[ALIV])(?!P)

alpha-lytic protease

[TASV]

Supported Modification Rules
¶

glycresoft
 supports the full range of 
UNIMOD

modification rules as well as some common alternative namings.

To be more specific, you are able to override modification targets when you
specify modification names by passing the permitted target rules enclosed in
parentheses following the modification name. For example “Deamidation (N)” will
only target Asparagine residues, unlike the plain “Deamidation” rule which will
target both Asparagine and Glutamine.

For more information about supported post-translational modifications, please
see 
Peptide Modifications
.

UniProt Integration
¶

When GlycResoft digests a protein, it parses the definition line looking for accession code, and tries to guess
if your definition contains a UniProt accession. If it thinks so, it will query UniProt’s web service or a local
annotation file if provided for additional information. Currently, only additional cleavage sites are used to predict
non-specific cleavage events at annotated locations. This is especially useful when a signal peptide cleavage site
is relatively close to a glycosylation site.

If you do not wish for annotations to be applied, pass 
-
 for 
--annotation-path
.

 «  
Glycome Network Definitions

   ::  

Contents

   ::  

Search and Analysis Tools
  »


# glycresoft  documentation

Contents

Index

Symbols

 | 
A

 | 
C

 | 
D

 | 
F

 | 
G

 | 
H

 | 
M

 | 
O

 | 
R

 | 
S

 | 
T

Symbols

 --adduct

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --analysis-name

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --analysis-path

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

 --annotation-path

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --apex-time-range

glycresoft-export-glycopeptide-chromatogram-records command line option

 --averagine

glycresoft-mzml-preprocess command line option

 --background-reduction

glycresoft-mzml-preprocess command line option

 --constant-modification

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --decoy-database-connection

glycresoft-analyze-search-glycopeptide command line option

 --decoy-hypothesis-identifier

glycresoft-analyze-search-glycopeptide-multipart command line option

 --default-precursor-ion-selection-window

glycresoft-mzml-preprocess command line option

 --delta-rt

glycresoft-analyze-search-glycan command line option

 --derivatization

glycresoft-build-hypothesis-glycan-combinatorial command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycan-text command line option

 --detatch-substituent

glycresoft-build-hypothesis-glycan-glyspace command line option

 --edge-strategy

glycresoft-build-hypothesis-glycan-network-build-network command line option

 --end-time

glycresoft-mzml-preprocess command line option

 --enzyme

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --export

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --expression-rule

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

 --extract-only-tandem-envelopes

glycresoft-mzml-preprocess command line option

 --fdr-estimation-strategy

glycresoft-analyze-search-glycopeptide-multipart command line option

 --fdr-threshold

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-export-glycopeptide-chromatogram-records command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-spectrum-matches command line option

glycresoft-export-write-csv-spectrum-library command line option

 --glycan-hypothesis

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

 --glycan-score-threshold

glycresoft-analyze-search-glycopeptide-multipart command line option

 --glycan-source

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --glycan-source-identifier

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --glycan-source-type

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --glycan-type

glycresoft-build-hypothesis-glycan-combinatorial command line option

 --glycopeptide-hypothesis

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

 --glycoproteome-smoothing-model

glycresoft-analyze-search-glycopeptide-multipart command line option

 --grouping-error-tolerance

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --ignore-msn

glycresoft-mzml-preprocess command line option

 --importable

glycresoft-export-glycan-hypothesis command line option

 --include-children

glycresoft-build-hypothesis-glycan-glyspace command line option

 --include-n-x-c-glycosylation

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --input-path

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

glycresoft-build-hypothesis-glycan-network-add-prebuilt-neighborhoods command line option

 --isotope-probing-range

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --isotopic-strictness

glycresoft-mzml-preprocess command line option

 --mass-error-tolerance

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --mass-offset

glycresoft-mzml-preprocess command line option

 --mass_shift

glycresoft-analyze-search-glycan command line option

 --mass_shift-combination-limit

glycresoft-analyze-search-glycan command line option

 --max-variable-modifications

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --maximum-charge

glycresoft-mzml-preprocess command line option

 --maximum-mass

glycresoft-analyze-search-glycopeptide command line option

 --memory-database-index

glycresoft-analyze-search-glycopeptide-multipart command line option

 --minimum-mass

glycresoft-analyze-search-glycan command line option

 --minimum-observations-for-specific-model

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

 --missed-cleavages

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --missed-peaks

glycresoft-mzml-preprocess command line option

 --motif-class

glycresoft-build-hypothesis-glycan-glyspace command line option

 --ms1-averaging

glycresoft-mzml-preprocess command line option

 --ms1-scoring-feature

glycresoft-analyze-search-glycan command line option

 --msn-averagine

glycresoft-mzml-preprocess command line option

 --msn-background-reduction

glycresoft-mzml-preprocess command line option

 --msn-isotopic-strictness

glycresoft-mzml-preprocess command line option

 --msn-mass-error-tolerance

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --msn-missed-peaks

glycresoft-mzml-preprocess command line option

 --msn-score-threshold

glycresoft-mzml-preprocess command line option

 --msn-transform

glycresoft-mzml-preprocess command line option

 --mzml-path

glycresoft-export-annotate-matched-spectra command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-training-mgf command line option

glycresoft-export-write-csv-spectrum-library command line option

 --name

glycresoft-build-hypothesis-glycan-combinatorial command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

glycresoft-build-hypothesis-glycan-network-add-prebuilt-neighborhoods command line option

glycresoft-build-hypothesis-glycan-text command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 --network-path

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-search-glycan command line option

 --no-require-multiple-observations

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

 --no-retention-time-modeling

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --not-full-crossproduct

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --occupied-glycosites

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --output-path

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

glycresoft-build-hypothesis-glycan-network-add-prebuilt-neighborhoods command line option

glycresoft-build-hypothesis-glycan-network-build-network command line option

glycresoft-export-annotate-matched-spectra command line option

glycresoft-export-glycan-hypothesis command line option

glycresoft-export-glycan-identification command line option

glycresoft-export-glycopeptide-chromatogram-records command line option

glycresoft-export-glycopeptide-hypothesis command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-spectrum-matches command line option

glycresoft-export-glycopeptide-training-mgf command line option

glycresoft-export-identified-glycans-from-glycopeptides command line option

glycresoft-export-write-csv-spectrum-library command line option

 --oxonium-threshold

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --peptide-length-range

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --peptide-masses-per-scan

glycresoft-analyze-search-glycopeptide-multipart command line option

 --permute-decoy-glycan-fragments

glycresoft-analyze-search-glycopeptide command line option

 --prefer-joint-model

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

 --processes

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 --psm-fdr-threshold

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --range-rule

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

 --rare-signatures

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --ratio-rule

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

 --reduction

glycresoft-build-hypothesis-glycan-combinatorial command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycan-text command line option

 --regularization-model-path

glycresoft-analyze-search-glycan command line option

 --regularize

glycresoft-analyze-search-glycan command line option

 --report

glycresoft-export-glycan-identification command line option

glycresoft-export-glycopeptide-identification command line option

 --require-msms-signature

glycresoft-analyze-search-glycan command line option

 --require-multiple-observations

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

 --retain-all-peptides

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --retention-time-modeling

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --reverse

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --save-intermediate-results

glycresoft-analyze-search-glycopeptide command line option

 --score-threshold

glycresoft-mzml-preprocess command line option

 --semispecific-digest

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --signal-to-noise-threshold

glycresoft-mzml-preprocess command line option

 --smoothing-limit

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

 --start-time

glycresoft-mzml-preprocess command line option

 --tandem-scoring-model

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 --target-hypothesis-identifier

glycresoft-analyze-search-glycopeptide-multipart command line option

 --target-taxon

glycresoft-build-hypothesis-glycan-glyspace command line option

 --test-chromatogram-csv-path

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

 --threshold

glycresoft-export-glycan-identification command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-spectrum-matches command line option

glycresoft-export-glycopeptide-training-mgf command line option

 --transform

glycresoft-mzml-preprocess command line option

 --unobserved-penalty-scale

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

 --use-peptide-mass-filter

glycresoft-analyze-search-glycopeptide command line option

 --use-retention-time-normalization

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

 --variable-modification

glycresoft-build-hypothesis-glycopeptide-fa command line option

 --verbose

glycresoft-mzml-preprocess command line option

 --workload-size

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 -a

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 -an

glycresoft-mzml-preprocess command line option

 -b

glycresoft-mzml-preprocess command line option

 -bn

glycresoft-mzml-preprocess command line option

 -C

glycresoft-build-hypothesis-glycopeptide-fa command line option

 -c

glycresoft-build-hypothesis-glycan-combinatorial command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 -D

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-mzml-preprocess command line option

 -d

glycresoft-analyze-search-glycan command line option

glycresoft-build-hypothesis-glycan-combinatorial command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycan-text command line option

 -e

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

glycresoft-build-hypothesis-glycan-network-build-network command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 -F

glycresoft-build-hypothesis-glycopeptide-fa command line option

 -f

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-spectrum-matches command line option

 -G

glycresoft-analyze-search-glycopeptide command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

 -g

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 -i

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

glycresoft-build-hypothesis-glycan-network-add-prebuilt-neighborhoods command line option

glycresoft-export-glycan-hypothesis command line option

glycresoft-mzml-preprocess command line option

 -in

glycresoft-mzml-preprocess command line option

 -k

glycresoft-analyze-search-glycan command line option

 -M

glycresoft-analyze-search-glycopeptide-multipart command line option

 -m

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-export-annotate-matched-spectra command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-training-mgf command line option

glycresoft-export-write-csv-spectrum-library command line option

glycresoft-mzml-preprocess command line option

 -mn

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-mzml-preprocess command line option

 -mo

glycresoft-mzml-preprocess command line option

 -n

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycan-combinatorial command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

glycresoft-build-hypothesis-glycan-network-add-prebuilt-neighborhoods command line option

glycresoft-build-hypothesis-glycan-text command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 -o

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

glycresoft-build-hypothesis-glycan-network-add-prebuilt-neighborhoods command line option

glycresoft-build-hypothesis-glycan-network-build-network command line option

glycresoft-export-annotate-matched-spectra command line option

glycresoft-export-glycan-hypothesis command line option

glycresoft-export-glycan-identification command line option

glycresoft-export-glycopeptide-chromatogram-records command line option

glycresoft-export-glycopeptide-hypothesis command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-spectrum-matches command line option

glycresoft-export-glycopeptide-training-mgf command line option

glycresoft-export-identified-glycans-from-glycopeptides command line option

glycresoft-export-write-csv-spectrum-library command line option

 -P

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 -p

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 -q

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 -R

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

 -r

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

glycresoft-analyze-search-glycan command line option

glycresoft-build-hypothesis-glycan-combinatorial command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

glycresoft-build-hypothesis-glycan-text command line option

glycresoft-export-glycan-identification command line option

glycresoft-export-glycopeptide-chromatogram-records command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-mzml-preprocess command line option

 -rn

glycresoft-mzml-preprocess command line option

 -S

glycresoft-analyze-search-glycopeptide-multipart command line option

 -s

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 -snr

glycresoft-mzml-preprocess command line option

 -T

glycresoft-analyze-search-glycopeptide-multipart command line option

 -t

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

glycresoft-analyze-search-glycan command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-export-glycan-identification command line option

glycresoft-export-glycopeptide-chromatogram-records command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-spectrum-matches command line option

glycresoft-export-glycopeptide-training-mgf command line option

glycresoft-export-write-csv-spectrum-library command line option

glycresoft-mzml-preprocess command line option

 -tn

glycresoft-mzml-preprocess command line option

 -u

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

 -V

glycresoft-build-hypothesis-glycopeptide-fa command line option

 -v

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-mzml-preprocess command line option

 -w

glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 -x

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

 -y

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

 -z

glycresoft-build-hypothesis-glycopeptide-fa command line option

A

 ANALYSIS_IDENTIFIER

glycresoft-export-annotate-matched-spectra command line option

glycresoft-export-glycan-identification command line option

glycresoft-export-glycopeptide-chromatogram-records command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-spectrum-matches command line option

glycresoft-export-glycopeptide-training-mgf command line option

glycresoft-export-identified-glycans-from-glycopeptides command line option

glycresoft-export-write-csv-spectrum-library command line option

C

 CHROMATOGRAM_CSV_PATH

glycresoft-analyze-retention-time-evaluate-glycopeptide-retention-time command line option

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

D

 DATABASE_CONNECTION

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

glycresoft-build-hypothesis-glycan-combinatorial command line option

glycresoft-build-hypothesis-glycan-glyspace command line option

glycresoft-build-hypothesis-glycan-network-build-network command line option

glycresoft-build-hypothesis-glycan-text command line option

glycresoft-build-hypothesis-glycopeptide-fa command line option

glycresoft-export-annotate-matched-spectra command line option

glycresoft-export-glycan-hypothesis command line option

glycresoft-export-glycan-identification command line option

glycresoft-export-glycopeptide-chromatogram-records command line option

glycresoft-export-glycopeptide-hypothesis command line option

glycresoft-export-glycopeptide-identification command line option

glycresoft-export-glycopeptide-spectrum-matches command line option

glycresoft-export-glycopeptide-training-mgf command line option

glycresoft-export-identified-glycans-from-glycopeptides command line option

glycresoft-export-write-csv-spectrum-library command line option

 DECOY_DATABASE_CONNECTION

glycresoft-analyze-search-glycopeptide-multipart command line option

F

 FASTA_FILE

glycresoft-build-hypothesis-glycopeptide-fa command line option

G

 glycresoft-analyze-fit-glycoproteome-smoothing-model command line option

--analysis-path

--fdr-threshold

--glycan-hypothesis

--glycopeptide-hypothesis

--network-path

--no-require-multiple-observations

--output-path

--processes

--require-multiple-observations

--smoothing-limit

--unobserved-penalty-scale

-a

-g

-i

-o

-p

-P

-q

-r

-u

-w

 glycresoft-analyze-retention-time-evaluate-glycopeptide-retention-time command line option

CHROMATOGRAM_CSV_PATH

MODEL_PATH

OUTPUT_PATH

 glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

--minimum-observations-for-specific-model

--prefer-joint-model

--test-chromatogram-csv-path

--use-retention-time-normalization

-m

-p

-r

-t

CHROMATOGRAM_CSV_PATH

OUTPUT_PATH

 glycresoft-analyze-search-glycan command line option

--analysis-name

--delta-rt

--export

--grouping-error-tolerance

--mass-error-tolerance

--mass_shift

--mass_shift-combination-limit

--minimum-mass

--ms1-scoring-feature

--msn-mass-error-tolerance

--network-path

--output-path

--processes

--regularization-model-path

--regularize

--require-msms-signature

-a

-d

-f

-g

-k

-m

-mn

-n

-o

-p

-r

-s

-t

-w

DATABASE_CONNECTION

HYPOTHESIS_IDENTIFIER

SAMPLE_PATH

 glycresoft-analyze-search-glycopeptide command line option

--adduct

--analysis-name

--decoy-database-connection

--export

--grouping-error-tolerance

--isotope-probing-range

--mass-error-tolerance

--maximum-mass

--msn-mass-error-tolerance

--no-retention-time-modeling

--output-path

--oxonium-threshold

--permute-decoy-glycan-fragments

--processes

--psm-fdr-threshold

--rare-signatures

--retention-time-modeling

--save-intermediate-results

--tandem-scoring-model

--use-peptide-mass-filter

--workload-size

-a

-D

-f

-g

-G

-m

-mn

-n

-o

-p

-q

-R

-s

-w

-x

DATABASE_CONNECTION

HYPOTHESIS_IDENTIFIER

SAMPLE_PATH

 glycresoft-analyze-search-glycopeptide-multipart command line option

--adduct

--analysis-name

--decoy-hypothesis-identifier

--export

--fdr-estimation-strategy

--glycan-score-threshold

--glycoproteome-smoothing-model

--grouping-error-tolerance

--isotope-probing-range

--mass-error-tolerance

--memory-database-index

--msn-mass-error-tolerance

--no-retention-time-modeling

--output-path

--oxonium-threshold

--peptide-masses-per-scan

--processes

--psm-fdr-threshold

--rare-signatures

--retention-time-modeling

--tandem-scoring-model

--target-hypothesis-identifier

--workload-size

-a

-D

-f

-g

-M

-m

-mn

-n

-o

-p

-P

-q

-R

-s

-S

-T

-w

-x

-y

DATABASE_CONNECTION

DECOY_DATABASE_CONNECTION

SAMPLE_PATH

 glycresoft-build-hypothesis-glycan-combinatorial command line option

--derivatization

--glycan-type

--name

--reduction

-c

-d

-n

-r

DATABASE_CONNECTION

RULE_FILE

 glycresoft-build-hypothesis-glycan-glyspace command line option

--derivatization

--detatch-substituent

--include-children

--motif-class

--name

--reduction

--target-taxon

-d

-i

-m

-n

-r

-s

-t

DATABASE_CONNECTION

 glycresoft-build-hypothesis-glycan-network-add-neighborhood command line option

--expression-rule

--input-path

--name

--output-path

--range-rule

--ratio-rule

-a

-e

-i

-n

-o

-r

 glycresoft-build-hypothesis-glycan-network-add-prebuilt-neighborhoods command line option

--input-path

--name

--output-path

-i

-n

-o

 glycresoft-build-hypothesis-glycan-network-build-network command line option

--edge-strategy

--output-path

-e

-o

DATABASE_CONNECTION

HYPOTHESIS_IDENTIFIER

 glycresoft-build-hypothesis-glycan-text command line option

--derivatization

--name

--reduction

-d

-n

-r

DATABASE_CONNECTION

TEXT_FILE

 glycresoft-build-hypothesis-glycopeptide-fa command line option

--annotation-path

--constant-modification

--enzyme

--glycan-source

--glycan-source-identifier

--glycan-source-type

--include-n-x-c-glycosylation

--max-variable-modifications

--missed-cleavages

--name

--not-full-crossproduct

--occupied-glycosites

--peptide-length-range

--processes

--retain-all-peptides

--reverse

--semispecific-digest

--variable-modification

-a

-c

-C

-e

-F

-g

-G

-m

-n

-p

-R

-s

-u

-v

-V

-y

-z

DATABASE_CONNECTION

FASTA_FILE

 glycresoft-export-annotate-matched-spectra command line option

--mzml-path

--output-path

-m

-o

ANALYSIS_IDENTIFIER

DATABASE_CONNECTION

 glycresoft-export-glycan-hypothesis command line option

--importable

--output-path

-i

-o

DATABASE_CONNECTION

HYPOTHESIS_IDENTIFIER

 glycresoft-export-glycan-identification command line option

--output-path

--report

--threshold

-o

-r

-t

ANALYSIS_IDENTIFIER

DATABASE_CONNECTION

 glycresoft-export-glycopeptide-chromatogram-records command line option

--apex-time-range

--fdr-threshold

--output-path

-o

-r

-t

ANALYSIS_IDENTIFIER

DATABASE_CONNECTION

 glycresoft-export-glycopeptide-hypothesis command line option

--output-path

-o

DATABASE_CONNECTION

HYPOTHESIS_IDENTIFIER

 glycresoft-export-glycopeptide-identification command line option

--fdr-threshold

--mzml-path

--output-path

--report

--threshold

-f

-m

-o

-r

-t

ANALYSIS_IDENTIFIER

DATABASE_CONNECTION

 glycresoft-export-glycopeptide-spectrum-matches command line option

--fdr-threshold

--output-path

--threshold

-f

-o

-t

ANALYSIS_IDENTIFIER

DATABASE_CONNECTION

 glycresoft-export-glycopeptide-training-mgf command line option

--mzml-path

--output-path

--threshold

-m

-o

-t

ANALYSIS_IDENTIFIER

DATABASE_CONNECTION

 glycresoft-export-identified-glycans-from-glycopeptides command line option

--output-path

-o

ANALYSIS_IDENTIFIER

DATABASE_CONNECTION

 glycresoft-export-write-csv-spectrum-library command line option

--fdr-threshold

--mzml-path

--output-path

-m

-o

-t

ANALYSIS_IDENTIFIER

DATABASE_CONNECTION

 glycresoft-mzml-preprocess command line option

--averagine

--background-reduction

--default-precursor-ion-selection-window

--end-time

--extract-only-tandem-envelopes

--ignore-msn

--isotopic-strictness

--mass-offset

--maximum-charge

--missed-peaks

--ms1-averaging

--msn-averagine

--msn-background-reduction

--msn-isotopic-strictness

--msn-missed-peaks

--msn-score-threshold

--msn-transform

--name

--processes

--score-threshold

--signal-to-noise-threshold

--start-time

--transform

--verbose

-a

-an

-b

-bn

-c

-D

-e

-g

-i

-in

-m

-mn

-mo

-n

-p

-r

-rn

-s

-snr

-t

-tn

-v

MS_FILE

OUTFILE_PATH

H

 HYPOTHESIS_IDENTIFIER

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-build-hypothesis-glycan-network-build-network command line option

glycresoft-export-glycan-hypothesis command line option

glycresoft-export-glycopeptide-hypothesis command line option

M

 MODEL_PATH

glycresoft-analyze-retention-time-evaluate-glycopeptide-retention-time command line option

 MS_FILE

glycresoft-mzml-preprocess command line option

O

 OUTFILE_PATH

glycresoft-mzml-preprocess command line option

 OUTPUT_PATH

glycresoft-analyze-retention-time-evaluate-glycopeptide-retention-time command line option

glycresoft-analyze-retention-time-fit-glycopeptide-retention-time command line option

R

 RULE_FILE

glycresoft-build-hypothesis-glycan-combinatorial command line option

S

 SAMPLE_PATH

glycresoft-analyze-search-glycan command line option

glycresoft-analyze-search-glycopeptide command line option

glycresoft-analyze-search-glycopeptide-multipart command line option

T

 TEXT_FILE

glycresoft-build-hypothesis-glycan-text command line option

Contents


# glycresoft  documentation

«  
Peptide Modifications

   ::  

Contents

Table Of Contents

Glycan Composition Classes

Class Names

Glycan Composition Classes
¶

GlycReSoft treats glycan compositions as N-glycans, O-glycans, or GAG-linkers based upon
user assertion. These do not directly impact their use in glycan database searches, but
they are required for glycopeptide database construction.

Class Names
¶

When referred to in text, these are the expected names for these classes. They are parsed case-insensitively.

N-glycan
: 
N
-linked glycans. Assumed to have a chitobiose core.

O-glycan
: 
O
-linked glycans. The default assumption is mucin-type 
O
-glycan but not strictly required.

GAG-linker
: Glycosaminoglycan linker saccharide. Assumed to contain the core trisaccharide.

 «  
Peptide Modifications

   ::  

Contents


# glycresoft  documentation

«  
Building a Glycan Hypothesis from glySpace

   ::  

Contents

   ::  

Building a Glycopeptide Hypothesis from a FASTA File
  »

Table Of Contents

Glycome Network Definitions

Building a Network

glycresoft build-hypothesis glycan-network build-network

Adding Pre-Defined Network Neighborhoods

glycresoft build-hypothesis glycan-network add-prebuilt-neighborhoods

Adding a Custom Network Neighborhood

glycresoft build-hypothesis glycan-network add-neighborhood

Glycome Network Definitions
¶

The glycome network smoothing method presented in “Klein, J., Carvalho, L., & Zaia, J. (2018). Application of network smoothing to glycan LC-MS profiling.
Bioinformatics, 34(20), 3511-3518. 
https://doi.org/10.1093/bioinformatics/bty397
” requires a network definition.

Building a Network
¶

The first step is to build a network for a glycan list, embodied in a 
GlycanHypothesis
 in a database file.
This produces a new text file defining the nodes of the network and builds edges between them.

glycresoft build-hypothesis glycan-network build-network
¶

glycresoft

build-hypothesis

glycan-network

build-network

[
OPTIONS
]

DATABASE_CONNECTION

HYPOTHESIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

-e
,

--edge-strategy

<choice>
¶

Strategy to use to decide when two nodes are connected by an edge [default: manhattan]

Choices: [

manhattan]

Arguments

DATABASE_CONNECTION
¶

Required argument
<string> A connection URI for a database, or a path on the file system

HYPOTHESIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycan hypothesis to use

Adding Pre-Defined Network Neighborhoods
¶

Add pre-programmed neighborhood rules to a network, writing them out to a new text file.

glycresoft build-hypothesis glycan-network add-prebuilt-neighborhoods
¶

glycresoft

build-hypothesis

glycan-network

add-prebuilt-neighborhoods

[
OPTIONS
]

Options

-i
,

--input-path

<path>
¶

-o
,

--output-path

<path>
¶

-n
,

--name

<choice>
¶

Set the neighborhood name [required]

Choices: [

n-glycan; mammalian-n-glycan]

Adding a Custom Network Neighborhood
¶

Add a custom neighborhood to a network, writing them out to a new text file. A neighborhood is
composed of one or more rule expressions.

glycresoft build-hypothesis glycan-network add-neighborhood
¶

glycresoft

build-hypothesis

glycan-network

add-neighborhood

[
OPTIONS
]

Options

-i
,

--input-path

<path>
¶

-o
,

--output-path

<path>
¶

-n
,

--name

<string>
¶

Set the neighborhood name [required]

-r
,

--range-rule

<string>
¶

Format: <expression:str> <low:int> <high:int> <required:bool> (May specify more than once)

-e
,

--expression-rule

<string>
¶

Format: <bool-expression:str> <required:bool> (May specify more than once)

-a
,

--ratio-rule

<string>
¶

Format: <numerator-expr:str> <denominator-expr:str> <threshold:float> <required:bool> (May specify more than once)

 «  
Building a Glycan Hypothesis from glySpace

   ::  

Contents

   ::  

Building a Glycopeptide Hypothesis from a FASTA File
  »


# glycresoft  documentation

«  
High Energy Glycopeptide Search Tutorial

   ::  

Contents

   ::  

Other Topics
  »

Table Of Contents

Multipart Score Glycopeptide Search Tutorial

Preparing the Data

Building the Search Space

Searching the Data

Build a Glycosite Smoothing Model

Build a Fragmentation Model

Multipart Score Glycopeptide Search Tutorial
¶

This tutorial will cover the steps involved in analyzing glycopeptide LC-MS/MS data acquired
with stepped collision energy, a scenario where there are abundant peptide+Y fragmentats,
especially for proteome scale search spaces, or otherwise when it is desirable to have an FDR
estimated separately for the peptide and the glycan components of a glycopeptide.

Preparing the Data
¶

To begin, please download the five Thermo .raw files from 
PXD005413
:

# using wget and the PRIDE FTP location programmatically may work

$

for

i

in

1

2

3

4

5
;

do

wget

ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2017/09/PXD005413/MouseHeart-Z-T-
${
i
}
.raw

done

On Windows, you can directly process these files in the next step, but for other platforms, please convert them to
mzML without peak picking. Then run the MS deconvolution tool 
glycresoft

mzml

preprocess

CLI Documentation
.

Note

Replace the 
$CPUS
 variable with the number of cores you want the program to use per command.

$

for

i

in

1

2

3

4

5
;

do

glycresoft

-l

preprocess-
${
i
}
.log

mzml

preprocess

\

-p

$CPUS

\

-v

\

-b

0

\

-g

1

\

-a

glycopeptide

\

-c

12

\

-an

peptide

\

-tn

10

\

MouseHeart-Z-T-
${
i
}
.
$ext

MouseHeart-Z-T-
${
i
}
.deconv.mzML

done

Depending upon the number of CPUs used, this may take 20-30 minutes per sample.

Building the Search Space
¶

Meanwhile, we need to prepare the database GlycReSoft will search. Please download the UniProt Mouse reference proteome and the glycan list:

wget

"https://rest.uniprot.org/uniprotkb/stream?format=fasta&query=%28%28proteome%3AUP000000589%29+AND+reviewed%3Dtrue%29"

-O

UP000000589_mouse_reference_proteome_sp_only.fa
wget

--no-check-certificate

"https://raw.githubusercontent.com/mobiusklein/glycresoft/master/docs/tutorials/combined_mouse_nglycans.txt"

-O

combined_mouse_nglycans.txt

The multi-part scoring scheme requires a separate target database and decoy database, and does not fully materialize the crossproduct of peptides and glycans,
dynamically computing them as needed at run time. Therefore we need to build the database twice, once for the targets and once for the decoys with the 
--reverse

flag set, and we pass the 
-F

/

--not-full-crossproduct
 flag to both database build commands to prevent the crossproduct from being generated
consequently taking substantially less time.

We build the target database:

glycresoft

-l

build-target-db.log

build-hypothesis

glycopeptide-fa

-p

$CPUS

\

-g

combined_mouse_nglycans.txt

-s

text

\

-u

1

\

-c

"Carbamidomethyl (C)"

\

-v

"Oxidation (M)"

\

-m

2

-e

trypsin

\

-C

\

-F

\

UP000000589_mouse_reference_proteome_sp_only.fa

UP000000589_mouse_sp_only_glycoproteome.db

export

TARGET_DB
=
UP000000589_mouse_sp_only_glycoproteome.db

and decoy database:

glycresoft

-l

build-decoy-db.log

build-hypothesis

glycopeptide-fa

-p

$CPUS

\

-g

combined_mouse_nglycans.txt

-s

text

\

-u

1

\

-c

"Carbamidomethyl (C)"

\

-v

"Oxidation (M)"

\

-m

2

-e

trypsin

\

-C

\

-F

\

--reverse

\

UP000000589_mouse_reference_proteome_sp_only.fa

UP000000589_mouse_sp_only_glycoproteome.decoy.db

export

DECOY_DB
=
UP000000589_mouse_sp_only_glycoproteome.decoy.db

The database build process should take between 5 and 10 minutes in total, depending upon UniProt’s average response time.

Searching the Data
¶

Once the databases are built and the spectra have been preprocessed, we can run the search step (
CLI Documentation
).

This will write both the complete results recorded a SQLite file as well as a more easily readable CSV file of the glycopeptide spectrum matches.

$

for

i

in

1

2

3

4

5
;

do

glycresoft

-l

search-
${
i
}
.log

analyze

search-glycopeptide-multipart

\

-p

$CPUS

\

-w

500

\

-m

5e-6

\

-mn

2e-5

\

-s

log_intensity_v3

\

-o

Mouse-Heart-
${
i
}
.search.db

\

-M

\

-a

Ammonium

2

\

--export

psm-csv

\

$TARGET_DB

$DECOY_DB

\

MouseHeart-Z-T-
${
i
}
.deconv.mzML

done

Depending upon the number of CPUs used, this may take 20-30 minutes per sample. Output files from a similar analysis are available as part of the
supplementary data files for 
TBD
.

Build a Glycosite Smoothing Model
¶

To build a site-specific glycome network smoothing model, we need to build a graph first.
(
CLI Documentation for build-network
, 
CLI Documentation for add-prebuilt-neighborhoods
)

$

glycresoft

build-hypothesis

glycan-network

build-network

$TARGET_DB

1

-o

mouse_glycan_network.txt
$

glycresoft

build-hypothesis

glycan-network

add-prebuilt-neighborhoods

-n

mammalian-n-glycan

\

-i

mouse_glycan_network.txt

\

-o

mouse_glycan_network.txt

Then, we can run the glycosite smoothing model building workflow (
CLI Documentation
):

$

glycresoft

analyze

fit-glycoproteome-smoothing-model

\

-p

$CPUS

-i

Mouse-Heart-1.search.db

1

\

-i

Mouse-Heart-2.search.db

1

\

-i

Mouse-Heart-3.search.db

1

\

-i

Mouse-Heart-4.search.db

1

\

-i

Mouse-Heart-5.search.db

1

\

-w

mouse_glycan_network.txt

\

-q

0
.01

\

-g

$TARGET_DB

1

\

-P

$TARGET_DB

1

\

-o

mouse-heart-heart-glycosite-models.json

This may take between 10 and 20 minutes depending upon the number of CPUs used.

Then we can re-analyze the dataset with the smoothing model:

$

for

i

in

1

2

3

4

5
;

do

glycresoft

-l

search-smoothed-
${
i
}
.log

analyze

search-glycopeptide-multipart

\

-p

$CPUS

\

-w

500

\

-m

5e-6

\

-mn

2e-5

\

-s

log_intensity_v3

\

-o

Mouse-Heart-
${
i
}
.search-smoothed.db

\

-M

\

-a

Ammonium

2

\

--export

psm-csv

\

-S

mouse-heart-heart-glycosite-models.json

\

$TARGET_DB

$DECOY_DB

\

MouseHeart-Z-T-
${
i
}
.deconv.mzML

done

This should take approximately the same amount of time as the original search.

Build a Fragmentation Model
¶

The first step is to export annotated MGF files from the identification results.

$

for

i

in

1

2

3

4

5
;

do

glycresoft

-l

export-training-mgf-
${
i
}
.log

glycopeptide-training-mgf

\

-o

Mouse-Heart-
${
i
}
.training.mgf

\

Mouse-Heart-
${
i
}
.search.db

1

This should take approximately 1-2 minutes or less per file.

Then, ensure 
glycopeptide_feature_learning

is installed. Now we can fit the fragmentation model:

$

DATAFILES
=
`
ls

Mouse-Heart-*.training.mgf
`

$

MODEL_NAME
=
mouse-heart-fragmodel
$

# Do the model training

$

glycopeptide-feature-learning

fit-model

-t

20

$DATAFILES

-o

${
MODEL_NAME
}
.json
$

# Convert the complete model into something easier for Python to read

$

glycopeptide-feature-learning

compile-model

${
MODEL_NAME
}
.json

${
MODEL_NAME
}
.pkl
$

# Evaluate the model fit

$

glycopeptide-feature-learning

calculate-correlation

-t

20

$DATAFILES

./correlation.
${
MODEL_NAME
}
.pkl

${
MODEL_NAME
}
.pkl

This should take approximately 10 minutes total.

 «  
High Energy Glycopeptide Search Tutorial

   ::  

Contents

   ::  

Other Topics
  »


# glycresoft  documentation

«  
Tutorials

   ::  

Contents

   ::  

Multipart Score Glycopeptide Search Tutorial
  »

Table Of Contents

High Energy Glycopeptide Search Tutorial

Preparing the Data

Building the Search Space

Search the Data

High Energy Glycopeptide Search Tutorial
¶

This tutorial will cover the steps involved in analyzing glycopeptide
LC-MS/MS data acquired with a high collision energy.

Preparing the Data
¶

You can download the raw data we will analyze from 
20150710_3um_AGP_001.mzML.gz
. Please download it and decompress it.

Deconvolution of Glycopeptide LC-MS/MS
¶

$

glycresoft

mzml

preprocess

-p

6

-v

-a

glycopeptide

-an

peptide

20150710_3um_AGP_001.mzML
\

20150710_3um_AGP_001.preprocessed.mzML

This will deconvolute the LC-MS run, using six worker processes. This may take a significant
amount of time, on the order of one to two hours.

Building the Search Space
¶

Meanwhile, we can begin setting up the hypothesis. This sample contains predominantly AGP
glycopeptides, so we can start by downloading the AGP protein sequences from UniProt:

$

echo

P19652

>>

accession.txt
$

echo

P02763

>>

accession.txt
$

glycresoft

tools

download-uniprot

-i

accession.txt

-o

agp.fa

Copy the following text into a file “combinatorial-rules.txt”

Glycan Combinatorial Rules
¶

Hex 3 10
HexNAc 2 9
Fuc 0 5
Neu5Ac 0 4

Fuc < HexNAc
HexNAc > NeuAc + 1

Next, we’ll build the glycan hypothesis

Build glycan hypothesis
¶

$

glycresoft

build-hypothesis

glycan-combinatorial

combinatorial-rules.txt

glycans.db

Now, we’ll build the glycopeptide hypothesis using these glycans and the protein
FASTA we downloaded earlier

Build glycopeptide hypothesis
¶

$

glycresoft

build-hypothesis

glycopeptide-fa

-g

glycans.db

-s

hypothesis

-G

1
\

-u

1

-e

trypsin

-m

1

-c

"Carbamidomethyl (C)"

-v

"Deamidation (N)"
\

-v

"Pyro-glu from Q (Q@N-term)"

-p

4

-n

"Alpha-1-acid Glycopeptide Hypothesis"
\

agp.fa

fasta-agp.db

This task should take a few minutes at most.

Search the Data
¶

Once both the glycopeptide hypothesis is built and the sample is deconvoluted, we can
run the database search:

Database search process
¶

$

glycresoft

analyze

search-glycopeptide

fasta-agp.db

20150710_3um_AGP_001.preprocessed.mzML

1
\

-o

agp-glycopepitdes-20150710_3um_AGP_001.db

-p

5

The search process should take between 2 and 10 minutes.

Once the database search process has completed, we can export the search results in CSV format
for downstream analysis.

CSV export
¶

$

glycresoft

export

glycopeptide-identification

agp-glycopepitdes-20150710_3um_AGP_001.db

1
\

-o

agp-glycopeptides.csv

 «  
Tutorials

   ::  

Contents

   ::  

Multipart Score Glycopeptide Search Tutorial
  »


# glycresoft  documentation

«  
Building a Glycan Hypothesis from a Text File

   ::  

Contents

   ::  

Glycome Network Definitions
  »

Table Of Contents

Building a Glycan Hypothesis from glySpace

glycresoft build-hypothesis glycan-glyspace

Usage Example

Bibliography

Building a Glycan Hypothesis from glySpace
¶

Warning

GlyCosmos has changed the graph structure such that these queries can no longer
function. This documentation is preserved for the future if they can be made to work.

The glycoinformatics community has developed a federation of
databases called 
glySpace
, which composes
the “namespace of all glycan structures”. It uses semantic web
technologies to describe and relate these entities, and gateways
into 
glySpace
 such as 
[GlyTouCan]

provide query-able access to this data.

You can build a glycan composition hypothesis from the set of annotated
glycan structures in 
glySpace
 for N-glycans and O-glycans,
with or without taxonomic filters.

glycresoft build-hypothesis glycan-glyspace
¶

glycresoft

build-hypothesis

glycan-glyspace

[
OPTIONS
]

DATABASE_CONNECTION

Options

-r
,

--reduction

<string>
¶

Reducing end modification

-d
,

--derivatization

<string>
¶

Chemical derivatization to apply

-n
,

--name

<string>
¶

The name for the hypothesis to be created

-m
,

--motif-class

<choice>
¶

Specify a glycan structure family to search for

Choices: [

n-linked; o-linked]

-t
,

--target-taxon

<string>
¶

Only select structures annotated with this taxonomy

-i
,

--include-children
¶

Include child taxa of –target-taxon. No effect otherwise. [default: False]

-s
,

--detatch-substituent

<substituent>
¶

Substituent type to detatch from all monosaccharides (May specify more than once)

Arguments

DATABASE_CONNECTION
¶

Required argument
<string> A connection URI for a database, or a path on the file system

For more information on reductions and derivatizations, please see 
Glycan Modifications

Usage Example
¶

# Get all human N-glycans

$

glycresoft

build-hypothesis

glycan-glyspace

-m

n-linked

-t

9606

glyspace-glycans.db

-n

"Human N-Linked Glycans"

Building

Glycan

Hypothesis

Human

N-Linked

Glycans

(
2
)

14
:35:34

-

glycresoft:log:175

-

INFO

-

Begin

N

Glycan

Glyspace

Hypothesis

Serializer

{
'composition_cache'
:

None,

'derivatization'
:

None,

'engine'
:

Engine
(
sqlite:///glyspace-glycans.db
)
,

'filter_functions'
:

[
TaxonomyFilter
({
'9606'
})]
,

'loader'
:

None,

'reduction'
:

None,

'seen'
:

{}
,

'simplify'
:

True,

'start_time'
:

datetime.datetime
(
2017
,

8
,

31
,

14
,

35
,

34
,

164000
)
,

'status'
:

'started'
,

'transformer'
:

None,

'uuid'
:

'471041e33f23485c9a570a5b4ea6e0d2'
}

14
:35:34

-

glycresoft:log:175

-

INFO

-

Querying

GlySpace

14
:35:54

-

glycresoft:log:175

-

INFO

-

Translating

Response

14
:36:19

-

glycresoft:log:175

-

INFO

-

Stored

976

glycan

structures

and

195

glycan

compositions

14
:38:24

-

glycresoft:log:175

-

INFO

-

Hypothesis

Completed

14
:38:24

-

glycresoft:log:175

-

INFO

-

End

N

Glycan

Glyspace

Hypothesis

Serializer

14
:38:24

-

glycresoft:log:175

-

INFO

-

Started

at

2017
-08-31

14
:35:34.164000.
Ended

at

2017
-08-31

14
:38:24.535000.
Total

time

elapsed:

0
:02:50.371000
NGlycanGlyspaceHypothesisSerializer

completed

successfully.

# Get all human O-glycans

$

glycresoft

build-hypothesis

glyspace-glycan

-m

o-linked

-t

9606

glyspace-glycans.db

-n

"Human O-Linked Glycans"

Building

Glycan

Hypothesis

Human

O-Linked

Glycans

15
:33:49

-

glycresoft:log:175

-

INFO

-

Begin

O

Glycan

Glyspace

Hypothesis

Serializer

{
'composition_cache'
:

None,

'derivatization'
:

None,

'engine'
:

Engine
(
sqlite:///glyspace-glycans.db
)
,

'filter_functions'
:

[
TaxonomyFilter
({
'9606'
})]
,

'loader'
:

None,

'reduction'
:

None,

'seen'
:

{}
,

'simplify'
:

True,

'start_time'
:

datetime.datetime
(
2017
,

8
,

31
,

15
,

33
,

49
,

601000
)
,

'status'
:

'started'
,

'transformer'
:

None,

'uuid'
:

'714d52c2525c499286f673e294672b9e'
}

15
:33:49

-

glycresoft:log:175

-

INFO

-

Querying

GlySpace

15
:33:55

-

glycresoft:log:175

-

INFO

-

Translating

Response

15
:34:01

-

glycresoft:log:175

-

INFO

-

Stored

315

glycan

structures

and

95

glycan

compositions

15
:34:01

-

glycresoft:log:175

-

INFO

-

Hypothesis

Completed

15
:34:01

-

glycresoft:log:175

-

INFO

-

End

O

Glycan

Glyspace

Hypothesis

Serializer

15
:34:01

-

glycresoft:log:175

-

INFO

-

Started

at

2017
-08-31

15
:33:49.601000.
Ended

at

2017
-08-31

15
:34:01.737000.
Total

time

elapsed:

0
:00:12.136000
OGlycanGlyspaceHypothesisSerializer

completed

successfully.

Bibliography
¶

[
GlyTouCan
]

Aoki-Kinoshita, K., Agravat, S., Aoki, N. P., Arpinar, S., Cummings, R. D., Fujita, A., … Narimatsu, H. (2015).
GlyTouCan 1.0 – The international glycan structure repository. Nucleic Acids Research, gkv1041.

https://doi.org/10.1093/nar/gkv1041

 «  
Building a Glycan Hypothesis from a Text File

   ::  

Contents

   ::  

Glycome Network Definitions
  »


# glycresoft  documentation

«  
Command Line Applications

   ::  

Contents

   ::  

Building a Combinatorial Glycan Hypothesis
  »

Database Building Tools
¶

glycresoft
 requires that a search space, called a “hypothesis”, be pre-specified prior to searching it. This is done explicitly so that
the database construction process is not repeated needlessly for multiple searches of the same space, and to allow slower
data organization and indexing steps to run independent of the main search program.

There are many different ways to build glycan and glycopeptide databases.

Building a Combinatorial Glycan Hypothesis

Building a Glycan Hypothesis from a Text File

Building a Glycan Hypothesis from glySpace

Building a Glycan Composition Graph

Building a Glycopeptide Hypothesis from a FASTA File

 «  
Command Line Applications

   ::  

Contents

   ::  

Building a Combinatorial Glycan Hypothesis
  »


# glycresoft  documentation

«  
Other Topics

   ::  

Contents

   ::  

Peptide Modifications
  »

IUPAClite Monosaccharide and Glycan Notation
¶

GlycReSoft uses 
glypy
’s 
IUPAClite
 notation for monosaccharides and glycan compositions.

A monosaccharide is denoted using IUPAC notation, omitting ring shape, anomeric state, chirality,
and modification positions (optionally). For example, 
a-D-Manp
 would be written 
Man
, or

b-D-Glcp2NAc
 would be written 
GlcNAc
.

You can also use generic base types like 
Hex
 or 
Pen
 for example, to denote a six or five
carbon monosaccharide. The notation is composable, so you can specify an arbitrarily modified
monosaccharide, like 
HexNAc(S)
 to specify a sulfated HexNAc, using the parenthesized convention
that separates substituent groups, or 
dHexN
 for a deoxy-Hexosamine.

You can also define “floating” substituent groups by prefixing their full lowercase
names with an 
@
-sign, like 
@sulfate
 for sulfate or 
@acetyl
 for an acetyl group.

A glycan composition is written as one or more 
<monosaccharide>:<count>
 occurrences separated by
a “; “ (semi-colon + space), enclosed in “{ }”. A few examples are shown below:

IUPAClite glycan compositions
¶

{
Hex
:
5
;

HexNAc
:
4
;

Neu5Ac
:
1
}

{
Hex
:
5
;

HexNAc
:
4
;

Neu5Ac
:
2
}

{
Fuc
:
1
;

Hex
:
5
;

HexNAc
:
4
;

Neu5Ac
:
2
}

{
Fuc
:
2
;

Hex
:
6
;

HexNAc
:
5
;

Neu5Ac
:
1
}

{
Fuc
:
1
;

Hex
:
6
;

HexNAc
:
5
;

Neu5Ac
:
2
}

 «  
Other Topics

   ::  

Contents

   ::  

Peptide Modifications
  »


# glycresoft  documentation

«  
Search and Analysis Tools

   ::  

Contents

   ::  

Searching a Processed Sample with a Glycan Database
  »

Table Of Contents

LC-MS/MS Data Preprocessing and Deconvolution

glycresoft mzml preprocess

Usage example

Averagine Models

Builtin Models

Supported File Formats

Signal Filters

Output Information

Bibliography

LC-MS/MS Data Preprocessing and Deconvolution
¶

Convert raw mass spectral data files into deisotoped neutral mass peak lists
written to a new 
mzML

[Martens2011]
 file. For tandem mass spectra,
recalculate precursor ion monoisotopic peaks.

This task is computationally intensive, and uses several collaborative processes
to share the work.

glycresoft mzml preprocess
¶

Convert raw mass spectra data into deisotoped neutral mass peak lists.

glycresoft

mzml

preprocess

[
OPTIONS
]

MS_FILE

OUTFILE_PATH

Options

-a
,

--averagine

<averagine>
¶

Averagine model to use for MS1 scans. Either a name or formula. May specify multiple times. (May specify more than once)

-an
,

--msn-averagine

<averagine>
¶

Averagine model to use for MS^n scans. Either a name or formula. May specify multiple times. (May specify more than once)

-s
,

--start-time

<float>
¶

Scan time to begin processing at in minutes

-e
,

--end-time

<float>
¶

Scan time to stop processing at in minutes

-c
,

--maximum-charge

<int>
¶

Highest absolute charge state to consider

-n
,

--name

<string>
¶

Name for the sample run to be stored. Defaults to the base name of the input data file

-t
,

--score-threshold

<float>
¶

Minimum score to accept an isotopic pattern fit in an MS1 scan

-tn
,

--msn-score-threshold

<float>
¶

Minimum score to accept an isotopic pattern fit in an MS^n scan

-m
,

--missed-peaks

<int>
¶

Number of missing peaks to permit before an isotopic fit is discarded

-mn
,

--msn-missed-peaks

<int>
¶

Number of missing peaks to permit before an isotopic fit is discarded in an MSn scan

-p
,

--processes

<int>
¶

Number of worker processes to use. Defaults to 4 or the number of CPUs, whichever is lower

-b
,

--background-reduction

<float>
¶

Background reduction factor. Larger values more aggresively remove low abundance signal in MS1 scans.

-bn
,

--msn-background-reduction

<float>
¶

Background reduction factor. Larger values more aggresively remove low abundance signal in MS^n scans.

-r
,

--transform

<func>
¶

Scan transformations to apply to MS1 scans. May specify more than once. (May specify more than once)

-rn
,

--msn-transform

<func>
¶

Scan transformations to apply to MS^n scans. May specify more than once. (May specify more than once)

-v
,

--extract-only-tandem-envelopes
¶

Only work on regions that will be chosen for MS/MS

--verbose
¶

Log additional diagnostic information for each scan.

-g
,

--ms1-averaging

<int>
¶

The number of MS1 scans before and after the current MS1 scan to average when picking peaks.

--ignore-msn
¶

Ignore MS^n scans

-i
,

--isotopic-strictness

<float>
¶

-in
,

--msn-isotopic-strictness

<float>
¶

-snr
,

--signal-to-noise-threshold

<float>
¶

Signal-to-noise ratio threshold to apply when filtering peaks

-mo
,

--mass-offset

<float>
¶

Shift peak masses by the given amount

-D
,

--default-precursor-ion-selection-window

<float>
¶

The isolation window width to assume when it is not specified.

Arguments

MS_FILE
¶

Required argument
<path>

OUTFILE_PATH
¶

Required argument
<path>

Usage example
¶

example usage
¶

glycresoft-cli

mzml

preprocess

-a

permethylated-glycan

-t

20

-p

6

\

-s

5
.0

-e

60
.0

"path/to/input"

"path/to/output.mzML"

Averagine Models
¶

Argument type for 
<averagine>
. The model selected influences how isotopic
patterns are estimated for an arbitrary mass. The value of this parameter may
be a builtin model name or a formula.

For a more complete discsussion of how “averagine” isotopic models work, see 
[Senko1995]
.

Builtin Models
¶

Error

Unable to execute python code at mzml-preprocess.rst:38:

cannot import name ‘AveragineParamType’ from ‘glycresoft.cli.validators’ (C:\Users\Joshua\Dev\glycresoft\src\glycresoft\cli\validators.py)

Traceback (most recent call last):
 File “C:\Users\Joshua\Dev\glycresoft\docs\_ext\exec_directive.py”, line 24, in run
 exec(‘\n’.join(self.content))
 File “<string>”, line 1, in <module>
ImportError: cannot import name ‘AveragineParamType’ from ‘glycresoft.cli.validators’ (C:\Users\Joshua\Dev\glycresoft\src\glycresoft\cli\validators.py)

Supported File Formats
¶

MS_FILE
 may be in mzML or mzXML format.

Signal Filters
¶

Prior to picking peaks, the raw mass spectral signal may be filtered a number
of ways. By default, a local noise reduction filter is applied, modulated by

-b
 and 
-bn
 options respectively. Other filers may be set using 
-r

and 
-rn
:

mean_below_mean
 - Remove all points below the mean of all points below the mean of all unfiltered points of this scan

median
 - Remove all points below the median intensity of this scan

one_percent_of_max
 - Remove all points with intensity less than 1% of the maximum intensity point of this scan

fticr_baseline
 - Apply the same background reduction algorithm used by 
-b
 and 
-bn

savitsky_golay
 - Apply Savtisky-Golay smoothing on the intensities of this scan

Output Information
¶

The resulting mzML file from this tool attempts to preserve as much metadata as possible
from the source data file, and records its own metadata in the appropriate sections of
the document.

Each scan has a standard set of 
cvParam
 entries covering scan polarity,
peak mode, and MS level. In addition to the normal 
m/z

array
 and 
intensity

array

entries, each scan also includes the standardized 
charge

array
, as well as two non-standard
arrays, 
deconvolution

score

array
 and 
isotopic

envelopes

array
. The 
deconvolution

score

array

is just the result of the goodness-of-fit function used to evaluate the isotopic envelopes resulting
in the reported peaks. The 
isotopic

envelopes

array
 is more complex, as it encodes the set of isotopic
peaks used to fit each reported peak, and does not have a one-to-one relationship with other arrays.

To unpack the 
isotopic

envelopes

array
 after decoding, the we use the following logic:

 1
def

decode_envelopes
(
array
):

 2

'''

 3
 Arguments

 4
 ---------

 5
 array: float32 array

 6
 '''

 7

envelope_list

=

[]

 8

current_envelope

=

[]

 9

i

=

0

10

n

=

len
(
array
)

11

while

i

<

n
:

12

# fetch the next two values

13

mz

=

array
[
i
]

14

intensity

=

array
[
i

+

1
]

15

i

+=

2

16

17

# if both numbers are zero, this denotes the beginning

18

# of a new envelope

19

if

mz

==

0

and

intensity

==

0
:

20

if

current_envelope

is

not

None
:

21

if

current_envelope
:

22

envelope_list
.
append
(
Envelope
(
current_envelope
))

23

current_envelope

=

[]

24

# otherwise add the current point to the existing envelope

25

else
:

26

current_envelope
.
append
(
EnvelopePair
(
mz
,

intensity
))

27

envelope_list
.
append
(
Envelope
(
current_envelope
))

28

return

envelope_list

Bibliography
¶

[
Senko1995
]

Senko, M. W., Beu, S. C., & McLafferty, F. W. (1995). Determination of
monoisotopic masses and ion populations for large biomolecules from resolved
isotopic distributions.
Journal of the American Society for Mass Spectrometry, 6(4), 229–233.

https://doi.org/10.1016/1044-0305(95)00017-8

[
Martens2011
]

Martens, L., Chambers, M., Sturm, M., Kessner, D., Levander, F., Shofstahl, J.,
… Deutsch, E. W. (2011). mzML–a community standard for mass spectrometry data.
Molecular & Cellular Proteomics : MCP, 10(1), R110.000133.

https://doi.org/10.1074/mcp.R110.000133

 «  
Search and Analysis Tools

   ::  

Contents

   ::  

Searching a Processed Sample with a Glycan Database
  »


# glycresoft  documentation

«  
IUPAClite Monosaccharide and Glycan Notation

   ::  

Contents

   ::  

Glycan Composition Classes
  »

Table Of Contents

Peptide Modifications

UNIMOD

Targeting Rules

Peptide Modifications
¶

glycresoft
 supports the full range of 
UNIMOD

modification rules as well as some common alternative namings. Targeting rules listed in this table are the known
specificities from the database, but arbitrary targets may be specified.

Additionally, custom modifications may be specified using the syntax 
@<name>-<mass>
, e.g. 
@custom

modification-410.5

for a modification “custom modification” with a mass delta of +410.5 Da, or 
@Antimatter--99999
 for a modification with the
name “Antimatter” with a mass delta of -99999.0 Da.

UNIMOD
¶

Warning

If you wish to specify a glycan modification, please use GlycReSoft’s system for glycan compositions, not UNIMOD’s.

UNIMOD

Name

Mass

Other Names

Targets

UNIMOD:1

Acetyl

42.010565

Acetylation

H, C, N-term, K, S, T, R, Y

UNIMOD:2

Amide

-0.984016

Amidation, Top-Down sequencing c-type fragment ion, Amidated

C-term

UNIMOD:3

Biotin

226.077598

Biotinylation

N-term, K

UNIMOD:4

Carbamidomethyl

57.021464

Carboxyamidomethylation, Iodoacetamide derivative

H, C, N-term, M, K, U, D, S, T, E, Y

UNIMOD:5

Carbamyl

43.005814

Carbamylation

C, N-term, M, K, S, T, R, Y

UNIMOD:6

Carboxymethyl

58.005479

Iodoacetic acid derivative, Carboxymethylation

C, N-term, K, U, W

UNIMOD:7

Deamidated

0.984016

Deamidation, Citrullination, phenyllactyl from N-term Phe

N, F @ Protein N-term, R, Q

UNIMOD:8

ICAT-G

486.251206

Gygi ICAT(TM) d0, Gygi_ICATd0

C

UNIMOD:9

Gygi_ICATd8

494.30142

Gygi ICAT(TM) d8, ICAT-G:2H(8)

C

UNIMOD:10

HSe

-29.992806

Homoserine, Met->Hse

M @ C-term

UNIMOD:11

Met->Hsl

-48.003371

Homoserine lactone, Hse_lact

M @ C-term

UNIMOD:12

ICAT-D:2H(8)

450.275205

Applied Biosystems original ICAT(TM) d8, AB_old_ICATd8

C

UNIMOD:13

ICAT-D

442.224991

AB_old_ICATd0, Applied Biosystems original ICAT(TM) d0

C

UNIMOD:17

NIPCAM

99.068414

Dimethylacrylamide, DMA, N-isopropylcarboxamidomethyl

C

UNIMOD:20

PEO-Biotin

414.193691

Pierce EZ-Link PEO-Iodoacetyl Biotin, Biotinyl-iodoacetamidyl-3,6-dioxaoctanediamine, PEO-Iodoacetyl-LC-Biotin

C

UNIMOD:21

Phospho

79.966331

Phosphorylation

H, C, K, TYS, S, D, TS, T, R, E, Y

UNIMOD:23

Dehydrated

-18.010565

C-terminal imide, Phospho+PL, Dehydration, didehydroalanine, D-Succinimide, Prompt loss of phosphate from phosphorylated residue

C @ N-term, N @ Protein C-term, S, D, T, Q @ Protein C-term, Y

UNIMOD:24

Propionamide

71.037114

Acrylamide adduct

N-term, K, C

UNIMOD:25

pyridylacetyl

119.037114

Pyridylacetyl

N-term, K

UNIMOD:26

Pyro-cmC

39.994915

Carbamidomethyl-Cys cyclization (N-terminus), Pyro-carbamidomethyl, S-carbamoylmethylcysteine cyclization (N-terminus), Carboxymethyl-Cys cyclization (N-terminus)

C @ N-term

UNIMOD:27

Pyro_glu

-18.010565

Glu->pyro-Glu, Pyro-glu from E

E @ N-term

UNIMOD:28

Pyro-glu

-17.026549

Gln->pyro-Glu, Pyro-glu from Q

Q @ N-term

UNIMOD:29

SMA

127.063329

N-Succinimidyl-2-morpholine acetate

N-term, K

UNIMOD:30

Sodiated

21.981943

Sodium adduct, Cation:Na

C-term, D, E

UNIMOD:31

Pyridylethyl

105.057849

S-pyridylethylation, S-pyridylethyl

C

UNIMOD:34

Methyl

14.01565

methyl ester, Methylation

H, C, N-term, N, C-term, K, D, S, T, R, E, I, Q, L

UNIMOD:35

Oxidation

15.994915

Hydroxylation, Oxidation or Hydroxylation

V, N, D, F, W, T, C, M, K, P, S, R, E, I, Y, L, H, U, G @ C-term, Q

UNIMOD:36

Dimethyl

28.0313

di-Methylation

N-term, N, K, P @ Protein N-term, R

UNIMOD:37

Trimethyl

42.04695

tri-Methylation

K, R, A @ Protein N-term

UNIMOD:39

MMTS

45.987721

b-methylthiol, Methyl methanethiosulfonate, Methylthio, Beta-methylthiolation

C, N-term, N, K, D

UNIMOD:40

Sulfo

79.956815

Sulfation, O-Sulfonation

S, Y, T, C

UNIMOD:41

Hex

162.052824

Hexose, Mannose, Galactose, Fructose, Glucose

C, N-term, N, K, S, W, T, R, Y

UNIMOD:42

Lipoyl

188.032956

K

UNIMOD:43

HexNAc

203.079373

N-Acetylhexosamine

S, N, T

UNIMOD:44

Farnesyl

204.187801

Farnesylation

C

UNIMOD:45

Myristoyl

210.198366

Myristoylation

K, G @ N-term, C

UNIMOD:46

Pyridoxal-phos

229.014009

Pyridoxal phosphate, PyridoxalPhosphate

K

UNIMOD:47

Palmitoyl

238.229666

Palmitoylation

C, N-term, K, S, T

UNIMOD:48

GeranylGeranyl

272.250401

Geranyl-geranyl

C

UNIMOD:49

p-pantetheine

340.085794

Phosphopantetheine

S

UNIMOD:50

FAD

783.141486

Flavin adenine dinucleotide

Y, H, C

UNIMOD:51

Tripalmitate

788.725777

N-acyl diglyceride cysteine

C @ Protein N-term

UNIMOD:52

Guanidinyl

42.021798

homoarginine, Guanidination

N-term, K

UNIMOD:53

HNE

156.11503

4-hydroxynonenal (HNE)

H, C, K, A, L

UNIMOD:54

Glucuronyl

176.032088

hexuronic acid, glucuronosyl, N-glucuronyl

N-term, S, T

UNIMOD:55

Glutathione

305.068156

glutathione disulfide

C

UNIMOD:56

Acetyl_heavy

45.029395

Acetate labeling reagent (N-term & K) (heavy form, +3amu), N-trideuteriumacetoxy, Acetyl:2H(3)

H, N-term, K, S, T, Y

UNIMOD:58

Propionyl

56.026215

Propionate labeling reagent light form (N-term & K), Propionyl_light

N-term, K, T, S

UNIMOD:59

Propionyl_heavy

59.036279

Propionate labeling reagent heavy form (+3amu), N-term & K, Propionyl:13C(3)

N-term, K

UNIMOD:60

Quat_0

127.099714

Quaternary amine labeling reagent light form (N-term & K), N-(4-trimethylammoniumbutanoxy)-NHS, GIST-Quat

N-term, K

UNIMOD:61

Quat_3

130.118544

GIST-Quat:2H(3), Quaternary amine labeling reagent heavy (+3amu) form, N-term & K

N-term, K

UNIMOD:62

Quat_6

133.137375

GIST-Quat:2H(6), Quaternary amine labeling reagent heavy form (+6amu), N-term & K

N-term, K

UNIMOD:63

Quat_9

136.156205

GIST-Quat:2H(9), Quaternary amine labeling reagent heavy form (+9amu), N-term & K

N-term, K

UNIMOD:64

Succinyl

100.016044

Succinic anhydride labeling reagent light form (N-term & K), Suc_anh_light

N-term, K

UNIMOD:65

Suc_anh+4H2

104.041151

Succinyl:2H(4), Succinic anhydride labeling reagent, heavy form (+4amu, 4H2), N-term & K

N-term, K

UNIMOD:66

Suc_anh+4C13

104.029463

Succinyl:13C(4), Succinic anhydride labeling reagent, heavy form (+4amu, 4C13), N-term & K

N-term, K

UNIMOD:89

Im_biotin

225.093583

Iminobiotin, Iminobiotinylation

N-term, K

UNIMOD:90

ESP

338.177647

ESP-Tag light d0, ESP-Tag_light

N-term, K

UNIMOD:91

ESP:2H(10)

348.240414

ESP-Tag heavy d10, ESP-Tag_heavy

N-term, K

UNIMOD:92

Biot_LC

339.161662

N-biotinyl-6-aminohexanoyl, NHS-LC-Biotin

N-term, K

UNIMOD:93

EDT-m-biotin

601.206246

EDT-maleimide-PEO-biotin

S, T

UNIMOD:94

IMID

68.037448

Lys imidazole, IMID_light, 2-methoxy-4,5-dihydro-1H-imidazole derivative, IMID d0

K

UNIMOD:95

IMID d4

72.062555

IMID:2H(4), IMID_heavy, 2-methoxy-4,5-dihydro-1H-imidazole derivative

K

UNIMOD:97

Acrylamide d3

74.055944

Propionamide:2H(3), Acrylamide_heavy

C

UNIMOD:105

ICAT-C

227.126991

Applied Biosystems cleavable ICAT(TM) light, ICAT_light

C

UNIMOD:106

ICAT_heavy

236.157185

ICAT-C:13C(9), Applied Biosystems cleavable ICAT(TM) heavy

C

UNIMOD:107

FormylMet

159.035399

Addition of N-formyl met, +N-formyl-met

N-term

UNIMOD:108

NEM

125.047679

N-ethylmaleimide on cysteines, CysNEM, Nethylmaleimide

C

UNIMOD:112

OxLysBiotinRed

354.172562

Oxidized lysine biotinylated with biotin-LC-hydrazide, reduced

K

UNIMOD:113

OxLysBiotin

352.156911

Oxidized lysine biotinylated with biotin-LC-hydrazide

K

UNIMOD:114

OxProBiotinRed

371.199111

Oxidized proline biotinylated with biotin-LC-hydrazide, reduced

P

UNIMOD:115

OxProBiotin

369.183461

Oxidized Proline biotinylated with biotin-LC-hydrazide

P

UNIMOD:116

OxArgBiotin

310.135113

Oxidized arginine biotinylated with biotin-LC-hydrazide

R

UNIMOD:117

OxArgBiotinRed

312.150763

Oxidized arginine biotinylated with biotin-LC-hydrazide, reduced

R

UNIMOD:118

EDT-i-biotin

490.174218

EDT-iodo-PEO-biotin, EDT-iodoacetyl-PEO-biotin

S, T

UNIMOD:119

IBTP

316.138088

Thio Ether Formation - BTP Adduct

C

UNIMOD:121

GG

114.042927

GlyGly, ubiquitinylation residue, glycineglycine

C, N-term, K, S, T

UNIMOD:122

Formyl

27.994915

Formylation

N-term, K, T, S

UNIMOD:123

ICAT-H

345.097915

Harbury_ICAT_C12, N-iodoacetyl, p-chlorobenzyl-12C6-glucamine, Harbury glyco-ICAT C12

C

UNIMOD:124

ICAT-H:13C(6)

351.118044

Harbury_ICAT_C13, N-iodoacetyl, p-chlorobenzyl-13C6-glucamine, Harbury glyco-ICAT C13

C

UNIMOD:126

DSP

87.998285

Cleaved and reduced DSP/DTSSP crosslinker, (Was Thioacyl), Xlink:DTSSP[88]

N-term, K

UNIMOD:127

Fphe

17.990578

fluorination, Fluoro

W, F, Y, A

UNIMOD:128

Fluorescein

387.074287

5-Iodoacetamidofluorescein (Molecular Probe, Eugene, OR)

C

UNIMOD:129

Iodo

125.896648

Iodination

Y, H

UNIMOD:130

Diiodo

251.793296

di-Iodination

Y, H

UNIMOD:131

Triiodo

377.689944

tri-Iodination

Y

UNIMOD:134

Myristoleyl

208.182715

C14:1 acylation, myristoleylation, myristoyl with one double bond, (cis-delta 5)-tetradecaenoyl

G @ Protein N-term

UNIMOD:135

myristoyl-4H

206.167065

myristoyl with 2 double bonds, (cis,cis-delta 5, delta 8)-tetradecadienoyl, Myristoyl+Delta:H(-4), C14:2 fatty acylation

G @ Protein N-term

UNIMOD:136

Benzoyl

104.026215

labeling reagent light form (N-term & K), benzoyl

N-term, K

UNIMOD:137

M5/Man5

1216.422863

N-glycan, N-linked glycan core, Hex(5)HexNAc(2)

N

UNIMOD:139

dansyl

233.051049

Dansyl, 5-dimethylaminonaphthalene-1-sulfonyl

N-term, K

UNIMOD:140

a-type-ion

-46.005479

Decarboxylation of C-terminus as reaction inside the mass spectrometer, ISD a-series (C-Term), a-type_Ion

C-term

UNIMOD:141

amidine

41.026549

N-terminal amines with methyl acetimidate, Amidine, amidination of lysines or N-terminal amines with methyl acetimidate, amidination of lysines

N-term, K

UNIMOD:142

HexNAc1dHex1

349.137281

HexNAc(1)dHex(1)

S, N, T

UNIMOD:143

HexNAc2

406.158745

HexNAc(2)

S, N, T

UNIMOD:144

Hex3

486.158471

Hex(3)

S, N, T

UNIMOD:145

HexNAc1dHex2

495.19519

HexNAc(1)dHex(2)

N

UNIMOD:146

Hex1HexNAc1dHex1

511.190105

Hex(1)HexNAc(1)dHex(1)

S, N, T

UNIMOD:147

HexNAc2dHex1

552.216654

HexNAc(2)dHex(1)

N

UNIMOD:148

Hex1HexNAc2

568.211569

Hex(1)HexNAc(2)

S, N, T

UNIMOD:149

Hex1HexNAc1NeuAc1

656.227613

Hex(1)HexNAc(1)NeuAc(1)

S, N, T

UNIMOD:150

HexNAc2dHex2

698.274563

HexNAc(2)dHex(2)

N

UNIMOD:151

Hex1HexNAc2Pent1

700.253828

Hex(1)HexNAc(2)Pent(1)

N

UNIMOD:152

Hex1HexNAc2dHex1

714.269478

Hex(1)HexNAc(2)dHex(1)

S, N, T

UNIMOD:153

Hex2HexNAc2

730.264392

Hex(2)HexNAc(2)

S, N, T

UNIMOD:154

Hex3HexNAc1Pent1

821.280102

Hex(3)HexNAc(1)Pent(1)

N

UNIMOD:155

Hex1HexNAc2dHex1Pent1

846.311736

Hex(1)HexNAc(2)dHex(1)Pent(1)

N

UNIMOD:156

Hex1HexNAc2dHex2

860.327386

Hex(1)HexNAc(2)dHex(2)

S, N, T

UNIMOD:157

Hex2HexNAc2Pent1

862.306651

Hex(2)HexNAc(2)Pent(1)

N

UNIMOD:158

Hex2HexNAc2dHex1

876.322301

Hex(2)HexNAc(2)dHex(1)

S, N, T

UNIMOD:159

M3/Man3

892.317216

Hex(3)HexNAc(2), chitobiose core, Hex3HexNAc2

S, N, T

UNIMOD:160

Hex1HexNAc1NeuAc2

947.323029

Hex HexNAc NeuAc(2) —OR— Hex HexNAc(3) HexA, Hex(1)HexNAc(1)NeuAc(2)

S, N, T

UNIMOD:161

Hex3HexNAc2Phos1

972.283547

Hex(3) HexNAc(2) Phos, Hex(3)HexNAc(2)Phos(1)

N

UNIMOD:162

SeMet

47.944449

Delta:S(-1)Se(1), Selenium replaces sulfur

M, C

UNIMOD:170

GN18Olabeling

2.988261

Delta:H(-1)N(-1)18O(1), glycosylated asparagine 18O labeling

N

UNIMOD:171

NBS-13C

159.008578

Shimadzu NBS-13C, NBS:13C(6), NBS reagent heavy

W

UNIMOD:172

NBS

152.988449

NBS-12C, Shimadzu NBS-12C, 2-nitrobenzenesulfenyl

W

UNIMOD:176

BHT

218.167065

Michael addition of BHT quinone methide to Cysteine and Lysine, Butylated Hydroxytoluene

K, H, C

UNIMOD:178

DAET

87.050655

phosphorylation to amine thiol, b-elimination thiol derivatization, ser_thr_DAET

S, T

UNIMOD:184

13C9

9.030193

Label:13C(9), 13C(9) Silac label, heavy tyrosine

F, Y

UNIMOD:185

13C9_Phospho_Tyr

88.996524

C13 label (Phosphotyrosine), heavy phosphotyrosine, Label:13C(9)+Phospho

Y

UNIMOD:186

HPG

132.021129

Hydroxyphenylglyoxal arginine, HPG arginine, Arg1HPG

R

UNIMOD:187

2HPG

282.052824

Arg2HPG, bis(hydroxphenylglyoxal) arginine

R

UNIMOD:188

13C6

6.020129

Label:13C(6), 13C(6) Silac label

I, K, R, L

UNIMOD:193

double_O18

4.008491

Double O18 (C-term), Label:18O(2), O18 label at both C-terminal oxygens

C-term

UNIMOD:194

AccQTag

170.048013

6-aminoquinolyl-N-hydroxysuccinimidyl carbamate

N-term, K

UNIMOD:195

QAT

171.149738

(3-acrylamidopropyl)trimethylammonium, APTA-d0

C

UNIMOD:196

QATd3

174.168569

(3-acrylamidopropyl)trimethylammonium, QAT:2H(3), APTA d3

C

UNIMOD:197

EQAT

184.157563

EAPTA d0

C

UNIMOD:198

EQATd5

189.188947

EAPTA d5, EQAT:2H(5)

C

UNIMOD:199

CHD2

32.056407

reductive amination, DiMethyl-CHD2, Dimethyl:2H(4)

N-term, K, R

UNIMOD:200

EDT

75.980527

Ethanedithiol

S, T

UNIMOD:205

Acrolein94

94.041865

Delta:H(6)C(6)O(1), Acrolein94, FDP, Acrolein addition +94

K

UNIMOD:206

Acrolein56

56.026215

Acrolein addition +56, Delta:H(4)C(3)O(1)

K, R, H, C

UNIMOD:207

Acrolein38

38.01565

Delta:H(2)C(3), Acrolein addition +38

K

UNIMOD:208

Acrolein76

76.0313

Delta:H(4)C(6), Acrolein addition +76

K

UNIMOD:209

Acrolein112

112.05243

Acrolein addition +112, Delta:H(8)C(6)O(2)

K

UNIMOD:211

NEIAA

85.052764

N-ethyl iodoacetamide-d0, NEIAA-d0

Y, C

UNIMOD:212

NEIAA-d5

90.084148

N-ethyl iodoacetamide-d5, NEIAA:2H(5)

Y, C

UNIMOD:213

ADP-Ribose

541.06111

ADP-Ribosyl, ADP Ribose addition

C, N, K, S, D, T, R, E

UNIMOD:214

iTRAQ

144.102063

Representative mass and accurate mass for 116 & 117, iTRAQ4plex, Applied Biosystems iTRAQ(TM) multiplexed quantitation chemistry, AKA iTRAQ4plex116/7

H, C, N-term, K, S, T, Y

UNIMOD:243

IGBP

296.016039

Light IDBEST tag for quantitation

C

UNIMOD:253

Croton

70.041865

Crotonaldehyde

K, H, C

UNIMOD:254

Acetald+26

26.01565

Delta:H(2)C(2), Acetaldehyde +26

N-term, K, H

UNIMOD:255

Acetald+28

28.0313

Delta:H(4)C(2), Acetaldehyde +28

N-term, K, H

UNIMOD:256

Propionald+40

40.0313

Propionaldehyde +40, Delta:H(4)C(3)

N-term, K, H

UNIMOD:258

O18_STY

2.004246

H2 18O Alkaline Phosphatase, Label:18O(1), O18 Labeling

S, Y, T, C-term

UNIMOD:259

13C6-15N2

8.014199

heavy lysine, Label:13C(6)15N(2), 13C(6) 15N(2) Silac label

K

UNIMOD:260

Thiophospho

95.943487

Thiophosphorylation, Thio-phospho

S, Y, T

UNIMOD:261

SPITC

214.971084

4-sulfophenyl isothiocyanate, N-terminal / lysine sulfonation

N-term, K

UNIMOD:262

D3_Label

3.01883

Trideuteration, Label:2H(3)

M, L

UNIMOD:264

PET

121.035005

phosphorylation to pyridyl thiol, b-elimination PET derivatisation

S, T

UNIMOD:267

13C6-15N4

10.008269

13C(6) 15N(4) Silac label, Label:13C(6)15N(4)

R

UNIMOD:268

13C5-15N1

6.013809

13C(5) 15N(1) Silac label, Label:13C(5)15N(1)

V, M, E, P

UNIMOD:269

13C9-15N1

10.027228

13C(9) 15N(1) Silac label, Label:13C(9)15N(1)

F

UNIMOD:270

Cytopiloyne

362.136553

nucleophilic addtion to cytopiloyne, cytopiloyne

C, N-term, K, P, S, R, Y

UNIMOD:271

cytopiloyne+H2O

380.147118

Cytopiloyne+water, nucleophilic addition to cytopiloyne+H2O

C, N-term, K, S, T, R, Y

UNIMOD:272

CAF

135.983029

Ettan CAF MALDI, sulfonation of N-terminus

N-term

UNIMOD:275

SNO

28.990164

Nitrosyl, nitrosylation

Y, C

UNIMOD:276

AEBS

183.035399

Aminoethylbenzenesulfonylation

H, N-term, K, S, Y

UNIMOD:278

EtOH

44.026215

Ethanolyl, Ethanolation, Hydroxy ethyl

K, R, C

UNIMOD:280

Ethyl

28.0313

Ethylation

N-term, C-term, K, D, E

UNIMOD:281

CysCoA

765.09956

CoenzymeA, Cysteine modified Coenzyme A

C

UNIMOD:284

DeMet

16.028204

Deuterium Methylation of Lysine, Methyl:2H(2)

N-term, K

UNIMOD:285

SA_light

155.004099

C-Terminal/Glutamate/Aspartate sulfonation, Light Sulfanilic Acid (SA) C12, SulfanilicAcid

C-term, D, E

UNIMOD:286

SA_heavy

161.024228

C-Terminal/Glutamate/Aspartate sulfonation, SulfanilicAcid:13C(6), Heavy Sulfanilic Acid (SA) C13

C-term, D, E

UNIMOD:288

oxolactone

13.979265

Tryptophan oxidation to oxolactone, Trp->Oxolactone

W

UNIMOD:289

BiotinPEO-amine

356.188212

Biotin-PEO-Amine, Biotin polyethyleneoxide amine

C-term, D, E

UNIMOD:290

Biotin-HPDP

428.191582

Biotin:Thermo-21341, Pierce EZ-Link Biotin-HPDP

C

UNIMOD:291

Hg

201.970617

Mercury Mercaptan, Delta:Hg(1)

C

UNIMOD:292

IodoU-AMP

322.020217

(Iodo)-uracil MP, UV induced linkage of Iodo-U-amp with WFY

W, F, Y

UNIMOD:293

CAMthiopropanoyl

145.019749

3-thiopropanoyl moiety from reduced DSP crosslinker, 3-thiopropanoyl moiety from reduced DSP crosslinker or NHS-SS-biotin, modified with Iodoacetamide, NHS-SS-biotin, modified with Iodoacetamide, 3-(carbamidomethylthio)propanoyl

N-term, K

UNIMOD:294

ied-biotin

326.141261

biotinoyl-iodoacetyl-ethylenediamine, IED-Biotin

C

UNIMOD:295

Fuc

146.057909

deoxyhexose, dHex, Fucose

S, N, T

UNIMOD:298

D3-Me-ester

17.03448

Methyl:2H(3), deuterated methyl ester

, K, D, R, E

UNIMOD:299

Carboxy

43.989829

Carboxylation, carboxyl

M @ Protein N-term, K, W, D, E

UNIMOD:301

Bromobimane

190.074228

mBromobimane, Monobromobimane derivative

C

UNIMOD:302

Menadione

170.036779

Menadione-Q, Vitamin k3 (Q), Menadione quinone derivative

K, C

UNIMOD:303

DeStreak

75.998285

Cysteine mercaptoethanol

C

UNIMOD:305

G0F

1444.53387

dHex(1)Hex(3)HexNAc(4), FA2/G0F

S, N, T

UNIMOD:307

G1F

1606.586693

FA2G1/G1F, dHex(1)Hex(4)HexNAc(4), dHex Hex(4) HexNAc(4) —OR— Hex(4) HexNAc(4) Pent Me

S, N, T

UNIMOD:308

G2F

1768.639517

FA2G2/G2F, dHex(1)Hex(5)HexNAc(4)

N

UNIMOD:309

G0

1298.475961

Hex(3)HexNAc(4), A2/G0

S, N, T

UNIMOD:310

G1

1460.528784

A2G1/G1, Hex(4)HexNAc(4)

S, N, T

UNIMOD:311

G2

1622.581608

A2G2/G2, Hex(5)HexNAc(4)

S, N, T

UNIMOD:312

Cysteinyl

119.004099

Cysteinylation

C

UNIMOD:313

-K

-128.094963

Loss of Lysine, Lys-loss

K @ Protein C-term, K

UNIMOD:314

NMM

111.032028

Nmethylmaleimide

K, C

UNIMOD:316

pyrrole

78.04695

DimethylpyrroleAdduct, 2,5-dimethypyrrole

K

UNIMOD:318

MDA62

62.01565

MDA adduct +62, Delta:H(2)C(5)

K

UNIMOD:319

MDA54

54.010565

MDA adduct +54, Delta:H(2)C(3)O(1)

K, R

UNIMOD:320

NEMhyd

143.058243

Nethylmaleimide+water, Nethylmaleimidehydrolysis

K, C

UNIMOD:323

bNis

713.093079

Xlink:B10621, BSR monolink, bis-((N-iodoacetyl)piperazinyl)sulfonerhodamine

C

UNIMOD:324

DTBP

87.01427

Cleaved and reduced DTBP crosslinker, Xlink:DTBP[87], dimethyl 3,3’-dithiobispropionimidate

N-term, K

UNIMOD:325

FP-Biotin

572.316129

O-ethyl-N-(biotinamidopentyl)decanamido phosphonate, 10-ethoxyphosphinyl-N-(biotinamidopentyl)decanamide

S, T, K, Y

UNIMOD:327

S-Eth

44.008456

Delta:H(4)C(2)O(-1)S(1), S-Ethylcystine from Serine

S

UNIMOD:329

Methyl-Heavy

18.037835

Methyl:2H(3)13C(1), monomethylation

N-term, K, R

UNIMOD:330

dimethylation

36.07567

Dimethyl:2H(6)13C(2), Dimethyl-Heavy

N-term, K, R

UNIMOD:332

Thiophos-S-S-biotin

525.142894

thiophosphate labeled with biotin-HPDP, Thiophos-biotin disulfide

S, Y, T

UNIMOD:333

Can-FP-biotin

447.195679

O-isopropyl-N-biotinylaminohexyl phosphonate, 6-N-biotinylaminohexyl isopropyl phosphate, Biotin:TRC-B394885

S, T, Y

UNIMOD:335

redHNE

158.13068

reduced 4-Hydroxynonenal, HNE+Delta:H(2)

K, H, C

UNIMOD:337

Methylamine

13.031634

MethylamineST, Michael addition with methylamine

S, T

UNIMOD:340

bromo

77.910511

Bromo, bromination

W, F, Y, H

UNIMOD:342

Amino

15.010899

Tyrosine oxidation to 2-aminotyrosine, aminotyrosine

Y

UNIMOD:343

Argbiotinhydrazide

199.066699

oxidized Arginine biotinylated with biotin hydrazide

R

UNIMOD:344

Arg->GluSA

-43.053433

Arginine oxidation to glutamic semialdehyde, Arginine oxidation to gamma-glutamyl semialdehyde, Argglutamicsealde

R

UNIMOD:345

Cysteic_acid

47.984744

Trioxidation, cysteine oxidation to cysteic acid

W, F, Y, C

UNIMOD:348

His2Asn

-23.015984

His->Asn, His->Asn substitution, histidine oxidation to aspargine

H

UNIMOD:349

His2Asp

-22.031969

His->Asp substitution, histidine oxidation to aspartic acid, His->Asp

H

UNIMOD:350

hydroxykynurenin

19.989829

tryptophan oxidation to hydroxykynurenin, Trp->Hydroxykynurenin

W

UNIMOD:351

kynurenin

3.994915

Trp->Kynurenin, tryptophan oxidation to kynurenin

W

UNIMOD:352

Lys->Allysine

-1.031634

Lysaminoadipicsealde, Lysine oxidation to aminoadipic semialdehyde

K

UNIMOD:353

Lysbiotinhydrazide

241.088497

oxidized Lysine biotinylated with biotin hydrazide

K

UNIMOD:354

Nitro

44.985078

Oxidation to nitro

W, F, Y

UNIMOD:357

probiotinhydrazide

258.115047

oxidized proline biotinylated with biotin hydrazide

P

UNIMOD:359

Pyroglutamic

13.979265

proline oxidation to pyroglutamic acid, Pro->pyro-Glu

P

UNIMOD:360

Pyrrolidinone

-30.010565

Proline oxidation to pyrrolidinone, Pro->Pyrrolidinone

P

UNIMOD:361

Thrbiotinhydrazide

240.104482

oxidized Threonine biotinylated with biotin hydrazide

T

UNIMOD:362

Diisopropylphosphate

164.060231

O-Diisopropylphosphate, O-Diisopropylphosphorylation

N-term, K, S, T, Y

UNIMOD:363

Isopropylphospho

122.013281

O-Isopropylphosphorylation, O-Isopropylphosphate

S, Y, T

UNIMOD:364

ICPL_heavy

111.041593

ICPL:13C(6), Bruker Daltonics SERVA-ICPL(TM) quantification chemistry, heavy form

N-term, K

UNIMOD:365

ICPL

105.021464

Bruker Daltonics SERVA-ICPL(TM) quantification chemistry, light form, ICPL_light

N-term, K

UNIMOD:366

Deamidation_O18

2.988261

Deamidation in presence of O18, Deamidated:18O(1)

N, Q

UNIMOD:368

Cys->Dha

-33.987721

DehydroalaC, Dehydroalanine (from Cysteine)

C

UNIMOD:369

OxPro

-27.994915

Pyrrolidone from Proline, Pro->Pyrrolidone

P

UNIMOD:371

HMVK

86.036779

hydroxymethylvinyl ketone, Michael addition of hydroxymethylvinyl ketone to cysteine, HMVK86

C

UNIMOD:372

Arg2Orn

-42.021798

Arg->Orn, Ornithine from Arginine

R

UNIMOD:374

Cystine

-1.007825

Dehydro, Half of a disulfide bridge

C

UNIMOD:375

Diphthamide

142.110613

H

UNIMOD:376

Hydroxyfarnesyl

220.182715

hydroxyfarnesyl

C

UNIMOD:377

Diacylglycerol

576.511761

diacylglycerol

C

UNIMOD:378

Carboxyethyl

72.021129

Ethoxyformylation, carboxyethyl

K, H

UNIMOD:379

hypusine

87.068414

Hypusine

K

UNIMOD:380

retinal

266.203451

Retinylidene

K

UNIMOD:381

aminoadipic

14.96328

alpha-amino adipic acid, Lys->AminoadipicAcid

K

UNIMOD:382

pyruvicC

-33.003705

pyruvic acid from N-term cys, Cys->PyruvicAcid

C @ Protein N-term

UNIMOD:385

Ammonia-loss

-17.026549

Loss of ammonia, N-oxobutanoic, oxobutanoic acid from N term Thr, pyruvic acid from N-term ser

S @ Protein N-term, N, T @ Protein N-term, C @ N-term

UNIMOD:387

phycobiliviolin

586.279135

Phycocyanobilin, phycocyanobilin

C

UNIMOD:388

phycoerythrobilin

588.294785

Phycoerythrobilin

C

UNIMOD:389

Phytochromobilin

584.263485

phytochromobilin

C

UNIMOD:390

Heme

616.177295

heme

H, C

UNIMOD:391

Molybdopterin

521.884073

molybdopterin

C

UNIMOD:392

quinone

29.974179

Quinone

W, Y

UNIMOD:393

glucosylgalactosyl

340.100562

glucosylgalactosyl hydroxylysine, Glucosylgalactosyl

K

UNIMOD:394

GPIanchor

123.00853

GPI-anchor, glycosylphosphatidylinositol

C-term

UNIMOD:395

pRibodePcoA

881.146904

PhosphoribosyldephosphoCoA, phosphoribosyl dephospho-coenzyme A

S

UNIMOD:396

GlycerylPE

197.04531

glycerylPE, glycerylphosphorylethanolamine

E

UNIMOD:397

triiodo

469.716159

Triiodothyronine

Y

UNIMOD:398

tetraiodo

595.612807

Thyroxine

Y

UNIMOD:400

Tyr->Dha

-94.041865

Dehydroalanine (from Tyrosine), DehydroalaY

Y

UNIMOD:401

didehydro

-2.01565

2-amino-3-oxo-butanoic_acid, oxoalanine, Didehydro

S, T, K @ C-term, Y

UNIMOD:402

oxoalanine

-17.992806

Cys->Oxoalanine, formylglycine

C

UNIMOD:403

lactic

-15.010899

Ser->LacticAcid, lactic acid from N-term Ser

S @ Protein N-term

UNIMOD:405

AMP

329.05252

p-adenosine, Phosphoadenosine

H, K, S, T, Y

UNIMOD:407

Hydroxycinnamyl

146.036779

hydroxycinnamyl

C

UNIMOD:408

Glycosyl

148.037173

glycosyl-L-hydroxyproline, glycosyl

P

UNIMOD:409

FMN

454.088965

flavin mononucleotide, FMNH

H, C

UNIMOD:410

Archaeol

634.662782

S-archaeol, S-diphytanylglycerol diether

C

UNIMOD:411

PIC

119.037114

Phenylisocyanate, phenyl isocyanate

N-term

UNIMOD:412

d5-PIC

124.068498

d5-phenyl isocyanate, Phenylisocyanate:2H(5)

N-term

UNIMOD:413

p-guanosine

345.047435

Phosphoguanosine, phospho-guanosine

K, H

UNIMOD:414

hydroxymethyl

30.010565

Hydroxymethyl

N

UNIMOD:415

molybdopterin-se

1620.930224

MolybdopterinGD+Delta:S(-1)Se(1), L-selenocysteinyl molybdenum bis(molybdopterin guanine dinucleotide)

C

UNIMOD:416

dipyrrole

418.137616

Dipyrrolylmethanemethyl, dipyrrolylmethanemethyl

C

UNIMOD:417

p-uridine

306.025302

PhosphoUridine, uridine phosphodiester

Y, H

UNIMOD:419

glycerophospho

154.00311

Glycerophospho

S

UNIMOD:420

thiocarboxy

15.977156

Carboxy->Thiocarboxy, thiocarboxylic acid

G @ Protein C-term

UNIMOD:421

Sulfide

31.972071

persulfide

D, W, C

UNIMOD:422

pyruv-iminyl

70.005479

N-pyruvic acid 2-iminyl, PyruvicAcidIminyl

V @ Protein N-term, K, C @ Protein N-term

UNIMOD:423

selenyl

79.91652

Delta:Se(1)

C

UNIMOD:424

MolybdopterinGD

1572.985775

molybdopterin-gd, molybdenum bis(molybdopterin guanine dinucleotide)

D, U, C

UNIMOD:425

dihydroxy

31.989829

Dioxidation

C, M, V, K, U, P, F, W, R, E, I, Y, L

UNIMOD:426

octanoyl

126.104465

Octanoyl

S, T, C

UNIMOD:428

p-GlcNAc

283.045704

N-acetylglucosamine-1-phosphoryl, PhosphoHexNAc

S, T

UNIMOD:429

p-Man

242.019154

phosphoglycosyl-D-mannose-1-phosphoryl, PhosphoHex

S, T

UNIMOD:431

palmitoleyl

236.214016

Palmitoleyl

S, T, C

UNIMOD:432

Cholesterol

368.344302

cholesterol ester, C-cholesterol

C-term

UNIMOD:433

didehydroretinyl

264.187801

Didehydroretinylidene, 3,4-didehydroretinylidene

K

UNIMOD:434

CHDH

294.183109

cis-14-hydroxy-10,13-dioxo-7-heptadecenoic ester, LTP1-lipid

D

UNIMOD:435

me-pyrroline

109.052764

4-methyl-delta-1-pyrroline-5-carboxyl, Methylpyrroline

K

UNIMOD:436

hydroxyheme

614.161645

Hydroxyheme

E

UNIMOD:437

MicrocinC7

386.110369

(3-aminopropyl)(L-aspartyl-1-amino)phosphoryl-5-adenosine, C-Asn-deriv

C-term

UNIMOD:438

cyano

24.995249

Cyano

C

UNIMOD:439

Fe-cluster

342.786916

hydrogenase diiron subcluster, Diironsubcluster

C

UNIMOD:440

amidino

42.021798

Amidino

C

UNIMOD:443

FMNC

456.104615

FMN3, S-(4a-FMN)

C

UNIMOD:444

CuSMo

922.834855

copper sulfido molybdopterin cytosine dinuncleotide

C

UNIMOD:445

trimethyl-OH

59.04969

Hydroxytrimethyl, 5-hydroxy-N6,N6,N6-trimethyl

K

UNIMOD:447

Deoxy

-15.994915

Threonine to a-aminobutyrate, reduction, Serine to Alanine, semialdehyde

D, S, T

UNIMOD:448

Microcin

831.197041

microcin, microcin E492 siderophore ester from serine

C-term

UNIMOD:449

lipid

154.135765

decanoyl, Decanoyl

S, T

UNIMOD:450

Glu

129.042593

-Glu-, monoglutamyl

C-term, E

UNIMOD:451

GluGlu

258.085186

-Glu-Glu-, diglutamyl

C-term, E

UNIMOD:452

GluGluGlu

387.127779

triglutamyl, -Glu-Glu-Glu-

C-term, E

UNIMOD:453

GluGluGluGlu

516.170373

-Glu-Glu-Glu-Glu-, tetraglutamyl

C-term, E

UNIMOD:454

HexN

161.068808

Hexosamine, Galactosamine, Glucosamine

N, K, S, W, T

UNIMOD:455

DMP-s

154.110613

Dimethyl pimelimidate, Xlink:DMP[154], Free monolink of DMP crosslinker

N-term, K

UNIMOD:456

Xlink:DMP

122.084398

Dimethyl pimelimidate crosslink

N-term, K

UNIMOD:457

NDA

175.042199

naphthalene-2,3-dicarboxaldehyde

N-term, K

UNIMOD:464

SPITC_Heavy

220.991213

4-sulfophenyl isothiocyanate (Heavy C13), SPITC:13C(6), N-terminal / lysine sulfonation

N-term, K

UNIMOD:472

AEC-MAEC

59.019355

aminoethylcysteine, beta-methylaminoethylcysteine

S, T

UNIMOD:476

TMAB

128.107539

TMAB-d0, 4-trimethyllammoniumbutyryl-

N-term, K

UNIMOD:477

TMAB-d9

137.16403

TMAB:2H(9), d9-4-trimethyllammoniumbutyryl-

N-term, K

UNIMOD:478

FTC

421.073241

fluorescein-5-thiosemicarbazide

C, K, P, S, R

UNIMOD:481

Lys4

4.025107

4,4,5,5-D4 Lysine, Label:2H(4), Alanine-2,3,3,3-d4

K, U, A, F, Y

UNIMOD:488

DHP

118.065674

Dehydropyrrolizidine alkaloid (dehydroretronecine) on cysteines, Dehydroretronecine

C

UNIMOD:490

Hep

192.063388

Heptose

N, K, S, T, R, Q

UNIMOD:493

BADGE

340.167459

Bisphenol A diglycidyl ether derivative

C

UNIMOD:494

CyDye-Cy3

672.298156

Formula needs confirmation, Cy3 CyDye DIGE Fluor saturation dye

C

UNIMOD:495

CyDye-Cy5

684.298156

Cy5 CyDye DIGE Fluor saturation dye

C

UNIMOD:498

K

234.16198

BHTOH, Michael addition of t-butyl hydroxylated BHT (BHTOH) to C, H, t-butyl hydroxylated BHT, Michael addition of t-butyl hydroxylated BHT (BHTOH) to C, H or K

, H, C

UNIMOD:499

IGBP:13C(2)

298.022748

Heavy IDBEST tag for quantitation

C

UNIMOD:500

NMMhyd

129.042593

Nmethylmaleimidehydrolysis, Nmethylmaleimide+water

C

UNIMOD:501

PyMIC

134.048013

3-methyl-2-pyridyl isocyanate

N-term

UNIMOD:503

LG-lactam-K

332.19876

Levuglandinyl - lysine lactam adduct

N-term, K

UNIMOD:504

LG-Hlactam-K

348.193674

Levuglandinyl - lysine hydroxylactam adduct

N-term, K

UNIMOD:505

LG-lactam-R

290.176961

Levuglandinyl - arginine lactam adduct

R

UNIMOD:506

LG-Hlactam-R

306.171876

Levuglandinyl - arginine hydroxylactam adduct

R

UNIMOD:510

C13HD2

34.063117

Dimethyl:2H(4)13C(2), DiMethyl-C13HD2

N-term, K, R

UNIMOD:512

lac

324.105647

Hex(2), Gal-Glu, Lactosylation

S, K, T, R

UNIMOD:513

C8-QAT

227.224915

[3-(2,5)-Dioxopyrrolidin-1-yloxycarbonyl)-propyl]dimethyloctylammonium

N-term, K

UNIMOD:514

PropylNAGthiazoline

232.064354

propyl-NAG-thiazoline, propyl-1,2-dideoxy-2'-methyl-alpha-D-glucopyranoso-[2,1-d]-Delta2'-thiazoline

C

UNIMOD:515

FNEM

427.069202

F-NEM, fluorescein-5-maleimide, fluorescein-NEM

C

UNIMOD:518

Diethyl

56.0626

Diethylation, analogous to Dimethylation, Diethylation

N-term, K

UNIMOD:519

BisANS

594.091928

4,4'-dianilino-1,1'-binaphthyl-5,5'-disulfonic acid

K

UNIMOD:520

Piperidine

68.0626

Piperidination

N-term, K

UNIMOD:522

Maleimide-Biotin

525.225719

Maleimide-PEO2-Biotin, Maleimide-PEG2-Biotin, Biotin:Thermo-21901

C

UNIMOD:523

Biot_LC_LC

452.245726

Biotin:Thermo-21338, Sulfo-NHS-LC-LC-Biotin

N-term, K

UNIMOD:525

CLIP_TRAQ_2

141.098318

CLIP_TRAQ_double

N-term, K, Y

UNIMOD:526

Dethiomethyl

-48.003371

Prompt loss of side chain from oxidised Met

M

UNIMOD:528

Methyl+Deamidated

14.999666

Deamidation followed by a methylation, Glutamate methyl ester

Q, N

UNIMOD:529

Delta:H(5)C(2)

29.039125

Dimethylation of proline residue

P

UNIMOD:530

Cation:K

37.955882

Replacement of proton by potassium

D, C-term, E

UNIMOD:531

Cation:Cu

61.921774

Cation:Cu[I], Replacement of proton by copper

D, C-term, E, H

UNIMOD:532

iTRAQ114

144.105918

Accurate mass for 114, Applied Biosystems iTRAQ(TM) multiplexed quantitation chemistry, iTRAQ4plex114

N-term, K, Y, C

UNIMOD:533

iTRAQ115

144.099599

iTRAQ4plex115, Accurate mass for 115, Applied Biosystems iTRAQ(TM) multiplexed quantitation chemistry

N-term, K, Y, C

UNIMOD:534

Dibromo

155.821022

Y

UNIMOD:535

LRGG

383.228103

LeuArgGlyGly, Addition of LRGG, Ubiquitination

K

UNIMOD:536

CLIP_TRAQ_3

271.148736

N-term, K, Y

UNIMOD:537

CLIP_TRAQ_4

244.101452

N-term, K, Y

UNIMOD:538

was 15dB-biotin

626.386577

15-deoxy-delta 12,14-Prostaglandin J2-biotinimide, Biotin:Cayman-10141

C

UNIMOD:539

was PGA1-biotin

660.428442

Biotin:Cayman-10013, Prostaglandin A1-biotinimide

C

UNIMOD:540

Ala->Ser

15.994915

Ala->Ser substitution

A

UNIMOD:541

Ala->Thr

30.010565

Ala->Thr substitution

A

UNIMOD:542

Ala->Asp

43.989829

Ala->Asp substitution

A

UNIMOD:543

Ala->Pro

26.01565

Ala->Pro substitution

A

UNIMOD:544

Ala->Gly

-14.01565

Ala->Gly substitution

A

UNIMOD:545

Ala->Glu

58.005479

Ala->Glu substitution

A

UNIMOD:546

Ala->Val

28.0313

Ala->Val substitution

A

UNIMOD:547

Cys->Phe

44.059229

Cys->Phe substitution

C

UNIMOD:548

Cys->Ser

-15.977156

Cys->Ser substitution

C

UNIMOD:549

Cys->Trp

83.070128

Cys->Trp substitution

C

UNIMOD:550

Cys->Tyr

60.054144

Cys->Tyr substitution

C

UNIMOD:551

Cys->Arg

53.091927

Cys->Arg substitution

C

UNIMOD:552

Cys->Gly

-45.987721

Cys->Gly substitution

C

UNIMOD:553

Asp->Ala

-43.989829

Asp->Ala substitution

D

UNIMOD:554

Asp->His

22.031969

Asp->His substitution

D

UNIMOD:555

Asp->Asn

-0.984016

Asp->Asn substitution

D

UNIMOD:556

Asp->Gly

-58.005479

Asp->Gly substitution

D

UNIMOD:557

Asp->Tyr

48.036386

Asp->Tyr substitution

D

UNIMOD:558

Asp->Glu

14.01565

Asp->Glu substitution

D

UNIMOD:559

Asp->Val

-15.958529

Asp->Val substitution

D

UNIMOD:560

Glu->Ala

-58.005479

Glu->Ala substitution

E

UNIMOD:561

Glu->Gln

-0.984016

Glu->Gln substitution

E

UNIMOD:562

Glu->Asp

-14.01565

Glu->Asp substitution

E

UNIMOD:563

Glu->Lys

-0.94763

Glu->Lys substitution

E

UNIMOD:564

Glu->Gly

-72.021129

Glu->Gly substitution

E

UNIMOD:565

Glu->Val

-29.974179

Glu->Val substitution

E

UNIMOD:566

Phe->Ser

-60.036386

Phe->Ser substitution

F

UNIMOD:567

Phe->Cys

-44.059229

Phe->Cys substitution

F

UNIMOD:568

Phe->Xle

-33.98435

Phe->Leu/Ile substitution

F

UNIMOD:569

Phe->Tyr

15.994915

Phe->Tyr substitution

F

UNIMOD:570

Phe->Val

-48.0

Phe->Val substitution

F

UNIMOD:571

Gly->Ala

14.01565

Gly->Ala substitution

G

UNIMOD:572

Gly->Ser

30.010565

Gly->Ser substitution

G

UNIMOD:573

Gly->Trp

129.057849

Gly->Trp substitution

G

UNIMOD:574

Gly->Glu

72.021129

Gly->Glu substitution

G

UNIMOD:575

Gly->Val

42.04695

Gly->Val substitution

G

UNIMOD:576

Gly->Asp

58.005479

Gly->Asp substitution

G

UNIMOD:577

Gly->Cys

45.987721

Gly->Cys substitution

G

UNIMOD:578

Gly->Arg

99.079647

Gly->Arg substitution

G

UNIMOD:580

His->Pro

-40.006148

His->Pro substitution

H

UNIMOD:581

His->Tyr

26.004417

His->Tyr substitution

H

UNIMOD:582

His->Gln

-9.000334

His->Gln substitution

H

UNIMOD:584

His->Arg

19.042199

His->Arg substitution

H

UNIMOD:585

His->Xle

-23.974848

His->Leu/Ile substitution

H

UNIMOD:588

Xle->Thr

-12.036386

Leu/Ile->Thr substitution

I, L

UNIMOD:589

Xle->Asn

0.958863

Leu/Ile->Asn substitution

I, L

UNIMOD:590

Xle->Lys

15.010899

Leu/Ile->Lys substitution

I, L

UNIMOD:594

Lys->Thr

-27.047285

Lys->Thr substitution

K

UNIMOD:595

Lys->Asn

-14.052036

Lys->Asn substitution

K

UNIMOD:596

Lys->Glu

0.94763

Lys->Glu substitution

K

UNIMOD:597

Lys->Gln

-0.036386

Lys->Gln substitution

K

UNIMOD:598

Lys->Met

2.945522

Lys->Met substitution

K

UNIMOD:599

Lys->Arg

28.006148

Lys->Arg substitution

K

UNIMOD:600

Lys->Xle

-15.010899

Lys->Leu/Ile substitution

K

UNIMOD:601

Xle->Ser

-26.052036

Leu/Ile->Ser substitution

I, L

UNIMOD:602

Xle->Phe

33.98435

Leu/Ile->Phe substitution

I, L

UNIMOD:603

Xle->Trp

72.995249

Leu/Ile->Trp substitution

I, L

UNIMOD:604

Xle->Pro

-16.0313

Leu/Ile->Pro substitution

I, L

UNIMOD:605

Xle->Val

-14.01565

Leu/Ile->Val substitution

I, L

UNIMOD:606

Xle->His

23.974848

Leu/Ile->His substitution

I, L

UNIMOD:607

Xle->Gln

14.974514

Leu/Ile->Gln substitution

I, L

UNIMOD:608

Xle->Met

17.956421

Leu/Ile->Met substitution

I, L

UNIMOD:609

Xle->Arg

43.017047

Leu/Ile->Arg substitution

I, L

UNIMOD:610

Met->Thr

-29.992806

Met->Thr substitution

M

UNIMOD:611

Met->Arg

25.060626

Met->Arg substitution

M

UNIMOD:613

Met->Lys

-2.945522

Met->Lys substitution

M

UNIMOD:614

Met->Xle

-17.956421

Also Met->norleucine, Met->Leu/Ile substitution

M

UNIMOD:615

Met->Val

-31.972071

Met->Val substitution

M

UNIMOD:616

Asn->Ser

-27.010899

Asn->Ser substitution

N

UNIMOD:617

Asn->Thr

-12.995249

Asn->Thr substitution

N

UNIMOD:618

Asn->Lys

14.052036

Asn->Lys substitution

N

UNIMOD:619

Asn->Tyr

49.020401

Asn->Tyr substitution

N

UNIMOD:620

Asn->His

23.015984

Asn->His substitution

N

UNIMOD:621

Asn->Asp

0.984016

Asn->Asp substitution

N

UNIMOD:622

Asn->Xle

-0.958863

Asn->Leu/Ile substitution

N

UNIMOD:623

Pro->Ser

-10.020735

Pro->Ser substitution

P

UNIMOD:624

Pro->Ala

-26.01565

Pro->Ala substitution

P

UNIMOD:625

Pro->His

40.006148

Pro->His substitution

P

UNIMOD:626

Pro->Gln

31.005814

Pro->Gln substitution

P

UNIMOD:627

Pro->Thr

3.994915

Pro->Thr substitution

P

UNIMOD:628

Pro->Arg

59.048347

Pro->Arg substitution

P

UNIMOD:629

Pro->Xle

16.0313

Pro->Leu/Ile substitution

P

UNIMOD:630

Gln->Pro

-31.005814

Gln->Pro substitution

Q

UNIMOD:631

Gln->Lys

0.036386

Gln->Lys substitution

Q

UNIMOD:632

Gln->Glu

0.984016

Gln->Glu substitution

Q

UNIMOD:633

Gln->His

9.000334

Gln->His substitution

Q

UNIMOD:634

Gln->Arg

28.042534

Gln->Arg substitution

Q

UNIMOD:635

Gln->Xle

-14.974514

Gln->Leu/Ile substitution

Q

UNIMOD:636

Arg->Ser

-69.069083

Arg->Ser substitution

R

UNIMOD:637

Arg->Trp

29.978202

Arg->Trp substitution

R

UNIMOD:638

Arg->Thr

-55.053433

Arg->Thr substitution

R

UNIMOD:639

Arg->Pro

-59.048347

Arg->Pro substitution

R

UNIMOD:640

Arg->Lys

-28.006148

Arg->Lys substitution

R

UNIMOD:641

Arg->His

-19.042199

Arg->His substitution

R

UNIMOD:642

Arg->Gln

-28.042534

Arg->Gln substitution

R

UNIMOD:643

Arg->Met

-25.060626

Arg->Met substitution

R

UNIMOD:644

Arg->Cys

-53.091927

Arg->Cys substitution

R

UNIMOD:645

Arg->Xle

-43.017047

Arg->Leu/Ile substitution

R

UNIMOD:646

Arg->Gly

-99.079647

Arg->Gly substitution

R

UNIMOD:647

Ser->Phe

60.036386

Ser->Phe substitution

S

UNIMOD:648

Ser->Ala

-15.994915

Ser->Ala substitution

S

UNIMOD:649

Ser->Trp

99.047285

Ser->Trp substitution

S

UNIMOD:650

Ser->Thr

14.01565

Ser->Thr substitution

S

UNIMOD:651

Ser->Asn

27.010899

Ser->Asn substitution

S

UNIMOD:652

Ser->Pro

10.020735

Ser->Pro substitution

S

UNIMOD:653

Ser->Tyr

76.0313

Ser->Tyr substitution

S

UNIMOD:654

Ser->Cys

15.977156

Ser->Cys substitution

S

UNIMOD:655

Ser->Arg

69.069083

Ser->Arg substitution

S

UNIMOD:656

Ser->Xle

26.052036

Ser->Leu/Ile substitution

S

UNIMOD:657

Ser->Gly

-30.010565

Ser->Gly substitution

S

UNIMOD:658

Thr->Ser

-14.01565

Thr->Ser substitution

T

UNIMOD:659

Thr->Ala

-30.010565

Thr->Ala substitution

T

UNIMOD:660

Thr->Asn

12.995249

Thr->Asn substitution

T

UNIMOD:661

Thr->Lys

27.047285

Thr->Lys substitution

T

UNIMOD:662

Thr->Pro

-3.994915

Thr->Pro substitution

T

UNIMOD:663

Thr->Met

29.992806

Thr->Met substitution

T

UNIMOD:664

Thr->Xle

12.036386

Thr->Leu/Ile substitution

T

UNIMOD:665

Thr->Arg

55.053433

Thr->Arg substitution

T

UNIMOD:666

Val->Phe

48.0

Val->Phe substitution

V

UNIMOD:667

Val->Ala

-28.0313

Val->Ala substitution

V

UNIMOD:668

Val->Glu

29.974179

Val->Glu substitution

V

UNIMOD:669

Val->Met

31.972071

Val->Met substitution

V

UNIMOD:670

Val->Asp

15.958529

Val->Asp substitution

V

UNIMOD:671

Val->Xle

14.01565

Val->Leu/Ile substitution

V

UNIMOD:672

Val->Gly

-42.04695

Val->Gly substitution

V

UNIMOD:673

Trp->Ser

-99.047285

Trp->Ser substitution

W

UNIMOD:674

Trp->Cys

-83.070128

Trp->Cys substitution

W

UNIMOD:675

Trp->Arg

-29.978202

Trp->Arg substitution

W

UNIMOD:676

Trp->Gly

-129.057849

Trp->Gly substitution

W

UNIMOD:677

Trp->Xle

-72.995249

Trp->Leu/Ile substitution

W

UNIMOD:678

Tyr->Phe

-15.994915

Tyr->Phe substitution

Y

UNIMOD:679

Tyr->Ser

-76.0313

Tyr->Ser substitution

Y

UNIMOD:680

Tyr->Asn

-49.020401

Tyr->Asn substitution

Y

UNIMOD:681

Tyr->His

-26.004417

Tyr->His substitution

Y

UNIMOD:682

Tyr->Asp

-48.036386

Tyr->Asp substitution

Y

UNIMOD:683

Tyr->Cys

-60.054144

Tyr->Cys substitution

Y

UNIMOD:684

BDMAPP

253.010225

Mass Defect Tag on lysine e-amino

H, N-term, K, W, Y

UNIMOD:685

NA-LNO2

325.225309

Michael addition of nitro-linoleic acid to Cys and His, Nitroalkylation by Nitro Linoleic Acid

H, C

UNIMOD:686

NA-OA-NO2

327.240959

Nitroalkylation by Nitro Oleic Acid, Michael addition of nitro-oleic acid to Cys and His

H, C

UNIMOD:687

ICPL:2H(4)

109.046571

ICPL_medium, Bruker Daltonics SERVA-ICPL(TM) quantification chemistry, medium form

N-term, K

UNIMOD:695

Label:13C(6)15N(1)

7.017164

13C(6) 15N(1) Silac label

I, L

UNIMOD:696

13C6-15N2-D9

17.07069

Label:2H(9)13C(6)15N(2), 13C(6) 15N(2) (D)9 SILAC label, heavy D9 lysine

K

UNIMOD:697

NIC

105.021464

Nicotinic Acid

N-term, K

UNIMOD:698

dNIC

109.048119

deuterated Nicotinic Acid

N-term, K

UNIMOD:720

HNE-Delta:H(2)O

138.104465

Dehydrated 4-hydroxynonenal

K, H, C

UNIMOD:721

4-ONE

154.09938

4-Oxononenal (ONE)

K, H, C

UNIMOD:723

O-Dimethylphosphate

107.997631

O-Dimethylphosphorylation

S, Y, T

UNIMOD:724

O-Methylphosphate

93.981981

O-Methylphosphorylation

S, Y, T

UNIMOD:725

Diethylphosphate

136.028931

O-Diethylphosphorylation

H, C, N-term, K, S, T, Y

UNIMOD:726

Ethylphosphate

107.997631

O-Ethylphosphorylation

N-term, K, S, T, Y

UNIMOD:727

O-pinacolylmethylphosphonate

162.080967

O-pinacolylmethylphosphonylation

H, K, S, T, Y

UNIMOD:728

Methylphosphonate

77.987066

Methylphosphonylation

S, Y, T

UNIMOD:729

O-Isopropylmethylphosphonate

120.034017

O-Isopropylmethylphosphonylation

S, Y, T

UNIMOD:730

iTRAQ8plex

304.20536

AKA iTRAQ8plex:13C(7)15N(1), Applied Biosystems iTRAQ(TM) multiplexed quantitation chemistry, Representative mass and accurate mass for 113, 114, 116 & 117

S, T, H, C, N-term, K, Y

UNIMOD:731

iTRAQ8plex:13C(6)15N(2)

304.19904

Applied Biosystems iTRAQ(TM) multiplexed quantitation chemistry, Accurate mass for 115, 118, 119 & 121

N-term, K, Y, C

UNIMOD:734

Ethanolamine

43.042199

Carboxyl modification with ethanolamine

C-term, D, E, C

UNIMOD:735

BEMAD_ST

136.001656

T followed by Michael addition of DTT, Beta elimination of modified S or T followed by Michael addition of DTT, Was DTT_ST, Beta elimination of modified S, threo-1,4-dimercaptobutane-2,3-diol

S, T

UNIMOD:736

BEMAD_C

120.0245

Was DTT_C, Beta elimination of alkylated Cys followed by Michael addition of DTT

C

UNIMOD:737

TMT6plex

229.162932

Tandem Mass Tag® and TMT® are registered Trademarks of Proteome Sciences plc., Tandem Mass Tag sixplex labelling kit Proteome Sciences, Sixplex Tandem Mass Tag®, This is a nominal. representative mass, Also applies to TMT10plex

H, N-term, K, S, T

UNIMOD:738

TMT2plex

225.155833

Duplex Tandem Mass Tag®, Tandem Mass Tag® and TMT® are registered Trademarks of Proteome Sciences plc., Tandem Mass Tag Duplex labelling kit Proteome Sciences

H, N-term, K, S, T

UNIMOD:739

TMT

224.152478

Native Tandem Mass Tag®, Tandem Mass Tag® and TMT® are registered Trademarks of Proteome Sciences plc.

H, N-term, K, S, T

UNIMOD:740

ExacTagThiol

972.365219

ExacTag Thiol label mass for 2-4-7-10 plex, PerkinElmer ExacTag Thiol kit

C

UNIMOD:741

ExacTagAmine

1046.347854

ExacTag Amine label mass for 2-4-7-10 plex, PerkinElmer ExacTag Amine kit

K

UNIMOD:743

4-ONE+Delta:H(-2)O(-1)

136.088815

Dehydrated 4-Oxononenal Michael adduct

K, H, C

UNIMOD:744

NO_SMX_SEMD

251.036462

Nitroso Sulfamethoxazole Sulphenamide thiol adduct

C

UNIMOD:746

NO_SMX_SIMD

267.031377

Nitroso Sulfamethoxazole Sulfinamide thiol adduct

C

UNIMOD:747

Malonyl

86.000394

Malonylation

S, K, C

UNIMOD:748

3sulfo

183.983029

derivatization by N-term modification using 3-Sulfobenzoic succinimidyl ester

N-term

UNIMOD:750

trifluoro

53.971735

trifluoroleucine replacement of leucine

L

UNIMOD:751

TNBS

210.986535

tri nitro benzene

N-term, K

UNIMOD:762

IDEnT

214.990469

Isotope Distribution Encoded Tag, 2,4-dichlorobenzylcarbamidomethyl

C

UNIMOD:763

BEMAD_ST:2H(6)

142.039317

Beta elimination of modified S, Was DTT_ST:2H(6), T followed by Michael addition of labelled DTT, Beta elimination of modified S or T followed by Michael addition of labelled DTT

S, T

UNIMOD:764

BEMAD_C:2H(6)

126.062161

Beta elimination of alkylated Cys followed by Michael addition of labelled DTT, Isotopically labeled Dithiothreitol (DTT) modification of cysteines, Was DTT_C:2H(6)

C

UNIMOD:765

Met-loss

-131.040485

Removal of initiator methionine from protein N-terminus

M @ Protein N-term

UNIMOD:766

Met-loss+Acetyl

-89.02992

Removal of initiator methionine from protein N-terminus, then acetylation of the new N-terminus

M @ Protein N-term

UNIMOD:767

Menadione-HQ

172.05243

Menadione hydroquinone derivative

K, C

UNIMOD:768

Methyl+Acetyl:2H(3)

59.045045

Mono-methylated lysine labelled with Acetyl_heavy

K

UNIMOD:771

lapachenole

240.11503

lapachenole photochemically added to cysteine

C

UNIMOD:772

13C5

5.016774

Label:13C(5), 13C(5) Silac label

P

UNIMOD:773

maleimide

97.016378

K, C

UNIMOD:774

Biotin-phenacyl

626.263502

Alkylation by biotinylated form of phenacyl bromide

S, H, C

UNIMOD:775

Carboxymethyl:13C(2)

60.012189

Iodoacetic acid derivative w/ 13C label, Carboxymethylation w/ 13C label

C

UNIMOD:776

CysNEM D5

130.079062

NEM:2H(5), D5 N-ethylmaleimide on cysteines

C

UNIMOD:792

T

63.044462

AEC-MAEC:2H(4), deuterium cysteamine modification to S or T, deuterium cysteamine modification to S

S,

UNIMOD:793

Hex1HexNAc1

365.132196

Hex(1)HexNAc(1)

S, N, T

UNIMOD:799

Label:13C(6)+GG

120.063056

13C6 labeled ubiquitinylation residue

K

UNIMOD:800

Biotin:Thermo-21345

311.166748

Used for labeling glutamine-donor substrate of transglutaminase, was PentylamineBiotin

Q

UNIMOD:801

Pentylamine

70.07825

Labeling transglutaminase substrate on glutamine side chain

Q

UNIMOD:811

Biotin:Thermo-21360

487.246455

Pierce EZ link biotin hydrazide prod no. 21360, was Biotin-PEO4-hydrazide

UNIMOD:821

Cy3b-maleimide

682.24612

Formula requires confirmation, Cy3b meleimide reacted with Cysteine, fluorescent dye that labels cysteines

C

UNIMOD:822

Gly-loss+Amide

-58.005479

Enzymatic glycine removal leaving an amidated C-terminus, Amidation requires presence of glycine at peptide terminus

G @ C-term

UNIMOD:824

Intact

220.048407

monolink BMOE crosslinker, Intact or monolink BMOE crosslinker, Xlink:BMOE

C

UNIMOD:825

Xlink:DFDNB

163.985807

Intact DFDNB crosslinker

Q, N, K, R

UNIMOD:827

TMPP-Ac

572.181134

tris(2,4,6-trimethoxyphenyl)phosphonium acetic acid N-hydroxysuccinimide ester derivative

N-term, K, Y

UNIMOD:830

Dihydroxyimidazolidine

72.021129

Dihydroxy methylglyoxal adduct

R

UNIMOD:834

Acetyl_K4

46.035672

Acetyl 4,4,5,5-D4 Lysine, Label:2H(4)+Acetyl

K

UNIMOD:835

Label:13C(6)+Acetyl

48.030694

Acetyl 13C(6) Silac label

K

UNIMOD:836

Acetyl_heavy lysine

50.024764

Acetyl_13C(6) 15N(2) Silac label, Label:13C(6)15N(2)+Acetyl

K

UNIMOD:837

Arg->Npo

80.985078

Arginine replacement by Nitropyrimidyl ornithine

R

UNIMOD:846

EQIGG

484.228162

Sumoylation, Sumo mutant Smt3-WT tail following trypsin digestion

K

UNIMOD:848

Arg2PG

266.057909

Adduct of phenylglyoxal with Arg

R

UNIMOD:849

cGMP

343.031785

S-guanylation

S, C

UNIMOD:851

cGMP+RMP-loss

150.041585

S-guanylation-2

S, C

UNIMOD:853

Label:2H(4)+GG

118.068034

Ubiquitination 2H4 lysine

K

UNIMOD:859

MG-H1

54.010565

Methylglyoxal-derived hydroimidazolone

R

UNIMOD:860

G-H1

39.994915

Glyoxal-derived hydroimiadazolone

R

UNIMOD:861

ZGB

758.380841

NHS ester linked Green Fluorescent Bodipy Dye

N-term, K

UNIMOD:862

SILAC

4.022185

Label:13C(1)2H(3)

M

UNIMOD:864

heavy glygly lysine

122.057126

13C(6) 15N(2) Lysine glygly, Label:13C(6)15N(2)+GG

K

UNIMOD:866

ICPL_10

115.0667

Bruker Daltonics SERVA-ICPL(TM) quantification chemistry, +10 Da form, ICPL:13C(6)2H(4)

N-term, K

UNIMOD:876

QEQTGG

600.250354

GlnGluGlnThrGlyGly, SUMOylation by SUMO-1

K

UNIMOD:877

QQQTGG

599.266339

SUMOylation by SUMO-2/3, GlnGlnGlnThrGlyGly

K

UNIMOD:884

was ChromoBiotin

695.310118

EZ-Link NHS-Chromogenic Biotin, Biotin:Thermo-21325

K

UNIMOD:885

Label:13C(1)2H(3)+Oxidation

20.0171

Oxidised methionine 13C(1)2H(3) SILAC label

M

UNIMOD:886

HydroxymethylOP

108.021129

2-ammonio-6-[4-(hydroxymethyl)-3-oxidopyridinium-1-yl]- hexanoate

K

UNIMOD:887

MDCC

383.148121

CAS 156571-46-9, covalent linkage of maleimidyl coumarin probe (Molecular Probes D-10253), 7-diethylamino-3-((((2-maleimidyl)ethyl)amino)carbonyl)coumarin

C

UNIMOD:888

mTRAQ

140.094963

Applied Biosystems mTRAQ(TM) reagent, mTRAQ light

H, N-term, K, S, T, Y

UNIMOD:889

mTRAQ medium

144.102063

Applied Biosystems mTRAQ(TM) reagent, mTRAQ:13C(3)15N(1), mTRAQ medium is identical to iTRAQ4plex 117

H, N-term, K, S, T, Y

UNIMOD:890

DyLight-maleimide

940.1999

Thiol-reactive dye for fluorescence labelling of proteins

C

UNIMOD:891

Methyl-PEO12-Maleimide

710.383719

C

UNIMOD:893

CarbamidomethylDTT

209.018035

Carbamidomethylated DTT modification of cysteine

C

UNIMOD:894

CarboxymethylDTT

210.00205

Carboxymethylated DTT modification of cysteine

C

UNIMOD:895

Biotin-PEG-PRA

578.317646

Biotin polyethyleneoxide (n=3) alkyne

M

UNIMOD:896

Met->Aha

-4.986324

Methionine replacement by azido homoalanine

M

UNIMOD:897

Label:15N(4)

3.98814

Metabolic labeling with ammonium sulfate (15N), SILAC 15N(4)

R

UNIMOD:898

pyrophospho

159.932662

pyrophosphorylation of Ser/Thr

S, T

UNIMOD:899

Met->Hpg

-21.987721

methionine replacement by homopropargylglycine

M

UNIMOD:901

4AcAllylGal

372.142033

2,3,4,6-tetra-O-Acetyl-1-allyl-alpha-D-galactopyranoside modification of cysteine

C

UNIMOD:902

DimethylArsino

103.960719

Reaction with dimethylarsinous (AsIII) acid

C

UNIMOD:903

Lys->CamCys

31.935685

Lys->Cys substitution and carbamidomethylation

K

UNIMOD:904

Phe->CamCys

12.962234

Phe->Cys substitution and carbamidomethylation

F

UNIMOD:905

Leu->MetOx

33.951335

Leu->Met substitution and sulfoxidation

L

UNIMOD:906

Lys->MetOx

18.940436

Lys->Met substitution and sulfoxidation

K

UNIMOD:907

Galactosyl

178.047738

Gluconoylation

N-term, K

UNIMOD:908

Xlink:SMCC[321]

321.205242

Monolink of SMCC terminated with 3-(dimethylamino)-1-propylamine

C

UNIMOD:910

DATDH

228.111007

Bacillosamine, 2,4-diacetamido-2,4,6-trideoxyglucopyranose, Asn-linked glycan from Gram-negative Bacterium

N

UNIMOD:911

MTSL

184.07961

Cys modification by (1-oxyl-2,2,5,5-tetramethyl-3-pyrroline-3-methyl)methanesulfonate (MTSL)

C

UNIMOD:912

HNE-BAHAH

511.319226

4-hydroxy-2-nonenal and biotinamidohexanoic acid hydrazide, reduced

K, H, C

UNIMOD:914

Methylmalonylation

100.016044

Methylmalonylation on Serine

S

UNIMOD:926

ethylamino

27.047285

ethyl amino

S, T

UNIMOD:928

MercaptoEthanol

60.003371

2-OH-ethyl thio-Ser

S, T

UNIMOD:931

Ethyl+Deamidated

29.015316

deamidation followed by esterification with ethanol

N, Q

UNIMOD:932

VFQQQTGG

845.403166

SUMOylation by SUMO-2/3 (formic acid cleavage), ValPheGlnGlnGlnThrGlyGly

K

UNIMOD:933

VIEVYQEQTGG

1203.577168

ValIleGluValTyrGlnGluGlnThrGlyGly, SUMOylation by SUMO-1 (formic acid cleavage)

K

UNIMOD:934

AMTzHexNAc2

502.202341

Photocleavable Biotin + GalNAz on O-GlcNAc

S, N, T

UNIMOD:935

Atto495Maleimide

474.250515

High molecular absorption maleimide label for proteins

C

UNIMOD:936

Chlorination

33.961028

Chlorination of tyrosine residues

W, Y

UNIMOD:937

Dichlorination

67.922055

dichlorination

Y, C

UNIMOD:938

AROD

820.336015

Cysteine modifier

C

UNIMOD:939

Cys->methylaminoAla

-2.945522

carbamidomethylated Cys that undergoes beta-elimination and Michael addition of methylamine

C

UNIMOD:940

Cys->ethylaminoAla

11.070128

Carbamidomethylated Cys that undergoes beta-elimination and Michael addition of ethylamine

C

UNIMOD:941

DNPS

198.981352

Trp modification with 2,4-Dinitrobenzenesulfenyl, 2,4-Dinitrobenzenesulfenyl

W, C

UNIMOD:942

SulfoGMBS

458.162391

High molecular absorption label for proteins

C

UNIMOD:943

DimethylamineGMBS

267.158292

Modified GMBS X linker

C

UNIMOD:944

SILAC label

11.050561

Label:15N(2)2H(9)

K

UNIMOD:946

LG-anhydrolactam

314.188195

Levuglandinyl-lysine anhydrolactam adduct

N-term, K

UNIMOD:947

LG-pyrrole

316.203845

Levuglandinyl-lysine pyrrole adduct

N-term, K, C

UNIMOD:948

LG-anhyropyrrole

298.19328

Levuglandinyl-lysine anhyropyrrole adduct

N-term, K

UNIMOD:949

3-deoxyglucosone

144.042259

Condensation product of 3-deoxyglucosone

R

UNIMOD:950

Cation:Li

6.008178

Replacement of proton by lithium

D, C-term, E

UNIMOD:951

Cation:Ca[II]

37.946941

Replacement of 2 protons by calcium

D, C-term, E

UNIMOD:952

Cation:Fe[II]

53.919289

Replacement of 2 protons by iron

D, C-term, E

UNIMOD:953

Cation:Ni[II]

55.919696

Replacement of 2 protons by nickel

D, C-term, E

UNIMOD:954

Cation:Zn[II]

61.913495

Replacement of 2 protons by zinc

D, C-term, E, H

UNIMOD:955

Cation:Ag

105.897267

Replacement of proton by silver

D, C-term, E

UNIMOD:956

Cation:Mg[II]

21.969392

Replacement of 2 protons by magnesium

D, C-term, E

UNIMOD:957

2-succinyl

116.010959

S-(2-succinyl) cysteine

C

UNIMOD:958

propargylamine

37.031634

Propargylamine

C-term, D, E

UNIMOD:959

Phosphopropargyl

116.997965

phospho-propargylamine

S, Y, T

UNIMOD:960

SUMO2135

2135.920496

ELGMEEEDVIEVYQEQTGG, SUMOylation by SUMO-1 after tryptic cleavage

K

UNIMOD:961

SUMO3549

3549.536568

FDGQPINETDTPAQLEMEDEDTIDVFQQQTGG, SUMOylation by SUMO-2/3 after tryptic cleavage

K

UNIMOD:967

thioacylPA

159.035399

membrane protein extraction

K

UNIMOD:971

maleimide3

969.366232

maleimide-3-saccharide

K, C

UNIMOD:972

maleimide5

1293.471879

maleimide-5-saccharide

K, C

UNIMOD:973

Puromycin

453.212452

C-term

UNIMOD:977

Carbofuran

57.021464

2,3-dihydro-2,2-dimethyl-7-benzofuranol N-methyl carbamate

S

UNIMOD:978

BITC

149.02992

Benzyl isothiocyanate

N-term, K, C

UNIMOD:979

PEITC

163.04557

Phenethyl isothiocyanate

N-term, K, C

UNIMOD:981

glucosone

160.037173

Condensation product of glucosone

R

UNIMOD:984

cysTMT

299.166748

Native cysteine-reactive Tandem Mass Tag®, Tandem Mass Tag® and TMT® are registered Trademarks of Proteome Sciences plc.

C

UNIMOD:985

cysTMT6plex

304.177202

cysteine-reactive Sixplex Tandem Mass Tag®, Tandem Mass Tag® and TMT® are registered Trademarks of Proteome Sciences plc.

C

UNIMOD:986

Label:13C(6)+Dimethyl

34.051429

Dimethyl 13C(6) Silac label

K

UNIMOD:987

Label:13C(6)15N(2)+Dimethyl

36.045499

Dimethyl 13C(6)15N(2) Silac label

K

UNIMOD:989

Ammonium

17.026549

replacement of proton with ammonium ion

C-term, D, E

UNIMOD:991

ISD_z+2_ion

-15.010899

ISD (z+2)-series

N-term

UNIMOD:993

Biotin:Sigma-B1267

449.17329

maleimide biotinylated, was Biotin-maleimide

C

UNIMOD:994

15N(1)

0.997035

Label:15N(1), Metabolic labeling with ammonium sulfate (15N)

C, V, M, G, A, S, F, P, D, T, E, I, Y, L

UNIMOD:995

15N(2)

1.99407

Label:15N(2), Metabolic labeling with ammonium sulfate (15N)

W, N, Q, K

UNIMOD:996

15N(3)

2.991105

Label:15N(3), Metabolic labeling with ammonium sulfate (15N)

H

UNIMOD:997

sulfo+amino

94.967714

aminotyrosine with sulfation, amino_sulfate

Y

UNIMOD:1000

AHA-Alkyne

107.077339

Azidohomoalanine (AHA) bound to propargylglycine-NH2 (alkyne)

M

UNIMOD:1001

AHA-Alkyne-KDDDD

695.280074

Azidohomoalanine (AHA) bound to DDDDK-propargylglycine-NH2 (alkyne)

M

UNIMOD:1002

EGCG1

456.069261

(-)-epigallocatechin-3-gallate

C

UNIMOD:1003

EGCG2

287.055563

(-)-dehydroepigallocatechin

C

UNIMOD:1004

Label:13C(6)15N(4)+Methyl

24.023919

Monomethylated Arg13C(6) 15N(4)

R

UNIMOD:1005

Label:13C(6)15N(4)+Dimethyl

38.039569

Dimethylated Arg13C(6) 15N(4)

R

UNIMOD:1006

Label:13C(6)15N(4)+Methyl:2H(3)13C(1)

28.046104

2H(3) 13C(1) monomethylated Arg13C(6) 15N(4)

R

UNIMOD:1007

Label:13C(6)15N(4)+Dimethyl:2H(6)13C(2)

46.083939

2H(6) 13C(2) Dimethylated Arg13C(6) 15N(4)

R

UNIMOD:1008

Cys->CamSec

104.965913

Carboxyamidomethylation of selenocysteine, Sec Iodoacetamide derivative

C

UNIMOD:1009

Thiazolidine

12.0

formaldehyde adduct, thiazolidine-2-carboxylic acid

H, C, N-term, K, W, F, R, Y

UNIMOD:1010

DEDGFLYMVYASQETFG

1970.824411

Addition of DEDGFLYMVYASQETFG

K

UNIMOD:1012

Biotin:Invitrogen-M1602

523.210069

Reference M1602 Invitrogen, Nalpha-(3-maleimidylpropionyl)biocytin

C

UNIMOD:1014

glycidamide

87.032028

glycidamide adduct

N-term, K

UNIMOD:1015

Ahx2+Hsl

309.205242

C-terminal homoserine lactone and two aminohexanoic acids

C-term

UNIMOD:1017

DMPO

111.068414

DMPO spin-trap nitrone adduct

Y, H, C

UNIMOD:1018

ICDID

138.06808

Isotope-Coded Dimedone light form

C

UNIMOD:1019

ICDID:2H(6)

144.10574

Isotope-Coded Dimedone heavy form

C

UNIMOD:1020

Xlink:DSS[156]

156.078644

Water-quenched monolink of DSS/BS3 crosslinker

N-term, K

UNIMOD:1021

Xlink:EGS[244]

244.058303

Ethylene glycolbis(succinimidylsuccinate), Water quenched monolink of EGS cross-linker

N-term, K

UNIMOD:1022

Xlink:DST[132]

132.005873

Water quenched monolink of DST crosslinker

N-term, K

UNIMOD:1023

Xlink:DTSSP[192]

191.991486

Water quenched monolink of DSP/DTSSP crosslinker, Also known as DSP

N-term, K

UNIMOD:1024

Xlink:SMCC[237]

237.100108

Water quenched monolink of SMCC

N-term, K, C

UNIMOD:1027

Xlink:DMP[140]

140.094963

Dimethyl pimelimidate dead-end crosslink, Water quenched monolink of DMP crosslinker

N-term, K

UNIMOD:1028

Xlink:EGS[115]

115.026943

Ethylene glycolbis(succinimidylsuccinate), Cleavage product of EGS protein crosslinks by hydroylamine treatment

N-term, K

UNIMOD:1031

desthiobiotin-GTP

196.121178

desthiobiotin-ADP, Biotin:Thermo-88310, desthiobiotin-ATP, desthiobiotin modification of lysine

K

UNIMOD:1032

2-nitrobenzyl

135.032028

Tyrosine caged with 2-nitrobenzyl (ONB)

Y

UNIMOD:1033

SecNEM

172.992127

N-ethylmaleimide on selenocysteines, Cys->SecNEM

C

UNIMOD:1034

SecNEM D5

178.023511

Cys->SecNEM:2H(5), D5 N-ethylmaleimide on selenocysteines

C

UNIMOD:1035

Thiadiazole

174.025169

Thiadiazolydation of Cys

C

UNIMOD:1036

Withaferin

470.266839

Modification of cystein by withaferin

C

UNIMOD:1037

desthiobiotin-FP

443.291294

desthiobiotin fluorophosphonate, Biotin:Thermo-88317, desthiobiotin-alkyl-FP

S, Y

UNIMOD:1038

TAMRA-FP

659.312423

TAMRA-alkyl-FP, TAMRA fluorophosphonate modification of serine, ActivX TAMRA-FP Serine Hydrolase Probe

S, Y

UNIMOD:1039

Biotin:Thermo-21901+H2O

543.236284

Maleimide-PEG2-Biotin + Water, Maleimide-Biotin + Water

C

UNIMOD:1041

Deoxyhypusine

71.073499

Q, K

UNIMOD:1042

Acetyldeoxyhypusine

113.084064

K

UNIMOD:1043

Acetylhypusine

129.078979

K

UNIMOD:1044

Ala->Cys

31.972071

Ala->Cys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1045

Ala->Phe

76.0313

Ala->Phe substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1046

Ala->His

66.021798

Ala->His substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1047

Ala->Xle

42.04695

Ala->Leu/Ile substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1048

Ala->Lys

57.057849

Ala->Lys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1049

Ala->Met

60.003371

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Ala->Met substitution, editing of the charged tRNA

A

UNIMOD:1050

Ala->Asn

43.005814

Misacylation of the tRNA or editing of the charged tRNA, Ala->Asn substitution, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1051

Ala->Gln

57.021464

Ala->Gln substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1052

Ala->Arg

85.063997

Ala->Arg substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1053

Ala->Trp

115.042199

Ala->Trp substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1054

Ala->Tyr

92.026215

Misacylation of the tRNA or editing of the charged tRNA, Ala->Tyr substitution, Misacylation of the tRNA, editing of the charged tRNA

A

UNIMOD:1055

Cys->Ala

-31.972071

Cys->Ala substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

C

UNIMOD:1056

Cys->Asp

12.017759

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Cys->Asp substitution, editing of the charged tRNA

C

UNIMOD:1057

Cys->Glu

26.033409

Cys->Glu substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

C

UNIMOD:1058

Cys->His

34.049727

Cys->His substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

C

UNIMOD:1059

Cys->Xle

10.07488

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Cys->Leu/Ile substitution

C

UNIMOD:1060

Cys->Lys

25.085779

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Cys->Lys substitution, editing of the charged tRNA

C

UNIMOD:1061

Cys->Met

28.0313

Cys->Met substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

C

UNIMOD:1062

Cys->Asn

11.033743

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Cys->Asn substitution, editing of the charged tRNA

C

UNIMOD:1063

Cys->Pro

-5.956421

Cys->Pro substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

C

UNIMOD:1064

Cys->Gln

25.049393

Misacylation of the tRNA or editing of the charged tRNA, Cys->Gln substitution, Misacylation of the tRNA, editing of the charged tRNA

C

UNIMOD:1065

Cys->Thr

-1.961506

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Cys->Thr substitution, editing of the charged tRNA

C

UNIMOD:1066

Cys->Val

-3.940771

Misacylation of the tRNA or editing of the charged tRNA, Cys->Val substitution, Misacylation of the tRNA, editing of the charged tRNA

C

UNIMOD:1067

Asp->Cys

-12.017759

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asp->Cys substitution, editing of the charged tRNA

D

UNIMOD:1068

Asp->Phe

32.041471

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asp->Phe substitution

D

UNIMOD:1069

Asp->Xle

-1.942879

Asp->Leu/Ile substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

D

UNIMOD:1070

Asp->Lys

13.06802

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asp->Lys substitution, editing of the charged tRNA

D

UNIMOD:1071

Asp->Met

16.013542

Asp->Met substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

D

UNIMOD:1072

Asp->Pro

-17.974179

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asp->Pro substitution, editing of the charged tRNA

D

UNIMOD:1073

Asp->Gln

13.031634

Asp->Gln substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

D

UNIMOD:1074

Asp->Arg

41.074168

Asp->Arg substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

D

UNIMOD:1075

Asp->Ser

-27.994915

Misacylation of the tRNA or editing of the charged tRNA, Asp->Ser substitution, Misacylation of the tRNA, editing of the charged tRNA

D

UNIMOD:1076

Asp->Thr

-13.979265

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asp->Thr substitution, editing of the charged tRNA

D

UNIMOD:1077

Asp->Trp

71.05237

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asp->Trp substitution, editing of the charged tRNA

D

UNIMOD:1078

Glu->Cys

-26.033409

Glu->Cys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

E

UNIMOD:1079

Glu->Phe

18.025821

Glu->Phe substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

E

UNIMOD:1080

Glu->His

8.016319

Glu->His substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

E

UNIMOD:1081

Glu->Xle

-15.958529

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Glu->Leu/Ile substitution, editing of the charged tRNA

E

UNIMOD:1082

Glu->Met

1.997892

Misacylation of the tRNA or editing of the charged tRNA, Glu->Met substitution, Misacylation of the tRNA, editing of the charged tRNA

E

UNIMOD:1083

Glu->Asn

-14.999666

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Glu->Asn substitution

E

UNIMOD:1084

Glu->Pro

-31.989829

Glu->Pro substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

E

UNIMOD:1085

Glu->Arg

27.058518

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Glu->Arg substitution, editing of the charged tRNA

E

UNIMOD:1086

Glu->Ser

-42.010565

Glu->Ser substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

E

UNIMOD:1087

Glu->Thr

-27.994915

Glu->Thr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

E

UNIMOD:1088

Glu->Trp

57.03672

Glu->Trp substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

E

UNIMOD:1089

Glu->Tyr

34.020735

Glu->Tyr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

E

UNIMOD:1090

Phe->Ala

-76.0313

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Phe->Ala substitution

F

UNIMOD:1091

Phe->Asp

-32.041471

Misacylation of the tRNA or editing of the charged tRNA, Phe->Asp substitution, Misacylation of the tRNA, editing of the charged tRNA

F

UNIMOD:1092

Phe->Glu

-18.025821

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Phe->Glu substitution

F

UNIMOD:1093

Phe->Gly

-90.04695

Phe->Gly substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

F

UNIMOD:1094

Phe->His

-10.009502

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Phe->His substitution, editing of the charged tRNA

F

UNIMOD:1095

Phe->Lys

-18.973451

Phe->Lys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

F

UNIMOD:1096

Phe->Met

-16.027929

Phe->Met substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

F

UNIMOD:1097

Phe->Asn

-33.025486

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Phe->Asn substitution, editing of the charged tRNA

F

UNIMOD:1098

Phe->Pro

-50.01565

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Phe->Pro substitution, editing of the charged tRNA

F

UNIMOD:1099

Phe->Gln

-19.009836

Phe->Gln substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

F

UNIMOD:1100

Phe->Arg

9.032697

Phe->Arg substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

F

UNIMOD:1101

Phe->Thr

-46.020735

Phe->Thr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

F

UNIMOD:1102

Phe->Trp

39.010899

Misacylation of the tRNA or editing of the charged tRNA, Phe->Trp substitution, Misacylation of the tRNA, editing of the charged tRNA

F

UNIMOD:1103

Gly->Phe

90.04695

Misacylation of the tRNA or editing of the charged tRNA, Gly->Phe substitution, Misacylation of the tRNA, editing of the charged tRNA

G

UNIMOD:1104

Gly->His

80.037448

Gly->His substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

G

UNIMOD:1105

Gly->Xle

56.0626

Gly->Leu/Ile substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

G

UNIMOD:1106

Gly->Lys

71.073499

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Gly->Lys substitution, editing of the charged tRNA

G

UNIMOD:1107

Gly->Met

74.019021

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Gly->Met substitution, editing of the charged tRNA

G

UNIMOD:1108

Gly->Asn

57.021464

Gly->Asn substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

G

UNIMOD:1109

Gly->Pro

40.0313

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Gly->Pro substitution, editing of the charged tRNA

G

UNIMOD:1110

Gly->Gln

71.037114

Misacylation of the tRNA or editing of the charged tRNA, Gly->Gln substitution, Misacylation of the tRNA, editing of the charged tRNA

G

UNIMOD:1111

Gly->Thr

44.026215

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Gly->Thr substitution

G

UNIMOD:1112

Gly->Tyr

106.041865

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Gly->Tyr substitution

G

UNIMOD:1113

His->Ala

-66.021798

His->Ala substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

H

UNIMOD:1114

His->Cys

-34.049727

His->Cys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

H

UNIMOD:1115

His->Glu

-8.016319

His->Glu substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

H

UNIMOD:1116

His->Phe

10.009502

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, His->Phe substitution, editing of the charged tRNA

H

UNIMOD:1117

His->Gly

-80.037448

His->Gly substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

H

UNIMOD:1119

His->Lys

-8.963949

Misacylation of the tRNA, Misacylation of the tRNA or editing of the charged tRNA, His->Lys substitution, editing of the charged tRNA

H

UNIMOD:1120

His->Met

-6.018427

Misacylation of the tRNA or editing of the charged tRNA, His->Met substitution, Misacylation of the tRNA, editing of the charged tRNA

H

UNIMOD:1121

His->Ser

-50.026883

His->Ser substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

H

UNIMOD:1122

His->Thr

-36.011233

His->Thr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

H

UNIMOD:1123

His->Val

-37.990498

His->Val substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

H

UNIMOD:1124

His->Trp

49.020401

His->Trp substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

H

UNIMOD:1125

Xle->Ala

-42.04695

Leu/Ile->Ala substitution

I, L

UNIMOD:1126

Xle->Cys

-10.07488

Leu/Ile->Cys substitution

I, L

UNIMOD:1127

Xle->Asp

1.942879

Leu/Ile->Asp substitution

I, L

UNIMOD:1128

Xle->Glu

15.958529

Leu/Ile->Glu substitution

I, L

UNIMOD:1129

Xle->Gly

-56.0626

Leu/Ile->Gly substitution

I, L

UNIMOD:1130

Xle->Tyr

49.979265

Leu/Ile->Tyr substitution

I, L

UNIMOD:1131

Lys->Ala

-57.057849

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Lys->Ala substitution

K

UNIMOD:1132

Lys->Cys

-25.085779

Lys->Cys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

K

UNIMOD:1133

Lys->Asp

-13.06802

Misacylation of the tRNA or editing of the charged tRNA, Lys->Asp substitution, Misacylation of the tRNA, editing of the charged tRNA

K

UNIMOD:1134

Lys->Phe

18.973451

Lys->Phe substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

K

UNIMOD:1135

Lys->Gly

-71.073499

Misacylation of the tRNA or editing of the charged tRNA, Lys->Gly substitution, Misacylation of the tRNA, editing of the charged tRNA

K

UNIMOD:1136

Lys->His

8.963949

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Lys->His substitution, editing of the charged tRNA

K

UNIMOD:1137

Lys->Pro

-31.042199

Misacylation of the tRNA or editing of the charged tRNA, Lys->Pro substitution, Misacylation of the tRNA, editing of the charged tRNA

K

UNIMOD:1138

Lys->Ser

-41.062935

Lys->Ser substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

K

UNIMOD:1139

Lys->Val

-29.026549

Lys->Val substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

K

UNIMOD:1140

Lys->Trp

57.98435

Lys->Trp substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

K

UNIMOD:1141

Lys->Tyr

34.968366

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Lys->Tyr substitution, editing of the charged tRNA

K

UNIMOD:1142

Met->Ala

-60.003371

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Met->Ala substitution, editing of the charged tRNA

M

UNIMOD:1143

Met->Cys

-28.0313

Met->Cys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

M

UNIMOD:1144

Met->Asp

-16.013542

Misacylation of the tRNA or editing of the charged tRNA, Met->Asp substitution, Misacylation of the tRNA, editing of the charged tRNA

M

UNIMOD:1145

Met->Glu

-1.997892

Met->Glu substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

M

UNIMOD:1146

Met->Phe

16.027929

Met->Phe substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

M

UNIMOD:1147

Met->Gly

-74.019021

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Met->Gly substitution, editing of the charged tRNA

M

UNIMOD:1148

Met->His

6.018427

Misacylation of the tRNA or editing of the charged tRNA, Met->His substitution, Misacylation of the tRNA, editing of the charged tRNA

M

UNIMOD:1149

Met->Asn

-16.997557

Met->Asn substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

M

UNIMOD:1150

Met->Pro

-33.987721

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Met->Pro substitution, editing of the charged tRNA

M

UNIMOD:1151

Met->Gln

-2.981907

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Met->Gln substitution, editing of the charged tRNA

M

UNIMOD:1152

Met->Ser

-44.008456

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Met->Ser substitution, editing of the charged tRNA

M

UNIMOD:1153

Met->Trp

55.038828

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Met->Trp substitution, editing of the charged tRNA

M

UNIMOD:1154

Met->Tyr

32.022844

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Met->Tyr substitution

M

UNIMOD:1155

Asn->Ala

-43.005814

Asn->Ala substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

N

UNIMOD:1156

Asn->Cys

-11.033743

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asn->Cys substitution, editing of the charged tRNA

N

UNIMOD:1157

Asn->Glu

14.999666

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asn->Glu substitution, editing of the charged tRNA

N

UNIMOD:1158

Asn->Phe

33.025486

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asn->Phe substitution, editing of the charged tRNA

N

UNIMOD:1159

Asn->Gly

-57.021464

Asn->Gly substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

N

UNIMOD:1160

Asn->Met

16.997557

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asn->Met substitution, editing of the charged tRNA

N

UNIMOD:1161

Asn->Pro

-16.990164

Asn->Pro substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

N

UNIMOD:1162

Asn->Gln

14.01565

Asn->Gln substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

N

UNIMOD:1163

Asn->Arg

42.058184

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asn->Arg substitution, editing of the charged tRNA

N

UNIMOD:1164

Asn->Val

-14.974514

Asn->Val substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

N

UNIMOD:1165

Asn->Trp

72.036386

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Asn->Trp substitution, editing of the charged tRNA

N

UNIMOD:1166

Pro->Cys

5.956421

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Pro->Cys substitution, editing of the charged tRNA

P

UNIMOD:1167

Pro->Asp

17.974179

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Pro->Asp substitution, editing of the charged tRNA

P

UNIMOD:1168

Pro->Glu

31.989829

Pro->Glu substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

P

UNIMOD:1169

Pro->Phe

50.01565

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Pro->Phe substitution, editing of the charged tRNA

P

UNIMOD:1170

Pro->Gly

-40.0313

Pro->Gly substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

P

UNIMOD:1171

Pro->Lys

31.042199

Pro->Lys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

P

UNIMOD:1172

Pro->Met

33.987721

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Pro->Met substitution, editing of the charged tRNA

P

UNIMOD:1173

Pro->Asn

16.990164

Pro->Asn substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

P

UNIMOD:1174

Pro->Val

2.01565

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Pro->Val substitution, editing of the charged tRNA

P

UNIMOD:1175

Pro->Trp

89.026549

Pro->Trp substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

P

UNIMOD:1176

Pro->Tyr

66.010565

Pro->Tyr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

P

UNIMOD:1177

Gln->Ala

-57.021464

Gln->Ala substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1178

Gln->Cys

-25.049393

Misacylation of the tRNA or editing of the charged tRNA, Gln->Cys substitution, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1179

Gln->Asp

-13.031634

Gln->Asp substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1180

Gln->Phe

19.009836

Misacylation of the tRNA or editing of the charged tRNA, Gln->Phe substitution, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1181

Gln->Gly

-71.037114

Gln->Gly substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1182

Gln->Met

2.981907

Misacylation of the tRNA or editing of the charged tRNA, Gln->Met substitution, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1183

Gln->Asn

-14.01565

Gln->Asn substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1184

Gln->Ser

-41.026549

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Gln->Ser substitution, editing of the charged tRNA

Q

UNIMOD:1185

Gln->Thr

-27.010899

Gln->Thr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1186

Gln->Val

-28.990164

Gln->Val substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1187

Gln->Trp

58.020735

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Gln->Trp substitution, editing of the charged tRNA

Q

UNIMOD:1188

Gln->Tyr

35.004751

Gln->Tyr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Q

UNIMOD:1189

Arg->Ala

-85.063997

Arg->Ala substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

R

UNIMOD:1190

Arg->Asp

-41.074168

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Arg->Asp substitution, editing of the charged tRNA

R

UNIMOD:1191

Arg->Glu

-27.058518

Arg->Glu substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

R

UNIMOD:1192

Arg->Asn

-42.058184

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Arg->Asn substitution

R

UNIMOD:1193

Arg->Val

-57.032697

Misacylation of the tRNA or editing of the charged tRNA, Arg->Val substitution, Misacylation of the tRNA, editing of the charged tRNA

R

UNIMOD:1194

Arg->Tyr

6.962218

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Arg->Tyr substitution, editing of the charged tRNA

R

UNIMOD:1195

Arg->Phe

-9.032697

Misacylation of the tRNA or editing of the charged tRNA, Arg->Phe substitution, Misacylation of the tRNA, editing of the charged tRNA

R

UNIMOD:1196

Ser->Asp

27.994915

Ser->Asp substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

S

UNIMOD:1197

Ser->Glu

42.010565

Misacylation of the tRNA or editing of the charged tRNA, Ser->Glu substitution, Misacylation of the tRNA, editing of the charged tRNA

S

UNIMOD:1198

Ser->His

50.026883

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Ser->His substitution

S

UNIMOD:1199

Ser->Lys

41.062935

Ser->Lys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

S

UNIMOD:1200

Ser->Met

44.008456

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Ser->Met substitution, editing of the charged tRNA

S

UNIMOD:1201

Ser->Gln

41.026549

Ser->Gln substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

S

UNIMOD:1202

Ser->Val

12.036386

Ser->Val substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

S

UNIMOD:1203

Thr->Cys

1.961506

Thr->Cys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

T

UNIMOD:1204

Thr->Asp

13.979265

Thr->Asp substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

T

UNIMOD:1205

Thr->Glu

27.994915

Thr->Glu substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

T

UNIMOD:1206

Thr->Phe

46.020735

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Thr->Phe substitution, editing of the charged tRNA

T

UNIMOD:1207

Thr->Gly

-44.026215

Thr->Gly substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

T

UNIMOD:1208

Thr->His

36.011233

Thr->His substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

T

UNIMOD:1209

Thr->Gln

27.010899

Thr->Gln substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

T

UNIMOD:1210

Thr->Val

-1.979265

Thr->Val substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

T

UNIMOD:1211

Thr->Trp

85.031634

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Thr->Trp substitution, editing of the charged tRNA

T

UNIMOD:1212

Thr->Tyr

62.01565

Thr->Tyr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

T

UNIMOD:1213

Val->Cys

3.940771

Val->Cys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

V

UNIMOD:1214

Val->His

37.990498

Misacylation of the tRNA or editing of the charged tRNA, Val->His substitution, Misacylation of the tRNA, editing of the charged tRNA

V

UNIMOD:1215

Val->Lys

29.026549

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Val->Lys substitution, editing of the charged tRNA

V

UNIMOD:1216

Val->Asn

14.974514

Val->Asn substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

V

UNIMOD:1217

Val->Pro

-2.01565

Val->Pro substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

V

UNIMOD:1218

Val->Gln

28.990164

Misacylation of the tRNA or editing of the charged tRNA, Val->Gln substitution, Misacylation of the tRNA, editing of the charged tRNA

V

UNIMOD:1219

Val->Arg

57.032697

Misacylation of the tRNA or editing of the charged tRNA, Val->Arg substitution, Misacylation of the tRNA, editing of the charged tRNA

V

UNIMOD:1220

Val->Ser

-12.036386

Val->Ser substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

V

UNIMOD:1221

Val->Thr

1.979265

Val->Thr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

V

UNIMOD:1222

Val->Trp

87.010899

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Val->Trp substitution, editing of the charged tRNA

V

UNIMOD:1223

Val->Tyr

63.994915

Val->Tyr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

V

UNIMOD:1224

Trp->Ala

-115.042199

Trp->Ala substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

W

UNIMOD:1225

Trp->Asp

-71.05237

Trp->Asp substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

W

UNIMOD:1226

Trp->Glu

-57.03672

Trp->Glu substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

W

UNIMOD:1227

Trp->Phe

-39.010899

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Trp->Phe substitution, editing of the charged tRNA

W

UNIMOD:1228

Trp->His

-49.020401

Trp->His substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

W

UNIMOD:1229

Trp->Lys

-57.98435

Trp->Lys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

W

UNIMOD:1230

Trp->Met

-55.038828

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Trp->Met substitution, editing of the charged tRNA

W

UNIMOD:1231

Trp->Asn

-72.036386

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Trp->Asn substitution, editing of the charged tRNA

W

UNIMOD:1232

Trp->Pro

-89.026549

Trp->Pro substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

W

UNIMOD:1233

Trp->Gln

-58.020735

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Trp->Gln substitution, editing of the charged tRNA

W

UNIMOD:1234

Trp->Thr

-85.031634

Misacylation of the tRNA or editing of the charged tRNA, Trp->Thr substitution, Misacylation of the tRNA, editing of the charged tRNA

W

UNIMOD:1235

Trp->Val

-87.010899

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Trp->Val substitution, editing of the charged tRNA

W

UNIMOD:1236

Trp->Tyr

-23.015984

Trp->Tyr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

W

UNIMOD:1237

Tyr->Ala

-92.026215

Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Tyr->Ala substitution, editing of the charged tRNA

Y

UNIMOD:1238

Tyr->Glu

-34.020735

Tyr->Glu substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Y

UNIMOD:1239

Tyr->Gly

-106.041865

Misacylation of the tRNA or editing of the charged tRNA, Tyr->Gly substitution, Misacylation of the tRNA, editing of the charged tRNA

Y

UNIMOD:1240

Tyr->Lys

-34.968366

Tyr->Lys substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Y

UNIMOD:1241

Tyr->Met

-32.022844

Misacylation of the tRNA or editing of the charged tRNA, Tyr->Met substitution, Misacylation of the tRNA, editing of the charged tRNA

Y

UNIMOD:1242

Tyr->Pro

-66.010565

Tyr->Pro substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Y

UNIMOD:1243

Tyr->Gln

-35.004751

Tyr->Gln substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Y

UNIMOD:1244

Tyr->Arg

-6.962218

Tyr->Arg substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Y

UNIMOD:1245

Tyr->Thr

-62.01565

Tyr->Thr substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Y

UNIMOD:1246

Tyr->Val

-63.994915

Tyr->Val substitution, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, editing of the charged tRNA

Y

UNIMOD:1247

Tyr->Trp

23.015984

editing of the charged tRNA, Misacylation of the tRNA or editing of the charged tRNA, Misacylation of the tRNA, Tyr->Trp substitution

Y

UNIMOD:1248

Tyr->Xle

-49.979265

Tyr->Leu/Ile substitution

Y

UNIMOD:1249

AHA-SS

195.075625

Azidohomoalanine coupled to reductively cleaved tag

M

UNIMOD:1250

AHA-SS_CAM

252.097088

carbamidomethylated form of reductively cleaved tag coupled to azidohomoalanine

M

UNIMOD:1251

Biotin:Thermo-33033

548.223945

Sulfo-SBED Label Photoreactive Biotin Crosslinker

UNIMOD:1252

Biotin:Thermo-33033-H

546.208295

Sulfo-SBED Label Photoreactive Biotin Crosslinker minus Hydrogen

UNIMOD:1253

2-monomethylsuccinyl

130.026609

S-(2-monomethylsuccinyl) cysteine

C

UNIMOD:1254

o-toluene

106.041865

Saligenin

K, H

UNIMOD:1255

Cresylphosphate

170.013281

o-toluyl-phosphorylation

H, K, S, T, R, Y

UNIMOD:1256

CresylSaligeninPhosphate

276.055146

Cresyl-Saligenin-phosphorylation

H, K, S, T, R, Y

UNIMOD:1257

Ub-Br2

100.063663

Ub Bromide probe addition, Active site DUB cystine modification with Br Ub probe, Trypsin Digestion reminant of Ub Br probe

C

UNIMOD:1258

Ub-VME

172.084792

Ubiquitin vinylmethylester, Trypsin Digestion reminant of Ub VMEr probe, Active site DUB cystine modification with VME Ub probe

C

UNIMOD:1261

Ub-fluorescein

597.209772

Active site DUB cystine modification with Fluorescein Ub probe, Ub Fluorescein probe addition, Trypsin Digestion reminant of Ub Fluorescein probe

C

UNIMOD:1262

2-dimethylsuccinyl

144.042259

S-(2-dimethylsuccinyl) cysteine

C

UNIMOD:1263

Gly

57.021464

Addition of Glycine

S, K, T

UNIMOD:1264

pupylation

243.085521

addition of GGE

K

UNIMOD:1270

HCysThiolactone

117.024835

N-Homocysteine thiolactone

K

UNIMOD:1271

HCysteinyl

133.019749

S-homocysteinylation

C

UNIMOD:1276

UgiJoullie

1106.48935

Side reaction of HisTag

D, E

UNIMOD:1277

Dipyridyl

225.090212

Cys modified with dipy ligand

C

UNIMOD:1278

Furan

66.010565

Chemical modification of the iodinated sites of thyroglobulin by Suzuki reaction

Y

UNIMOD:1279

Difuran

132.021129

Chemical modification of the diiodinated sites of thyroglobulin by Suzuki reaction

Y

UNIMOD:1281

BMP-piperidinol

263.131014

(4-hydroxy-1-methyl-4-phenylpiperidin-3-yl)(phenyl)methanone, 1-methyl-3-benzoyl-4-hydroxy-4-phenylpiperidine, 3-benzoyl-1-methyl-4-phenyl-4-piperidinol

M, C

UNIMOD:1282

glutamic acid

154.074228

Side reaction of PG with Side chain of aspartic or glutamic acid, Side reaction of PG with Side chain of aspartic, UgiJoullieProGly

D, E

UNIMOD:1286

IMEHex(2)NeuAc(1)

688.199683

Glycosylation with IME linked Hex(2) NeuAc, (2-Imino-2-methoxyethyl 1-thioglycoside)

K

UNIMOD:1287

Arg-loss

-156.101111

Loss of arginine due to transpeptidation

R @ C-term

UNIMOD:1288

Arg

156.101111

Addition of arginine due to transpeptidation

N-term

UNIMOD:1289

Butyryl

70.041865

K

UNIMOD:1290

Dicarbamidomethyl

114.042927

Double Carbamidomethylation

H, C, N-term, K, R

UNIMOD:1291

Dimethyl:2H(6)

34.068961

Dimethyl-Medium

N-term, K, R

UNIMOD:1292

GGQ

242.101505

SUMOylation leaving GlyGlyGln

K

UNIMOD:1293

QTGG

343.149184

SUMOylation leaving GlnThrGlyGly

K

UNIMOD:1296

Label:13C(3)

3.010064

13C3 label for SILAC

A

UNIMOD:1298

Label:13C(4)15N(1)

5.010454

13C4 15N1 label for SILAC

D

UNIMOD:1299

2H(10) label

10.062767

Label:2H(10)

L

UNIMOD:1300

Label:2H(4)13C(1)

5.028462

R

UNIMOD:1301

Lys

128.094963

Addition of lysine due to transpeptidation

N-term

UNIMOD:1302

mTRAQ heavy

148.109162

Applied Biosystems mTRAQ(TM) reagent, mTRAQ:13C(6)15N(2)

H, N-term, K, S, T, Y

UNIMOD:1303

NeuAc

291.095417

N-acetyl neuraminic acid

S, N, T

UNIMOD:1304

NeuGc

307.090331

N-glycoyl neuraminic acid

S, N, T

UNIMOD:1305

Propyl

42.04695

N-term, C-term, K, D, E

UNIMOD:1306

Propyl:2H(6)

48.084611

N-term, K

UNIMOD:1310

Propiophenone

132.057515

H, C, K, S, W, T, R

UNIMOD:1312

Delta:H(6)C(3)O(1)

58.041865

Reduced acrolein addition +58

N-term, K, H, C

UNIMOD:1313

Delta:H(8)C(6)O(1)

96.057515

Reduced acrolein addition +96

N-term, K

UNIMOD:1314

biotinAcrolein298

298.146347

biotin hydrazide labeled acrolein addition +298

N-term, K, H, C

UNIMOD:1315

MM-diphenylpentanone

265.146664

3-methyl-5-(methylamino)-1,3-diphenylpentan-1-one

C

UNIMOD:1317

EHD-diphenylpentanone

266.13068

3-benzoyl-1-methyl-4-phenyl-4-piperidinol demethyl damino, 2-ethyl-3-hydroxy-1,3-diphenylpentan-1-one

M, C

UNIMOD:1320

Biotin:Thermo-21901+2H2O

561.246849

Maleimide-PEG2-Biotin + 2Water, Maleimide-Biotin + 2Water

C

UNIMOD:1321

DiLeu4plex115

145.12

Accurate mass for DiLeu 115 isobaric tag

N-term, K, Y

UNIMOD:1322

DiLeu4plex

145.132163

Accurate mass for DiLeu 116 isobaric tag, Representative, nominal mass for all four tags

N-term, K, Y

UNIMOD:1323

DiLeu4plex117

145.128307

Accurate mass for DiLeu 117 isobaric tag

N-term, K, Y

UNIMOD:1324

DiLeu4plex118

145.140471

Accurate mass for DiLeu 118 isobaric tag

N-term, K, Y

UNIMOD:1326

NEMS

157.019749

N-ethylmaleimideSulfur, NEMsulfur

C

UNIMOD:1327

SulfurDioxide

63.9619

C

UNIMOD:1328

NEMSwater

175.030314

NEMsulfurWater, N-ethylmaleimideSulfurWater

C

UNIMOD:1330

bisANS-sulfonates

434.178299

BisANS with loss of both sulfonates

S, K, T

UNIMOD:1331

DNCB_hapten

166.001457

Chemical reaction with 2,4-dinitro-1-chloro benzene (DNCB)

K, Y, H, C

UNIMOD:1340

Thermo #21911

921.461652

Biotin-PEG11-maleimide, Biotin:Thermo-21911

C

UNIMOD:1341

iodoTMT

324.216141

iodoTMTzero, Native iodoacetyl Tandem Mass Tag®, iodoTMT0, Tandem Mass Tag® and TMT® are registered Trademarks of Proteome Sciences plc.

H, C, K, D, E

UNIMOD:1342

iodoTMT6

329.226595

iodoTMT6plex, Sixplex iodoacetyl Tandem Mass Tag®, Tandem Mass Tag® and TMT® are registered Trademarks of Proteome Sciences plc., iodoTMTsixplex

H, C, K, D, E

UNIMOD:1344

Phosphogluconoylation

258.014069

N-term, K

UNIMOD:1345

PS_Hapten

120.021129

reaction with phenyl salicylate (PS)

K, H, C

UNIMOD:1348

Cy3-maleimide

753.262796

Cy3 mono-functional maleimides are used for the selective labeling of molecules containing free sulfhydryl groups, such as cysteine residues in proteins and peptides and oligonucleotides., Cy3 Maleimide mono-Reactive dye

C

UNIMOD:1349

benzylguanidine

132.068748

modification of the lysine side chain from NH2 to guanidine with a H removed in favor of a benzyl group, adds 132 Da to K side chain

K

UNIMOD:1350

CarboxymethylDMAP

162.079313

A fixed +1 charge tag attached to the N-terminus of peptides, Added mass is reduced by 1 H to reflect that first charge state requires no proton addition, Replaces one of the N-terminal H atoms with C9H12N2O

N-term

UNIMOD:1355

azole

-20.026215

Formation of five membered aromatic heterocycle

S, C

UNIMOD:1356

phosphoRibosyl

212.00859

phosphate-ribosylation

D, R, E

UNIMOD:1358

NEM:2H(5)+H2O

148.089627

CysNEM D5 hydrolised, D5 N-ethylmaleimide+water on cysteines

C

UNIMOD:1363

Crotonyl

68.026215

Crotonylation

K

UNIMOD:1364

tabun

135.044916

O-ethyl, N-dimethyl phosphate, O-Et-N-diMePhospho

S

UNIMOD:1365

aged tabun

107.013615

N-dimethylphosphate

S

UNIMOD:1367

Hex1dHex1

308.110732

dHex(1)Hex(1)

S, T

UNIMOD:1368

Methyl:2H(3)+Acetyl:2H(3)

62.063875

3-fold methylated lysine labelled with Acetyl_heavy

K

UNIMOD:1371

Trimethyl:2H(9)

51.103441

3-fold methylation with deuterated methyl groups

K, R

UNIMOD:1372

Acetyl:13C(2)

44.017274

heavy acetylation

N-term, K

UNIMOD:1375

Hex2dHex1

470.163556

dHex(1)Hex(2)

S, T

UNIMOD:1376

Hex3dHex1

632.216379

dHex(1)Hex(3)

S, T

UNIMOD:1377

Hex4dHex1

794.269203

dHex(1)Hex(4)

S, T

UNIMOD:1378

Hex5dHex1

956.322026

dHex(1)Hex(5)

S, T

UNIMOD:1379

Hex6dHex1

1118.37485

dHex(1)Hex(6)

S, T

UNIMOD:1380

methylsulfonylethyl

106.00885

protein alkylation by Michael acceptor methyl vinyl sulfone, reaction with methyl vinyl sulfone

K, H, C

UNIMOD:1381

ethylsulfonylethyl

120.0245

protein alkylation by Michael acceptor ethyl vinyl sulfone, reaction with ethyl vinyl sulfone

K, H, C

UNIMOD:1382

phenylsulfonylethyl

168.0245

protein alkylation by Michael acceptor phenyl vinyl sulfone, reaction with phenyl vinyl sulfone

C

UNIMOD:1383

PyridoxalPhosphateH2

231.02966

PLP bound to lysine reduced by sodium borohydride (NaBH4) to create amine linkage, Pyridoxal Phosphate reduced

K

UNIMOD:1384

Homocysteic_acid

33.969094

methionine oxidation to homocysteic acid

M

UNIMOD:1385

Hydroxamic_acid

15.010899

Conversion of carboxylic acid to hydroxamic acid, ADP-ribosylation followed by conversion to hydroxamic acid via hydroxylamine

D, E

UNIMOD:1387

3-phosphoglyceryl

167.982375

K

UNIMOD:1388

HN2_mustard

101.084064

Modification by hydroxylated mechloroethamine (HN-2)

K, H, C

UNIMOD:1389

HN3_mustard

131.094629

Modification by hydroxylated tris-(2-chloroethyl)amine (HN-3)

K, H, C

UNIMOD:1390

Oxidation+NEM

141.042593

N-ethylmaleimide on cysteine sulfenic acid

C

UNIMOD:1391

NHS-fluorescein

471.131802

fluorescein-hexanoate-NHS hydrolysis

K

UNIMOD:1392

DiART6plex

217.162932

AKA DiART6plex114, Isobaric labeling reagent from Omic Biosystems, Inc., Representative mass and accurate mass for 114

N-term, K, Y

UNIMOD:1393

DiART6plex115

217.156612

Accurate mass for DiART6plex 115, Isobaric labeling reagent from Omic Biosystems, Inc.

N-term, K, Y

UNIMOD:1394

DiART6plex116/119

217.168776

Accurate mass for DiART6plex 116 and 119, Isobaric labeling reagent from Omic Biosystems, Inc.

N-term, K, Y

UNIMOD:1395

DiART6plex117

217.162456

Accurate mass for DiART6plex 117, Isobaric labeling reagent from Omic Biosystems, Inc.

N-term, K, Y

UNIMOD:1396

DiART6plex118

217.175096

Accurate mass for DiART6plex 118, Isobaric labeling reagent from Omic Biosystems, Inc.

N-term, K, Y

UNIMOD:1397

Iodoacetanilide

133.052764

iodoacetanilide derivative

N-term, K, C

UNIMOD:1398

Iodoacetanilide:13C(6)

139.072893

13C labelled iodoacetanilide derivative

N-term, K, C

UNIMOD:1399

Dap-DSP

364.076278

Diaminopimelic acid-DSP monolinked

K, E, A

UNIMOD:1400

MurNAc

275.100502

N-Acetylmuramic acid

A

UNIMOD:1402

Label:2H(7)15N(4)

11.032077

Cambridge Isotopes DNLM-7543-PK

R

UNIMOD:1403

Label:2H(6)15N(1)

7.034695

Arg-Pro conversion of Label:2H(7)15N(4)

P

UNIMOD:1405

EEEDVIEVYQEQTGG

1705.73189

Sumoylation by SUMO-1 after Cyanogen bromide (CNBr) cleavage

K

UNIMOD:1406

EDEDTIDVFQQQTGG

1662.700924

Sumoylation by SUMO-2/3 after Cyanogen bromide (CNBr) cleavage

K

UNIMOD:1408

A2G2S2/G2S2

2204.772441

Hex(5)HexNAc(4)NeuAc(2)

N

UNIMOD:1409

A2G2S1/G2S1

1913.677025

Hex(5)HexNAc(4)NeuAc(1)

N

UNIMOD:1410

FA2G2S1/G2FS1

2059.734933

dHex(1)Hex(5)HexNAc(4)NeuAc(1)

N

UNIMOD:1411

FA2G2S2/G2FS2

2350.83035

dHex(1)Hex(5)HexNAc(4)NeuAc(2)

N

UNIMOD:1412

s-GlcNAc

283.036187

O3S1HexNAc1

S, T

UNIMOD:1413

H1O3P1Hex2

404.071978

PhosphoHex(2)

S, N, T

UNIMOD:1414

Trimethyl:13C(3)2H(9)

54.113505

3-fold methylation with fully labelled methyl groups

K, R

UNIMOD:1419

15N-oxobutanoic

-18.023584

pyruvic acid from N-term ser, Loss of ammonia (15N), oxobutanoic acid from N term Thr

S @ Protein N-term, T @ Protein N-term, C @ N-term

UNIMOD:1420

spermine

185.189198

spermine adduct

Q

UNIMOD:1421

spermidine

128.131349

spermidine adduct

Q

UNIMOD:1423

Biotin_PEG4

473.219571

Biotin:Thermo-21330, NHS-PEG4-Biotin

N-term, K

UNIMOD:1425

Pentose

132.042259

S, T

UNIMOD:1426

Hex Pent

294.095082

Hex(1)Pent(1)

S, T

UNIMOD:1427

Hex HexA

338.084912

Hex(1)HexA(1)

S, T

UNIMOD:1428

Hex Pent(2)

426.137341

Hex(1)Pent(2)

S, T

UNIMOD:1429

Hex HexNAc Phos

445.098527

Hex(1)HexNAc(1)Phos(1)

S, T

UNIMOD:1430

Hex HexNAc Sulf

445.089011

Hex(1)HexNAc(1)Sulf(1)

S, T

UNIMOD:1431

Hex(1)NeuAc(1)

453.14824

Hex NeuAc —OR— HexNAc Kdn

S, T

UNIMOD:1432

Hex NeuGc

469.143155

Hex(1)NeuGc(1)

S, T

UNIMOD:1433

HexNAc(3)

609.238118

S, T

UNIMOD:1434

HexNAc NeuAc

494.174789

HexNAc(1)NeuAc(1)

S, T

UNIMOD:1435

HexNAc NeuGc

510.169704

HexNAc(1)NeuGc(1)

S, T

UNIMOD:1436

Hex HexNAc dHex Me

525.205755

Hex(1)HexNAc(1)dHex(1)Me(1)

S, T

UNIMOD:1437

Hex HexNAc dHex Me(2)

539.221405

Hex(1)HexNAc(1)dHex(1)Me(2)

S, T

UNIMOD:1438

Hex(2) HexNAc

527.18502

Hex(2)HexNAc(1)

S, N, T

UNIMOD:1439

Hex HexA HexNAc

541.164284

Hex(1)HexA(1)HexNAc(1)

S, T

UNIMOD:1440

Hex(2) HexNAc Me

541.20067

Hex(2)HexNAc(1)Me(1)

S, T

UNIMOD:1441

Hex Pent(3)

558.1796

Hex(1)Pent(3)

S, T

UNIMOD:1442

Hex NeuAc Pent

585.190499

Hex(1)NeuAc(1)Pent(1)

S, T

UNIMOD:1443

Hex(2) HexNAc Sulf

607.141834

Hex(2)HexNAc(1)Sulf(1)

S, T

UNIMOD:1444

Hex(2)NeuAc(1)

615.201064

Hex(2) NeuAc —OR— Hex HexNAc Kdn

S, T

UNIMOD:1445

Hex2 dHex2

616.221465

dHex(2)Hex(2)

S, T

UNIMOD:1446

dHex Hex(2) HexA

646.195644

dHex(1)Hex(2)HexA(1)

S, T

UNIMOD:1447

Hex HexNAc(2) Sulf

648.168383

Hex(1)HexNAc(2)Sulf(1)

S, T

UNIMOD:1448

Hex(4)

648.211294

S, T

UNIMOD:1449

dHex Hex(2) HexNAc(2) Pent

1008.36456

dHex(1)Hex(2)HexNAc(2)Pent(1)

N

UNIMOD:1450

Hex(2)HexNAc(2)NeuAc(1)

1021.359809

Hex(2) HexNAc(2) NeuAc —OR— dHex Hex HexNAc(2) NeuGc

S, N, T

UNIMOD:1451

Hex(3) HexNAc(2) Pent

1024.359475

Hex(3)HexNAc(2)Pent(1)

N

UNIMOD:1452

Hex(4)HexNAc(2)

1054.370039

Hex(4) HexNAc(2)

N

UNIMOD:1453

dHex Hex(4) HexNAc Pent

1129.390834

dHex(1)Hex(4)HexNAc(1)Pent(1)

N

UNIMOD:1454

dHex Hex(3) HexNAc(2) Pent

1170.417383

dHex(1)Hex(3)HexNAc(2)Pent(1)

N

UNIMOD:1455

Hex(3) HexNAc(2) NeuAc

1183.412632

Hex(3)HexNAc(2)NeuAc(1)

N

UNIMOD:1456

Hex(4) HexNAc(2) Pent

1186.412298

Hex(4)HexNAc(2)Pent(1)

N

UNIMOD:1457

Hex(3) HexNAc(3) Pent

1227.438847

Hex(3)HexNAc(3)Pent(1)

N

UNIMOD:1458

Hex(5) HexNAc(2) Phos

1296.389194

Hex(5)HexNAc(2)Phos(1)

N

UNIMOD:1459

dHex Hex(4) HexNAc(2) Pent

1332.470207

dHex(1)Hex(4)HexNAc(2)Pent(1)

N

UNIMOD:1460

Hex(7) HexNAc

1337.449137

Hex(7)HexNAc(1)

N

UNIMOD:1461

Hex(4)HexNAc(2)NeuAc(1)

1345.465456

Hex(4) HexNAc(2) NeuAc —OR— Hex(3) HexNAc(2) dHex NeuGc

S, N, T

UNIMOD:1462

dHex Hex(5) HexNAc(2)

1362.480772

dHex(1)Hex(5)HexNAc(2)

N

UNIMOD:1463

dHex Hex(3) HexNAc(3) Pent

1373.496756

dHex(1)Hex(3)HexNAc(3)Pent(1)

N

UNIMOD:1464

Hex(3) HexNAc(4) Sulf

1378.432776

Hex(3)HexNAc(4)Sulf(1)

N

UNIMOD:1465

M6/Man6

1378.475686

Hex(6)HexNAc(2)

N

UNIMOD:1466

Hex(4) HexNAc(3) Pent

1389.491671

Hex(4)HexNAc(3)Pent(1)

N

UNIMOD:1467

dHex Hex(4) HexNAc(3)

1403.507321

dHex(1)Hex(4)HexNAc(3)

N

UNIMOD:1468

Hex(5)HexNAc(3)

1419.502235

Hex(5) HexNAc(3)

N

UNIMOD:1469

Hex(3) HexNAc(4) Pent

1430.51822

Hex(3)HexNAc(4)Pent(1)

N

UNIMOD:1470

Hex(6) HexNAc(2) Phos

1458.442017

Hex(6)HexNAc(2)Phos(1)

N

UNIMOD:1471

dHex Hex(4) HexNAc(3) Sulf

1483.464135

dHex(1)Hex(4)HexNAc(3)Sulf(1)

N

UNIMOD:1472

dHex Hex(5) HexNAc(2) Pent

1494.52303

dHex(1)Hex(5)HexNAc(2)Pent(1)

N

UNIMOD:1473

Hex(8) HexNAc

1499.501961

Hex(8)HexNAc(1)

N

UNIMOD:1474

dHex(1)Hex(3)HexNAc(3)Pent(2)

1505.539015

dHex Hex(3) HexNAc(3) Pent(2)

N

UNIMOD:1475

dHex(2)Hex(3)HexNAc(3)Pent(1)

1519.554665

dHex(2) Hex(3) HexNAc(3) Pent

N

UNIMOD:1476

dHex Hex(3) HexNAc(4) Sulf

1524.490684

dHex(1)Hex(3)HexNAc(4)Sulf(1)

N

UNIMOD:1477

dHex Hex(6) HexNAc(2)

1524.533595

dHex(1)Hex(6)HexNAc(2)

N

UNIMOD:1478

dHex Hex(4) HexNAc(3) Pent

1535.549579

dHex(1)Hex(4)HexNAc(3)Pent(1)

N

UNIMOD:1479

Hex(4) HexNAc(4) Sulf

1540.485599

Hex(4)HexNAc(4)Sulf(1)

N

UNIMOD:1480

M7/Man7

1540.52851

Hex(7)HexNAc(2)

N

UNIMOD:1481

dHex(2)Hex(4)HexNAc(3)

1549.56523

dHex(2) Hex(4) HexNAc(3)

N

UNIMOD:1482

Hex(5) HexNAc(3) Pent

1551.544494

Hex(5)HexNAc(3)Pent(1)

N

UNIMOD:1483

Hex(4) HexNAc(3) NeuGc

1564.539743

Hex(4)HexNAc(3)NeuGc(1)

N

UNIMOD:1484

dHex Hex(5) HexNAc(3)

1565.560144

dHex(1)Hex(5)HexNAc(3)

N

UNIMOD:1485

dHex Hex(3) HexNAc(4) Pent

1576.576129

dHex(1)Hex(3)HexNAc(4)Pent(1)

N

UNIMOD:1486

Hex(3) HexNAc(5) Sulf

1581.512148

Hex(3)HexNAc(5)Sulf(1)

N

UNIMOD:1487

Hex(6)HexNAc(3)

1581.555059

Hex(6) HexNAc(3)

N

UNIMOD:1488

Hex(3)HexNAc(4)NeuAc(1)

1589.571378

Hex(3) HexNAc(4) NeuAc —OR— Hex(2) HexNAc(4) dHex NeuGc

N

UNIMOD:1489

Hex(4) HexNAc(4) Pent

1592.571043

Hex(4)HexNAc(4)Pent(1)

N

UNIMOD:1490

Hex(7) HexNAc(2) Phos

1620.494841

Hex(7)HexNAc(2)Phos(1)

N

UNIMOD:1491

Hex(4) HexNAc(4) Me(2) Pent

1620.602343

Hex(4)HexNAc(4)Me(2)Pent(1)

N

UNIMOD:1492

dHex(1)Hex(3)HexNAc(3)Pent(3)

1637.581274

dHex Hex(3) HexNAc(3) Pent(3) —OR— Hex(4) HexNAc(2) dHex(2) NeuAc

N

UNIMOD:1493

dHex Hex(5) HexNAc(3) Sulf

1645.516959

dHex(1)Hex(5)HexNAc(3)Sulf(1)

N

UNIMOD:1494

dHex(2)Hex(3)HexNAc(3)Pent(2)

1651.596924

dHex(2) Hex(3) HexNAc(3) Pent(2)

N

UNIMOD:1495

Hex(6) HexNAc(3) Phos

1661.52139

Hex(6)HexNAc(3)Phos(1)

N

UNIMOD:1496

Hex(4)HexNAc(5)

1663.608157

Hex(4) HexNAc(5)

N

UNIMOD:1497

dHex(3)Hex(3)HexNAc(3)Pent(1)

1665.612574

dHex(3) Hex(3) HexNAc(3) Pent

N

UNIMOD:1498

dHex(2)Hex(4)HexNAc(3)Pent(1)

1681.607488

dHex(2) Hex(4) HexNAc(3) Pent

N

UNIMOD:1499

dHex Hex(4) HexNAc(4) Sulf

1686.543508

dHex(1)Hex(4)HexNAc(4)Sulf(1)

N

UNIMOD:1500

dHex Hex(7) HexNAc(2)

1686.586419

dHex(1)Hex(7)HexNAc(2)

N

UNIMOD:1501

dHex(1)Hex(4)HexNAc(3)NeuAc(1)

1694.602737

dHex Hex(4) HexNAc(3) NeuAc —OR— dHex(2) Hex(3) HexNAc(3) NeuGc

S, N, T

UNIMOD:1502

Hex(7)HexNAc(2)Phos(2)

1700.461172

Hex(7) HexNAc(2) Phos(2)

N

UNIMOD:1503

Hex(5) HexNAc(4) Sulf

1702.538423

Hex(5)HexNAc(4)Sulf(1)

N

UNIMOD:1504

M8/Man8

1702.581333

Hex(8)HexNAc(2)

N

UNIMOD:1505

dHex(1)Hex(3)HexNAc(4)Pent(2)

1708.618387

dHex Hex(3) HexNAc(4) Pent(2)

N

UNIMOD:1506

dHex(1)Hex(4)HexNAc(3)NeuGc(1)

1710.597652

dHex Hex(4) HexNAc(3) NeuGc —OR— Hex(5) HexNAc(3) NeuAc

N

UNIMOD:1507

dHex(2)Hex(3)HexNAc(4)Pent(1)

1722.634037

dHex(2) Hex(3) HexNAc(4) Pent

N

UNIMOD:1508

dHex Hex(3) HexNAc(5) Sulf

1727.570057

dHex(1)Hex(3)HexNAc(5)Sulf(1)

N

UNIMOD:1509

dHex Hex(6) HexNAc(3)

1727.612968

dHex(1)Hex(6)HexNAc(3)

N

UNIMOD:1510

dHex Hex(3) HexNAc(4) NeuAc

1735.629286

dHex(1)Hex(3)HexNAc(4)NeuAc(1)

N

UNIMOD:1511

dHex(3)Hex(3)HexNAc(4)

1736.649688

dHex(3) Hex(3) HexNAc(4)

N

UNIMOD:1512

dHex Hex(4) HexNAc(4) Pent

1738.628952

dHex(1)Hex(4)HexNAc(4)Pent(1)

N

UNIMOD:1513

Hex(4) HexNAc(5) Sulf

1743.564972

Hex(4)HexNAc(5)Sulf(1)

N

UNIMOD:1514

Hex(7)HexNAc(3)

1743.607882

Hex(7) HexNAc(3)

N

UNIMOD:1515

dHex Hex(4) HexNAc(3) NeuAc Sulf

1774.559552

dHex(1)Hex(4)HexNAc(3)NeuAc(1)Sulf(1)

N

UNIMOD:1516

Hex(5) HexNAc(4) Me(2) Pent

1782.655167

Hex(5)HexNAc(4)Me(2)Pent(1)

N

UNIMOD:1517

Hex(3) HexNAc(6) Sulf

1784.591521

Hex(3)HexNAc(6)Sulf(1)

N

UNIMOD:1518

dHex Hex(6) HexNAc(3) Sulf

1807.569782

dHex(1)Hex(6)HexNAc(3)Sulf(1)

N

UNIMOD:1519

dHex Hex(4) HexNAc(5)

1809.666066

dHex(1)Hex(4)HexNAc(5)

N

UNIMOD:1520

dHex Hex(5) HexA HexNAc(3) Sulf

1821.549047

dHex(1)Hex(5)HexA(1)HexNAc(3)Sulf(1)

N

UNIMOD:1521

Hex(7) HexNAc(3) Phos

1823.574213

Hex(7)HexNAc(3)Phos(1)

N

UNIMOD:1522

Hex(6)HexNAc(4)Me(3)

1826.681382

Hex(6) HexNAc(4) Me(3)

N

UNIMOD:1523

dHex(2) Hex(4) HexNAc(4) Sulf

1832.601417

dHex(2)Hex(4)HexNAc(4)Sulf(1)

N

UNIMOD:1524

Hex(4)HexNAc(3)NeuAc(2)

1839.640245

Hex(4) HexNAc(3) NeuAc(2)

N

UNIMOD:1525

dHex(1)Hex(3)HexNAc(4)Pent(3)

1840.660646

dHex Hex(3) HexNAc(4) Pent(3)

N

UNIMOD:1526

dHex(2)Hex(5)HexNAc(3)Pent(1)

1843.660312

dHex(2) Hex(5) HexNAc(3) Pent

N

UNIMOD:1527

dHex Hex(5) HexNAc(4) Sulf

1848.596331

dHex(1)Hex(5)HexNAc(4)Sulf(1)

N

UNIMOD:1528

dHex(2)Hex(3)HexNAc(4)Pent(2)

1854.676296

dHex(2) Hex(3) HexNAc(4) Pent(2)

N

UNIMOD:1529

dHex Hex(5) HexNAc(3) NeuAc

1856.655561

dHex(1)Hex(5)HexNAc(3)NeuAc(1)

N

UNIMOD:1530

Hex(3)HexNAc(6)Sulf(2)

1864.548335

Hex(3) HexNAc(6) Sulf(2)

N

UNIMOD:1531

M9/Man9

1864.634157

Hex(9)HexNAc(2)

N

UNIMOD:1532

Hex(4)HexNAc(6)

1866.68753

Hex(4) HexNAc(6)

N

UNIMOD:1533

dHex(3)Hex(3)HexNAc(4)Pent(1)

1868.691946

dHex(3) Hex(3) HexNAc(4) Pent

N

UNIMOD:1534

dHex(1)Hex(5)HexNAc(3)NeuGc(1)

1872.650475

dHex Hex(5) HexNAc(3) NeuGc —OR— Hex(6) HexNAc(3) NeuAc

N

UNIMOD:1535

dHex(2) Hex(4) HexNAc(4) Pent

1884.686861

dHex(2)Hex(4)HexNAc(4)Pent(1)

N

UNIMOD:1536

dHex Hex(4) HexNAc(5) Sulf

1889.62288

dHex(1)Hex(4)HexNAc(5)Sulf(1)

N

UNIMOD:1537

dHex Hex(7) HexNAc(3)

1889.665791

dHex(1)Hex(7)HexNAc(3)

N

UNIMOD:1538

dHex Hex(5) HexNAc(4) Pent

1900.681776

dHex(1)Hex(5)HexNAc(4)Pent(1)

N

UNIMOD:1539

dHex Hex(5) HexA HexNAc(3) Sulf(2)

1901.505861

dHex(1)Hex(5)HexA(1)HexNAc(3)Sulf(2)

N

UNIMOD:1540

Hex(3)HexNAc(7)

1907.714079

Hex(3) HexNAc(7)

N

UNIMOD:1541

dHex(2)Hex(5)HexNAc(4)

1914.697426

dHex(2) Hex(5) HexNAc(4)

N

UNIMOD:1542

dHex(2) Hex(4) HexNAc(3) NeuAc Sulf

1920.617461

dHex(2)Hex(4)HexNAc(3)NeuAc(1)Sulf(1)

N

UNIMOD:1543

dHex(1)Hex(5)HexNAc(4)Sulf(2)

1928.553146

dHex Hex(5) HexNAc(4) Sulf(2)

N

UNIMOD:1544

dHex Hex(5) HexNAc(4) Me(2) Pent

1928.713076

dHex(1)Hex(5)HexNAc(4)Me(2)Pent(1)

N

UNIMOD:1545

Hex(5) HexNAc(4) NeuGc

1929.671939

Hex(5)HexNAc(4)NeuGc(1)

N

UNIMOD:1546

dHex Hex(3) HexNAc(6) Sulf

1930.64943

dHex(1)Hex(3)HexNAc(6)Sulf(1)

N

UNIMOD:1547

dHex Hex(6) HexNAc(4)

1930.69234

dHex(1)Hex(6)HexNAc(4)

N

UNIMOD:1548

dHex Hex(5) HexNAc(3) NeuAc Sulf

1936.612375

dHex(1)Hex(5)HexNAc(3)NeuAc(1)Sulf(1)

N

UNIMOD:1549

Hex(7)HexNAc(4)

1946.687255

Hex(7) HexNAc(4)

N

UNIMOD:1550

dHex Hex(5) HexNAc(3) NeuGc Sulf

1952.60729

dHex(1)Hex(5)HexNAc(3)NeuGc(1)Sulf(1)

N

UNIMOD:1551

Hex(4) HexNAc(5) NeuAc

1954.703574

Hex(4)HexNAc(5)NeuAc(1)

N

UNIMOD:1552

Hex(6) HexNAc(4) Me(3) Pent

1958.72364

Hex(6)HexNAc(4)Me(3)Pent(1)

N

UNIMOD:1553

dHex Hex(7) HexNAc(3) Sulf

1969.622606

dHex(1)Hex(7)HexNAc(3)Sulf(1)

N

UNIMOD:1554

dHex Hex(7) HexNAc(3) Phos

1969.632122

dHex(1)Hex(7)HexNAc(3)Phos(1)

N

UNIMOD:1555

dHex Hex(5) HexNAc(5)

1971.718889

dHex(1)Hex(5)HexNAc(5)

N

UNIMOD:1556

dHex Hex(4) HexNAc(4) NeuAc Sulf

1977.638925

dHex(1)Hex(4)HexNAc(4)NeuAc(1)Sulf(1)

N

UNIMOD:1557

dHex(3)Hex(4)HexNAc(4)Sulf(1)

1978.659326

dHex(3) Hex(4) HexNAc(4) Sulf

N

UNIMOD:1558

Hex(3) HexNAc(7) Sulf

1987.670893

Hex(3)HexNAc(7)Sulf(1)

N

UNIMOD:1559

A3G3

1987.713804

Hex(6)HexNAc(5)

N

UNIMOD:1560

Hex(5) HexNAc(4) NeuAc Sulf

1993.633839

Hex(5)HexNAc(4)NeuAc(1)Sulf(1)

N

UNIMOD:1561

Hex(3) HexNAc(6) NeuAc

1995.730123

Hex(3)HexNAc(6)NeuAc(1)

N

UNIMOD:1562

dHex(2)Hex(3)HexNAc(6)

1996.750524

dHex(2) Hex(3) HexNAc(6)

N

UNIMOD:1563

Hex HexNAc NeuGc

672.222527

Hex(1)HexNAc(1)NeuGc(1)

S, T

UNIMOD:1564

dHex Hex(2) HexNAc

673.242928

dHex(1)Hex(2)HexNAc(1)

S, T

UNIMOD:1565

HexNAc(3) Sulf

689.194932

HexNAc(3)Sulf(1)

S, T

UNIMOD:1566

Hex(3) HexNAc

689.237843

Hex(3)HexNAc(1)

S, N, T

UNIMOD:1567

Hex HexNAc Kdn Sulf

695.157878

Hex(1)HexNAc(1)Kdn(1)Sulf(1)

S, T

UNIMOD:1568

HexNAc(2) NeuAc

697.254162

HexNAc(2)NeuAc(1)

S, T

UNIMOD:1570

HexNAc(1)Kdn(2)

703.217108

HexNAc Kdn(2) —OR— Hex(2) HexNAc HexA

S, T

UNIMOD:1571

Hex(3) HexNAc Me

703.253493

Hex(3)HexNAc(1)Me(1)

S, T

UNIMOD:1572

Hex(2) HexA Pent Sulf

712.136808

Hex(2)HexA(1)Pent(1)Sulf(1)

S, T

UNIMOD:1573

HexNAc(2) NeuGc

713.249076

HexNAc(2)NeuGc(1)

S, T

UNIMOD:1575

Hex(4) Phos

728.177625

Hex(4)Phos(1)

S, T

UNIMOD:1577

Hex HexNAc NeuAc Sulf

736.184427

Hex(1)HexNAc(1)NeuAc(1)Sulf(1)

S, T

UNIMOD:1578

Hex HexA HexNAc(2)

744.243657

Hex(1)HexA(1)HexNAc(2)

S, T

UNIMOD:1579

dHex Hex(2) HexNAc Sulf

753.199743

dHex(1)Hex(2)HexNAc(1)Sulf(1)

S, T

UNIMOD:1580

dHex HexNAc(3)

755.296027

dHex(1)HexNAc(3)

S, T

UNIMOD:1581

dHex(1)Hex(1)HexNAc(1)Kdn(1)

761.258973

dHex Hex HexNAc Kdn —OR— Hex(2) dHex NeuAc

S, T

UNIMOD:1582

Hex HexNAc(3)

771.290941

Hex(1)HexNAc(3)

S, T

UNIMOD:1583

HexNAc(2) NeuAc Sulf

777.210976

HexNAc(2)NeuAc(1)Sulf(1)

S, T

UNIMOD:1584

dHex(2)Hex(3)

778.274288

dHex(2) Hex(3)

S, T

UNIMOD:1585

Hex(2) HexA HexNAc Sulf

783.173922

Hex(2)HexA(1)HexNAc(1)Sulf(1)

S, T

UNIMOD:1586

dHex(2) Hex(2) HexA

792.253553

dHex(2)Hex(2)HexA(1)

S, T

UNIMOD:1587

dHex Hex HexNAc(2) Sulf

794.226292

dHex(1)Hex(1)HexNAc(2)Sulf(1)

S, T

UNIMOD:1588

dHex Hex HexNAc NeuAc

802.285522

dHex(1)Hex(1)HexNAc(1)NeuAc(1)

S, T

UNIMOD:1589

Hex(2) HexNAc(2) Sulf

810.221207

Hex(2)HexNAc(2)Sulf(1)

S, T

UNIMOD:1590

Hex(5)

810.264117

S, T

UNIMOD:1591

HexNAc(4)

812.31749

S, T

UNIMOD:1592

HexNAc NeuGc(2)

817.260035

HexNAc(1)NeuGc(2)

S, T

UNIMOD:1593

dHex(1)Hex(1)HexNAc(1)NeuGc(1)

818.280436

dHex Hex HexNAc NeuGc —OR— Hex(2) HexNAc NeuAc

S, T

UNIMOD:1594

dHex(2) Hex(2) HexNAc

819.300837

dHex(2)Hex(2)HexNAc(1)

S, T

UNIMOD:1595

Hex(2) HexNAc NeuGc

834.275351

Hex(2)HexNAc(1)NeuGc(1)

S, T

UNIMOD:1596

dHex Hex(3) HexNAc

835.295752

dHex(1)Hex(3)HexNAc(1)

S, T

UNIMOD:1597

dHex Hex(2) HexA HexNAc

849.275017

dHex(1)Hex(2)HexA(1)HexNAc(1)

S, T

UNIMOD:1598

Hex HexNAc(3) Sulf

851.247756

Hex(1)HexNAc(3)Sulf(1)

S, T

UNIMOD:1599

Hex(4) HexNAc

851.290667

Hex(4)HexNAc(1)

S, N, T

UNIMOD:1600

Hex HexNAc(2) NeuAc

859.306985

Hex(1)HexNAc(2)NeuAc(1)

S, T

UNIMOD:1602

Hex HexNAc(2) NeuGc

875.3019

Hex(1)HexNAc(2)NeuGc(1)

S, T

UNIMOD:1604

Hex(5) Phos

890.230448

Hex(5)Phos(1)

S, T

UNIMOD:1606

dHex(2) Hex HexNAc Kdn

907.316881

dHex(2)Hex(1)HexNAc(1)Kdn(1)

S, T

UNIMOD:1607

dHex Hex(3) HexNAc Sulf

915.252567

dHex(1)Hex(3)HexNAc(1)Sulf(1)

S, T

UNIMOD:1608

dHex Hex HexNAc(3)

917.34885

dHex(1)Hex(1)HexNAc(3)

S, T

UNIMOD:1609

dHex Hex(2) HexA HexNAc Sulf

929.231831

dHex(1)Hex(2)HexA(1)HexNAc(1)Sulf(1)

S, T

UNIMOD:1610

Hex(2)HexNAc(3)

933.343765

Hex(2) HexNAc(3)

S, N, T

UNIMOD:1611

Hex HexNAc(2) NeuAc Sulf

939.2638

Hex(1)HexNAc(2)NeuAc(1)Sulf(1)

S, T

UNIMOD:1612

dHex(2)Hex(4)

940.327112

dHex(2) Hex(4)

S, T

UNIMOD:1614

dHex(2) HexNAc(2) Kdn

948.34343

dHex(2)HexNAc(2)Kdn(1)

S, T

UNIMOD:1615

dHex Hex(2) HexNAc(2) Sulf

956.279116

dHex(1)Hex(2)HexNAc(2)Sulf(1)

S, T

UNIMOD:1616

dHex HexNAc(4)

958.375399

dHex(1)HexNAc(4)

S, T

UNIMOD:1617

Hex HexNAc NeuAc NeuGc

963.317944

Hex(1)HexNAc(1)NeuAc(1)NeuGc(1)

S, T

UNIMOD:1618

dHex(1)Hex(1)HexNAc(2)Kdn(1)

964.338345

dHex Hex HexNAc(2) Kdn —OR— Hex(2) HexNAc dHex NeuAc

S, T

UNIMOD:1619

Hex HexNAc NeuGc(2)

979.312859

Hex(1)HexNAc(1)NeuGc(2)

S, T

UNIMOD:1620

Ac Hex HexNAc NeuAc(2)

989.333594

Hex(1)HexNAc(1)NeuAc(2)Ac(1)

S, T

UNIMOD:1621

dHex(2) Hex(2) HexA HexNAc

995.332925

dHex(2)Hex(2)HexA(1)HexNAc(1)

S, T

UNIMOD:1622

dHex Hex HexNAc(3) Sulf

997.305665

dHex(1)Hex(1)HexNAc(3)Sulf(1)

S, T

UNIMOD:1623

Hex(2) HexA NeuAc Pent Sulf

1003.232225

Hex(2)HexA(1)NeuAc(1)Pent(1)Sulf(1)

S, T

UNIMOD:1624

dHex Hex HexNAc(2) NeuAc

1005.364894

dHex(1)Hex(1)HexNAc(2)NeuAc(1)

S, T

UNIMOD:1625

dHex Hex(3) HexA HexNAc

1011.32784

dHex(1)Hex(3)HexA(1)HexNAc(1)

S, T

UNIMOD:1626

Hex(2) HexNAc(3) Sulf

1013.300579

Hex(2)HexNAc(3)Sulf(1)

S, T

UNIMOD:1627

Hex(5) HexNAc

1013.34349

Hex(5)HexNAc(1)

S, N, T

UNIMOD:1628

HexNAc(5)

1015.396863

S, T

UNIMOD:1630

Ac(2) Hex HexNAc NeuAc(2)

1031.344159

Hex(1)HexNAc(1)NeuAc(2)Ac(2)

S, T

UNIMOD:1631

Hex(2) HexNAc(2) NeuGc

1037.354723

Hex(2)HexNAc(2)NeuGc(1)

S, T

UNIMOD:1632

Hex(5)Phos(3)

1050.16311

Hex(5) Phos(3)

S, T

UNIMOD:1633

Hex(6) Phos

1052.283272

Hex(6)Phos(1)

S, T

UNIMOD:1634

dHex Hex(2) HexA HexNAc(2)

1052.354389

dHex(1)Hex(2)HexA(1)HexNAc(2)

S, T

UNIMOD:1635

dHex(2) Hex(3) HexNAc Sulf

1061.310475

dHex(2)Hex(3)HexNAc(1)Sulf(1)

S, T

UNIMOD:1636

Hex HexNAc(3) NeuAc

1062.386358

Hex(1)HexNAc(3)NeuAc(1)

S, T

UNIMOD:1637

dHex(2) Hex HexNAc(3)

1063.406759

dHex(2)Hex(1)HexNAc(3)

S, T

UNIMOD:1638

Hex HexNAc(3) NeuGc

1078.381273

Hex(1)HexNAc(3)NeuGc(1)

S, T

UNIMOD:1639

dHex Hex HexNAc(2) NeuAc Sulf

1085.321709

dHex(1)Hex(1)HexNAc(2)NeuAc(1)Sulf(1)

S, T

UNIMOD:1640

dHex Hex(3) HexA HexNAc Sulf

1091.284655

dHex(1)Hex(3)HexA(1)HexNAc(1)Sulf(1)

S, T

UNIMOD:1641

dHex Hex HexA HexNAc(3)

1093.380938

dHex(1)Hex(1)HexA(1)HexNAc(3)

S, T

UNIMOD:1642

Hex(2) HexNAc(2) NeuAc Sulf

1101.316623

Hex(2)HexNAc(2)NeuAc(1)Sulf(1)

S, T

UNIMOD:1643

dHex(2) Hex(2) HexNAc(2) Sulf

1102.337025

dHex(2)Hex(2)HexNAc(2)Sulf(1)

S, T

UNIMOD:1644

dHex(2)Hex(1)HexNAc(2)Kdn(1)

1110.396254

dHex(2) Hex HexNAc(2) Kdn —OR— Hex(2) HexNAc dHex(2) NeuAc

S, T

UNIMOD:1645

dHex Hex HexNAc(4)

1120.428223

dHex(1)Hex(1)HexNAc(4)

S, T

UNIMOD:1646

Hex(2)HexNAc(4)

1136.423137

Hex(2) HexNAc(4)

S, N, T

UNIMOD:1647

Hex(2) HexNAc NeuGc(2)

1141.365682

Hex(2)HexNAc(1)NeuGc(2)

S, T

UNIMOD:1648

dHex(2) Hex(4) HexNAc

1143.406484

dHex(2)Hex(4)HexNAc(1)

S, T

UNIMOD:1649

Hex HexNAc(2) NeuAc(2)

1150.402402

Hex(1)HexNAc(2)NeuAc(2)

S, T

UNIMOD:1650

dHex(2) Hex HexNAc(2) NeuAc

1151.422803

dHex(2)Hex(1)HexNAc(2)NeuAc(1)

S, T

UNIMOD:1651

dHex Hex(2) HexNAc(3) Sulf

1159.358488

dHex(1)Hex(2)HexNAc(3)Sulf(1)

S, T

UNIMOD:1652

dHex HexNAc(5)

1161.454772

dHex(1)HexNAc(5)

S, T

UNIMOD:1653

dHex(2)Hex(1)HexNAc(2)NeuGc(1)

1167.417718

dHex(2) Hex HexNAc(2) NeuGc —OR— Hex(2) HexNAc(2) dHex NeuAc —OR— Hex HexNAc(3) dHex Kdn

S, T

UNIMOD:1654

dHex(3)Hex(2)HexNAc(2)

1168.438119

dHex(3) Hex(2) HexNAc(2)

S, T

UNIMOD:1655

Hex(3) HexNAc(3) Sulf

1175.353403

Hex(3)HexNAc(3)Sulf(1)

S, N, T

UNIMOD:1656

dHex(2)Hex(2)HexNAc(2)Sulf(2)

1182.293839

dHex(2) Hex(2) HexNAc(2) Sulf(2)

S, T

UNIMOD:1657

dHex(1)Hex(2)HexNAc(2)NeuGc(1)

1183.412632

dHex Hex(2) HexNAc(2) NeuGc —OR— Hex(3) HexNAc(2) NeuAc

S, N, T

UNIMOD:1658

dHex Hex HexNAc(3) NeuAc

1208.444267

dHex(1)Hex(1)HexNAc(3)NeuAc(1)

S, T

UNIMOD:1659

Hex(6)Phos(3)

1212.215934

Hex(6) Phos(3)

S, T

UNIMOD:1660

dHex Hex(3) HexA HexNAc(2)

1214.407213

dHex(1)Hex(3)HexA(1)HexNAc(2)

S, T

UNIMOD:1661

dHex(1)Hex(1)HexNAc(3)NeuGc(1)

1224.439181

dHex Hex HexNAc(3) NeuGc —OR— Hex(2) HexNAc(3) NeuAc

S, T

UNIMOD:1662

Hex HexNAc(2) NeuAc(2) Sulf

1230.359217

Hex(1)HexNAc(2)NeuAc(2)Sulf(1)

S, T

UNIMOD:1663

dHex(2) Hex(3) HexA HexNAc Sulf

1237.342563

dHex(2)Hex(3)HexA(1)HexNAc(1)Sulf(1)

S, T

UNIMOD:1664

Hex HexNAc NeuAc(3)

1238.418446

Hex(1)HexNAc(1)NeuAc(3)

S, T

UNIMOD:1665

Hex(2) HexNAc(3) NeuGc

1240.434096

Hex(2)HexNAc(3)NeuGc(1)

S, T

UNIMOD:1666

dHex Hex(2) HexNAc(2) NeuAc Sulf

1247.374532

dHex(1)Hex(2)HexNAc(2)NeuAc(1)Sulf(1)

S, T

UNIMOD:1667

dHex(3) Hex HexNAc(2) Kdn

1256.454163

dHex(3)Hex(1)HexNAc(2)Kdn(1)

S, T

UNIMOD:1668

dHex(2) Hex(3) HexNAc(2) Sulf

1264.389848

dHex(2)Hex(3)HexNAc(2)Sulf(1)

S, T

UNIMOD:1669

dHex(2)Hex(2)HexNAc(2)Kdn(1)

1272.449077

dHex(2) Hex(2) HexNAc(2) Kdn

S, T

UNIMOD:1670

dHex(2) Hex(2) HexA HexNAc(2) Sulf

1278.369113

dHex(2)Hex(2)HexA(1)HexNAc(2)Sulf(1)

S, T

UNIMOD:1671

dHex Hex(2) HexNAc(4)

1282.481046

dHex(1)Hex(2)HexNAc(4)

S, N, T

UNIMOD:1672

Hex HexNAc NeuGc(3)

1286.40319

Hex(1)HexNAc(1)NeuGc(3)

S, T

UNIMOD:1673

dHex Hex HexNAc(3) NeuAc Sulf

1288.401081

dHex(1)Hex(1)HexNAc(3)NeuAc(1)Sulf(1)

S, T

UNIMOD:1674

dHex Hex(3) HexA HexNAc(2) Sulf

1294.364027

dHex(1)Hex(3)HexA(1)HexNAc(2)Sulf(1)

S, T

UNIMOD:1675

dHex Hex HexNAc(2) NeuAc(2)

1296.460311

dHex(1)Hex(1)HexNAc(2)NeuAc(2)

S, T

UNIMOD:1676

dHex(3) HexNAc(3) Kdn

1297.480712

dHex(3)HexNAc(3)Kdn(1)

S, T

UNIMOD:1678

Hex(2) HexNAc(3) NeuAc Sulf

1304.395996

Hex(2)HexNAc(3)NeuAc(1)Sulf(1)

S, T

UNIMOD:1679

dHex(2)Hex(2)HexNAc(3)Sulf(1)

1305.416397

dHex(2) Hex(2) HexNAc(3) Sulf

S, T

UNIMOD:1680

dHex(2)HexNAc(5)

1307.512681

dHex(2) HexNAc(5)

S, T

UNIMOD:1681

Hex(2)HexNAc(2)NeuAc(2)

1312.455225

Hex(2) HexNAc(2) NeuAc(2)

S, T

UNIMOD:1682

dHex(2)Hex(2)HexNAc(2)NeuAc(1)

1313.475627

dHex(2) Hex(2) HexNAc(2) NeuAc —OR— Hex HexNAc(3) dHex(2) Kdn

S, T

UNIMOD:1683

dHex Hex(3) HexNAc(3) Sulf

1321.411312

dHex(1)Hex(3)HexNAc(3)Sulf(1)

S, T

UNIMOD:1684

dHex(2)Hex(2)HexNAc(2)NeuGc(1)

1329.470541

dHex(2) Hex(2) HexNAc(2) NeuGc —OR— Hex(3) HexNAc(2) dHex NeuAc —OR— Hex(2) HexNAc(3) dHex Kdn

S, T

UNIMOD:1685

Hex(2)HexNAc(5)

1339.50251

Hex(2) HexNAc(5)

S, T

UNIMOD:1686

dHex Hex(3) HexNAc(2) NeuGc

1345.465456

dHex(1)Hex(3)HexNAc(2)NeuGc(1)

S, T

UNIMOD:1687

Hex HexNAc(3) NeuAc(2)

1353.481775

Hex(1)HexNAc(3)NeuAc(2)

S, T

UNIMOD:1688

dHex Hex(2) HexNAc(3) NeuAc

1370.49709

dHex(1)Hex(2)HexNAc(3)NeuAc(1)

S, T

UNIMOD:1689

dHex(3)Hex(2)HexNAc(3)

1371.517491

dHex(3) Hex(2) HexNAc(3)

S, T

UNIMOD:1690

Hex(7)Phos(3)

1374.268757

Hex(7) Phos(3)

S, T

UNIMOD:1691

dHex Hex(4) HexA HexNAc(2)

1376.460036

dHex(1)Hex(4)HexA(1)HexNAc(2)

S, T

UNIMOD:1692

Hex(3)HexNAc(3)NeuAc(1)

1386.492005

Hex(3) HexNAc(3) NeuAc —OR— Hex(2) HexNAc(3) dHex NeuGc —OR— Hex(2) HexNAc(4) Kdn

S, T

UNIMOD:1693

dHex(1)Hex(3)HexA(2)HexNAc(2)

1390.439301

dHex Hex(3) HexA(2) HexNAc(2)

S, T

UNIMOD:1694

Hex(2) HexNAc(2) NeuAc(2) Sulf

1392.41204

Hex(2)HexNAc(2)NeuAc(2)Sulf(1)

S, T

UNIMOD:1695

dHex(2) Hex(2) HexNAc(2) NeuAc Sulf

1393.432441

dHex(2)Hex(2)HexNAc(2)NeuAc(1)Sulf(1)

S, T

UNIMOD:1696

Hex(3) HexNAc(3) NeuGc

1402.48692

Hex(3)HexNAc(3)NeuGc(1)

S, T

UNIMOD:1697

dHex(4) Hex HexNAc(2) Kdn

1402.512072

dHex(4)Hex(1)HexNAc(2)Kdn(1)

S, T

UNIMOD:1698

dHex(3)Hex(2)HexNAc(2)Kdn(1)

1418.506986

dHex(3) Hex(2) HexNAc(2) Kdn

S, T

UNIMOD:1699

dHex(3) Hex(2) HexA HexNAc(2) Sulf

1424.427021

dHex(3)Hex(2)HexA(1)HexNAc(2)Sulf(1)

S, T

UNIMOD:1700

Hex(2) HexNAc(4) NeuAc

1427.518554

Hex(2)HexNAc(4)NeuAc(1)

S, T

UNIMOD:1701

dHex(2)Hex(2)HexNAc(4)

1428.538955

dHex(2) Hex(2) HexNAc(4)

S, T

UNIMOD:1702

dHex(2) Hex(3) HexA HexNAc(2) Sulf

1440.421936

dHex(2)Hex(3)HexA(1)HexNAc(2)Sulf(1)

S, T

UNIMOD:1703

dHex(4) HexNAc(3) Kdn

1443.538621

dHex(4)HexNAc(3)Kdn(1)

S, T

UNIMOD:1705

Hex(2) HexNAc NeuGc(3)

1448.456013

Hex(2)HexNAc(1)NeuGc(3)

S, T

UNIMOD:1706

dHex(4) Hex HexNAc Kdn(2)

1449.501567

dHex(4)Hex(1)HexNAc(1)Kdn(2)

S, T

UNIMOD:1707

dHex Hex(2) HexNAc(3) NeuAc Sulf

1450.453905

dHex(1)Hex(2)HexNAc(3)NeuAc(1)Sulf(1)

S, T

UNIMOD:1708

dHex Hex(2) HexNAc(2) NeuAc(2)

1458.513134

dHex(1)Hex(2)HexNAc(2)NeuAc(2)

S, T

UNIMOD:1709

dHex(3) Hex HexNAc(3) Kdn

1459.533535

dHex(3)Hex(1)HexNAc(3)Kdn(1)

S, T

UNIMOD:1711

Hex(3) HexNAc(3) NeuAc Sulf

1466.44882

Hex(3)HexNAc(3)NeuAc(1)Sulf(1)

S, T

UNIMOD:1712

Hex(3)HexNAc(2)NeuAc(2)

1474.508049

Hex(3) HexNAc(2) NeuAc(2)

S, T

UNIMOD:1713

Hex(3) HexNAc(3) NeuGc Sulf

1482.443734

Hex(3)HexNAc(3)NeuGc(1)Sulf(1)

S, T

UNIMOD:1714

dHex(1)Hex(2)HexNAc(2)NeuGc(2)

1490.502964

dHex Hex(2) HexNAc(2) NeuGc(2)

S, T

UNIMOD:1715

dHex(2)Hex(3)HexNAc(2)NeuGc(1)

1491.523365

dHex(2) Hex(3) HexNAc(2) NeuGc —OR— Hex(4) HexNAc(2) dHex NeuAc

S, T

UNIMOD:1716

dHex Hex(3) HexA HexNAc(3) Sulf

1497.4434

dHex(1)Hex(3)HexA(1)HexNAc(3)Sulf(1)

S, T

UNIMOD:1717

Hex(2)HexNAc(3)NeuAc(2)

1515.534598

Hex(2) HexNAc(3) NeuAc(2)

S, T

UNIMOD:1718

dHex(2) Hex(2) HexNAc(3) NeuAc

1516.554999

dHex(2)Hex(2)HexNAc(3)NeuAc(1)

S, T

UNIMOD:1719

dHex(4)Hex(2)HexNAc(3)

1517.5754

dHex(4) Hex(2) HexNAc(3)

S, T

UNIMOD:1720

Hex(2) HexNAc(3) NeuAc NeuGc

1531.529513

Hex(2)HexNAc(3)NeuAc(1)NeuGc(1)

S, T

UNIMOD:1721

dHex(2)Hex(2)HexNAc(3)NeuGc(1)

1532.549914

dHex(2) Hex(2) HexNAc(3) NeuGc —OR— Hex(3) HexNAc(3) dHex NeuAc —OR— Hex(2) HexNAc(4) dHex Kdn

S, T

UNIMOD:1722

dHex(3)Hex(3)HexNAc(3)

1533.570315

dHex(3) Hex(3) HexNAc(3)

S, T

UNIMOD:1723

Hex(8)Phos(3)

1536.321581

Hex(8) Phos(3)

S, T

UNIMOD:1724

dHex Hex(2) HexNAc(2) NeuAc(2) Sulf

1538.469949

dHex(1)Hex(2)HexNAc(2)NeuAc(2)Sulf(1)

S, T

UNIMOD:1725

Hex(2)HexNAc(3)NeuGc(2)

1547.524427

Hex(2) HexNAc(3) NeuGc(2)

S, T

UNIMOD:1726

dHex(4) Hex(2) HexNAc(2) Kdn

1564.564895

dHex(4)Hex(2)HexNAc(2)Kdn(1)

S, T

UNIMOD:1727

dHex Hex(2) HexNAc(4) NeuAc

1573.576463

dHex(1)Hex(2)HexNAc(4)NeuAc(1)

S, T

UNIMOD:1728

dHex(3)Hex(2)HexNAc(4)

1574.596864

dHex(3) Hex(2) HexNAc(4)

S, T

UNIMOD:1729

Hex HexNAc NeuGc(4)

1593.493521

Hex(1)HexNAc(1)NeuGc(4)

S, T

UNIMOD:1730

dHex(4) Hex HexNAc(3) Kdn

1605.591444

dHex(4)Hex(1)HexNAc(3)Kdn(1)

S, T

UNIMOD:1732

Hex(4)HexNAc(4)Sulf(2)

1620.442414

Hex(4) HexNAc(4) Sulf(2)

S, T

UNIMOD:1733

dHex(3)Hex(2)HexNAc(3)Kdn(1)

1621.586359

dHex(3) Hex(2) HexNAc(3) Kdn —OR— Hex(3) HexNAc(2) dHex(3) NeuAc

S, T

UNIMOD:1735

dHex(2)Hex(2)HexNAc(5)

1631.618328

dHex(2) Hex(2) HexNAc(5)

S, T

UNIMOD:1736

dHex(2) Hex(3) HexA HexNAc(3) Sulf

1643.501309

dHex(2)Hex(3)HexA(1)HexNAc(3)Sulf(1)

S, T

UNIMOD:1737

dHex Hex(4) HexA HexNAc(3) Sulf

1659.496223

dHex(1)Hex(4)HexA(1)HexNAc(3)Sulf(1)

S, T

UNIMOD:1738

Hex(3)HexNAc(3)NeuAc(2)

1677.587422

Hex(3) HexNAc(3) NeuAc(2)

S, T

UNIMOD:1739

dHex(2)Hex(3)HexNAc(3)NeuAc(1)

1678.607823

dHex(2) Hex(3) HexNAc(3) NeuAc —OR— Hex(2) HexNAc(4) dHex(2) Kdn

S, T

UNIMOD:1740

dHex(4)Hex(3)HexNAc(3)

1679.628224

dHex(4) Hex(3) HexNAc(3)

S, T

UNIMOD:1742

Hex(9)Phos(3)

1698.374404

Hex(9) Phos(3)

S, T

UNIMOD:1743

dHex(2)HexNAc(7)

1713.671426

dHex(2) HexNAc(7)

S, T

UNIMOD:1744

Hex(2) HexNAc NeuGc(4)

1755.546345

Hex(2)HexNAc(1)NeuGc(4)

S, T

UNIMOD:1745

Hex(3) HexNAc(3) NeuAc(2) Sulf

1757.544236

Hex(3)HexNAc(3)NeuAc(2)Sulf(1)

S, T

UNIMOD:1746

dHex(2)Hex(3)HexNAc(5)

1793.671151

dHex(2) Hex(3) HexNAc(5)

S, N, T

UNIMOD:1747

dHex Hex(2) HexNAc(2) NeuGc(3)

1797.593295

dHex(1)Hex(2)HexNAc(2)NeuGc(3)

S, T

UNIMOD:1748

dHex(2) Hex(4) HexA HexNAc(3) Sulf

1805.554132

dHex(2)Hex(4)HexA(1)HexNAc(3)Sulf(1)

S, T

UNIMOD:1749

Hex(2)HexNAc(3)NeuAc(3)

1806.630015

Hex(2) HexNAc(3) NeuAc(3)

S, T

UNIMOD:1750

dHex(1)Hex(3)HexNAc(3)NeuAc(2)

1823.64533

dHex Hex(3) HexNAc(3) NeuAc(2)

S, T

UNIMOD:1751

dHex(3)Hex(3)HexNAc(3)NeuAc(1)

1824.665732

dHex(3) Hex(3) HexNAc(3) NeuAc

S, T

UNIMOD:1752

Hex(2)HexNAc(3)NeuGc(3)

1854.614759

Hex(2) HexNAc(3) NeuGc(3)

S, T

UNIMOD:1753

Hex(10)Phos(3)

1860.427228

Hex(10) Phos(3)

S, T

UNIMOD:1754

dHex(1)Hex(2)HexNAc(4)NeuAc(2)

1864.67188

dHex Hex(2) HexNAc(4) NeuAc(2)

S, T

UNIMOD:1755

Hex HexNAc NeuGc(5)

1900.583852

Hex(1)HexNAc(1)NeuGc(5)

S, T

UNIMOD:1756

Hex(4) HexNAc(4) NeuAc Sulf(2)

1911.53783

Hex(4)HexNAc(4)NeuAc(1)Sulf(2)

S, T

UNIMOD:1757

Hex(4) HexNAc(4) NeuGc Sulf(2)

1927.532745

Hex(4)HexNAc(4)NeuGc(1)Sulf(2)

S, T

UNIMOD:1758

dHex(2)Hex(3)HexNAc(3)NeuAc(2)

1969.703239

dHex(2) Hex(3) HexNAc(3) NeuAc(2)

S, T

UNIMOD:1759

Hex(4)HexNAc(4)NeuAc(1)Sulf(3)

1991.494645

Hex(4) HexNAc(4) NeuAc Sulf(3)

S, T

UNIMOD:1760

dHex(2)Hex(2)HexNAc(2)

1022.38021

dHex(2) Hex(2) HexNAc(2)

S, N, T

UNIMOD:1761

dHex Hex(3) HexNAc(2)

1038.375125

dHex(1)Hex(3)HexNAc(2)

S, N, T

UNIMOD:1762

dHex Hex(2) HexNAc(3)

1079.401674

dHex(1)Hex(2)HexNAc(3)

S, N, T

UNIMOD:1763

Hex(3)HexNAc(3)

1095.396588

Hex(3) HexNAc(3)

S, N, T

UNIMOD:1764

dHex Hex(3) HexNAc(2) Sulf

1118.331939

dHex(1)Hex(3)HexNAc(2)Sulf(1)

S, N, T

UNIMOD:1765

dHex(2)Hex(3)HexNAc(2)

1184.433033

dHex(2) Hex(3) HexNAc(2)

S, N, T

UNIMOD:1766

dHex Hex(4) HexNAc(2)

1200.427948

dHex(1)Hex(4)HexNAc(2)

S, N, T

UNIMOD:1767

dHex(2)Hex(2)HexNAc(3)

1225.459583

dHex(2) Hex(2) HexNAc(3)

S, N, T

UNIMOD:1768

dHex Hex(3) HexNAc(3)

1241.454497

dHex(1)Hex(3)HexNAc(3)

S, N, T

UNIMOD:1769

Hex(4)HexNAc(3)

1257.449412

Hex(4) HexNAc(3)

S, N, T

UNIMOD:1770

dHex(2)Hex(4)HexNAc(2)

1346.485857

dHex(2) Hex(4) HexNAc(2)

S, N, T

UNIMOD:1771

dHex(2)Hex(3)HexNAc(3)

1387.512406

dHex(2) Hex(3) HexNAc(3)

S, N, T

UNIMOD:1772

A3

1501.555334

Hex(3)HexNAc(5)

S, N, T

UNIMOD:1773

Hex(4)HexNAc(3)NeuAc(1)

1548.544828

Hex(4) HexNAc(3) NeuAc —OR— Hex(3) HexNAc(4) Kdn

S, N, T

UNIMOD:1774

dHex(2)Hex(3)HexNAc(4)

1590.591779

dHex(2) Hex(3) HexNAc(4)

S, N, T

UNIMOD:1775

dHex Hex(3) HexNAc(5)

1647.613242

dHex(1)Hex(3)HexNAc(5)

S, N, T

UNIMOD:1776

A4

1704.634706

Hex(3)HexNAc(6)

S, N, T

UNIMOD:1777

Hex(4) HexNAc(4) NeuAc

1751.624201

Hex(4)HexNAc(4)NeuAc(1)

S, N, T

UNIMOD:1778

dHex(2)Hex(4)HexNAc(4)

1752.644602

dHex(2) Hex(4) HexNAc(4) —OR— Hex(4) HexNAc(4) dHex Pent Me

S, N, T

UNIMOD:1779

Hex(6)HexNAc(4)

1784.634431

Hex(6) HexNAc(4)

S, N, T

UNIMOD:1780

Hex(5)HexNAc(5)

1825.660981

Hex(5) HexNAc(5)

S, N, T

UNIMOD:1781

dHex Hex(3) HexNAc(6)

1850.692615

dHex(1)Hex(3)HexNAc(6)

S, N, T

UNIMOD:1782

dHex(1)Hex(4)HexNAc(4)NeuAc(1)

1897.68211

dHex Hex(4) HexNAc(4) NeuAc —OR— Hex(3) HexNAc(5) dHex Kdn

S, N, T

UNIMOD:1783

dHex(3)Hex(4)HexNAc(4)

1898.702511

dHex(3) Hex(4) HexNAc(4)

S, N, T

UNIMOD:1784

dHex Hex(3) HexNAc(5) NeuAc

1938.708659

dHex(1)Hex(3)HexNAc(5)NeuAc(1)

S, N, T

UNIMOD:1785

dHex(2)Hex(4)HexNAc(5)

1955.723975

dHex(2) Hex(4) HexNAc(5)

S, N, T

UNIMOD:1786

Ac Hex HexNAc NeuAc

698.238177

Hex(1)HexNAc(1)NeuAc(1)Ac(1)

S, T

UNIMOD:1787

K4

4.00078

13C(2) 15N(2), Label:13C(2)15N(2)

K

UNIMOD:1789

Xlink:DSS[155]

155.094629

Ammonium-quenched monolink of DSS/BS3 crosslinker

N-term, K

UNIMOD:1799

NQIGG

469.228496

SUMOylation by Giardia lamblia

K

UNIMOD:1800

Carboxyethylpyrrole

122.036779

K

UNIMOD:1801

Fluorescein-tyramine

493.116152

Fluorescein-tyramine adduct by peroxidase activity

Y

UNIMOD:1824

GEE

86.036779

transamidation of glycine ethyl ester to glutamine

Q

UNIMOD:1825

RNPXL

324.035867

Simulate peptide-RNA conjugates

K @ N-term, R @ N-term

UNIMOD:1826

Glu->pyro-Glu+Methyl

-3.994915

Pyro-Glu from E + Methylation

E @ N-term

UNIMOD:1827

Glu->pyro-Glu+Methyl:2H(2)13C(1)

-0.979006

Pyro-Glu from E + Methylation Medium

E @ N-term

UNIMOD:1828

LRGG+methyl

397.243753

LRGG ubiquitin remnant with methylated Arg, LeumethylArgGlyGly

K

UNIMOD:1829

LRGG+dimethyl

411.259403

LeudimethylArgGlyGly, LRGG ubiquitin remnant with dimethylated Arg

K

UNIMOD:1830

Biotin-Phenol

361.146012

Biotin-tyramide

Y

UNIMOD:1831

Tris

104.071154

tris adduct at asparagine, tris adduct causes 104 Da addition at asparagine-succinimide intermediate, tris adduct at IgG causes 104 Da addition

N

UNIMOD:1832

IASD

452.034807

Iodoacetamide derivative of stilbene (reaction product with thiol), 4-acetamido-4’-((iodoacetyl)amino)stilbene-2,2’-disulfonic acid

C

UNIMOD:1833

NP40

220.182715

NP-40 synthetic polymer terminus

N-term

UNIMOD:1834

Tween20

165.164326

Tween 20 synthetic polymer terminus

N-term

UNIMOD:1835

Tween80

263.237491

Tween 80 synthetic polymer terminus

C-term

UNIMOD:1836

Triton

188.156501

Triton synthetic polymer terminus

C-term, N-term

UNIMOD:1837

Brij35

168.187801

Brij 35 synthetic polymer terminus

N-term

UNIMOD:1838

Brij58

224.250401

Brij 58 synthetic polymer terminus

N-term

UNIMOD:1839

betaFNA

454.210387

beta-Funaltrexamine

K, C

UNIMOD:1840

dHex(1)Hex(7)HexNAc(4)

2092.745164

Fucosylated biantennary + 2 alphaGal

N

UNIMOD:1841

Biotin:Thermo-21328

389.090154

also Biotin:Thermo-21331, EZ-Link Sulfo-NHS-SS-Biotin

N-term, K

UNIMOD:1842

Xlink:DSSO

158.003765

disuccinimidyl sulfoxide CID cleavable cross-link

N-term, K

UNIMOD:1843

CMP

305.041287

Cytidine monophosphate, PhosphoCytidine

S, Y, T

UNIMOD:1844

Xlink:EGS

226.047738

Ethylene glycolbis(succinimidylsuccinate), EGS cross-linker

N-term, K

UNIMOD:1845

AzidoF

41.001397

Azidophenylalanine

F

UNIMOD:1846

Dimethylaminoethyl

71.073499

Cys alkylation by dimethylaminoethyl halide

C

UNIMOD:1848

Glutarylation

114.031694

Gluratylation

K

UNIMOD:1849

hydroxyisobutyryl

86.036779

2-hydroxyisobutanoyl, 3-hydroxybutanoyl, 2-hydroxyisobutyrylation

K

UNIMOD:1868

MeMePhosphorothioate

107.979873

S-Methyl Methyl phosphorothioate

S

UNIMOD:1870

Cation:Fe[III]

52.911464

Replacement of 3 protons by iron

D, C-term, E

UNIMOD:1871

DTT

151.996571

DTT adduct of cysteine

C

UNIMOD:1872

DYn-2

161.09664

Sulfenic Acid specific probe

C

UNIMOD:1873

MesitylOxide

98.073165

Acetone chemical artifact

N-term, K, H

UNIMOD:1875

methylol

30.010565

formaldehyde induced modifications

W, K, Y

UNIMOD:1876

Xlink:DSS

138.06808

disuccinimidyl suberate (DSS), bis[sulfosuccinimidyl] suberate (BS3)

N-term, K

UNIMOD:1877

Xlink:DSS[259]

259.141973

Tris-quenched monolink of DSS/BS3 crosslinker

N-term, K

UNIMOD:1878

Xlink:DSSO[176]

176.01433

Water-quenched monolink of DSSO crosslinker

N-term, K

UNIMOD:1879

Xlink:DSSO[175]

175.030314

Ammonia-quenched monolink of DSSO crosslinker

N-term, K

UNIMOD:1880

Xlink:DSSO[279]

279.077658

Tris-quenched monolink of DSSO crosslinker

N-term, K

UNIMOD:1881

Xlink:DSSO[54]

54.010565

Alkene fragment of DSSO crosslinker

N-term, K

UNIMOD:1882

Xlink:DSSO[86]

85.982635

Thiol fragment of DSSO crosslinker

N-term, K

UNIMOD:1883

Xlink:DSSO[104]

103.9932

Sulfenic acid fragment of DSSO crosslinker

N-term, K

UNIMOD:1884

Xlink:BuUrBu

196.084792

Also known as DSBU, disuccinimidyl dibutyric urea CID cleavable cross-link

N-term, K

UNIMOD:1885

Xlink:BuUrBu[111]

111.032028

BuUr fragment of BuUrBu crosslinker

N-term, K

UNIMOD:1886

Xlink:BuUrBu[85]

85.052764

Bu fragment of BuUrBu crosslinker

N-term, K

UNIMOD:1887

Xlink:BuUrBu[213]

213.111341

Ammonia quenched monolink of BuUrBu crosslinker

N-term, K

UNIMOD:1888

Xlink:BuUrBu[214]

214.095357

Water quenched monolink of BuUrBu crosslinker

N-term, K

UNIMOD:1889

Xlink:BuUrBu[317]

317.158686

Tris quenched monolink of BuUrBu crosslinker

N-term, K

UNIMOD:1891

Xlink:DTBP

172.01289

dimethyl 3,3'-dithiobispropionimidate

N-term, K

UNIMOD:1892

Xlink:DST

113.995309

Disuccinimidyl tartrate crosslinker

N-term, K

UNIMOD:1893

Xlink:DTSSP

173.980921

dithiobis[succinimidylpropionate], Also known as DSP

N-term, K

UNIMOD:1895

Xlink:SMCC

219.089543

succinimidyl 4-[N-maleimidomethyl]cyclohexane-1-carboxylate

N-term, K, C

UNIMOD:1896

Xlink:DSSO[158]

158.003765

Intact DSSO crosslinker

N-term, K

UNIMOD:1897

Xlink:EGS[226]

226.047738

Ethylene glycolbis(succinimidylsuccinate), Intact EGS cross-linker

N-term, K

UNIMOD:1898

Xlink:DSS[138]

138.06808

Intact DSS/BS3 crosslinker

N-term, K

UNIMOD:1899

Xlink:BuUrBu[196]

196.084792

Intact BuUrBu crosslinker

N-term, K

UNIMOD:1900

Xlink:DTBP[172]

172.01289

Intact DTBP crosslinker, dimethyl 3,3’-dithiobispropionimidate

N-term, K

UNIMOD:1901

Xlink:DST[114]

113.995309

Intact DST crosslinker

N-term, K

UNIMOD:1902

Xlink:DTSSP[174]

173.980921

Intact DSP/DTSSP crosslinker, Also known as DSP

N-term, K

UNIMOD:1903

Xlink:SMCC[219]

219.089543

Intact SMCC cross-link

N-term, K, C

UNIMOD:1905

Xlink:BS2G[96]

96.021129

Intact BS2-G crosslinker

N-term, K

UNIMOD:1906

Xlink:BS2G[113]

113.047679

Ammonium-quenched monolink of BS2-G crosslinker

N-term, K

UNIMOD:1907

Xlink:BS2G[114]

114.031694

Water-quenched monolink of BS2-G crosslinker

N-term, K

UNIMOD:1908

Xlink:BS2G[217]

217.095023

Tris-quenched monolink of BS2-G crosslinker

N-term, K

UNIMOD:1909

Xlink:BS2G

96.021129

bis[sulfosuccinimidyl] glutarate

N-term, K

UNIMOD:1910

Cation:Al[III]

23.958063

Replacement of 3 protons by aluminium

D, C-term, E

UNIMOD:1911

Xlink:DMP[139]

139.110947

Dimethyl pimelimidate dead-end crosslink, Ammonia quenched monolink of DMP crosslinker

N-term, K

UNIMOD:1912

Xlink:DMP[122]

122.084398

Intact DMP crosslinker, Dimethyl pimelimidate

N-term, K

UNIMOD:1913

glyoxalAGE

21.98435

glyoxal-derived AGE

R

UNIMOD:1914

Met->AspSA

-32.008456

Methionine oxidation to aspartic semialdehyde

M

UNIMOD:1915

Decarboxylation

-30.010565

D, E

UNIMOD:1916

Aspartylurea

-10.031969

H

UNIMOD:1917

Formylasparagine

4.97893

In Bachi as Formylaspargine (typo?)

H

UNIMOD:1918

Carbonyl

13.979265

aldehyde and ketone modifications

V, A, S, R, E, I, Q, L

UNIMOD:1920

AFB1_Dialdehyde

310.047738

adduction of aflatoxin B1 Dialdehyde to lysine

K

UNIMOD:1922

Pro->HAVA

18.010565

Proline oxidation to 5-hydroxy-2-aminovaleric acid

P

UNIMOD:1923

Delta:H(-4)O(2)

27.958529

Tryptophan oxidation to beta-unsaturated-2,4-bis-tryptophandione

W

UNIMOD:1924

Delta:H(-4)O(3)

43.953444

Tryptophan oxidation to hydroxy-bis-tryptophandione

W

UNIMOD:1925

Delta:O(4)

63.979659

Tryptophan oxidation to dihydroxy-N-formaylkynurenine

W

UNIMOD:1926

Delta:H(4)C(3)O(2)

72.021129

methylglyoxal-derived carboxyethyllysine

K

UNIMOD:1927

Delta:H(4)C(5)O(1)

80.026215

methylglyoxal-derived argpyrimidine

R

UNIMOD:1928

Delta:H(10)C(8)O(1)

122.073165

crotonaldehyde-derived dimethyl-FDP-lysine

K

UNIMOD:1929

Delta:H(6)C(7)O(4)

154.026609

methylglyoxal-derived tetrahydropyrimidine

R

UNIMOD:1930

Pent(2)

264.084518

S, T

UNIMOD:1931

Pent HexNAc

335.121631

Pent(1)HexNAc(1)

S, T

UNIMOD:1932

Hex(2)Sulf(1)

404.062462

Hex(2) O(3) S

S, T

UNIMOD:1933

Hex:1 Pent:2 Me:1

440.152991

Hex(1)Pent(2)Me(1)

S, T

UNIMOD:1934

HexNAc(2) Sulf

486.11556

HexNAc(2)Sulf(1)

S, T

UNIMOD:1935

Hex Pent(3) Me

572.19525

Hex(1)Pent(3)Me(1)

S, T

UNIMOD:1936

Hex(2)Pent(2)

588.190165

Hex(2) Pent(2)

S, T

UNIMOD:1937

Hex(2) Pent(2) Me

602.205815

Hex(2)Pent(2)Me(1)

S, T

UNIMOD:1938

Hex(4) HexA

824.243382

Hex(4)HexA(1)

S, T

UNIMOD:1939

Hex(2) HexNAc Pent HexA

835.259366

Hex(2)HexNAc(1)Pent(1)HexA(1)

S, T

UNIMOD:1940

Hex(3) HexNAc HexA

865.269931

Hex(3)HexNAc(1)HexA(1)

S, T

UNIMOD:1941

Hex HexNAc(2) dHex(2) Sulf

940.284201

Hex(1)HexNAc(2)dHex(2)Sulf(1)

S, T

UNIMOD:1942

HexA(2)HexNAc(3)

961.302294

HexA(2) HexNAc(3)

S, T

UNIMOD:1943

dHex Hex(4) HexA

970.301291

dHex(1)Hex(4)HexA(1)

S, T

UNIMOD:1944

Hex(5) HexA

986.296206

Hex(5)HexA(1)

S, T

UNIMOD:1945

Hex(4) HexA HexNAc

1027.322755

Hex(4)HexA(1)HexNAc(1)

S, T

UNIMOD:1946

dHex(3) Hex(3) HexNAc

1127.41157

dHex(3)Hex(3)HexNAc(1)

S, T

UNIMOD:1947

Hex(6) HexNAc

1175.396314

Hex(6)HexNAc(1)

N

UNIMOD:1948

Sulf dHex Hex HexNAc(4)

1200.385037

Hex(1)HexNAc(4)dHex(1)Sulf(1)

S, T

UNIMOD:1949

dHex Hex(2) HexNAc NeuAc(2)

1255.433762

dHex(1)Hex(2)HexNAc(1)NeuAc(2)

S, T

UNIMOD:1950

dHex(3)Hex(3)HexNAc(2)

1330.490942

dHex(3) Hex(3) HexNAc(2)

S, T

UNIMOD:1951

dHex(2) Hex HexNAc(4) Sulf

1346.442946

dHex(2)Hex(1)HexNAc(4)Sulf(1)

S, T

UNIMOD:1952

dHex Hex(2) HexNAc(4) Sulf(2)

1442.394675

dHex(1)Hex(2)HexNAc(4)Sulf(2)

S, T

UNIMOD:1953

Hex(9)

1458.475412

N

UNIMOD:1954

dHex(2)Hex(3)HexNAc(3)Sulf(1)

1467.469221

Sulf dHex(2) Hex(3) HexNAc(3)

S, T

UNIMOD:1955

Me dHex(2) Hex(5) HexNAc(2)

1522.554331

dHex(2)Hex(5)HexNAc(2)Me(1)

S, T

UNIMOD:1956

dHex(2)Hex(2)HexNAc(4)Sulf(2)

1588.452584

Sulf(2) dHex(2) Hex(2) HexNAc(4)

S, T

UNIMOD:1957

Hex(9) HexNAc

1661.554784

Hex(9)HexNAc(1)

N

UNIMOD:1958

dHex(3)Hex(2)HexNAc(4)Sulf(2)

1734.510493

dHex(3) Hex(2) HexNAc(4) Sulf(2)

S, T

UNIMOD:1959

Hex(4) HexNAc(4) NeuGc

1767.619116

Hex(4)HexNAc(4)NeuGc(1)

S, N, T

UNIMOD:1960

dHex(4)Hex(3)HexNAc(2)NeuAc(1)

1767.644268

dHex(4) Hex(3) HexNAc(2) NeuAc(1)

S, T

UNIMOD:1961

Hex(3)HexNAc(5)NeuAc(1)

1792.65075

Hex(3) HexNAc(5) NeuAc(1)

N

UNIMOD:1962

Hex(10)HexNAc(1)

1823.607608

Hex(10) HexNAc(1)

N

UNIMOD:1963

dHex Hex(8) HexNAc(2)

1848.639242

dHex(1)Hex(8)HexNAc(2)

N

UNIMOD:1964

Hex(3)HexNAc(4)NeuAc(2)

1880.666794

Hex(3) HexNAc(4) NeuAc(2)

N

UNIMOD:1965

dHex(2)Hex(3)HexNAc(4)NeuAc(1)

1881.687195

dHex(2) Hex(3) HexNAc(4) NeuAc

N

UNIMOD:1966

dHex(2) Hex(2) HexNAc(6) Sulf

1914.654515

dHex(2)Hex(2)HexNAc(6)Sulf(1)

S, T

UNIMOD:1967

Hex(5) HexNAc(4) NeuAc Ac

1955.687589

Hex(5)HexNAc(4)NeuAc(1)Ac(1)

N

UNIMOD:1968

Hex(3)HexNAc(3)NeuAc(3)

1968.682838

Hex(3) HexNAc(3) NeuAc(3)

S, T

UNIMOD:1969

Hex(5) HexNAc(4) NeuAc Ac(2)

1997.698154

Hex(5)HexNAc(4)NeuAc(1)Ac(2)

N

UNIMOD:1970

Unknown:162

162.125595

The elemental composition should be assumed incorrect, Unidentified modification of 162.1258 found in open search, Although it happens to be that of Diethylene glycol butyl ether, a common solvent.

D, N-term, C-term, E

UNIMOD:1971

Unknown:177

176.744957

The elemental composition should be assumed incorrect, Unidentified modification of 176.7462 found in open search

D, N-term, C-term, E

UNIMOD:1972

Unknown:210

210.16198

Unidentified modification of 210.1616 found in open search, The elemental composition should be assumed incorrect

D, N-term, C-term, E

UNIMOD:1973

Unknown:216

216.099774

Unidentified modification of 216.1002 found in open search, The elemental composition should be assumed incorrect

D, N-term, C-term, E

UNIMOD:1974

Unknown:234

234.073953

Unidentified modification of 234.0742 found in open search, The elemental composition should be assumed incorrect

D, N-term, C-term, E

UNIMOD:1975

Unknown:248

248.19876

The elemental composition should be assumed incorrect, Unidentified modification of 248.1986 found in open search

D, N-term, C-term, E

UNIMOD:1976

Unknown:250

249.981018

Unidentified modification of 249.981 found in open search, The elemental composition should be assumed incorrect

D, N-term, C-term, E

UNIMOD:1977

Unknown:302

301.986514

Unidentified modification of 301.9864 found in open search, The elemental composition should be assumed incorrect

D, N-term, C-term, E

UNIMOD:1978

Unknown:306

306.095082

The elemental composition should be assumed incorrect, Unidentified modification of 306.0952 found in open search

D, N-term, C-term, E

UNIMOD:1979

Unknown:420

420.051719

Unidentified modification of 420.0506 found in open search, Probable non-covalent adduct of CAM-DTT-DTT-CAM

C-term, N-term

UNIMOD:1986

Diethylphosphothione

152.006087

O-diethylphosphothione

H, C, K, S, T, Y

UNIMOD:1987

O-ethylphosphothione

123.974787

Dimethylphosphothione, O-dimethylphosphothione

H, C, K, S, T, Y

UNIMOD:1989

O-methylphosphothione

109.959137

monomethylphosphothione

H, C, K, S, T, Y

UNIMOD:1990

CIGG

330.136176

Ubiquitin D (FAT10) leaving after chymotrypsin digestion Cys-Ile-Gly-Gly

K

UNIMOD:1991

GNLLFLACYCIGG

1324.6308

Ubiquitin D (FAT10) leaving after trypsin digestion Gly-Asn-Leu-Leu-Phe-Leu-Ala-Cys-Tyr-Cys-Ile-Gly-Gly

K

UNIMOD:1992

serotonylation

159.068414

5-glutamyl serotonin

Q

UNIMOD:1993

TMPP-Ac:13C(9)

581.211328

heavy tris(2,4,6-trimethoxyphenyl)phosphonium acetic acid N-hydroxysuccinimide ester derivative

N-term, K, Y

UNIMOD:1999

Xlink:DST[56]

55.989829

DST crosslinker cleaved by sodium periodate

N-term, K

UNIMOD:2000

Xlink:SDA

82.041865

NHS-Diazirine crosslinker

N-term, K

UNIMOD:2001

ZQG

320.100836

carbobenzoxy-L-glutaminyl-glycine

K

UNIMOD:2006

Haloxon

203.950987

O-Dichloroethylphosphate

H, C, K, S, T, Y

UNIMOD:2007

Methamidophos-S

108.975121

S-methyl amino phosphinate

H, C, K, S, T, Y

UNIMOD:2008

Methamidophos-O

92.997965

O-methyl amino phosphinate

H, C, K, S, T, Y

UNIMOD:2014

Nitrene

12.995249

Loss of O2; nitro photochemical decomposition

Y

UNIMOD:2015

shTMT

235.176741

Super Heavy Tandem Mass Tag, Tandem Mass Tag and TMT are registered Trademarks of Proteome Sciences plc., Super Heavy TMT

N-term, K

UNIMOD:2016

TMTpro

304.207146

Also applies to TMTpro 18plex mass tags, TMTpro 16plex Tandem Mass Tag, Tandem Mass Tag and TMT are registered Trademarks of Proteome Sciences plc.

H, N-term, K, S, T

UNIMOD:2017

TMTpro_zero

295.189592

Native TMTpro Tandem Mass Tag, Tandem Mass Tag and TMT are registered Trademarks of Proteome Sciences plc.

H, N-term, K, S, T

UNIMOD:2018

Xlink:EDC

-18.010565

1-ethyl-3-(3-dimethylaminopropyl)carbodiimide hydrochloride, carbodiimide crosslinker

C-term, N-term, K, D, E

UNIMOD:2020

Xlink:Disulfide

-2.01565

Intact disulfide bridge

C

UNIMOD:2022

Ser/Thr-KDO

220.058303

2-Keto-3-Deoxy-D-Mannooctanoic Acid, Glycosylation of Serine/Threonine residues with KDO, 3-deoxy-D-manno-oct-2-ulosonic acid

S, T

UNIMOD:2025

Andro-H2O

332.19876

andrographolide with the loss of H2O

C

UNIMOD:2026

Xlink:KQisopeptide

-17.026549

transglutamination, Isopeptide bond formation with loss of ammonia

Q, K

UNIMOD:2027

His+O(2)

169.048741

Photo-induced histidine adduct

H

UNIMOD:2028

A3G3S3

2861.000054

Hex(6)HexNAc(5)NeuAc(3)

N

UNIMOD:2029

A4G4

2352.846

Hex(7)HexNAc(6)

S, N, T

UNIMOD:2033

Met+O(2)

163.030314

Photo-induced Methionine Adduct

H

UNIMOD:2034

Gly+O(2)

89.011293

Photo-induced Glycine Adduct

H

UNIMOD:2035

Pro+O(2)

129.042593

Photo-induced Proline adduct

H

UNIMOD:2036

Lys+O(2)

160.084792

Photo-induced Lysine adduct

H

UNIMOD:2037

Glu+O(2)

161.032422

Photo-induced Glutamate adduct

H

UNIMOD:2039

LTX+Lophotoxin

416.147118

addition of lophotoxin to tyrosine

Y

UNIMOD:2040

MBS+peptide

1482.77

MBS_233p24 plus peptide 1250p53

C

UNIMOD:2041

3-hydroxybenzyl-phosphate

186.008196

3-hydroxybenzyl phosphate

S, K, T, Y

UNIMOD:2042

phenyl-phosphate

155.997631

phenyl phosphate

S, K, T, Y

UNIMOD:2044

RBS-ID_Uridine

244.069536

RNA-protein UVC-crosslinked, hydrofluoride-digested uridine adduct

Y

UNIMOD:2050

shTMTpro

313.231019

Super Heavy TMTpro, 13C15 H25 15N3 O3, Tandem Mass Tag and TMT are registered Trademarks of Proteome Sciences plc., TMTpro-SH

N-term, K

UNIMOD:2052

Biotin:Aha-DADPS

922.465403

Intact DADPS Biotin Alkyne tag

M

UNIMOD:2053

Biotin:Aha-PC

690.24316

Intact PC Biotin Alkyne tag

M

UNIMOD:2054

pRBS-ID_4-thiouridine

226.058972

RNA-protein UVA-crosslinked, hydrofluoride-digested 4-thiouridine adduct

F

UNIMOD:2055

pRBS-ID_6-thioguanosine

265.081104

RNA-protein UVA-crosslinked, hydrofluoride-digested 6-thioguanosine adduct

W

UNIMOD:2057

A52285

221.081695

Iodoacetamido-LC-Phosphonic Acid derivative, 6C-CysPAT, Carbamido-LC-phosphoyrlation

H, C, N-term, K, D, S, T, E, Y

UNIMOD:2058

A52286, A52287

209.97181

Xlink:DSPP[210], PhoX, tBu-PhoX (acid deprotected), Intact DSPP/TBDSPP crosslinker

N-term, K

UNIMOD:2060

C12 H14 N O8 P

331.045704

A52286, A52287, Tris-quenched monolink of DSPP/TBDSPP crosslinker, Xlink:DSPP[331], PhoX, tBu-PhoX (acid deprotected) Tris quench

N-term, K

UNIMOD:2062

DBIA

296.184841

desthiobiotinylation of cysteine with DBIA probe

C

UNIMOD:2067

Mono_Nγ-propargyl-L-Gln_desthiobiotin

580.333296

Enrichment of modified peptides using desthiobiotin-tag on streptavidin beads, Monomodification of Nγ-propargyl-L-Gln probe with clicked desthiobiotin-azide

D, E

UNIMOD:2068

Di_L-Glu_Nγ-propargyl-L-Gln_desthiobiotin

709.375889

Dimodification of L-Glu and Nγ-propargyl-L-Gln probe with clicked desthiobiotin-azide

D, E

UNIMOD:2069

Di_L-Gln_Nγ-propargyl-L-Gln_desthiobiotin

708.391873

Dimodification of L-Gln and Nγ-propargyl-L-Gln probe with clicked desthiobiotin-azide

D, E

UNIMOD:2070

L-Gln

128.058578

Monomodification with glutamine

D, E

UNIMOD:2072

Glyceroyl

88.016044

Glyceroylation

N-term, K

Targeting Rules
¶

When specifying modification target restrictions, following a modification rule’s name, write 
(<target>)
 where 
<target>

may be a series of amino acids or peptide terminals 
N-term
 or 
C-term
 or 
<AA>

@

<terminal>
, e.g. 
Deamidated

(N)

to specify the Deamidated modification rule on asparagine.

 «  
IUPAClite Monosaccharide and Glycan Notation

   ::  

Contents

   ::  

Glycan Composition Classes
  »


# 404

404

File not found

 The site configured at this address does not
 contain the requested file.

 If this is your site, make sure that the filename case matches the URL
 as well as any file permissions.

 For root URLs (like 
http://example.com/
) you must provide an

index.html
 file.

Read the full documentation

 for more information about using 
GitHub Pages
.

GitHub Status
 —

@githubstatus


# glycresoft  documentation

«  
Searching a Processed Sample with a Glycopeptide Database

   ::  

Contents

   ::  

Output Files and Exporting
  »

Table Of Contents

(Deprecated) Retention Time Modeling for Glycopeptide Identifications

Preparing Input Data

glycresoft export glycopeptide-chromatogram-records

Fitting The Model

glycresoft analyze retention-time fit-glycopeptide-retention-time

Re-using A Fitted Model

glycresoft analyze retention-time evaluate-glycopeptide-retention-time

(Deprecated) Retention Time Modeling for Glycopeptide Identifications
¶

Note

This topic covers the stand-alone RT model described in
Klein, J., & Zaia, J. (2020). Relative Retention Time Estimation Improves N-Glycopeptide Identifications by LC–MS/MS. Journal of Proteome Research, 19(5), 2113–2121.

https://doi.org/10.1021/acs.jproteome.0c00051
.

Another retention time modeling strategy was added to the glycopeptide search tools that is more robust and has more features. This is retained
purely for reference to that publication.

GlycReSoft can use chromatographic retention time to refine glycan composition assignments
to glycopeptides using related glycoforms as points of reference.

Preparing Input Data
¶

The model fitting process uses a simplified representation of glycopeptide chromatograms
which can be exported as a CSV file from a result database.

glycresoft export glycopeptide-chromatogram-records
¶

Export a simplified table of glycopeptide chromatographic features, formatted for retention time
modeling.

glycresoft

export

glycopeptide-chromatogram-records

[
OPTIONS
]

DATABASE_CONNECTION

ANALYSIS_IDENTIFIER

Options

-o
,

--output-path

<path>
¶

Path to write to instead of stdout

-r
,

--apex-time-range

<tuple>
¶

The range of apex times to include [default: 0, inf]

-t
,

--fdr-threshold

<float>
¶

[default: 0.05]

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

ANALYSIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycopeptide analysis to use

Fitting The Model
¶

glycresoft analyze retention-time fit-glycopeptide-retention-time
¶

Fit a retention time model to glycopeptide chromatogram observations, and evaluate the chromatographic apex
retention of each glycopeptide based upon the model fit. For large enough groups, try to fit a model to just
that group.

glycresoft

analyze

retention-time

fit-glycopeptide-retention-time

[
OPTIONS
]

CHROMATOGRAM_CSV_PATH

OUTPUT_PATH

Options

-t
,

--test-chromatogram-csv-path

<path>
¶

Path to glycopeptide CSV chromatograms to evaluate with the model, but not fit on

-p
,

--prefer-joint-model
¶

Prefer the joint model over the peptide-specific ones [default: False]

-m
,

--minimum-observations-for-specific-model

<int>
¶

The minimum number of observations required to fit a peptide sequence-specific model [default: 20]

-r
,

--use-retention-time-normalization
¶

[default: False]

Arguments

CHROMATOGRAM_CSV_PATH
¶

Required argument
<path>

OUTPUT_PATH
¶

Required argument
<path>

Re-using A Fitted Model
¶

glycresoft analyze retention-time evaluate-glycopeptide-retention-time
¶

Predict and evaluate the retention time of glycopeptides using an existing model.

The model file should have the extension .pkl and have been produced by the same version of GlycReSoft.

glycresoft

analyze

retention-time

evaluate-glycopeptide-retention-time

[
OPTIONS
]

MODEL_PATH

CHROMATOGRAM_CSV_PATH

OUTPUT_PATH

Arguments

MODEL_PATH
¶

Required argument
<path>

CHROMATOGRAM_CSV_PATH
¶

Required argument
<path>

OUTPUT_PATH
¶

Required argument
<path>

 «  
Searching a Processed Sample with a Glycopeptide Database

   ::  

Contents

   ::  

Output Files and Exporting
  »


# glycresoft  documentation

«  
LC-MS/MS Data Preprocessing and Deconvolution

   ::  

Contents

   ::  

Searching a Processed Sample with a Glycopeptide Database
  »

Table Of Contents

Searching a Processed Sample with a Glycan Database

glycresoft analyze search-glycan

Usage Example

Adducts

Network Regularization

MS/MS Signatures

Searching a Processed Sample with a Glycan Database
¶

Match features from a deconvoluted LC-MS or LC-MS/MS
data file with released glycan compositions from a
glycan hypothesis (see 
Combinatorial
,

Text
, and 
glySpace

glycan database construction methods).

glycresoft analyze search-glycan
¶

Identify glycan compositions from preprocessed LC-MS data, stored in mzML
format.

glycresoft

analyze

search-glycan

[
OPTIONS
]

DATABASE_CONNECTION

SAMPLE_PATH

HYPOTHESIS_IDENTIFIER

Options

-m
,

--mass-error-tolerance

<relative

mass

error>
¶

Mass accuracy constraint, in parts-per-million error, for matching. [default: 1e-05]

-mn
,

--msn-mass-error-tolerance

<relative

mass

error>
¶

Mass accuracy constraint, in parts-per-million error, for matching MS^n ions. [default: 2e-05]

-g
,

--grouping-error-tolerance

<relative

mass

error>
¶

Mass accuracy constraint, in parts-per-million error, for grouping chromatograms. [default: 1.5e-05]

-n
,

--analysis-name

<string>
¶

Name for analysis to be performed.

-a
,

--mass_shift

<string>
¶

Adducts to consider. Specify name or formula, and a multiplicity. (May specify more than once)

--mass_shift-combination-limit

<int>
¶

Maximum number of mass_shift combinations to consider [default: 8]

-d
,

--minimum-mass

<float>
¶

The minimum mass to consider signal at. [default: 500.0]

-o
,

--output-path

<string>
¶

Path to write resulting analysis to. [required]

-f
,

--ms1-scoring-feature

<choice>
¶

Additional features to include in evaluating chromatograms (May specify more than once)

Choices: [

null-charge; permethylated-ammonium-adducts; methyl-loss;

permethylated-ammonium-adducts-methyl-loss; formate-adduct-model]

-r
,

--regularize

<regularization

parameter>
¶

Apply Laplacian regularization with either a specified weight or “grid” to grid search, or a pair of values separated by a / to specify a weight or grid search for model fitting and a separate weight for scoring

-w
,

--regularization-model-path

<path>
¶

Path to a file containing a neighborhood model for regularization

-k
,

--network-path

<path>
¶

Path to a file containing the glycan composition network and neighborhood rules

-t
,

--delta-rt

<float>
¶

The maximum time between observed data points before splitting features [default: 0.5]

--export

<choice>
¶

export command to after search is complete (May specify more than once)

Choices: [

csv; glycan-list; html;

model]

-s
,

--require-msms-signature

<float>
¶

Minimum oxonium ion signature required in MS/MS scans to include. [default: 0.0]

-p
,

--processes

<int>
¶

Number of worker processes to use. Defaults to 4 or the number of CPUs, whichever is lower [default: 4]

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

SAMPLE_PATH
¶

Required argument
<path> The path to the deconvoluted sample file

HYPOTHESIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycan hypothesis to use

Usage Example
¶

$

glycresoft

analyze

search-glycan

-a

Formate

1

-o

agp-native-results.db
\

../hypothesis/native-n-glycans.db

path/to/sample.preprocessed.mzML

1
\

--export

csv

Adducts
¶

Adducts are mass shifts that may represent alternative charge carriers
such as formate or sodium, or chemical defects such as water loss or
incomplete permethylation. The software internally refers to these as
“mass shifts”.

Adducts are considered combinatorially, so if you were to pass 
-a

Ammonium

3

and 
-a

"C-1H-2"

1
 to indicate up to three ammonium adducts and up to one
incomplete permethylation, the program would search for

0 Ammonium, 0 C-1H-2

1 Ammonium, 0 C-1H-2

2 Ammonium, 0 C-1H-2

3 Ammonium, 0 C-1H-2

0 Ammonium, 1 C-1H-2

1 Ammonium, 1 C-1H-2

2 Ammonium, 1 C-1H-2

3 Ammonium, 1 C-1H-2

At this time, adduction models do not have any interaction with charge state.

Network Regularization
¶

Apply network smoothing by laplacian regularization to the glycan composition identification
scores. This procedure is described in detail in “Klein, J., Carvalho, L., & Zaia, J. (2018). Application of network smoothing to glycan LC-MS profiling.
Bioinformatics, 34(20), 3511-3518. 
https://doi.org/10.1093/bioinformatics/bty397
”. By default, the network used is simply
the full set of all glycan compositions in the hypothesis, with edges between compositions
whose composition-distance is less than or equal to 
\(1\)
 and with neighborhoods defined
for 
N
-glycans.

MS/MS Signatures
¶

Though this tool is designed to annotate putative glycan compositions from
LC-MS, this can lead to lots of strange matches. If your data contain MS/MS
scans, passing a non-zero value to 
--require-msms-signature
 causes the
program to include only features which contain MS/MS scans which look
“glycan-like”. Here “glycan-like” means containing abundant peaks which have
masses derived from mono-, di-, or tri-saccharide losses. The value of this
parameter sets the minimum ratio score.

 «  
LC-MS/MS Data Preprocessing and Deconvolution

   ::  

Contents

   ::  

Searching a Processed Sample with a Glycopeptide Database
  »


# glycresoft  documentation

«  
Searching a Processed Sample with a Glycan Database

   ::  

Contents

   ::  

(Deprecated) Retention Time Modeling for Glycopeptide Identifications
  »

Table Of Contents

Searching a Processed Sample with a Glycopeptide Database

Traditional Database Search

glycresoft analyze search-glycopeptide

Usage Example

Multi-component Database Search

glycresoft analyze search-glycopeptide-multipart

Usage Example

Memory Consumption and Workload Size

Build a Glycosite Network Smoothing Model

glycresoft analyze fit-glycoproteome-smoothing-model

Adducts

Searching a Processed Sample with a Glycopeptide Database
¶

The end-goal of all of these tools is to be able to identify glycopeptides
from experimental data. After you’ve constructed a glycopeptide database
and deconvoluted an LC-MS/MS data file, you’re ready to do just that.

Traditional Database Search
¶

glycresoft analyze search-glycopeptide
¶

Identify glycopeptide sequences from processed LC-MS/MS data. This algorithm requires a fully materialized
cross-product database (the default), and uses a reverse-peptide decoy by default, evaluated on the total score.

For a search algorithm that applies separate FDR control on the peptide and the glycan, see

Multi-component Database Search

glycresoft

analyze

search-glycopeptide

[
OPTIONS
]

DATABASE_CONNECTION

SAMPLE_PATH

HYPOTHESIS_IDENTIFIER

Options

-m
,

--mass-error-tolerance

<relative

mass

error>
¶

Mass accuracy constraint, in parts-per-million error, for matching MS^1 ions. [default: 1e-05]

-mn
,

--msn-mass-error-tolerance

<relative

mass

error>
¶

Mass accuracy constraint, in parts-per-million error, for matching MS^n ions. [default: 2e-05]

-g
,

--grouping-error-tolerance

<relative

mass

error>
¶

Mass accuracy constraint, in parts-per-million error, for grouping chromatograms. [default: 1.5e-05]

-n
,

--analysis-name

<string>
¶

Name for analysis to be performed.

-q
,

--psm-fdr-threshold

<float>
¶

Minimum FDR Threshold to use for filtering GPSMs when selecting identified glycopeptides [default: 0.05]

-s
,

--tandem-scoring-model

<choice

or>
¶

Select a scoring function to use for evaluating glycopeptide-spectrum matches [default: coverage_weighted_binomial]

Choices: [

binomial; simple; coverage_weighted_binomial;

log_intensity; penalized_log_intensty; peptide_only_cw_binomial;

log_intensity_v3; log_intensity_reweighted]

-x
,

--oxonium-threshold

<float>
¶

Minimum HexNAc-derived oxonium ion abundance ratio to filter MS/MS scans. Defaults to 0.05. [default: 0.05]

-a
,

--adduct

<string>
¶

Adducts to consider. Specify name or formula, and a multiplicity. (May specify more than once)

-f
,

--use-peptide-mass-filter
¶

Filter putative spectrum matches by estimating the peptide backbone mass from the precursor mass and stub glycopeptide signature ions [default: False]

-p
,

--processes

<int>
¶

Number of worker processes to use. Defaults to 4 or the number of CPUs, whichever is lower [default: 4]

--export

<choice>
¶

export command to after search is complete (May specify more than once)

Choices: [

csv; html; psm-csv]

-o
,

--output-path

<path>
¶

Path to write resulting analysis to. [required]

-w
,

--workload-size

<int>
¶

Number of spectra to process at once [default: 500]

--save-intermediate-results

<path>
¶

Save intermediate spectrum matches to a file

--maximum-mass

<float>
¶

[default: inf]

-D
,

--decoy-database-connection

<string>
¶

Provide an alternative hypothesis to draw decoy glycopeptides from instead of the simpler reversed-peptide decoy. This is especially necessary when the stub peptide+Y ions account for a large fraction of MS2 signal.

-G
,

--permute-decoy-glycan-fragments
¶

Whether or not to permute decoy glycopeptides’ peptide+Y ions. The intact mass, peptide, and peptide+Y1 ions are unchanged. [default: False]

--isotope-probing-range

<int>
¶

The maximum number of isotopic peak errors to allow when searching for untrusted precursor masses [default: 3]

-R
,

--rare-signatures
¶

Look for rare signature ions when scoring glycan oxonium signature [default: False]

--retention-time-modeling
,

--no-retention-time-modeling
¶

Whether or not to model relative retention time to correct for common glycan composition errors. [default: True]

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

SAMPLE_PATH
¶

Required argument
<path> The path to the deconvoluted sample file

HYPOTHESIS_IDENTIFIER
¶

Required argument
<string> The ID number or name of the glycopeptide hypothesis to use

Usage Example
¶

$

glycresoft

analyze

search-glycopeptide

-m

5e-6

-mn

1e-5

fasta-glycopeptides.db

path/to/processed/sample.mzML

1
\

-o

"agp-glycopepitdes-in-sample.db"

Multi-component Database Search
¶

glycresoft analyze search-glycopeptide-multipart
¶

Search preprocessed data for glycopeptide sequences scored for both peptide and glycan components.

This search strategy requires an explicit decoy database, like one created with the 
–reverse

flag from 
glycopeptide-fa
.

glycresoft

analyze

search-glycopeptide-multipart

[
OPTIONS
]

DATABASE_CONNECTION

DECOY_DATABASE_CONNECTION

SAMPLE_PATH

Options

-T
,

--target-hypothesis-identifier

<int>
¶

The ID number or name of the glycopeptide hypothesis to use [default: 1]

-D
,

--decoy-hypothesis-identifier

<int>
¶

The ID number or name of the glycopeptide hypothesis to use [default: 1]

-M
,

--memory-database-index
¶

Whether to load the entire peptide database into memory during spectrum mapping. Uses more memory but substantially accelerates the process [default: False]

-m
,

--mass-error-tolerance

<relative

mass

error>
¶

Mass accuracy constraint, in parts-per-million error, for matching MS^1 ions. [default: 1e-05]

-mn
,

--msn-mass-error-tolerance

<relative

mass

error>
¶

Mass accuracy constraint, in parts-per-million error, for matching MS^n ions. [default: 2e-05]

-g
,

--grouping-error-tolerance

<relative

mass

error>
¶

Mass accuracy constraint, in parts-per-million error, for grouping chromatograms. [default: 1.5e-05]

-n
,

--analysis-name

<string>
¶

Name for analysis to be performed.

-q
,

--psm-fdr-threshold

<float>
¶

Minimum FDR Threshold to use for filtering GPSMs when selecting identified glycopeptides [default: 0.05]

-f
,

--fdr-estimation-strategy

<choice>
¶

The FDR estimation strategy to use. The joint estimate uses both peptide and glycan scores, peptide uses only peptide scores, glycan uses only glycan scores, and any uses the smallest FDR of the joint, peptide, and glycan estiamtes. [default: joint]

Choices: [

joint; peptide; glycan;

any]

-s
,

--tandem-scoring-model

<choice

or>
¶

Select a scoring function to use for evaluating glycopeptide-spectrum matches [default: log_intensity]

Choices: [

log_intensity; simple; penalized_log_intensty;

log_intensity_v3]

-y
,

--glycan-score-threshold

<float>
¶

The minimum glycan score required to consider a peptide mass [default: 1.0]

-a
,

--adduct

<string>
¶

Adducts to consider. Specify name or formula, and a multiplicity. (May specify more than once)

-p
,

--processes

<int>
¶

Number of worker processes to use. Defaults to 4 or the number of CPUs, whichever is lower [default: 4]

--export

<choice>
¶

export command to after search is complete (May specify more than once)

Choices: [

csv; html; psm-csv]

-o
,

--output-path

<path>
¶

Path to write resulting analysis to. [required]

-w
,

--workload-size

<int>
¶

Number of spectra to process at once [default: 100]

-R
,

--rare-signatures
¶

Look for rare signature ions when scoring glycan oxonium signature [default: False]

--retention-time-modeling
,

--no-retention-time-modeling
¶

Whether or not to model relative retention time to correct for common glycan composition errors. [default: True]

--isotope-probing-range

<int>
¶

The maximum number of isotopic peak errors to allow when searching for untrusted precursor masses [default: 3]

-S
,

--glycoproteome-smoothing-model

<path>
¶

Path to a glycoproteome site-specific glycome model

-x
,

--oxonium-threshold

<float>
¶

Minimum HexNAc-derived oxonium ion abundance ratio to filter MS/MS scans. Defaults to 0.05. [default: 0.05]

-P
,

--peptide-masses-per-scan

<int>
¶

The maximum number of peptide masses to consider per scan [default: 60]

Arguments

DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

DECOY_DATABASE_CONNECTION
¶

Required argument
<databaseconnectionparam> A connection URI for a database, or a path on the file system

SAMPLE_PATH
¶

Required argument
<path> The path to the deconvoluted sample file

Usage Example
¶

Please see the 
SCE Tutorial for example usage

Memory Consumption and Workload Size
¶

Extensive use of caching and work-sharing has been done to make searching enormous
databases still tractable. If you find you are running out of memory during a search
consider shrinking the 
-w
 parameter.

Build a Glycosite Network Smoothing Model
¶

glycresoft analyze fit-glycoproteome-smoothing-model
¶

glycresoft

analyze

fit-glycoproteome-smoothing-model

[
OPTIONS
]

Options

-p
,

--processes

<int>
¶

Number of worker processes to use. Defaults to 4 or the number of CPUs, whichever is lower [default: 4]

-i
,

--analysis-path

<string>
¶

[required] (May specify more than once)

-o
,

--output-path

<path>
¶

[required]

-q
,

--fdr-threshold

<float>
¶

The FDR threshold to apply when selecting identified glycopeptides [default: 0.05]

-P
,

--glycopeptide-hypothesis

<tuple>
¶

-g
,

--glycan-hypothesis

<tuple>
¶

-u
,

--unobserved-penalty-scale

<float>
¶

A penalty to scale unobserved-but-suggested glycans by. Defaults to 1.0, no penalty. [default: 1.0]

-a
,

--smoothing-limit

<float>
¶

An upper bound on the network smoothness to use when estimating the posterior probability. [default: 0.2]

-r
,

--require-multiple-observations
,

--no-require-multiple-observations
¶

Require a glycan/glycosite combination be observed in multiple samples to treat it as real. Defaults to False. [default: False]

-w
,

--network-path

<path>
¶

The path to a text file defining the glycan network and its neighborhoods, as produced by 
glycresfoft build-hypothesis glycan-network
, otherwise the default human N-glycan network will be used with the glycans defined in 
-g
.

Adducts
¶

Unlike the glycan search tool, the glycopeptide search tool does not apply combinatorial expansion of adducts.
It will not mix mass shifts of different types together, so if both 
Ammonium

2
 and 
Na1H-1

1
 are specified,
the algorithm will only search for 0, 1, or 2 
Ammonium
 shifts and 0 or 1 
Na1H-1
 shifts. This is in order to
keep the search space tractable, but also in tested datasets, most multiply adducted ion species are low in abundance.

 «  
Searching a Processed Sample with a Glycan Database

   ::  

Contents

   ::  

(Deprecated) Retention Time Modeling for Glycopeptide Identifications
  »


# glycresoft  documentation

Contents

Search

 Please activate JavaScript to enable the search
 functionality.

 Searching for multiple words only shows matches that contain
 all words.

Contents


# glycresoft  documentation

«  
Building a Combinatorial Glycan Hypothesis

   ::  

Contents

   ::  

Building a Glycan Hypothesis from glySpace
  »

Table Of Contents

Building a Glycan Hypothesis from a Text File

glycresoft build-hypothesis glycan-text

Usage Example

Building a Glycan Hypothesis from a Text File
¶

When a comprehensive list of glycan compositions is available,
you can translate them into a list encoded in 
IUPAClite Monosaccharide and Glycan Notation

with one composition per line. Lines may have the form

<composition> [<classifier>[ <classifier>...]]
<composition> [<classifier>[ <classifier>...]]
...

where classifier is one of the recognized glycan composition classifiers
such as 
N-glycan
, 
O-glycan
, or 
GAG-linker
. For more information
on glycan composition classifiers, please see 
Glycan Composition Classes
.

glycresoft build-hypothesis glycan-text
¶

glycresoft

build-hypothesis

glycan-text

[
OPTIONS
]

TEXT_FILE

DATABASE_CONNECTION

Options

-r
,

--reduction

<string>
¶

Reducing end modification

-d
,

--derivatization

<string>
¶

Chemical derivatization to apply

-n
,

--name

<string>
¶

The name for the hypothesis to be created

Arguments

TEXT_FILE
¶

Required argument
<path>

DATABASE_CONNECTION
¶

Required argument
<string> A connection URI for a database, or a path on the file system

For more information on reductions and derivatizations, please see 
Glycan Modifications

Usage Example
¶

$

head

glycan-compositions.txt

{
Hex:5
;

HexNAc:4
;

Neu5Ac:1
}

N-Glycan

{
Hex:5
;

HexNAc:4
;

Neu5Ac:2
}

N-Glycan

{
Fuc:1
;

Hex:5
;

HexNAc:4
;

Neu5Ac:2
}

N-Glycan

{
Fuc:2
;

Hex:6
;

HexNAc:5
;

Neu5Ac:1
}

N-Glycan

{
Fuc:1
;

Hex:6
;

HexNAc:5
;

Neu5Ac:2
}

N-Glycan

$

wc

-l

glycan-compositions.txt

41

glycan-compositions.txt

$

glycresoft

build-hypothesis

glycan-text

glycan-compositions.txt

glycan-list.db

-n

"N-Glycan short list"

Building

Glycan

Hypothesis

N-Glycan

short

list

13
:51:54

-

glycresoft:log:175

-

INFO

-

Begin

Text

File

Glycan

Hypothesis

Serializer

{
'derivatization'
:

None,

'engine'
:

Engine
(
sqlite:///glycan-list.db
)
,

'glycan_file'
:

u
'glycan-compositions.txt'
,

'loader'
:

None,

'reduction'
:

None,

'start_time'
:

datetime.datetime
(
2017
,

8
,

31
,

13
,

51
,

54
,

636000
)
,

'status'
:

'started'
,

'transformer'
:

None,

'uuid'
:

'10e06326097c41f4beeeddd8e17bbc0e'
}

13
:51:54

-

glycresoft:log:175

-

INFO

-

Loading

Glycan

Compositions

from

Stream

for

GlycanHypothesis
(
id
=
1
,

name
=
N-Glycan

short

list
)

13
:51:54

-

glycresoft:log:175

-

INFO

-

Generated

41

glycan

compositions

13
:51:54

-

glycresoft:log:175

-

INFO

-

Hypothesis

Completed

13
:51:54

-

glycresoft:log:175

-

INFO

-

End

Text

File

Glycan

Hypothesis

Serializer

13
:51:54

-

glycresoft:log:175

-

INFO

-

Started

at

2017
-08-31

13
:51:54.636000.
Ended

at

2017
-08-31

13
:51:54.761000.
Total

time

elapsed:

0
:00:00.125000
TextFileGlycanHypothesisSerializer

completed

successfully.

 «  
Building a Combinatorial Glycan Hypothesis

   ::  

Contents

   ::  

Building a Glycan Hypothesis from glySpace
  »

