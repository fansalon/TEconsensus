# Generate a TE consensus sequence file

This file is part of the TEspeX manual, some of the TEspeX prerequisites (https://github.com/fansalon/TEspeX#prerequisites) may be prerequisites also for the installation commmands listed here.

Theoretically, TEspeX is supposed to work on any type of TE consensus sequence. However, providing a good input file may increase the accuracy of TEspeX.

TEspeX has been largely tested on Dfam and RepBase consensus sequences, instructions to use both databases are reported below.

## **Dfam** ##

To use the Dfam database as source of TE consensus sequences follow the instructions listed below:

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
wget https://www.dfam.org/releases/Dfam_3.3/families/Dfam_curatedonly.h5.gz
zcat Dfam_curatedonly.h5.gz > Dfam_curatedonly.h5
#Install the pyhton3 script to retrieve fasta TE sequences from Dfam database
wget https://raw.githubusercontent.com/Dfam-consortium/FamDB/master/famdb.py
```

Having installed both the Dfam database and the famdb.py script you can then move to the  next steps.

Although several operations could be done using the Dfam database and the famdb.py script (look: https://github.com/Dfam-consortium/FamDB) here only how to extract the TE consensus nucleotide fasta sequence is explained. Also, note that this is how **WE** recommend to extract TE sequences to be used with TEspeX. This **does not mean** this is the worflow to be followed when using other tools.

```./famdb.py -i Dfam_curatedonly.h5 families -f fasta_name -ad 'species of interest' > speciesOfInterse.Dfam.fa```

* ```-i``` indicates the databse to be used
* ```-f``` indicates the output format (i.e., fasta_name meand fasta format with the following header format: ```>MIR @Mammalia [S:40,60,65]```
* ```-a``` and ```-d``` include ancestors/descendants as with lineage. We recommend to use both flags




For further information please consult https://github.com/Dfam-consortium/FamDB and https://dfam.org/home
