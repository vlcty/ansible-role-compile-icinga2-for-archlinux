# ansible-role-compile-icinga2-for-archlinux

This ansible role prepares an Archlinux server to compile Icinga 2 for Archlinux.

## How it works

Steps done:
* Setup the system like installing needed libs, setting the compiler threads etc
* Download the Icinga 2 source code via git from GitHub
* Extract the latest git tag
* Upload an PKGBUILD file to tell makepkg what to do
* Compile the packages
* Download the packages

You should always update your system you are compiling on before running this role. If you are #DevOps enough, you can use a vagrant box or even better: a LXC container. I use a freshly installed LXC container.

**Hint -> Used threads**: You can set the variable `compiler_threads` to adjust the number of threads used while compiling. The default is 4. If your system can handle more (I use 12 for example) you can speed it up a bit.

**Hint -> nice level**: By default the compiler process gets a nice level of 10. This is done to prevent others from eating up to much CPU. You can set a different nice value by setting `compiler_nicelevel`. The value range is 0 (normal priority) to 19 (ultra low prio).

## Requirements

You need the following to use this role:
* A working ansible environment
* A compiler machine running Archlinux with
 * a normal UNIX user you connect to first (Login directly at root will fuck up everything and you should be ashamed if you do something like that!)
 * 2 CPU cores but more will speed things up. More than 12 is not helping
 * 4+ GByte of RAM
 * A few GByte diskspace
* A strategy to distribute your new packages

## Security

This role deploys a sudoers file under `/etc/sudoers.d/sudo-icingacompiler`. The UNIX user which whom you connect to the system is allowed to run pacman with sudo without supplying a password. **This allows the user to install ANYTHING!**. Please keep that in mind!  
If you ask me you should destroy the compiler machine right after you finished downloaded the packages.

## Precompiled packages

I run my own Archlinux mirror server distributing the Icinga2 packages. The packages there are generated with this role. This mirror is unofficial but public available.  
If you want to use it check this out: https://blog.veloc1ty.de/2018/01/24/inofficial-icinga-2-archlinux-mirror-server/
