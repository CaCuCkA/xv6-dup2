# Extra task: Add syscall dup2() to xv6

**Authors:** <a href="https://github.com/CaCuCkA">Mykola Yakovkin</a>

## Prerequisites

❯ Ubuntu or WSL

❯ GCC

❯ Git

❯ xv6

❯ QEMU

> **Note**
>
> To install `QEMU` you should run this code on your computer: <br>
> **Important this code works only on Ubuntu!**
> ```shell
>    $ sudo apt update
>    $ sudo apt install qemu-kvm qemu virt-manager virt-viewer libvirt-bin
> ```

## Installation

To install this project, you need to clone the repository first:

```shell
$ mkdir ~/workdir
$ cd ~/workdir
$ git clone https://github.com/CaCuCkA/xv6-dup2.git
$ cd xv6-dup2
```

## Compilation

* To run the emulation without a graphical interface, use the command below in your terminal:
   ```shell
  $ make qemu-nox 
  ```
* To run the emulation with QEMU's terminal-like interface in a new window:
   ```shell
   $ make qemu
   ```

## Implementation

**_From Linux manual:_**

`dup2(oldfd, newfd)` duplicates the file descriptor `oldfd` onto `newfd`. If `newfd` is already open,
it's first closed. If `oldfd` is an invalid descriptor, an error is returned, If the descriptors are identically, then `
newfd` is returned immediately without being closed. The returned descriptor shares the same file position and flags as
the original.

**_Therefore, our steps are:_**

First, verify if `oldfd` is valid using the `argfd` function, which checks its presence in the file table. Next, ensure
`newfd` is non-negative and less than `NOFILE`. If `oldfd` and `newfd` are identical, simply return `newfd`. If `newfd`
points to an open file, close it. Then, point `newfd` to the `oldfd` file and duplicate it using `filedup(fds)`.