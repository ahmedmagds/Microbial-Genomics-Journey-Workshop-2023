# Microbial Genomics Journey Workshop 2023

__Instructor__<br/>
Ahmed M. Moustafa

__Teaching Assistants__<br/>
Daniel P. Morreale (Dan)<br/>
Qianxuan She (Sean)<br/>
Erin Marie Theiller (Erin)<br/>



## Why is it important to learn microbial genomics?

You might already know the answer.

[__Torsten__ __Seemann__](https://github.com/tseemann/)

An hour of bioinformatics can save a week in the lab.
ALSO
An hour in the lab can save a week of bioinformatics.

## Aim

Acquire enough knowledge and skills to use microbial genomics approaches in Your project.

## Tips

1. Practice, practice, practice.

2. Map your Knowledge out.

3. Update your Knowledge Map.

4. know<-do not know<- do not know that I do not know.

5. You do not necessarily need denovo scripting (coding) to process data.

6. Use defaults unless you know what you are doing.

7. Attending workshops and courses are important, but the practice is more important.

8. Reproducibility.

9. Coding would help in data reformatting and analyses in unique ways.

## Knowledge Map
Map your Microbial Genomics knowledge out. Knowledge mapping helps communicate information and solve complex problems. Think about the concepts, procedure and competencies. It will help you advance your current understandings of microbial genomics or any other field. Knowledge maps will also serve as a visual aid representing ideas and their resources. We will start the course with a knowledge map and update it in the last session of the course. Please use the provided example to map out your microbial genomics knowledge. The Microbial genomics knowledge map shared [here](Knowledge_Map_1.pptx) is very simple. Try to be very detailed about each step and process. You can produce more than one map for the different main processes and we will try to combine them in one map by the end of the course.

---

## Unix

### Why would I need to use command-line?

- Many bioinformatics tools do not have GUI and would need to be run on the command-line.
- Combining multiple tools into automated scripts.
- Using High performance computing (HPC) to run complex analyses (e.g. Analyze 1 million genomes or more).

### How to open a terminal?

The terminal window reads a line of input and execute the commands and print an output to the terminal. You can open several Terminal windows at once.

#### Windows
Follow instructions for installing Windows Subsystem for Linux (WSL) on https://docs.microsoft.com/en-us/windows/wsl/install-win10<br/>
Briefly:
1. Open PowerShell as Administrator (right click to see that option) and run:
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
2. Install Linux distribution app from Microsoft Store (tested on [Ubuntu 18.04 LTS](https://www.microsoft.com/store/apps/9N9TNGVNDL3Q)).
3. Set up username and password. You can try a command to test the system.
```
pwd
```
This will print your parent directory path.

Note: Your Windows C:\Users\ gets mapped to /mnt/c/Users/ in WSL. You can copy between the two directories using a command like:
```
cp /mnt/c/Users/Windows_username/Desktop/file.fasta /home/Ubuntu_user_name/
```

#### Mac
To bring up the command-line, use the Finder to navigate to _Applications->Utilities_ and double-click on the _Terminal_.<br/>
```
pwd
```
This will print your parent directory path.

### Make a New Folder
Make a new folder (MGJW) to use as a working directory during the workshop.
```
cd
mkdir MGJW
cd MGJW
```

### Running Commands
Type in a command and press the Enter key.  An example command and output:
```
ahmed$ ls
Applications					Library						Public
Desktop						Movies						Work
Documents					Music						Downloads					
```

The command `ls` produces a listing of files and directories in the current directory.

Run a command in the background by adding an ampersand after the command. When the command is finished, the output (if it has one) will be printed to the window.

```
ahmed$ any_application &
ahmed$
```

Use Multiple options with `ls`:

```
ahmed$ ls -lafh
total 744
drwxr-x---@  40 ahmed  1768498755   1.3K Dec 16 19:30 .
drwxr-xr-x    6 root        admin        192B Oct 13 02:06 ..
drwx------+   4 ahmed  1768498755   128B Jul  8 18:35 Music
drwx------+   6 ahmed  1768498755   192B Dec 31 10:33 Desktop
drwxr-xr-x    3 ahmed  1768498755    96B Dec 16 19:30 .matplotlib
drwxr-x---   40 ahmed  1768498755   1.3K Nov 23 12:10 Work
drwx------@   3 ahmed  1768498755    96B Jun 16  2022 Applications
```
> Each directory contains two special hidden directories named `.` and `..`. The first, `.` refers always to the current directory. `..` refers to the parent directory.  This lets you move upward in the directory hierarchy like this:


### Useful Unix Commands

Unix commands are actually executable programs.  When you type a command, it will search through all the directories in the PATH environment variable. If found, it will be executed. If not, it will give a "command not found" error.

| Command                     | Description                              |
| --------------------------- | ---------------------------------------- |
| `pwd`   | Command to see where you are "Parent Working Directory" |
| `cd /go_to_path/`    | Change current working directory |
| `ls`    | Lists files in a directory. `ls -l` (long listing), `ls -lh` (human-readable format, must be used with `-l`), `ls -a` (all, including hidden files). |
| `mv filename1 filename2`    | Move or rename a file or directory.  |
| `cp filename1 filename2`    | Copy a file. Use `-R` for copying directories recursively         |
| `chmod` | Change the read, write, and execute permissions of a file or directory. Use `ls -l` to show the permissions of files in a directory |
| `mkdir` | Make a directory.                         |
| `rm`    | Remove (delete) a file. This will delete it permanently. Use `-R` to remove directories and their contents recursively. You can use `rm -i`, which will ask for confirmation before deleting anything.           |
| `rmdir` | Remove a directory.                       |
| `ln`    | Create a symbolic or hard link.          |
| `head`            | View the first few lines of a file.  You can control how many lines to view. |
| `tail`            | View the end of a file. |
| `cat`             | Can be used to concatenate multiple files together into a single file, or to view the contents of a file in the terminal. |
| `more filename`            | Scroll through a file page by page.  Hit the space bar to see more or q to quit. |
| `less`            | A version of `more` with more features.                      |
| `diff filename1 filename2` | Compares files, and shows where they differ.                       |
| `wc filename`              | Count `-w` words, `-l` lines and/or `-m` characters in a file. This can be used for knowing how many reads are in a fastq file   |
| `tr`              | Substitute one character for another or deleting characters. |
| `sort`            | Sort the lines in a file.      |
| `uniq`            | Remove duplicates and keep only unique lines in a file.                           |
| `cut`             | Remove sections from each line of files, columns from each line of a file can be removed using this command.            |
| `grep`            | Print lines matching a specified pattern or that don't match a specified pattern. `grep -c '>' file.fasta` will print number of `>` in a file which is a proxy for number of contigs in a fasta file |
| `gzip` (`gunzip`) | Compress (uncompress) a file.                                |
| `tar`             | Archive or unarchive an entire directory into a single file. You may receive your run from a sequencing core in this format |
| `emacs`           | Run the Emacs text editor. It needs to be installed first.                |
| `gzcat`           | Look at a gzipped file without actually having to gunzip it. |
| `echo`            | Print a copy of some text to the screen. E.g. `echo 'Microbial Genomics'` |
| `apropos`         | Search for commands matching a keyword or phrase (e.g. text) |
| `find ./Work/ -name *.fasta` | Walk a directory hierarchy and find path for a file |
| `ssh`                  | Securely (encrypted) log into machines.               |
| `scp`                  | Securely copy files to and from remote machines. |
| `ftp`/ `sftp` (secure) | Transfer files using the File Transfer Protocol.             |
| `history` | Show all the commands that were run recently |


### Getting Information About Commands


The `man` command will give a synopsis of a command.

```
ahmed$ man chmod
CHMOD(1)                              General Commands Manual                              CHMOD(1)
NAME
     chmod – change file modes or Access Control Lists
SYNOPSIS
     chmod [-fhv] [-R [-H | -L | -P]] mode file ...
     chmod [-fhv] [-R [-H | -L | -P]] [-a | +a | =a] ACE file ...
     chmod [-fhv] [-R [-H | -L | -P]] [-E] file ...
     chmod [-fhv] [-R [-H | -L | -P]] [-C] file ...
     chmod [-fhv] [-R [-H | -L | -P]] [-N] file ...
DESCRIPTION
     The chmod utility modifies the file mode bits of the listed files as specified by the mode
     operand. It may also be used to modify the Access Control Lists (ACLs) associated with the
     listed files.
```

### Useful Command-Line Shortcuts and Keystrokes

| Shortcut/Keystroke          | Description                              |
| --------------------------- | ---------------------------------------- |
| Backspace or delete                 | Delete the previous character and back up one. |
| tab | Automatic command completion using the tab key. |
| Left arrow &larr; and right arrow &rarr; | Move the text insertion point (cursor) one character to the left or right. |
| Up &uarr; and down &darr; arrows | Move up and down in the command history. This lets you edit/rerun previous commands. |
| control-a (^a) | Move the cursor to the start of the line. |
| control-e (^e) | Move the cursor to the end of the line. |
| control-r (^r) | Enter text and search the history for commands that match it. |
| control-d (^d) | Delete the character currently under the cursor. D=Delete. |
| control-k (^k) | Delete the entire line from the cursor to the end. This can also act like cut. |
| control-y (^y) | Paste the contents of the control-k starting at the cursor. |
| `!<number>` | Reissue an old command, based on its number (you can get by running `history` command). |
| `!!`        | Reissue the last command. |
| `!<partial command string>` | Reissue the previous command that began with the indicated letters.  For example, `!m` would reissue the`man` command from the earlier example. `man chmod` |


Use Example:

```
ahmed$ ch<tab><tab>
ahmed$ ch
chat      checknr   chfn      chmod     chpass    chsh      
checkgid  chflags   chgrp     chown     chroot
ahmed$
```

### Wildcards

`*` stands for zero or more characters.  `?` stands for any single character.  For example, to list all fasta files with the extension ".fasta", run `ls` with the wildcard pattern "*.fasta"

```
ahmed$ ls *.fasta
sample1.fasta  sample2.fasta
```

To match files that begin with the characters "a" or "c" and end with ".pdf", you can put both characters inside square brackets `[ac]`. There are more advanced types of wildcard patterns (e.g. range of characters `[b-g]` or `[6-9]`).

Use Example:

```
ahmed$ ls [ac]*.pdf
algorithms-in-python.pdf
circular_genome.pdf
```

### Command Line Arguments


Many commands take arguments.  Positional arguments are often the names of one or more files that the command will operate on.  Most commands take "options", which fine-tune what the command does.  For example, while `wc -l file_name.txt` or `wc --lines file_name.txt` will only count number of lines in the file, `wc file_name.txt` will count number of lines, words and bytes in the file. Also, many commands will give a brief usage summary when you call help `-h` or `--help`.

Use Example for what we learnt so far:

```
ahmed$ cd
ahmed$ cd /
(/) ahmed$ ls -F
Applications/	Users/		dev/		private/	var/
Library/	Volumes/	etc/		sbin/
Network/	bin/		home/		tmp/
System/		cores/		opt/		usr/
(/) ahmed$ cd ~/Work/
(~/Work) ahmed$ pwd
/Users/ahmed/Work
(~/Work) ahmed$ cd ../project/
(~/project) ahmed$ ls
genomics/               file.fasta
(~/project) ahmed$ less file.fasta (use q to exit)
(~/project) ahmed$ wc -lmw file.fasta
       7     141    1033 3333.txt
```


### Standard Input and Output Redirection

Unix programs start out with three connections to the outside world. By default, commands take 1) input from the standard input and send the 2) results and 3) errors to standard output and standard error, respectively. By default, this is your screen. You can use a file as input and write the results of a command to a file, which is called input/output redirection.

