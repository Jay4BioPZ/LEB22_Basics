# LINUX Server Configuration

## Conda Basics

### Directory structure

```bash

(base) leb3a@bbt:~$ ll /rd1/home/leb3a/miniconda3/
total 76
drwxr-xr-x 16 leb3a leb  4096 Apr 11 11:52 ./
drwxr-xr-x 15 leb3a leb  4096 Apr 14 11:53 ../
drwxr-xr-x  2 leb3a leb  4096 Apr 11 11:51 bin/
drwxr-xr-x  2 leb3a leb  4096 Apr 11 11:51 compiler_compat/
drwxr-xr-x  2 leb3a leb  4096 Apr 11 11:51 condabin/
drwxr-xr-x  2 leb3a leb  4096 Apr 11 11:51 conda-meta/
drwxr-xr-x  3 leb3a leb  4096 Apr 11 11:57 envs/                            #环境配置的位置
drwxr-xr-x  4 leb3a leb  4096 Apr 11 11:51 etc/
drwxr-xr-x  8 leb3a leb  4096 Apr 11 11:51 include/
drwxr-xr-x 15 leb3a leb  4096 Apr 11 11:51 lib/
-rw-r--r--  1 leb3a leb 11055 Jan 28 04:05 LICENSE.txt
drwxr-xr-x 47 leb3a leb  4096 Apr 11 11:58 pkgs/                            #解压缩档放置的位置
drwxr-xr-x 10 leb3a leb  4096 Apr 11 11:51 share/
drwxr-xr-x  3 leb3a leb  4096 Apr 11 11:51 shell/
drwxr-xr-x  3 leb3a leb  4096 Apr 11 11:51 ssl/
drwxr-xr-x  3 leb3a leb  4096 Apr 11 11:51 x86_64-conda_cos6-linux-gnu/
drwxr-xr-x  3 leb3a leb  4096 Apr 11 11:51 x86_64-conda-linux-gnu/
```

### Managing environments

```bash
# To create a env
conda create --name <myenv>
# To create with specific version of Python or packages
conda create -n <myenv> python=3.6
conda create -n <myenv> scipy

# Activate and deactivate envs
conda activate <myenv>
conda deactivate

# Remove env
conda remove --name <myenv> --all
```

### Managing conda packages

```bash
# Install a package
conda install -n <myenv> <pack_name>

# If necessary (for example, conda didn't work out), use pip install
pip install <pack_name>

# Add mirrored channel to .condarc
conda config --add channels <address>
# Common packages from 清华镜像
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud

```

## Git Basics

![Git_concept](~/../../work_bench/photo/Git_explain.png)

### Git initialization and remote linkage

```bash
# Initialize a git
git init

# Set up global configuration 
git config --global user.name "Jay"
git config --global user.email "ycjaychiang@pku.edu.cn"

# Set up a SSH key for remote linkage on Github
ssh-keygen -t rsa -b 4096 -C "ycjaychiang@pku.edu.cn"
# A ssh public key and private key would be generated in /rd1/home/leb3a/.ssh/
# in which id_rsa is the private one while id_rsa.pub is the public one
# Copy and paste the public key to https://github.com/settings/keys

# Link it with a Github repository
# Add a remote repo (github) to our local repo (linux server)
git remote add origin git@github.com:Jay4BioPZ/LEB22_Basics.git

# Once you have added the ssh-key to your github account, remote origin would be successfully linked. Everyting is ready now.
```

### Git usage

After git init, you are having a new project being tracked by git. You are initially in a master branch.

You can make a "commit" to the remote repo from your local repo once you finish programming and "add" your progress from working tree to index/staging area.

```bash
# Check git status
git status

# (demo)
On branch master # which branch you are on now
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean # current changes waiting to be committed

# Add file to staging area
git add <filename> # add a specific file
git add . # add all files that are not yet added

# Make a commit
git commit -m "a change have been made for..."
#-m: leave a message
```

### Git branching

Git allows users to writing their code on a branch without effecting the main project. Once they hope to update latest and functional parts to the remote repo (i.e., where the main project locates), they "push" their commitment from local branch.

Similarly, if they wish to receive progresses made by other users that have been uploaded to the remote repo, they "pull" it down to their working tree and then go through the "add-commit" workflow to put them to the local branch.

```bash
# Create/go to a branch
git checkout <new_branch>
git checkout master

# List available branches
git branch

# (demo by git status)
On branch <new_branch> # where I am now
nothing to commit, working tree clean

# After commitment, upload progress on local branch to remote repo
git push -u origin <branch_name>
# the <branch_name> here should be substitute by your branch name (i.e., master...)

# Pull from remote repo
git pull origin master
```

## R environments

We gonna set up a R environment on the linux server through VS code. Such settings would help us use different programming languages in a more integrated and efficient way.

There are two way to do R programming in VS code:

- Run R in non-interactive cell environment
  - Create a new conda env.
  - Install R and R-debug extensions on VS code at marketplace.
  - Install r-base (CRAN) or radian (alternative console) using conda install.
  - Open an R terminal.
  - Install necessary packages in R terminal.
  - Set up r.path in settings, enable bracketedPaste.
- Run R in interactive cell environment (i.e., jupyter)
  - Create a new conda env.
  - Install jupyter using conda install.
  - Install r-base (CRAN) using conda install.
  - Open an R terminal.
  - Install IRkernel package, register it to current R installation.
  - Start programming in jupyter.

### Common steps

```bash
# Make a new environment
conda create -n R_test
conda activate R_test

# Install radian and r-base
conda install -c conda-forge radian
conda install -c conda-forge r-base

# Run R: open a new terminal [ctrl+shift+`] and type in:
# "R": for activating r-base
# "radian": for activation radian
# The terminal default user interface would change
>   #r-base
r$> #radian
# Quit R
q()

# Install languageserver and other necessary packages 
# an implementation of the Microsoft's Language Server Protocol for the language of R (perform auto complete, documentation...)
> install.packages('languageserver')
# select 1:0-https as mirror
> install.packages('jsonlite')
> install.packages('rlang')
```

### VS Code settings
- Specify "r.path for linux" in *File > Preferences > Settings* on local VS code.
- Enable "R:bracketedPaste" in *File > Preferences > Settings* on local VS code.

### Jupyter configuration

```bash
# Install Jupyter in your conda env
conda install jupyter

# In R terminal, install IRkernel
> install.packages("IRkernel")
> IRkernel::installspec
```

## Common keyboard shortcuts in VS Code

- `[ctrl+shift+P]` show command palette
- ``[ctrl+shift+`]`` show terminal
- `[ctrl+shift+V]` show markdown preview

## Reference

- For the user guide of Conda, click [here](https://docs.conda.io/projects/conda/en/latest/index.html).
- For refereing 清华镜像, click [here](https://mirrors.tuna.tsinghua.edu.cn/).
- For the man page and cheat sheet regarding Git, click [here](https://git-scm.com/docs).
- For the tutorial of how to set up an R environment on VS code, click [r-base](https://anaconda.org/conda-forge/r-base), [radian](https://github.com/randy3k/radian#how-to-switch-to-a-different-r-or-specify-the-version-of-r), [VS_code_tutorial_blog](https://www.infoworld.com/article/3625488/how-to-run-r-in-visual-studio-code.html) or [VScode_tutorial_video](https://www.youtube.com/watch?v=INP-FsluDuk&t=2s).
