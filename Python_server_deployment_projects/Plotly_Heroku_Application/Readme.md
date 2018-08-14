# Analyzing police Activity in California

Download the respective data file from this [location](https://openpolicing.stanford.edu/data/) to perform data analysis and build the application using heroku.  

The size of csv file for the state of California is 5 GB, which is very large for jupyter notebook to read. We need to reduce the size of the file by using command line. 


## Reducing the size of a file using command line

You can either use powershell or git bash. I have used git bash as we need to use some linux commands which are not supported by the command prompt. 

open git bash and navigate to the directory where the data file is present using *cd*.

To list the files present in the folder:

```
$ ls
```

To view the column names of a file:

```
$ awk  ‘FNR  == 1 {print}’ filename
```
So for our data file it should be *$ awk 'FNR == 1 {print} CA_cleaned.csv*. Note the column numbers of the file. 

To select a specific column present in the file say column number 2:

```
$ cut -d <delimiter> -f 2 inputfilename 
```
So for example *cut -d ',' -f 2 CA_cleaned.csv*. It will display the 2nd column of the file. Do not try on this data file as its huge and will take some time to display the content.

To select multiple columns of a file and save it into new file:

```
cut -d ‘,’ -f 2,3,4-6 inputfilename > outputfilename 
```
So for example *cut -d ',' -f 3,4,6,10,12,14,16,17,19-22  CA_cleaned.csv > CA_data.csv*. These are columns which are most important for the analysis. It will take some time and you will find the new data file in the same folder, with a reduced file size of 2.5 GB.