| Connection Type     | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| standard input  | Input from keyboard or expects a file path on the command line. Redirect standard input from a file with < |
| standard output | It prints to your screen. Redirect standard output to a file with > |
| standard error  | It prints to your screen. Redirect standard error to a file with 2> |

Use Example:

```
ahmed$ ls -la
ahmed$ ls -la > list_directory.txt
ahmed$ less < list_directory.txt
ahmed$ less list_directory.txt
ahmed$ less list_directory.tt
ahmed$ less list_directory.tt 2> stderr.txt
```
In this example, I ran the `ls` command and it printed stdout to screen.  In the second command, I redirected stdout to a file. I then used the `less` command to read the file using the standard input `<`  symbol. I can also do that without the `<` symbol as most programs expect a file path on the command line. In command 5, I used a wrong file name `.tt` that does not exist in this folder so an error message was printed to the screen `list_directory.tt: No such file or directory`. In the following command, I redirected the error to stderr file that I named stderr.txt. You can also redirect both stdout and stderr to a file using `&> myfile.txt`.


```
ahmed$ wc -l list_directory.txt
ahmed$ ls -la | wc -l list_directory.txt
```
The `wc` program counts `-w` words, `-l` lines and/or `-m` characters in a file or in data sent to its standard input. In the first command we used the file we created in the previous commands `list_directory.txt` to count number of lines in this file. We can also redirect standard output to another command with the pipe `|`. This is a very powerful approach where you can pipe multiple commands. Imagine having a fastq file with all the sequence reads (each read is written in 4 lines) and you only want to know how many reads in the file and you do not need to save `wc -l` output. You can try the following command
```
wc -l list_directory.txt | awk '{print $1/3}'
```

