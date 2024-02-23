---
layout: default
title: Page Title
---
# Unix Tools

1. [find](#find)
2. [grep](#grep)
3. [rsync](#Rsync)
4. [awk](#awk)
5. [sed](#sed)
6. [glob](#glob)
7. [word-count](#WC)

## find

To get information of find command one can use it's manual with `man find`. This will hold for any linux commad.

1. Listing all the files and directories with a particular directory

   - ```bash
     # List all files in the given directory
     find MyDir
      
     # To list all the directories in a particular Directory
     # use type setter and d as it's value
     find MyDir -type d
     find MyDir -type f	# will list all the files in MyDir.
     ```

2. Search for a file by name in a particular hierarchy.

   - ```bash
     # will return the path of test.py in the MyDir hierarchy
     find MyDir -type f -name "test.py"
      
     # will return the mp3 files
     find MyDir -type f -name "*mp3"		# name will be case sensitive.
     find MyDir -type f -iname "*mp3"	# will make mp3 case insensitive.
     ```

3. Search for files using their meta-data

   - ```bash
     # search for the file modified less than 10 min ago
     find MyDir -type f -mmin -10
      
     # search for the file modified more than 10 min ago
     find MyDir -type f -mmin +10
      
     # search for a file modified less than 5 min ago
     # and also more than 2 min ago.
     find MyDir -type f -mmin -5 -mmin +2
      
     # search for a file modified less than 10 days ago
     find MyDir -type f -mtime -10
     ```

   - One can also use `amin, atime` for access minutes and access days. `ctime, cmin` for change minutes and change days.

4. Searching for files according to their size

   - ```bash
     # search for all the files greater than 100MB
     find MyDir -size +100M
      
     # k can be used for KB's and G can be used for GB's
     find MyDir -size -2K # will list all the files less than 2KB's
     ```

5. List all the empty files

   - ```bash
     # list all the empty files
     find MyDir -empty
      
     # count all the empty files
     find MyDir -empty | wc -l
     ```

6. To search all files with a specific permission

   - ```bash
     # find all the files with read, write permission
     find MyDir -perm 666	# write:2 -> read: 4.
     ```

### Performing actions on output results

`-exec` can be used to execute a particular command on the results of find.

1. Change the Owner of all files with the permission of 777

   - ```bash
     find MyDir -perm 777 -exec chown user:group {} +
     ```

   - While executing chown normally we will replace {} with the file name, but here in this case {} will automatically replace the files we get in the result of find command.

   - We use `+` sign or `\;` to terminate the -exec value.

2. Remove all the files with `.jpg` extension in current directory only

   - ```bash
     find MyDir -iname "*.jpg" -maxdepth 1 -exec rm {} +
     ```

## Grep

Grep is used to find patterns in files. **Combined with others like find, awk, sed** it can be very useful.

1. Search for a Name in a file

   - ```bash
     # will return all the lines containing Virat Kohli
     # also return the line if search element is a substring.
     grep "Virat Kohli" cricketers.txt
      
     # will return only the lines which matches completely
     grep -w "Rohit Sharma" cricketers.txt
      
     # Search shikhar dhawan but ignore case sensitivity
     grep -wi "shikhar dhawan" cricketers.txt
      
     # Show line number of the match
     grep -wn "Rohit Sharma" cricketers.txt
      
     ```

2. Search for a name in contacts and also print nearby text i.e. 4 lines up or 4 lines down.

   - ```bash
     # will list all the names matching Akshay and also
     # content upto 4 lines behind the match
     grep -win -B 4 "Akshay" contacts.txt
      
     # will list all the names matching Akshay and also
     # content upto 4 lines ahead the match
     grep -win -A 4 "Akshay" contacts.txt
      
     # will print 4 lines ahead and 4 lines before
     grep -win -C 4 "Rahul" contacts.txt
     ```

3. Search for a name in multiple files

   - ```bash
     # list all files within current directory containing linux
     grep -win "Linux" ./*.txt
      
     # list all files from hierarchy containing linux
     grep -winr "Linux" ./
      
     # to list only files without getting matched line also
     grep -wirl "Linux" ./
      
     # to list number of matches corresponding to each file use c
     grep -wirlc "Linux" .
     ```

4. Filter the lines from history file on linux of git commit

   - ```bash
     cat .bash_history | grep "git commit"
      
     # get git-commit of dot files only
     cat .bash_history | grep "git commit" | grep "dotfiles"
     ```

5. Search for phone numbers in a file

   - ```bash
     # search a phone number in format 123-456-7890
     grep "...-...-...." contacts.txt
     ```

   - More advanced regular expressions won't work for linux until we specify it to by using -P.

     - ```bash
       grep -Po "\d{3}-\d{3}-\d{4}" # -P will enable Perl regex.
       ```

     - `o` is used to print only the match.

6. Exclude directories from grep search

   - ```bash
     grep -winr --exclude-dir={.config,.local,.cache} "export" .
     ```

7. Include exclude files from grep search

   - ```bash
     grep pattern -r --include=\*.{cpp,h} rootdir
     ```

     

## Rsync

Rsync is used to synchronize files. It has many advantages over manual sync. It will only sync changed files and if due to some reason rsync stops or fails, on restart it will start sync from where it was left. **Not only it can be used as remote but also as a local sync tool**.

1. Sync files from one directory to Backup directory

   - ```bash
     # will sync all files from Images to Backup Dir
     rsync Images/* Backup/
      
     # sync whole directory structure to Backup Dir
     rsync -r Images/ Backup/
     ```

2. Sync files from one dir to Backup keeping it's permissions and meta data same

   - ```bash
     # use a(archive) option which is a combination of rlptgoD
     rsync -a Images/ Backup/
     ```

3. List the files before syncing them using **--dry-run**

   - ```bash
     rsync -av --dry-run Origional/ ./Backup/
     ```

   - `--dry-run` will run rsync as a mock test and v is verbose which will show the list of files to be synced.

4. Delete unnecessory files from Backup dir while syncing

   - ```bash
     rsync -av --delete Origional/ ./Backup/
     ```

   - This will delete any file which is present in Backup dir but not in origional dir.

5. Send files to a remote machine using ssh

   - ```bash
     rsync -zaP Images/ mysshmachine@192.168.1.22:~/Backup/Images/
     ```

   - z option will compress the file size before sending, a will archive the files (keeping permissions and meta data similar), P will show the progress of the operation.

6. We can also check the progress of backup using `--progress`

   - `rsync -r --progress Images/ Backup/`.

## awk

1. Print specific fields

   - ```bash
     awk '{print $2}' file # will print 2nd field from each line
     awk '{print $3, $2}' file # will print 3rd and 2nd field separated by commas
     ```

2. Conditional Printing

   - ```bash
     awk '$2 == "sed" {print}' file # will print line if 2nd argument of line is sed
     awk 'length($1) > 10 {print}' file # will print line if 1st arg size is more than 10
     ```

   - ```bash
     awk '$2 == "warning" && $1 != "system" {print}' file	# Print lines with "warning" in the second field, but only if the first field is not "system"
     ```

   - ```bash
     awk '$2 == "error" {print toupper($0)}' file # Print lines containing "error", converting them to uppercase
     ```

3. Calculations on values

   - ```bash
     awk '{total += $2} END {print total}' file	# This calculates the sum of values in the second field across all lines
     
     awk '{total += $3; count++} END {print total/count}' file	# Calculate average of the third field
     ```

4. String Manipulation

   - ```bash
     awk '$1 == "old" {$1 = "new"} {print}' file	# Replace "old" with "new" in the first field
     ```

5. Using Functions with awk

   - ```bash
     awk 'function isEven(x) { return x % 2 == 0 } {if (isEven($1)) print}' file
     ```

## sed

1. Delete a match from a file

   - ```bash
     sed -i '/mystring/d' file	# i is for inline edit and d is to delete match
     sed -i '/^he.*l$/d' file	# delete any line starting with he and ending with l
     ```

   - ```bash
     # delete line based on line number
     sed '10d' file # will remove 10th line from the file
     ```

2. Inserting text in the begining of a line

   - ```bash
     sed -i 's/^/Num /' file # this will add Num as prefix in each line of file
     sed -i 's/$/ end/' file # will add end as prefix in each line of file
     sed -i '/PATTERN/a my test text ' file # a is used to indicate to insert text after.
     # will add ' my test text' after each maching pattern in a new line.
     ```

3. To add changes inline while also creating a backup file one can use `-i.bak`

   - ```bash
     sed -i.bak '/mystring/d' file # in place edit file and create a backup file "file.bak"
     ```

4. Searching and replacing a word from a file

   - ```bash
     sed -i 's/hello/bye/g' file # s indicates substitute and g indicates for all instances
     # all instances of hello will be replaced with bye
     ```

   - ```bash
     sed -i 's/hello/bye/gi' file # here i indicates we that we can ignore case senstivity
     
     sed -i 's/hello/& boy/g' file # here & represents the pattern and it will substitute hello boy for every match.
     ```

5. Printing lines

   - ```bash
     sed '5,10p' file	# printing line 5 to 10 of file.
     ```

     

## glob

## WC

