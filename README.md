# Trying to install VHDL_LS (VHDL Language Server/Support) for VSCode on CentOS 7 machine
## Workaround
You may install VSCode VHDL_LS (ver. 0.3.1) extension and look at the error pop-up when you open your vhd file, something like this:

   ***... version `GLIBC_2.18' not found (required by ... )***
 
The other way to reproducing this error is
```
/home/username/.vscode/extensions/hbohlin.vhdl-ls-0.3.1/server/vhdl_ls/0.15.0/vhdl_ls-x86_64-unknown-linux-gnu/bin/vhdl_ls --version

```


1. CentOS 7 using GLIBC 2.17 but for VHDL_LS we need version 2.18, so install (/opt as dir for the lib):
```
cd ~/Downloads
wget http://ftp.gnu.org/gnu/glibc/glibc-2.18.tar.gz
tar zxvf glibc-2.18.tar.gz
cd glibc-2.18
mkdir build
cd build
../configure --prefix=/opt/glibc-2.18
make -j4
sudo make install
```
2. Install pathelf:
```
a) http://download-ib01.fedoraproject.org/pub/epel/7/x86_64/
b) rpm -Uvh epel-release*rpm
c) yum install patchelf
```
3. Execute 
```
patchelf --set-interpreter /opt/glibc-2.18/lib/ld-linux-x86-64.so.2 --set-rpath /opt/glibc-2.18/lib:/usr/lib64 /home/username/.vscode/extensions/hbohlin.vhdl-ls-0.3.1/server/vhdl_ls/0.15.0/vhdl_ls-x86_64-unknown-linux-gnu/bin/vhdl_ls
```
4. Install VHDL_LS extension (if it's not installed yet)
5. Create .toml files: main for std libs in home folder, custom in the project dir

As an example: 
- "hidden.vhdl_ls.toml" rename to ".vhdl_ls.toml" and place to your ~/
- vhdl_ls.toml is in your project folder custom library file

