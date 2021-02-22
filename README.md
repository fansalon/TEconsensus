# Generate a TE consensus sequence file for TEspeX

This file is part of the TEspeX manual, some of the TEspeX prerequisites (https://github.com/fansalon/TEspeX#prerequisites) may be prerequisites also for the installation commmands listed here.

Theoretically, TEspeX is supposed to work on any type of TE consensus sequence. However, providing a good input file may increase the accuracy of TEspeX.

TEspeX has been largely tested on Dfam and RepBase consensus sequences, instructions to use both databases are reported below.

## **Dfam** ##

**Installation**

To use the Dfam database as source of TE consensus sequences follow the instructions listed below:

If working on Unix:
```
#Install h5py to read and write files in HDF5 format
pip3 install --user h5py
#Download and unzip the Dfam curated database
wget https://www.dfam.org/releases/Dfam_3.3/families/Dfam_curatedonly.h5.gz
zcat Dfam_curatedonly.h5.gz > Dfam_curatedonly.h5
#Install the pyhton3 script to retrieve fasta TE sequences from Dfam database
wget https://raw.githubusercontent.com/Dfam-consortium/FamDB/master/famdb.py
chmod +x famdb.py
```

If working on Mac Os:
```
#Install h5py to read and write files in HDF5 format
pip3 install --user h5py
#Download and unzip the Dfam curated database
curl -L -o Dfam_curatedonly.h5.gz https://www.dfam.org/releases/Dfam_3.3/families/Dfam_curatedonly.h5.gz
gunzip -c Dfam_curatedonly.h5.gz > Dfam_curatedonly.h5
#Install the pyhton3 script to retrieve fasta TE sequences from Dfam database
curl -L -o famdb.py https://raw.githubusercontent.com/Dfam-consortium/FamDB/master/famdb.py
chmod +x famdb.py
```

The Dfam curated-only database contains TE sequences of H. sapiens, M. musculus, D. rerio, D. melanogaster, C. elegans.\
If working with other species, plelase consider to download the full Dfam databse (it may take some time ~10hours - ~15GB).

```wget https://www.dfam.org/releases/Dfam_3.2/families/Dfam.h5.gz```



**TE consensus sequence retrieval (FASTA)**

Having installed both the Dfam database and the famdb.py script you can then move to the  next steps.

Although several operations could be done using the Dfam database and the famdb.py script (look: https://github.com/Dfam-consortium/FamDB) how to extract the TE consensus nucleotide fasta sequence is only explained here. Also, note that this is how **WE** recommend to extract TE sequences to be used with TEspeX. This **does not necessarily mean** this is the worflow to be followed when using other tools.

```./famdb.py -i Dfam_curatedonly.h5 families --include-class-in-name -f fasta_name -ad 'species of interest' > speciesOfInterset.all.Dfam.fa```

* ```-i``` indicates the database to be used
* ```-f``` indicates the output format (i.e., fasta_name meand fasta format with the following header format: ```>MIR @Mammalia [S:40,60,65]```
* ```-a``` and ```-d``` include ancestors/descendants as with lineage. We recommend to use both flags
* ```--include-class-in-name``` includes the RepeatMasker type/subtype in the family name, e.g. HERV16#LTR/ERVL.
* ```'species of interest'```: case insensitive scientific name (e.g 'homo sapiens', 'mus musculus')\


**Important**

The command above will download all the types of repeated sequences (including satellite, simple repeat, rRNA, tRNA, artefacts, ..). If you are interested in quantifying exclusively the expression of Transposable Elements (e.g. LINE, SINE, LTR, DNA, RC, Other, Retroposon \[SVA\]) we recommend to retrieve from the database only the TEs belonging to such classes.\
If so, type the following line of code to generate the TE consensus sequence file:

```
rm speciesOfInterset.Dfam.fa # to avoid appending sequences to an already existing file
array=("LINE" "SINE" "DNA" "LTR" "RC" "Other" "Retroposon")
for cls in ${array[@]}
do
  ./famdb.py -i Dfam_curatedonly.h5 families --include-class-in-name --class $cls -f fasta_name -ad 'species of interest' >> speciesOfInterset.Dfam.fa
done
```


For further information please consult https://github.com/Dfam-consortium/FamDB and https://dfam.org/home


## **RepBase** ##

Please note how RepBase database is no more freely available and an account is needed to retrieve the TE consensus sequences in fasta format.

If you have an account on the GIRI RepBase database (https://www.girinst.org) the best solution is to download the TE consensus of interest directly from the website. Otherwise, if you have the RepBaseRepeatMaskerEdition-XXXX.tar.gz file, having no longer a valid account, you may consider to query the database through the RepeatMasker software following the below-reported instructions.

The following instructions assume you have the RepBase database copy by your own (the required file is named RepBaseRepeatMaskerEdition-XXXX.tar.gz).

**Installation**

RepBase relies on RepeatMasker to retrieve TE consensus sequences. Therefore, first RepeatMasker has to installed, then RepBase has to be properly configured.

Prerequisite: perl 5.8.0 or higher installed

If working on Unix:
```
#get repeatmasker
wget http://www.repeatmasker.org/RepeatMasker/RepeatMasker-open-4-0-7.tar.gz
tar -zxvf RepeatMasker-open-4-0-7.tar.gz
mv RepeatMasker RepeatMasker-4-0-7
cd RepeatMasker-4-0-7/

#get the RepBase Libraries. Please NOTE that this file is no more freely accesible - assuming you have one copy somewhere
cp /path/to/RepBase/RepBaseRepeatMaskerEdition-XXX.tar.gz .
tar -zxvf RepBaseRepeatMaskerEdition-XXX.tar.gz
rm RepBaseRepeatMaskerEdition-XXX.tar.gz

#get Tandem Repeats Finder
# download at http://tandem.bu.edu/trf/trf409.linux64.download.html
mv trf409.linux64 trf
chmod +x trf

#get rmblast
wget http://www.repeatmasker.org/rmblast-2.10.0+-x64-linux.tar.gz
tar -zxvf rmblast-2.10.0+-x64-linux.tar.gz
rm rmblast-2.10.0+-x64-linux.tar.gz

# configure!
./configure
#this prompts you to a different window
# press ENTER when asked when asked for perl
# press ENTER when asked when asked for RepeatMasker installation directory
# type trf full path /path/to/RepeatMasker-4-0-7/trf
# when asked 'Add a Search Engine' type 2
# type full path to rmblast bin directory /path/to/RepeatMasker-4-0-7/rmblast-2.10.0/bin/
# type Y
# type 5
```

If working on Mac os:
```
#get repeatmasker
curl -L -o RepeatMasker-open-4-0-7.tar.gz http://www.repeatmasker.org/RepeatMasker/RepeatMasker-open-4-0-7.tar.gz
tar -zxvf RepeatMasker-open-4-0-7.tar.gz
mv RepeatMasker RepeatMasker-4-0-7
cd RepeatMasker-4-0-7/

#get the RepBase Libraries. Please NOTE that this file is no more freely accesible - assuming you have one copy somewhere
cp /path/to/RepBase/RepBaseRepeatMaskerEdition-XXX.tar.gz .
tar -zxvf RepBaseRepeatMaskerEdition-XXX.tar.gz
rm RepBaseRepeatMaskerEdition-XXX.tar.gz

#get Tandem Repeats Finder
# download at http://tandem.bu.edu/trf/trf409.linux64.download.html
mv trf409.linux64 trf
chmod +x trf

#get rmblast
curl -L -o rmblast-2.10.0+-x64-linux.tar.gz http://www.repeatmasker.org/rmblast-2.10.0+-x64-linux.tar.gz
tar -zxvf rmblast-2.10.0+-x64-linux.tar.gz
rm rmblast-2.10.0+-x64-linux.tar.gz

# configure!
./configure
#this prompts you to a different window
# press ENTER when asked when asked for perl
# press ENTER when asked when asked for RepeatMasker installation directory
# type trf full path /path/to/RepeatMasker-4-0-7/trf
# when asked 'Add a Search Engine' type 2
# type full path to rmblast bin directory /path/to/RepeatMasker-4-0-7/rmblast-2.10.0/bin/
# type Y
# type 5
```


**TE consensus sequence retrieval (FASTA)**

Having installed RepeatMasker coonfigured with RepBase, TE consensus sequences in fasta format can now be retrieved.

Note that this is how **WE** recommend to extract TE sequences to be used with TEspeX. This **does not necessarily mean** this is the worflow to be followed when using other tools.

```./queryRepeatDatabase.pl -species 'species of interest' > speciesOfInterset.RepBase.fa```

* ```'species of interest'```: case insensitive scientific name (e.g 'homo sapiens', 'mus musculus')


For further information please consult http://www.repeatmasker.org/RepeatMasker/
