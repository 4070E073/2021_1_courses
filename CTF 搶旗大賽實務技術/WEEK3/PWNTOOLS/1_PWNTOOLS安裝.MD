# 撰寫安裝SCRIPT
```
apt-get update
apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade pwntools
```
# 執行
```
root@kali:~# gedit pwntools_install.sh
root@kali:~# bash pwntools_install.sh 
```
```
一路yes到底
```
# pwn -h
```
Command Line Tools
https://docs.pwntools.com/en/latest/commandline.html#pwn
```
```
usage: pwn [-h]
           {asm,checksec,constgrep,cyclic,debug,disasm,disablenx,elfdiff,elfpatch,errno,hex,
            phd,pwnstrip,scramble,shellcraft,template,unhex,update,version}
           ...

Pwntools Command-line Interface

positional arguments:
  {asm,checksec,constgrep,cyclic,debug,disasm,disablenx,elfdiff,elfpatch,errno,hex,phd,pwnstrip,scramble,
    shellcraft,template,unhex,update,version}
    asm                 Assemble shellcode into bytes
    checksec            Check binary security settings
    constgrep           Looking up constants from header files. Example:
                        constgrep -c freebsd -m ^PROT_ '3 + 4'
    cyclic              Cyclic pattern creator/finder
    debug               Debug a binary in GDB
    disasm              Disassemble bytes into text format
    disablenx           Disable NX for an ELF binary
    elfdiff             Compare two ELF files
    elfpatch            Patch an ELF file
    errno               Prints out error messages
    hex                 Hex-encodes data provided on the command line or stdin
    phd                 Pwnlib HexDump
    pwnstrip            Strip binaries for CTF usage
    scramble            Shellcode encoder
    shellcraft          Microwave shellcode -- Easy, fast and delicious
    template            Generate an exploit template
    unhex               Decodes hex-encoded data provided on the command line
                        or via stdin.
    update              Check for pwntools updates
    version             Pwntools version

optional arguments:
  -h, --help            show this help message and exit
```
