# MGJW Problem Set 1

## Kahoot
Let's test what we learnt last session [here](https://play.kahoot.it/v2/lobby?quizId=86abc674-602a-44a7-b7af-98bebbe396a6).
### Useful Unix Commands

| Command                     | Description                              |
| --------------------------- | ---------------------------------------- |
| `pwd`   | Command to see where you are "Parent Working Directory" |
| `cd /go_to_path/`    | Change current working directory |
| `ls`    | Lists files in a directory. `ls -l` (long listing), `ls -lh` (human-readable format, must be used with `-l`), `ls -a` (all, including hidden files). |
| `mv filename1 filename2`    | Move or rename a file or directory.  |
| `cp filename1 filename2`    | Copy a file. Use `-R` for copying directories recursively         |
| `chmod` | Change the read, write, and execute permissions of a file or directory. Use `ls -l` to show the permissions of files in a directory |
| `mkdir` | Make a directory.                         |
| `head`            | View the first few lines of a file.  You can control how many lines to view. |
| `cat`             | Can be used to concatenate multiple files together into a single file, or to view the contents of a file in the terminal. |
| `less`            | A version of `more` with more features.                      |
| `wc filename`              | Count `-w` words, `-l` lines and/or `-m` characters in a file. This can be used for knowing how many reads are in a fastq file   |
| `grep`            | Print lines matching a specified pattern or that don't match a specified pattern. `grep -c '>' file.fasta` will print number of `>` in a file which is a proxy for number of contigs in a fasta file |
| `gzip` (`gunzip`) | Compress (uncompress) a file.                                |
| `tar`             | Archive or unarchive an entire directory into a single file. You may receive your run from a sequencing core in this format |
| `emacs`           | Run the Emacs text editor. It needs to be installed first.                |
| `echo`            | Print a copy of some text to the screen. E.g. `echo 'Microbial Genomics'` |
| `history` | Show all the commands that were run recently |

## Part 1
* Check your parent working directory
* Change your directory to home directory
* Change your directory to your MGJW directory or create it if it does not exist
* Install emacs text editor using Conda
* Make a new file using vi and name the file script.sh and type in it
```
echo "Hello Microbes"
echo "Hello Bacteria"
echo "Hello Viruses"
echo "Hello Bacteria"
```
This is quick manual of how to use vi (https://www.marquette.edu/mathematical-and-statistical-sciences/basic-vi-editor-commands.php). After editing, you will need to use esc button and then :wq to save and exit.
* Count the number of lines in script.sh and redirect output to a new file called number_of_lines.txt
* Try to run this shell script using `./script.sh`, did it work?
* Change permissions of the file to be readable, writable and executable for everyone (google chmod 777)
* Try to rerun the file `./script.sh`, did it work this time?

## Part 2
* Install wget using conda in a new environment (optional)
* Download this file from this dropbox URL [problem_set1.tar.gz](https://www.dropbox.com/s/fel9pq2hxj58gmo/problem_set1.tar.gz?dl=0) and put it in MGJW or using `wget -O problem_set1.tar.gz https://www.dropbox.com/s/fel9pq2hxj58gmo/problem_set1.tar.gz?dl=0 --no-check-certificate`
* Make a new directory called problem_set1 in MGJW
* Unarchive the file (tar -xf problem_set1.tar.gz -C ./problem_set1)
* What do you see after you unarchived the file? Do you have one directory/file or multiple?
* List all fasta files with the extension ".fasta.gz" in the fasta folder
* Uncompress all files ending with .fasta.gz
* Use a command to count how many '>' characters are in each fasta file?
* Make a new directory in your current directory and name it project1
* Copy genome2.fasta in the same directory to a file called genome2_copy.fasta
* Move genome2_copy.fasta to project1 folder
* Go to GitHub and make an account
* Make a new directory in MGJW and name it github_repos
* Change your directory to github_repos
* Clone Microbial-Genomics-Journey-Workshop repository to your local computer
* Save your history of commands in a file called commands_notes.txt