---

## GitHub
[GitHub](https://github.com/) is a Version Control System Server which is good for collaborations, storing versions, restoring previous versions, and managing backups. The concept is simple. You have a local copy of the project/repository and a remote copy. The local copy is stored on your computer and the remote is online (e.g. GitHub). You can use a web browser or terminal to interact with the remote server (GitHub) and the terminal to interact with the local copy.

### Make an account
Please make an account on [GitHub](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home) (Free). You will need the account to import or create repositories, collaborate with others, and connect with the GitHub community (e.g. posting issues in the tool repository).

### Cloning a Repository

Sometimes you want to download and a repository from GitHub. Let's clone the Workshop material.

1. Go to our [MGJW GitHub Repository](https://github.com/ahmedmagds/Microbial-Genomics-Journey-Workshop-2023.git)
2. Click the 'Code' Button
3. Copy the URL
4. _Clone_ the repository to your local machine
   `git clone https://github.com/ahmedmagds/Microbial-Genomics-Journey-Workshop-2023.git`

Now you have a copy of the Workshop material on your computer!

**Tip: Don't clone a git repository into another git repository.**

### Updating a Local Repository

If changes are made to any of these files in the online, remote repository, you can use this command to update your local copy.
```
cd Microbial-Genomics-Journey-Workshop-2023
git pull
```
---

## Conda
[Conda](https://docs.conda.io/projects/conda/en/latest/index.html) is an open-source package management system and environment management system that runs on Windows, macOS, and Linux. Conda quickly installs, runs, and updates packages and their dependencies. Conda easily creates, saves, loads, and switches between environments on your local computer. It was created for Python programs but it can package and distribute software for any language.

### Install miniconda for Windows
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash ./Miniconda3-py39_4.12.0-Linux-x86_64.sh
source .bashrc
```
Follow instructions after the bash command and keep confirming and accepting.<br/>
You can then use the source command to activate conda in your current session or close and reopen the app.<br/>

### Install miniconda for Mac
Paste this link in your browser https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh
```
cd ~/Downloads
bash ./Miniconda3-latest-MacOSX-x86_64.sh
source .bash_profile
```
Follow instructions after the bash command and keep confirming and accepting.<br/>
You can then use the source command to activate conda in your current session or close and reopen the app.<br/>

### Install mamba
Let's try conda. First tool to install is a tool that replaces conda :).<br/>
[Mamba](https://mamba.readthedocs.io/en/latest/index.html) is a fast, robust, and cross-platform package manager. It is a CLI tool to manage conda s environments. It is considered as a drop-in replacement for conda, offering higher speed and more reliable environment solutions.
```
conda install -c conda-forge mamba
```
Mamba is now installed in the conda base environment.
## Bioconda
* [Bioconda](https://bioconda.github.io/) lets you install thousands of software packages related to biomedical research using the conda package manager.
* You will need a one-time set up to set up the channels for bioconda.
```
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict
```
**Now you are ready to install new tools. It is highly recommended to create a new environement for each tool separetely. Why do you think we need new env?**
### Install FASTQC
[FASTQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) is a quality control tool for high throughput sequence data.
```
mamba create -n fastqc -c bioconda fastqc
```
Let's check it.
```
fastqc -h
```
As you installed the tool in a new environment and not in the base, you need to activate the environment first.
```
conda activate fastqc
fastqc -h
```
You can switch between environments using `conda activate env_name` and `conda deactivate`.
### Install MultiQC
[MultiQC](https://github.com/ewels/MultiQC) is a tool to create a single report with interactive plots for multiple bioinformatics analyses across many samples.
```
conda deactivate
mamba create -n multiqc -c bioconda multiqc
```
Let's check it.
```
conda activate multiqc
multiqc -h
```
**Important Lesson**
Did it work? Or you get this error?
```
Traceback (most recent call last):
  File "/Users/ahmed/miniconda3/envs/multiqc/bin/multiqc", line 6, in <module>
    from multiqc.__main__ import run_multiqc
  File "/Users/ahmed/miniconda3/envs/multiqc/lib/python3.6/site-packages/multiqc/__init__.py", line 16, in <module>
    from .multiqc import run
  File "/Users/ahmed/miniconda3/envs/multiqc/lib/python3.6/site-packages/multiqc/multiqc.py", line 27, in <module>
    import rich_click as click
  File "/Users/ahmed/miniconda3/envs/multiqc/lib/python3.6/site-packages/rich_click/__init__.py", line 16, in <module>
    from rich_click.rich_command import RichCommand
  File "/Users/ahmed/miniconda3/envs/multiqc/lib/python3.6/site-packages/rich_click/rich_command.py", line 5, in <module>
    from rich_click.rich_click import rich_abort_error, rich_format_error, rich_format_help
  File "/Users/ahmed/miniconda3/envs/multiqc/lib/python3.6/site-packages/rich_click/rich_click.py", line 5, in <module>
    import rich.markdown
  File "/Users/ahmed/miniconda3/envs/multiqc/lib/python3.6/site-packages/rich/markdown.py", line 1
    from __future__ import annotations
    ^
SyntaxError: future feature annotations is not defined
```
If you get an error, you can Google it. It may be because of broken installtion on bioconda (not always will work well). Let's check the tool [MultiQC](https://github.com/ewels/MultiQC) GitHub page and see if we can install it through a different way.
```
conda deactivate
mamba create -n multiqc python
conda activate multiqc
pip install multiqc
multiqc -h
```
### Install Mash
[Mash](https://mash.readthedocs.io/en/latest/index.html)
```
conda deactivate
mamba create -n mash -c bioconda mash
conda activate mash
mash
```
You will need to download a database for [Mash](https://obj.umiacs.umd.edu/mash/screen/RefSeq88n.msh.gz).
### Install Shovill
[Shovill](https://github.com/tseemann/shovill)
```
conda deactivate
mamba create -n shovill -c conda-forge -c bioconda -c defaults shovill
conda activate shovill
shovill --check
```
### Install Quast
[Quast](https://quast.sourceforge.net/install.html)
```
conda deactivate
mamba create -n quast -c bioconda quast
conda activate quast
quast -h
```
### Install CheckM
[CheckM](https://github.com/Ecogenomics/CheckM)]
```
conda deactivate
mamba create -n checkm python=3.9
pip3 install numpy
pip3 install matplotlib
pip3 install pysam
pip3 install checkm-genome
conda activate checkm
checkm -h
```
**If you try `conda install -c bioconda checkm-genome`, it will not work. This is an example of where you have to check multiple sources**
### Install Busco
[Busco](https://busco.ezlab.org/)]
```
conda deactivate
mamba create -n busco -c conda-forge -c bioconda busco=5.4.4
conda activate busco
busco -h
```
**If you try` mamba create -n busco -c bioconda busco`, it will not work. This is an example of where you have to check multiple sources**
### Install Prokka
[Prokka](https://github.com/tseemann/prokka)]
```
conda deactivate
mamba create -n prokka -c bioconda prokka
conda activate prokka
prokka
```
### Install Bakta
[Bakta](https://github.com/oschwengers/bakta)]
```
conda deactivate
mamba create -n bakta -c conda-forge -c bioconda bakta
conda activate bakta
bakta
which bakta
```
You will need to download a database for annotations with bakta. I would save it in the bakta env. Use the path from last command in the next command.
```
bakta_db download --output /something/miniconda3/envs/bakta/
```
### Install Snippy
[Snippy](https://github.com/tseemann/snippy)
```
conda deactivate
mamba create -n snippy -c conda-forge -c bioconda -c defaults snippy
snippy
```
### Install abricate
[abricate](https://github.com/tseemann/abricate)
```
conda deactivate
mamba create -n abricate -c conda-forge -c bioconda -c defaults abricate
conda activate abricate
abricate --check
```
### Install mlst
[mlst](https://github.com/tseemann/mlst)]
```
conda deactivate
mamba create -n mlst -c conda-forge -c bioconda -c defaults mlst
conda activate mlst
mlst
```
### Install blast
[blast](https://www.ncbi.nlm.nih.gov/books/NBK279690/)]
```
conda deactivate
mamba create -n blast -c bioconda blast
conda activate blast
blastn
```
---

## Text Editors

Sometimes you will need to create and write text to a file while using the terminal. In this case, you will need to use a terminal text editor. There are many text editors available (Emacs, vim, Nano, Sublime and many more). We are going to try `emacs`.

### emacs
Let's install emacs using conda and make a new text file and write something in it and explore it with `less`. You can use control-x (^x) followed by control-s (^s) to save and then control-x (^x) followed by control-c (^c) to exit. A cheat sheet can be found [here](https://www.gnu.org/software/emacs/refcards/pdf/refcard.pdf).
```
cd MGJW
conda deactivate
mamba create -n emacs -c conda-forge emacs
conda activate emacs
emacs -nw test.txt
less test.txt
```
---
## Further Readings
* [Unix Command Reference](https://files.fosswire.com/2007/08/fwunixref.pdf)
* [Illumina Sequencing by Synthesis Video](https://youtu.be/fCd6B5HRaZ8)
* [Biostar](https://www.biostars.org/) An Online Question & Answer Resource for the Bioinformatics Community
