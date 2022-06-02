# Introduction to Linux command-line environments

## Basic Linux operations

Print the current working directory

```bash
pwd
```

List files in the current working directory

```bash
# list files
ls

# list files as a table with human-readable sizes
ls -lh

# list files and hidden files as a table with human-readable sizes
ls -lah
```

Create a file

```bash
# use touch to create an empyt file
touch test_file.txt

# list files to see the new file
ls -lh
```

Create a directory

```bash
# make a new directory
mkdir test_dir

# list files to see the new directory
ls -lh
```

## File attributes and permissions

```bash
# list files
ls -lh
```

```
total 0
drwxr-xr-x 2 nfahlgren bioinfo 6    May 23 20:47  test_dir
-rw-r--r-- 1 nfahlgren bioinfo 0    May 23 20:47  test_file.txt
----------   --------- ------- -    ------------  -------------
permissions  user      group   size modified date filename
```

### File attributes (first character)

```
-  Regular file
b  Block special file
c  Character special file
d  Directory
l  Symbolic link
n  Network file
p  FIFO
s  Socket
```

### File permissions (three triplets)

```
r  Permission to read file
w  Permission to write to file
x  Permission to execute file
a  Archive bit is on (file has not been backed up)
c  Compressed file
s  System file
h  Hidden file
t  Temporary file
```

Source: https://www.mkssoftware.com/docs/man1/ls.1.asp

### Change permissions

```
total 0
drwxr-xr-x 2 nfahlgren bioinfo 6    May 23 20:47  test_dir
-rw-r--r-- 1 nfahlgren bioinfo 0    May 23 20:47  test_file.txt

rw-   r--   r--
----- ----- -----
user  group other
```

```bash
# add group write permission
chmod g+w test_file.txt

# add all execute permission
chmod ugo+x test_file.txt

# remove other execute permission
chmod o-x test_file.txt
```

## Navigation

Change directory to move to another location

```bash
# move into the new directory
cd test_dir

# print the current working directory
pwd
```

Return to home from any location

```bash
# cd without arguments returns to home
cd

# print working directory
pwd
```

### Relative paths

```bash
# change directory to the new directory
cd test_dir

# list files
ls -lh

# list files in current location
ls -lh .

# list files in the upstream directory
ls -lh ..
```

## File manipulation

Move a file to a new location

```bash
# move file from upstream directory to the current location
mv ../test_file.txt .

# list files
ls -lh
```

Rename a file

```bash
# use the move command to rename a file
mv test_file.txt my_file.txt

# list files
ls -lh
```

Copy a file

```bash
# copy the file from the current location to the upstream directory
cp my_file.txt ..

# list files in the current location
ls -lh

# list files in the upstream directory
ls -lh ..
```

Remove (delete) a file

```bash
# remove the file
rm my_file.txt

# list files
ls -lh
```

Remove (delete) a directory

```bash
# change directory to the upstream directory
cd ..

# remove the directory
rmdir test_dir

# list files
ls -lh
```
