# 项目结构

![][https://courses.edx.org/assets/courseware/v1/8a874934d6c335342808150e3be7a2d0/asset-v1:HarvardX+PH125.5x+1T2024+type@asset+block/data_science_1_rev.png]

---

#### Useful Unix Commands 有用的 Unix 命令

|**Command 命令**|**Description 描述**|**Examples 例子**|
|---|---|---|
|`ls`|List directory content 列出目录内容||
|`mkdir` _dir_  `mkdir` 目录|Make a directory 建立一个目录|`mkdir projects` –make the directory projects  <br>`mkdir projects` –创建目录项目<br><br>`mkdir docs` –make the directory docs  <br>`mkdir docs` –创建目录 docs<br><br>`mkdir junk` –make the directory junk  <br>`mkdir junk` – 使目录成为垃圾|
|`rmdir` _dir_  `rmdir` 目录|Remove a directory (directory must be empty; otherwise use “rm”)  <br>删除目录（目录必须为空；否则使用“rm”）|`rmdir junk` –remove the directory `junk`  <br>`rmdir junk` –删除目录 `junk`|
|`cd` _dir_  `cd` 目录|Change directory 更改目录|`cd /projects` – move to the `projects` directory (an absolute path)  <br>`cd /projects` – 移动到 `projects` 目录（绝对路径）<br><br>`cd projects` – move to the `projects` directory, assuming we are already in the home directory (a relative path)  <br>`cd projects` – 移动到 `projects` 目录，假设我们已经在主目录中（相对路径）|
|`cd ..`|Go up one directory to the parent directory  <br>上一级目录到上一级目录|`cd ../..` – move up two parent directories from our current directory  <br>`cd ../..` – 从当前目录向上移动两个父目录|
|`cd ~`|Go to the home directory  <br>进入主目录||
|`cd -`|Go to whatever directory you just left  <br>转到您刚刚离开的目录||
|`pwd`|Print the present working directory  <br>打印当前工作目录||
|Tab key Tab键|Autocomplete 自动完成|`cd d` + tab – autocompletes to `docs` if it is the only directory that begins with d; or list the different options.  <br>`cd d` + tab – 如果它是唯一以 d 开头的目录，则自动补全到 `docs` ；或列出不同的选项。|
|`mv` _file1 file2_  `mv` 文件1 文件2|Move or rename files  <br>移动或重命名文件<br><br>_Warning –this is permanent, and you will not get a warning message if you are overwriting files.  <br>警告 – 这是永久性的，如果您覆盖文件，您将不会收到警告消息。_|`mv ~/docs/resumes/cv.tex ~/docs/reports/` –move the `cv.tex` file from the resume folder to the reports folder  <br>`mv ~/docs/resumes/cv.tex ~/docs/reports/` – 将 `cv.tex` 文件从简历文件夹移动到报告文件夹<br><br>`mv cv.tex resume.tex` – rename `cv.tex` to `resume.tex`  <br>`mv cv.tex resume.tex` – 将 `cv.tex` 重命名为 `resume.tex`<br><br>`mv ~/docs/resumes ~ /docs/reports/` - move the resume folder into the reports folder  <br>`mv ~/docs/resumes ~ /docs/reports/` - 将简历文件夹移动到报告文件夹中|
|`cp` _file1 file2_  `cp` 文件1 文件2|Copy file1 to file2  <br>将文件1复制到文件2|`cp ~ ~/docs/reports/` – make a copy of the `cv.tex` file from the `resume` folder in the `reports` folder  <br>`cp ~ ~/docs/reports/` – 从 `reports` 文件夹中的 `resume` 文件夹中复制 `cv.tex` 文件|
|`rm` _file_  `rm` 文件|Delete file 删除文件<br><br>_Warning – this is permanent! You cannot retrieve files from the recycling bin!  <br>警告——这是永久性的！您无法从回收站检索文件！_|`rm ~/docs/resumes/cv.tex` – delete the file `cv.tex`  <br>`rm ~/docs/resumes/cv.tex` – 删除文件 `cv.tex`|
|`less` _file_  `less` 文件|View file 查看文件|`less ~/docs/resumes/cv.tex` –open `cv.tex` in the less text viewer  <br>`less ~/docs/resumes/cv.tex` – 在 less 文本查看器中打开 `cv.tex`|
|`rm -r` _dir_  `rm -r` 目录|Remove recursively all folders in directory _dir_ and the directory itself.  <br>递归删除目录 dir 中的所有文件夹以及目录本身。||
|`ls -a`|List all directory content, including hidden files  <br>列出所有目录内容，包括隐藏文件||
|`ls -l`|List all directory content in long form (including permissions, size and date)  <br>以长格式列出所有目录内容（包括权限、大小和日期）||
|`ls -t`|List all directory content in chronological order  <br>按时间顺序列出所有目录内容|`ls -lart` – show more information for all files in reverse chronological order for your current directory  <br>`ls -lart` – 按时间倒序显示当前目录中所有文件的更多信息|
|`man` _command_  `man` 命令|Show the manual for the command. Note – this does not work for GitBash  <br>显示该命令的手册。注意 – 这不适用于 GitBash|`man ls` – show the manual instructions for the command ls.  <br>`man ls` – 显示命令 ls 的手动说明。|
|`help`|Show the manual for the command in GitBash  <br>在 GitBash 中显示该命令的手册|`ls --help` – show help instructions for the command `ls`  <br>`ls --help` – 显示命令 `ls` 的帮助说明|
|_command1_ \| _command2_ 命令1 \|命令2|Pipe the results of command 1 to command 2  <br>将命令 1 的结果通过管道传输到命令 2|`man ls \| less` – show the help instructions for the command ls in the less viewer  <br>`man ls \| less` – 在 less 查看器中显示命令 ls 的帮助说明|
|`*` (wildcard)  `*` （通配符）||`ls *.html` –list all the files ending in html in your current directory  <br>`ls *.html` –列出当前目录中所有以 html 结尾的文件<br><br>`rm *.html` – remove all files ending in html in your current directory  <br>`rm *.html` – 删除当前目录中所有以 html 结尾的文件|
|`?` (any character)  <br>`?` （任何字符）||`rm file.???.html` – remove all files whose names follow the pattern; eg file-001.html, file-002.html etc.  <br>`rm file.???.html` – 删除名称遵循该模式的所有文件；例如 file-001.html、file-002.html 等。<br><br>`rm file.???.*` – remove all files whose names follow the pattern regardless of their extension; eg file-001.html, file-002.csv, file-any.R, etc.  <br>`rm file.???.*` – 删除名称遵循该模式的所有文件，无论其扩展名如何；例如 file-001.html、file-002.csv、file-any.R 等。|
|$_var_|>$ identifies a variable  <br>>$ 标识一个变量|`echo $HOME` – print your home directory  <br>`echo $HOME` – 打印你的主目录<br><br>`echo $SHELL` – print your shell name  <br>`echo $SHELL` – 打印你的 shell 名称|
|`export` _val=value_  `export` val=值|Change the value of the variable _val_ (Bash shell specific)  <br>更改变量 val 的值（Bash shell 特定）||
|`open` _file_ (mac)_file_ (windows)  <br>`open` 文件 (mac) 文件 (windows)|Opens a file or program  <br>打开文件或程序|`open Report.Rmd` – open Report.Rmd in RStudio  <br>`open Report.Rmd` – 在 RStudio 中打开 Report.Rmd|

#### Absolute path vs. relative path  
绝对路径与相对路径

A full path specifies the location of a file from the root directory. It is independent of your present directory, and must begin with either a “/” or a “~”. In this example, the full path to our “project-1” file is:   
完整路径指定根目录中文件的位置。它独立于您当前的目录，并且必须以“/”或“~”开头。在此示例中，“project-1”文件的完整路径是：

`/home/projects/project-1`

A relative path is the path relative to your present working directory. If our present working directory is the “projects” folder, then the relative path to our “project-1” file is simply:   
相对路径是相对于当前工作目录的路径。如果我们当前的工作目录是“projects”文件夹，那么“project-1”文件的相对路径就是：

`project-1`

#### Path shortcuts 路径快捷方式

One period “.” is your current working directory  
一个句点“.”是你当前的工作目录

Two periods “..” is the parent directory (up one from your present working directory)   
两个句点“..”是父目录（从当前工作目录向上一级）

A tilde “~” is your home directory.  
波形符“~”是您的主目录。

#### More path examples 更多路径示例

Example 1.     Your current working directory is `~/projects` and you want to move to the `figs` directory in the `project-1` folder  
示例 1. 您当前的工作目录是 `~/projects` ，并且您想要移动到 `project-1` 文件夹中的 `figs` 目录

- Solution 1: `cd ~/projects/project-1/figs` (absolute)  
    解决方案 1： `cd ~/projects/project-1/figs` （绝对）
- Solution 2:  `cd project-1/figs` (relative)  
    解决方案 2： `cd project-1/figs` （相对）

Example 2.     Your current working directory is `~/projects` and you want to move to the `reports` folder in the `docs` directory  
示例 2. 您当前的工作目录是 `~/projects` ，并且您想要移动到 `docs` 目录中的 `reports` 文件夹

- Solution 1: `cd ~/dos/reports` (absolute)  
    解决方案 1： `cd ~/dos/reports` （绝对）
- Solution 2: `cd ../docs/reports` (relative)  
    解决方案 2： `cd ../docs/reports` （相对）

Example 3.     Your current working directory is `~/projects/project-1/figs` and you want to move to the `project-2` folder in the `projects` directory.  
示例 3. 您当前的工作目录是 `~/projects/project-1/figs` ，并且您想要移动到 `projects` 目录中的 `project-2` 文件夹。

- Solution 1: `cd ~/projects/project-2` (absolute)  
    解决方案 1： `cd ~/projects/project-2` （绝对）
- Solution 2: `cd ../../project-2` (relative)  
    解决方案 2： `cd ../../project-2` （相对）