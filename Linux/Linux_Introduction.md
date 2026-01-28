---
title: Linux Introduction
tags: [Linux]

---


# Documentation

[The Linux Kernel documentation](https://docs.kernel.org/)

## [Kernel-doc comments](https://docs.kernel.org/doc-guide/kernel-doc.html)
### How to format kernel-doc comments
The opening comment mark `/**` is used for `kernel-doc` comments. The kernel-doc tool will extract comments marked this way. The rest of the comment is formatted like a normal multi-line comment with a column of asterisks on the left side, closing with `*/` on a line by itself.

### How to use kernel-doc to generate man pages
```
$ scripts/kernel-doc -man \
  $(git grep -l '/\*\*' -- :^Documentation :^tools) \
  | scripts/split-man.pl /tmp/man
```
## [man page](https://man7.org/linux/man-pages/)(manual page)
(1) General commands
(2) System calls
(3) Library functions, covering in particular the C standard library
(4) Special files (usually devices, those found in /dev) and drivers
(5) File formats and conventions
(6) Games and screensavers
(7) Miscellaneous
(8) System administration commands and daemons

## API
[The Linux Kernel API](https://www.kernel.org/doc/htmldocs/kernel-api/)

[Core api](https://github.com/torvalds/linux/tree/master/Documentation/core-api)

Printk:
> Extending printk() in this manner allowed Torvalds—who authored the patch—to add two new types to printk(): **%pS** for symbolic pointers and **%pF** for symbolic function pointers. In both cases, the code uses kallsyms to turn the pointer value into a symbol name. Instead of a kernel developer having to read long address strings and then trying to find them in the system map, the kernel will do that work for them.


# image
**vmlinux**:
--
The uncompressed kernel image file. The original image. It includes everything.

**Image**:
--
the generic Linux kernel <mark>binary image file</mark>.

**zImage**:
--
a <mark>compressed version</mark> of the Linux kernel <mark>binary image</mark> that is self-extracting.

**uImage**:
--
an image file that has a U-Boot wrapper (installed by the mkimage utility) that includes the OS type and loader information.

A very common practice (e.g. the typical Linux kernel Makefile) is to use a zImage file. Since a zImage file is self-extracting (i.e. needs no external decompressors), the wrapper would indicate that this kernel is "not compressed" even though it actually is

# makefile

``` console
$ make menuconfig
$ make oldconfig
$ make defconfig
$ make xconfig
$ make localmodconfig
$ make cscope
$ make mrproper
$ make tags

$ scripts/config --disable SYSTEM_REVOCATION_KEYS
$ scripts/config --disable SYSTEM_TRUSTED_KEYS
```

```
$ make prepare
The file `include/generated/autoconf` in this phase.
```


```
$ make htmldocs
$ make pdfdocs
$ make cleandocs
```


# Upstream

[Submitting Your First Patch to the Linux Kernel and Responding to Feedback](http://nickdesaulniers.github.io/blog/2017/05/16/submitting-your-first-patch-to-the-linux-kernel-and-responding-to-feedback/)

## Step 1: Setting up an email client
using command, git send-email, to send mail.
In ~/.gitconfig, add:

```
[sendemail]
  ; setup for using git send-email; prompts for password
  smtpuser = myemailaddr@gmail.com
  smtpserver = smtp.googlemail.com
  smtpencryption = tls
  smtpserverport = 587
```


## Step 2: Make fixes
[bugzilla](https://bugzilla.kernel.org/describecomponents.cgi)

[coding style](h[ttps://](https://www.kernel.org/doc/html/v4.12/process/coding-style.html#codingstyle))

[clang-format](https://clang.llvm.org/docs/ClangFormat.html)



## Step 3: Thoughtful commit messages

[How to Write a Git Commit Message](https://cbea.ms/git-commit/)

 A team’s approach to its commit log should be no different. In order to create a useful revision history, teams should first agree on a commit message convention that defines at least the following three things:

**Style**. Markup syntax, wrap margins, grammar, capitalization, punctuation. Spell these things out, remove the guesswork, and make it all as simple as possible. The end result will be a remarkably consistent log that’s not only a pleasure to read but that actually does get read on a regular basis.

**Content**. What kind of information should the body of the commit message (if any) contain? What should it not contain?

**Metadata**. How should issue tracking IDs, pull request numbers, etc. be referenced?

Fortunately, there are well-established conventions as to what makes an idiomatic Git commit message. Indeed, many of them are assumed in the way certain Git commands function. There’s nothing you need to re-invent. Just follow the seven rules below and you’re on your way to committing like a pro.

The seven rules of a great Git commit message Keep in mind: This has all been said before.
* Separate subject from body with a blank line
* Limit the subject line to 50 characters
* Capitalize the subject line
* Do not end the subject line with a period
* Use the imperative mood in the subject line
* Wrap the body at 72 characters
* Use the body to explain what and why vs. how


## Step 4: Generate Patch file
```
git format-patch HEAD~
```

## Step 5: check patch
check if patch file format is correct or not
```
scripts/checkpatch.pl --terse <patch_file.patch>
```


## Step 6: email the patch to yourself

## Step 7: fire off the patch
Linux is huge, and has a trusted set of maintainers for various subsystems. The MAINTAINERS file keeps track of these, but Linux has a tool to help you figure out where to send your patch:
```
$ ./scripts/get_maintainer.pl 0001-x86-build-don-t-add-maccumulate-outgoing-args-w-o-co.patch
Person A <person@a.com> (maintainer:X86 ARCHITECTURE (32-BIT AND 64-BIT))
Person B <person@b.com> (maintainer:X86 ARCHITECTURE (32-BIT AND 64-BIT))
Person C <person@c.com> (maintainer:X86 ARCHITECTURE (32-BIT AND 64-BIT))
x86@kernel.org (maintainer:X86 ARCHITECTURE (32-BIT AND 64-BIT))
linux-kernel@vger.kernel.org (open list:X86 ARCHITECTURE (32-BIT AND 64-BIT))
```

With some additional flags, we can feed this output directly into git send-email.

```
$ git send-email \
--cc-cmd='./scripts/get_maintainer.pl --norolestats 0001-my.patch' \
--cc manbing3@gmail.com \
0001-my.patch
```

Make sure to cc yourself when prompted. Otherwise if you don’t subscribe to LKML, then it will be difficult to reply to feedback. It’s also a good idea to cc any other author that has touched this functionality recently.


# Debug

[Linux kernel oops](https://en.wikipedia.org/wiki/Linux_kernel_oops)
[Kernel panic](https://en.wikipedia.org/wiki/Kernel_panic)

crash
> crash(8) - Analyze Linux crash dump data or a live system

# Directory

## ./include/uapi/
the UAPI header is for user-space program includes.
It is responsible for reveling kernel information to user-space.
[The UAPI header file split](https://lwn.net/Articles/507794/)

./include/uapi/linux/types.h
> it defines data type. e.g., __u16, __u32, __u64 and so on.

./include/uapi/linux/if.h
> it defines attribute of net device. e.g., IFNAMSIZ and so on.

./include/uapi/linux/wireless.h
> it revels wireless relation struct. e.g., iw_freq{} and so on.

# Command-Line Parameters

mem=nn[KMG]
> [KNL,BOOT] Force usage of a specific amount of memory Amount of memory to be used when the kernel is not able to see the whole system memory or for test. [X86] Work as limiting max address. Use together with memmap= to avoid physical address space collisions. Without memmap= PCI devices could be placed at addresses belonging to unused RAM.


# Context
`Context` is the environment which code is running. `Context` has many attribue. i.e., Interrupt, Preemption and so on.


|  | Interrupt-able | Preemption-able |
| -------- | -------- |  -------- | 
| Interrupt Context   | ✕     |  ✕     |
| Process Context     | ❍     | ❍     |
| Atomic Context     | ✕   | ✕     |

``` c
/* ./include/linux/preempt.h */

/*
 * We put the hardirq and softirq counter into the preemption
 * counter. The bitmask has the following meaning:
 *
 * - bits 0-7 are the preemption count (max preemption depth: 256)
 * - bits 8-15 are the softirq count (max # of softirqs: 256)
 *
 * The hardirq count could in theory be the same as the number of
 * interrupts in the system, but we run all interrupt handlers with
 * interrupts disabled, so we cannot have nesting interrupts. Though
 * there are a few palaeontologic drivers which reenable interrupts in
 * the handler, so we need more than one bit here.
 *
 *         PREEMPT_MASK:    0x000000ff
 *         SOFTIRQ_MASK:    0x0000ff00
 *         HARDIRQ_MASK:    0x000f0000
 *             NMI_MASK:    0x00f00000
 * PREEMPT_NEED_RESCHED:    0x80000000
 */
#define PREEMPT_BITS    8
#define SOFTIRQ_BITS    8
#define HARDIRQ_BITS    4
#define NMI_BITS    4

/*
 * Macros to retrieve the current execution context:
 *
 * in_nmi()     - We're in NMI context
 * in_hardirq()     - We're in hard IRQ context
 * in_serving_softirq() - We're in softirq context
 * in_task()        - We're in task context
 */
#define in_nmi()        (nmi_count())
#define in_hardirq()        (hardirq_count())
#define in_serving_softirq()    (softirq_count() & SOFTIRQ_OFFSET)
#ifdef CONFIG_PREEMPT_RT
# define in_task()      (!((preempt_count() & (NMI_MASK | HARDIRQ_MASK)) | in_serving_softirq()))
#else
# define in_task()      (!(preempt_count() & (NMI_MASK | HARDIRQ_MASK | SOFTIRQ_OFFSET)))
#endif

/*
 * Are we running in atomic context?  WARNING: this macro cannot
 * always detect atomic context; in particular, it cannot know about
 * held spinlocks in non-preemptible kernels.  Thus it should not be
 * used in the general case to determine whether sleeping is possible.
 * Do not use in_atomic() in driver code.
 */
#define in_atomic() (preempt_count() != 0)



/*
 * These macro definitions avoid redundant invocations of preempt_count()
 * because such invocations would result in redundant loads given that
 * preempt_count() is commonly implemented with READ_ONCE().
 */

#define nmi_count() (preempt_count() & NMI_MASK)
#define hardirq_count() (preempt_count() & HARDIRQ_MASK)
#ifdef CONFIG_PREEMPT_RT
# define softirq_count()    (current->softirq_disable_cnt & SOFTIRQ_MASK)
# define irq_count()        ((preempt_count() & (NMI_MASK | HARDIRQ_MASK)) | softirq_count())
#else
# define softirq_count()    (preempt_count() & SOFTIRQ_MASK)
# define irq_count()        (preempt_count() & (NMI_MASK | HARDIRQ_MASK | SOFTIRQ_MASK))
#endif
```

# DMA
[Linux DMA Engine framework(1)_概述](http://www.wowotech.net/linux_kenrel/dma_engine_overview.html)

[DMA Engine API Guide](https://docs.kernel.org/driver-api/dmaengine/client.html)

# Interrupt
[Interrupt](https://en.wikipedia.org/wiki/Interrupt)
# Documentation

[The Linux Kernel documentation](https://docs.kernel.org/)

## [Kernel-doc comments](https://docs.kernel.org/doc-guide/kernel-doc.html)
### How to format kernel-doc comments
The opening comment mark `/**` is used for `kernel-doc` comments. The kernel-doc tool will extract comments marked this way. The rest of the comment is formatted like a normal multi-line comment with a column of asterisks on the left side, closing with `*/` on a line by itself.

### How to use kernel-doc to generate man pages
```
$ scripts/kernel-doc -man \
  $(git grep -l '/\*\*' -- :^Documentation :^tools) \
  | scripts/split-man.pl /tmp/man
```
## [man page](https://man7.org/linux/man-pages/)(manual page)
(1) General commands
(2) System calls
(3) Library functions, covering in particular the C standard library
(4) Special files (usually devices, those found in /dev) and drivers
(5) File formats and conventions
(6) Games and screensavers
(7) Miscellaneous
(8) System administration commands and daemons

## API
[The Linux Kernel API](https://www.kernel.org/doc/htmldocs/kernel-api/)

[Core api](https://github.com/torvalds/linux/tree/master/Documentation/core-api)

Printk:
> Extending printk() in this manner allowed Torvalds—who authored the patch—to add two new types to printk(): **%pS** for symbolic pointers and **%pF** for symbolic function pointers. In both cases, the code uses kallsyms to turn the pointer value into a symbol name. Instead of a kernel developer having to read long address strings and then trying to find them in the system map, the kernel will do that work for them.


# image
**vmlinux**:
--
The uncompressed kernel image file. The original image. It includes everything.

**Image**:
--
the generic Linux kernel <mark>binary image file</mark>.

**zImage**:
--
a <mark>compressed version</mark> of the Linux kernel <mark>binary image</mark> that is self-extracting.

**uImage**:
--
an image file that has a U-Boot wrapper (installed by the mkimage utility) that includes the OS type and loader information.

A very common practice (e.g. the typical Linux kernel Makefile) is to use a zImage file. Since a zImage file is self-extracting (i.e. needs no external decompressors), the wrapper would indicate that this kernel is "not compressed" even though it actually is

# makefile

``` console
$ make menuconfig
$ make oldconfig
$ make defconfig
$ make xconfig
$ make localmodconfig
$ make cscope
$ make mrproper
$ make tags

$ scripts/config --disable SYSTEM_REVOCATION_KEYS
$ scripts/config --disable SYSTEM_TRUSTED_KEYS
```

```
$ make prepare
The file `include/generated/autoconf` in this phase.
```


```
$ make htmldocs
$ make pdfdocs
$ make cleandocs
```


# Upstream

[Submitting Your First Patch to the Linux Kernel and Responding to Feedback](http://nickdesaulniers.github.io/blog/2017/05/16/submitting-your-first-patch-to-the-linux-kernel-and-responding-to-feedback/)

## Step 1: Setting up an email client
using command, git send-email, to send mail.
In ~/.gitconfig, add:

```
[sendemail]
  ; setup for using git send-email; prompts for password
  smtpuser = myemailaddr@gmail.com
  smtpserver = smtp.googlemail.com
  smtpencryption = tls
  smtpserverport = 587
```


## Step 2: Make fixes
[bugzilla](https://bugzilla.kernel.org/describecomponents.cgi)

[coding style](h[ttps://](https://www.kernel.org/doc/html/v4.12/process/coding-style.html#codingstyle))

[clang-format](https://clang.llvm.org/docs/ClangFormat.html)



## Step 3: Thoughtful commit messages

[How to Write a Git Commit Message](https://cbea.ms/git-commit/)

 A team’s approach to its commit log should be no different. In order to create a useful revision history, teams should first agree on a commit message convention that defines at least the following three things:

**Style**. Markup syntax, wrap margins, grammar, capitalization, punctuation. Spell these things out, remove the guesswork, and make it all as simple as possible. The end result will be a remarkably consistent log that’s not only a pleasure to read but that actually does get read on a regular basis.

**Content**. What kind of information should the body of the commit message (if any) contain? What should it not contain?

**Metadata**. How should issue tracking IDs, pull request numbers, etc. be referenced?

Fortunately, there are well-established conventions as to what makes an idiomatic Git commit message. Indeed, many of them are assumed in the way certain Git commands function. There’s nothing you need to re-invent. Just follow the seven rules below and you’re on your way to committing like a pro.

The seven rules of a great Git commit message Keep in mind: This has all been said before.
* Separate subject from body with a blank line
* Limit the subject line to 50 characters
* Capitalize the subject line
* Do not end the subject line with a period
* Use the imperative mood in the subject line
* Wrap the body at 72 characters
* Use the body to explain what and why vs. how


## Step 4: Generate Patch file
```
git format-patch HEAD~
```

## Step 5: check patch
check if patch file format is correct or not
```
scripts/checkpatch.pl --terse <patch_file.patch>
```


## Step 6: email the patch to yourself

## Step 7: fire off the patch
Linux is huge, and has a trusted set of maintainers for various subsystems. The MAINTAINERS file keeps track of these, but Linux has a tool to help you figure out where to send your patch:
```
$ ./scripts/get_maintainer.pl 0001-x86-build-don-t-add-maccumulate-outgoing-args-w-o-co.patch
Person A <person@a.com> (maintainer:X86 ARCHITECTURE (32-BIT AND 64-BIT))
Person B <person@b.com> (maintainer:X86 ARCHITECTURE (32-BIT AND 64-BIT))
Person C <person@c.com> (maintainer:X86 ARCHITECTURE (32-BIT AND 64-BIT))
x86@kernel.org (maintainer:X86 ARCHITECTURE (32-BIT AND 64-BIT))
linux-kernel@vger.kernel.org (open list:X86 ARCHITECTURE (32-BIT AND 64-BIT))
```

With some additional flags, we can feed this output directly into git send-email.

```
$ git send-email \
--cc-cmd='./scripts/get_maintainer.pl --norolestats 0001-my.patch' \
--cc manbing3@gmail.com \
0001-my.patch
```

Make sure to cc yourself when prompted. Otherwise if you don’t subscribe to LKML, then it will be difficult to reply to feedback. It’s also a good idea to cc any other author that has touched this functionality recently.


# Debug

[Linux kernel oops](https://en.wikipedia.org/wiki/Linux_kernel_oops)
[Kernel panic](https://en.wikipedia.org/wiki/Kernel_panic)

crash
> crash(8) - Analyze Linux crash dump data or a live system

# Directory

## ./include/uapi/
the UAPI header is for user-space program includes.
It is responsible for reveling kernel information to user-space.
[The UAPI header file split](https://lwn.net/Articles/507794/)

./include/uapi/linux/types.h
> it defines data type. e.g., __u16, __u32, __u64 and so on.

./include/uapi/linux/if.h
> it defines attribute of net device. e.g., IFNAMSIZ and so on.

./include/uapi/linux/wireless.h
> it revels wireless relation struct. e.g., iw_freq{} and so on.

# Command-Line Parameters

mem=nn[KMG]
> [KNL,BOOT] Force usage of a specific amount of memory Amount of memory to be used when the kernel is not able to see the whole system memory or for test. [X86] Work as limiting max address. Use together with memmap= to avoid physical address space collisions. Without memmap= PCI devices could be placed at addresses belonging to unused RAM.


# Context
`Context` is the environment which code is running. `Context` has many attribue. i.e., Interrupt, Preemption and so on.


|  | Interrupt-able | Preemption-able |
| -------- | -------- |  -------- | 
| Interrupt Context   | ✕     |  ✕     |
| Process Context     | ❍     | ❍     |
| Atomic Context     | ✕   | ✕     |

``` c
/* ./include/linux/preempt.h */

/*
 * We put the hardirq and softirq counter into the preemption
 * counter. The bitmask has the following meaning:
 *
 * - bits 0-7 are the preemption count (max preemption depth: 256)
 * - bits 8-15 are the softirq count (max # of softirqs: 256)
 *
 * The hardirq count could in theory be the same as the number of
 * interrupts in the system, but we run all interrupt handlers with
 * interrupts disabled, so we cannot have nesting interrupts. Though
 * there are a few palaeontologic drivers which reenable interrupts in
 * the handler, so we need more than one bit here.
 *
 *         PREEMPT_MASK:    0x000000ff
 *         SOFTIRQ_MASK:    0x0000ff00
 *         HARDIRQ_MASK:    0x000f0000
 *             NMI_MASK:    0x00f00000
 * PREEMPT_NEED_RESCHED:    0x80000000
 */
#define PREEMPT_BITS    8
#define SOFTIRQ_BITS    8
#define HARDIRQ_BITS    4
#define NMI_BITS    4

/*
 * Macros to retrieve the current execution context:
 *
 * in_nmi()     - We're in NMI context
 * in_hardirq()     - We're in hard IRQ context
 * in_serving_softirq() - We're in softirq context
 * in_task()        - We're in task context
 */
#define in_nmi()        (nmi_count())
#define in_hardirq()        (hardirq_count())
#define in_serving_softirq()    (softirq_count() & SOFTIRQ_OFFSET)
#ifdef CONFIG_PREEMPT_RT
# define in_task()      (!((preempt_count() & (NMI_MASK | HARDIRQ_MASK)) | in_serving_softirq()))
#else
# define in_task()      (!(preempt_count() & (NMI_MASK | HARDIRQ_MASK | SOFTIRQ_OFFSET)))
#endif

/*
 * Are we running in atomic context?  WARNING: this macro cannot
 * always detect atomic context; in particular, it cannot know about
 * held spinlocks in non-preemptible kernels.  Thus it should not be
 * used in the general case to determine whether sleeping is possible.
 * Do not use in_atomic() in driver code.
 */
#define in_atomic() (preempt_count() != 0)



/*
 * These macro definitions avoid redundant invocations of preempt_count()
 * because such invocations would result in redundant loads given that
 * preempt_count() is commonly implemented with READ_ONCE().
 */

#define nmi_count() (preempt_count() & NMI_MASK)
#define hardirq_count() (preempt_count() & HARDIRQ_MASK)
#ifdef CONFIG_PREEMPT_RT
# define softirq_count()    (current->softirq_disable_cnt & SOFTIRQ_MASK)
# define irq_count()        ((preempt_count() & (NMI_MASK | HARDIRQ_MASK)) | softirq_count())
#else
# define softirq_count()    (preempt_count() & SOFTIRQ_MASK)
# define irq_count()        (preempt_count() & (NMI_MASK | HARDIRQ_MASK | SOFTIRQ_MASK))
#endif
```

# DMA
[Linux DMA Engine framework(1)_概述](http://www.wowotech.net/linux_kenrel/dma_engine_overview.html)

[DMA Engine API Guide](https://docs.kernel.org/driver-api/dmaengine/client.html)

# Interrupt
[Interrupt](https://en.wikipedia.org/wiki/Interrupt)