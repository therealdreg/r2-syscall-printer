# r2-syscall-printer

**WARNING: this is a POC, and the code is pure CRAP**

The current amd64 call convention in r2 supported is User-level call convention: %rdi, %rsi, %rdx, **%rcx, %r8, %r9**

I created this r2pipe script to support x86 & amd64 kernel interface call convention: %rdi, %rsi, %rdx, **%r10, %r8, %r9**

**Also you can use this tool as standalone-app to print syscall table info for Linux x86 & amd64**

64 bit process:
![alt text](r2-syscall-printer64.png)

32 bit process:
![alt text](r2-syscall-printer.png)

The official r2-support for amd64 kernel interface call convention is comming, check my pr and pancake pr: 
* https://github.com/radareorg/radare2/pull/17954 
* https://github.com/radareorg/radare2/pull/17960

## Usage:

For 64bit processes:
```
#!pipe python3 r2-syscall-printer.py 
```

For 32bit processes inside r2 session use /32: 
```
#!pipe python3 r2-syscall-printer.py /32
```

To display extra info (like in the screenshot image) use /extra:
```
#!pipe python3 r2-syscall-printer.py /extra
#!pipe python3 r2-syscall-printer.py /32 /extra
```

## Use as standalone tool

* /sysinfoX: for hexadecimal syscall ID 
* /sysinfoD: for decimal syscall ID
* /printable: prints full syscall table
* /exit: show info & exit
* /32: put the current sys_call_table mode for 32 bit (default 64)

Getting 32bits-table-info about syscall 0xAB and 3 (decimal):
```
dreg@fr33project:~# python3 /root/r2-syscall-printer/r2-syscall-printer.py /32 /sysinfoXAB /sysinfoD3 /exit
arch: 32 bits
syscall: 171 (decimal)
Entry(name='getresgid', params=[Param(reg='$ebx', param='gid_t *rgidp'), Param(reg='$ecx', param='gid_t *egidp'), Param(reg='$edx', param='gid_t *sgidp')])
syscall: 3 (decimal)
Entry(name='read', params=[Param(reg='$ebx', param='unsigned int fd'), Param(reg='$ecx', param='char *buf'), Param(reg='$edx', param='size_t count')])
```

Getting 64bits-table-info about syscall 0xFF and 13 (decimal):
```
dreg@fr33project:~# python3 /root/r2-syscall-printer/r2-syscall-printer.py /sysinfoXFF /sysinfoD13 /exit
arch: 64 bits
syscall: 255 (decimal)
Entry(name='inotify_rm_watch', params=[Param(reg='$rdi', param='int fd'), Param(reg='$rsi', param='__s32 wd')])
syscall: 13 (decimal)
Entry(name='rt_sigaction', params=[Param(reg='$rdi', param='int sig'), Param(reg='$rsi', param='const struct sigaction *act'), Param(reg='$rdx', param='struct sigaction *oact'), Param(reg='$r10', param='size_t sigsetsize')])
```

Printing full 32bits-table-info syscall info:
```
dreg@fr33project:~# python3 /root/r2-syscall-printer/r2-syscall-printer.py /32 /printable /exit
arch: 32 bits
{   0: Entry(name='restart_syscall', params=[]),
    1: Entry(name='exit', params=[Param(reg='$ebx', param='int error_code')]),
    2: Entry(name='fork', params=[]),
    3: Entry(name='read', params=[Param(reg='$ebx', param='unsigned int fd'), Param(reg='$ecx', param='char *buf'), Param(reg='$edx', param='size_t count')]),
    4: Entry(name='write', params=[Param(reg='$ebx', param='unsigned int fd'), Param(reg='$ecx', param='const char *buf'), Param(reg='$edx', param='size_t count')]),
    ...
```

Printing full 64bits-table-info syscall info:
```
dreg@fr33project:~# python3 /root/r2-syscall-printer/r2-syscall-printer.py /printable /exit
arch: 64 bits
{   0: Entry(name='read', params=[Param(reg='$rdi', param='unsigned int fd'), Param(reg='$rsi', param='char *buf'), Param(reg='$rdx', param='size_t count')]),
    1: Entry(name='write', params=[Param(reg='$rdi', param='unsigned int fd'), Param(reg='$rsi', param='const char *buf'), Param(reg='$rdx', param='size_t count')]),
    2: Entry(name='open', params=[Param(reg='$rdi', param='const char *filename'), Param(reg='$rsi', param='int flags'), Param(reg='$rdx', param='umode_t mode')]),
    3: Entry(name='close', params=[Param(reg='$rdi', param='unsigned int fd')]),
    4: Entry(name='stat', params=[Param(reg='$rdi', param='const char *filename'), Param(reg='$rsi', param='struct __old_kernel_stat *statbuf')]),
    5: Entry(name='fstat', params=[Param(reg='$rdi', param='unsigned int fd'), Param(reg='$rsi', param='struct __old_kernel_stat *statbuf')]),
    ...
```

# Credits

Tables from GEF-extras (syscall-args) - GDB Enhanced Features for exploit devs & reversers
* http://gef.rtfd.io/
* https://github.com/hugsy/gef
* https://github.com/hugsy/gef-extras

# TODO

* Improve the code, more pythonic please
* Auto detect process arch32/64 for default display in r2

# Contributors

* nobody loves me

