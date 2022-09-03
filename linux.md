# Linux
###### tags: `Linux`

## manual page/man page
(1) General commands
(2) System calls
(3) Library functions, covering in particular the C standard library
(4) Special files (usually devices, those found in /dev) and drivers
(5) File formats and conventions
(6) Games and screensavers
(7) Miscellaneous
(8) System administration commands and daemons

## image
**Image**: the generic Linux kernel binary image file.

**zImage**: a compressed version of the Linux kernel image that is self-extracting.

**uImage**: an image file that has a U-Boot wrapper (installed by the mkimage utility) that includes the OS type and loader information.

A very common practice (e.g. the typical Linux kernel Makefile) is to use a zImage file. Since a zImage file is self-extracting (i.e. needs no external decompressors), the wrapper would indicate that this kernel is "not compressed" even though it actually is

## makefile

make menuconfig
make oldconfig
make defconfig
make xconfig
make localmodconfig
make cscope
make mrproper
make tags

## Upstream

[Submitting Your First Patch to the Linux Kernel and Responding to Feedback](http://nickdesaulniers.github.io/blog/2017/05/16/submitting-your-first-patch-to-the-linux-kernel-and-responding-to-feedback/)

### Step 1: Setting up an email client
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


### Step 2: Make fixes
[bugzilla](https://bugzilla.kernel.org/describecomponents.cgi)

[coding style](h[ttps://](https://www.kernel.org/doc/html/v4.12/process/coding-style.html#codingstyle))

[clang-format](https://clang.llvm.org/docs/ClangFormat.html)



### Step 3: Thoughtful commit messages

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


### Step 4: Generate Patch file
```
git format-patch HEAD~
```

### Step 5: check patch
check if patch file format is correct or not
```
scripts/checkpatch.pl --terse <patch_file.patch>
```


### Step 6: email the patch to yourself

### Step 7: fire off the patch
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


## Debug

crash
> crash(8) - Analyze Linux crash dump data or a live system


