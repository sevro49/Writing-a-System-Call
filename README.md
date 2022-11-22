# Writing-a-System-Call
This repository is a Medeniyet University BIL301 - Operating Systems (Fall22) HomeWork1

One or two details may differ depending on the kernel versions you are using. Since the purpose of this repository is to show you how to add system call, there are many examples on the web, you can also benefit from them

## What to Know in Advance
- C and system programming (Calling a system calls via C program)
- Kernel, User space, Kernel space etc. definitions
- Linux Terminal

## Learnin Objectives
-  Compile the Linux kernel
-  Adding a new system call by making changes on the kernel
-  Copying data from user space to kernel space (with copy_from_user or strncpy_from_user).
-  Learn how to test the operation of the added system call by calling the C program (syscall (2) - Linux manual page) and to experience debugging the problems and errors that will arise in these stages

## Resources
- The following system call installation, test programs, and steps are largely taken from: [Tutorial - Write a System Call • Stephen Brennan](https://brennan.io/2016/11/14/kernel-dev-ep3/ "Tutorial - Write a System Call • Stephen Brennan")
- For more details [Adding a New System Call — The Linux Kernel documentation](http://www.kernel.org/doc/html/latest/process/adding-syscalls.html "Adding a New System Call — The Linux Kernel documentation")

## Kernel Download
First of all, in order not to harm your own system, I recommend you to install VMware or VirtualBox.

Then install Linux (ubuntu, archlinux, kali, fedora, opensuse, whatever) using the .iso file you downloaded by defining a new machine with virtualbox. (I use ubuntu).

If you have good Linux-terminal knowledge, you can install without GUI and minimally (it'll be faster to run).

First download the linux kernel from [kernel.org](http://kernel.org "kernel.org") by running the Linux you installed in VirtualBox (if you want, it can be the same version as the kernel you use on your system.)

You can download it with the command below, the it downloads it to the /usr/src/ folder

```shell
sudo apt-get source linux-source
```

To see the currently running kernel version on your system:

```shell
uname -r
```

You can download via terminal or browser, does not matter. You can use **wget, curl** etc. in terminal

### Configuring the downloaded Kernel

Extract the tar file to a folder:

```shell
tar xvf linux-TheVersionYouDownloaded
```

Than enter this folder from terminal

```shell
cd linux-TheVersionYouDownloaded
zcat /proc/config.gz > .config
```
Depending on the system, the config file may be in different location (it is inside boot in ubuntu)
```shell
zcat /proc/config.gz
zcat /boot/config
zcat /boot/config-$(uname -r)
```
If the config extension is not gz, you can use the regular **cat** command

Or you can use **cp** command. [Click here](https://www.cyberciti.biz/faq/how-to-copy-one-file-contents-to-another-file-in-linux/ "Click here")

Then, to edit the kernel config file that you will create from the existing config file in the system:

```shell
yes "" | make oldconfig
```

Or to copy exactly what is in the system:

```shell
make olddefconfig
```

Change the version name in the .config as follows (it will be added with this name when the kernel is added to system)

- Edit **CONFIG_LOCALVERSION="..."**
- **CONFIG_LOCALVERSION="-yourname"**

Note: If in the future during compile it gives an error about certificates ("deb..cert.perm" not found), delete the debian..cert.perm mentioned in the message from the .config file and just leave it blank:
**CONFIG_SYSTEM_TRUSTED_KEYS = ""**
**SYSTEM_REVOCATION_KEYS = ""**

Or via terminal:

```shell
scripts/config --disable SYSTEM_TRUSTED_KEYS
scripts/config --disable SYSTEM_REVOCATION_KEYS
```

## System Call Addition Steps

In the Linux kernel you downloaded, go to the file **arch/x86/entry/syscalls/syscall_64.tbl** and add your own system call with the next number to the end of the 300 system call. for example, if the next number is 335:

335&emsp;common&emsp; your_name&emsp;sys_your_name
(there are tab spaces between every parameter)

