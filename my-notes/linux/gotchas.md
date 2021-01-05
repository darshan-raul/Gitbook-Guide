# Gotchas

* Files on Linux \(or any Unix\) are case sensitive. This means that FILE1 is different from file1, and /etc/hosts is different from /etc/Hosts \(the latter one does not exist on a typical Linux computer\)
* The Bash shell has two built-in commands called **pushd** and **popd**. Both commands work with a common stack of previous directories. Pushd adds a directory to the stack and changes to a new current directory, popd removes a directory from the stack and sets the current directory. 
* Should you be convinced that a man page exists, but you can't access it, then try running mandb on Debian/Mint. or run makewhatis on CentOS/Redhat.
* **everything is a file** 
  *  ****directory is a special kind of file, but it is still a \(case sensitive!\) file. 
  * Each terminal window \(for example /dev/pts/4\), 
  * any hard disk or partition \(for example /dev/sdb1\) and 
  * any process are all represented somewhere in the file system as a file.
* The file utility determines the file type. Linux does not use extensions to determine the file type. The command line does not care whether a file ends in .txt or .pdf. As a system administrator, you should use the file command to determine the file type.
* There are some differences in the filesystems between Linux distributions. For help about your machine, enter **man hier**
* 


