*Challenge Description*

The challenge description gave us a link to http://golf.so.pwni.ng/upload, which contained the following instructions:
```
Upload a 64-bit ELF shared object of size at most 1024 bytes. It should spawn a shell (execute execve("/bin/sh", ["/bin/sh"], ...)) when used like

LD_PRELOAD=<upload> /bin/true
```
