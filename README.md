# r2-syscall-printer

**WARNING: this is a POC, and the code is pure CRAP

The current amd64 call convention in r2 supported is User-level call convention: %rdi, %rsi, %rdx, **%rcx, %r8, %r9**

I created this r2pipe script to support amd64 kernel interface call convention: %rdi, %rsi, %rdx, **%r10, %r8, %r9**

The official r2-support for this call convention is comming, check my pr and pancake pr: https://github.com/radareorg/radare2/pull/17954 https://github.com/radareorg/radare2/pull/17960

![alt text](r2-syscall-printer.png)

## Usage:

```
#!pipe python3 r2-syscall-printer.py 
```

For 32bit processes inside r2 session use /32: 

```
#!pipe python3 r2-syscall-printer.py /32
```

Display extra info use /extra:
```
#!pipe python3 r2-syscall-printer.py /extra
#!pipe python3 r2-syscall-printer.py /32 /extra
```

# TODO

* Improve the code, more pythonic please
* Add support to specify a syscall_index value, something like: #!pipe python3 r2-syscall-printer.py /32 syscall_idex=3

# Contributors

* nobody loves me

