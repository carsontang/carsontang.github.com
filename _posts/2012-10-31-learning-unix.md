---
layout: post
title: "Random Thoughts about Learning the Unix Philosophy"
category: Unix
tags: [Unix]
---
{% include JB/setup %}

###Why O_APPEND exists
Why use `O_APPEND` when you can just use `lseek(fd, 0, SEEK_END)` to move
the file offset to the end of the file and call `write` from there?

The `O_APPEND` flag used in the Unix `open(const char *path, int oflag, ...)`
system call exists because it guarantees an atomic appending write.

Consider the following code prone to a race condition:

    char buf[1024];
    if (lseek(fd, 0, SEEK_END) == -1) {
      fprintf(stderr, "Error with lseek");
      perror("lseek");
      exit(EXIT_FAILURE);
    }
    
    /* HAZARDOUS!!! WINDOW FOR FAILURE! RACE CONDITION POSSIBLE! */

    if (write(fd, buf, 1024) != 1024) {
      fprintf(stderr, "Error with write");
      perror("write");
      exit(EXIT_FAILURE); 
    }

Between the `lseek` and `write` system calls (the `/* HAZARDOUS!!! WINDOW FOR FAILURE */` section), another process, P2, running the
same program could run. Thus, when the CPU returns to this process, P1,
the file offset set by P1's `lseek` will be incorrect and P1's `write` system call
will overwrite whatever P2 wrote starting at the file offset
set by P1's `lseek`.

To address this, an atomic operation is needed, a need that `O_APPEND` fulfills.