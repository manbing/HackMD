---
title: Miscellaneous
tags: [Linux]

---

To run 32-bit ELF binaries on 64-bit platform, install `lib32z1` package.
--
``` console
$ sudo apt update
$ sudo apt install lib32z1
$ dpkg -L libc6-i386 | grep -w ld 
/lib32/ld-2.31.so
/lib/ld-linux.so.2
/lib32/ld-linux.so.2

$ /usr/lib/ld-linux.so.2 ./ncx32app
```

---
``` c
printk("%pF");
```



Linux Magic System Request Key
--
https://docs.kernel.org/admin-guide/sysrq.html

Process Stack size
--
In Linux, you can change the maximum size of a process's call stack using shell commands, programmatically within a C/C++ program, or for individual threads. The stack limit is a resource limit (`RLIMIT_STACK`) that determines how much memory the stack can use before a segmentation fault occurs. 

* View the current stack size limit:
``` console
$ ulimit -s
```
This typically shows the size in kilobytes (e.g., 8192 for 8MB).

* Programmatically using `setrlimit` (Within a C/C++ program)
``` c
#include <sys/resource.h>
#include <stdio.h>

int main() {
    struct rlimit stack_limit;

    // Get current limit
    getrlimit(RLIMIT_STACK, &stack_limit);
    printf("Current stack size: %lu bytes\n", stack_limit.rlim_cur);

    // Set new soft limit (e.g., to 32 MB)
    stack_limit.rlim_cur = 32 * 1024 * 1024; 
    
    // Check if the new soft limit is within the hard limit
    if (stack_limit.rlim_cur > stack_limit.rlim_max) {
        stack_limit.rlim_cur = stack_limit.rlim_max;
    }

    if (setrlimit(RLIMIT_STACK, &stack_limit) != 0) {
        perror("setrlimit failed");
    } else {
        printf("New stack size set to: %lu bytes\n", stack_limit.rlim_cur);
    }

    // Your application code follows...
    return 0;
}
```

Edge Trigger / Level Trigger
---
`epoll` V.S. `poll`, `select` 

[边缘触发(Edge Trigger)和条件触发(Level Trigger)](https://blog.csdn.net/josunna/article/details/6269235)To run 32-bit ELF binaries on 64-bit platform, install `lib32z1` package.
--
``` console
$ sudo apt update
$ sudo apt install lib32z1
$ dpkg -L libc6-i386 | grep -w ld 
/lib32/ld-2.31.so
/lib/ld-linux.so.2
/lib32/ld-linux.so.2

$ /usr/lib/ld-linux.so.2 ./ncx32app
```

---
``` c
printk("%pF");
```



Linux Magic System Request Key
--
https://docs.kernel.org/admin-guide/sysrq.html

Process Stack size
--
In Linux, you can change the maximum size of a process's call stack using shell commands, programmatically within a C/C++ program, or for individual threads. The stack limit is a resource limit (`RLIMIT_STACK`) that determines how much memory the stack can use before a segmentation fault occurs. 

* View the current stack size limit:
``` console
$ ulimit -s
```
This typically shows the size in kilobytes (e.g., 8192 for 8MB).

* Programmatically using `setrlimit` (Within a C/C++ program)
``` c
#include <sys/resource.h>
#include <stdio.h>

int main() {
    struct rlimit stack_limit;

    // Get current limit
    getrlimit(RLIMIT_STACK, &stack_limit);
    printf("Current stack size: %lu bytes\n", stack_limit.rlim_cur);

    // Set new soft limit (e.g., to 32 MB)
    stack_limit.rlim_cur = 32 * 1024 * 1024; 
    
    // Check if the new soft limit is within the hard limit
    if (stack_limit.rlim_cur > stack_limit.rlim_max) {
        stack_limit.rlim_cur = stack_limit.rlim_max;
    }

    if (setrlimit(RLIMIT_STACK, &stack_limit) != 0) {
        perror("setrlimit failed");
    } else {
        printf("New stack size set to: %lu bytes\n", stack_limit.rlim_cur);
    }

    // Your application code follows...
    return 0;
}
```

Edge Trigger / Level Trigger
---
`epoll` V.S. `poll`, `select` 

[边缘触发(Edge Trigger)和条件触发(Level Trigger)](https://blog.csdn.net/josunna/article/details/6269235)