---
title: How to operate xsel in Linux?
---

1. 
first download xsel in Linux by using this command: sudo apt-get install xsel
or 
download the xsel(the link is in my other blog--Link Collection)
modify the ~/.bashrc by vim or emacs
For example:
```
vim ~/.bashrc
in the last line add: export PATH='~/XselDirectory/'
source ~/.bashrc //make the operation effective
```

2. Some operation
xsel < file //read file content to xsel 
xsel > file //write xsel content to xsel 
xsel  // show xsel content
cat file | xsel -b -i  //file to xsel
cat file | xsel -b -a  //add file content to xsel last line 

more content can read README in XselDirectory
