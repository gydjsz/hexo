---
title: 缩略linux路径
---
emacs ~/.bashrc  将

if [ "$colovr prompt" = yes ]; then

  PS1='${debian chroot:+($debian chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else

  PS1='${debian chroot:+($debian chroot)}\u@\h:\w\$ '

里面的小写w改为大写的W就可以缩略路径名，避免进入的目录多了之后导致路径名太长，可以使用pwd来查看全目录
