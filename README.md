# Plate-reader
This page contains a UNIX script to extract datafiles from multiple folders generated by the plate reader: PerkinElmer EnspireTM 2300 Multilabel 132 Reader (Turku, Finland).


## Plate reader data\
Make a for-loop for extracting data from several folders\

Below I have made a 'for-loop within a for-loop' that will help you go into multiple folders and within each folder copy specific rows from every file in that folder. The code is designed specifically for plate reader data where you will have multiples folders named with the date of the reading, and within each of these folders you will have all your files from the plate reader that day. This code will extract all the data from specified row numbers and save the folder name, the file name and the data from these rows into **one** single csv file.\

In order to run the code you must first change directory `cd` (in Terminal) to the directory that contains all the dated folders from the plate reader. If you type `ls` (which lists all files) you will see something like this: `20210908 20210910 20210913 20210916`.\

Depending on if you have a 24-well plate or a 96-well plate data you need to change one line below noted with three hashtags `###` with one of the two first lines below.

```
sed -n '7,11p' "${files[$i]}" >> ./../Plate_reader.csv; #for 24-well plate data
sed -n '7,15p' "${files[$i]}" >> ./../Plate_reader.csv; #for 96-well plate data
```

When you have the chosen the correct plate, you can run the script below:
```
for f in 20*; do
BACK=$(pwd);
cd $f;
var=$(pwd);
basename $(pwd);
mydir="$(basename $PWD)";
echo "### $mydir ###" >> ./../Plate_reader.csv;
files=(*);
len="${#files[@]}";
for (( i=0; i<$len; i++ )); do
echo "${files[$i]}" >> ./../Plate_reader.csv;
sed -n '7,15p' "${files[$i]}" >> ./../Plate_reader.csv; ###CHANGE THIS LINE###
echo "${files[$i]}"
done;
cd $BACK; 
done
```

It is also possible to run this script as an executable file.

- Start by making a bash file `vim script_name.sh`
- Start the first line with `#!/bin/bash`
- Change the permission of the file `sudo chmod u+r+x script_name.sh`
- Now you should be able to execute the file by double clicking on it.

```
#!/bin/bash

for f in 20*; do
BACK=$(pwd);
cd $f;
var=$(pwd);
basename $(pwd);
mydir="$(basename $PWD)";
echo "### $mydir ###" >> ./../Plate_reader.csv;
files=(*);
len="${#files[@]}";
for (( i=0; i<$len; i++ )); do
echo "${files[$i]}" >> ./../Plate_reader.csv;
sed -n '7,11p' "${files[$i]}" >> ./../Plate_reader.csv; ###CHANGE THIS LINE###
echo "${files[$i]}"
done;
cd $BACK;
done
```
