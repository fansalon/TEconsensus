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

For further information please consult https://github.com/Dfam-consortium/FamDB and https://dfam.org/home
