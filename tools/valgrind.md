内存泄露检测工具，[官网地址](https://valgrind.org/)

## 问题描述

即使使用调试器，我们也可能**无法捕获所有错误**。 有些错误就是我们所说的“bohrbug”，这意味着它们在明确定义但可能未知的条件下可靠地表现出来。 其他错误就是我们所说的“heisenbug”，它们不是决定性的，而是在人们试图研究它们时消失或改变其行为。 我们可以使用调试器检测第一种类型，但第二种类型可能会被我们忽视，因为它们（至少在 C 语言中）通常是由于**内存管理不当**造成的。 请记住，与其他编程语言不同，C 要求您（程序员）**手动管理内存**。



## Quick Start

```c
#include <stdlib.h>

void f(void)
{
	int* x = malloc(10 * sizeof(int)); // 问题一：X没有被回收
	x[10] = 0; // 问题2：溢出
}

int main(void)
{
	f();
	return 0;
}
```

执行检查：
```sh
$ valgrind --leak-check=yes ./a.out
==77656== Memcheck, a memory error detector
==77656== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==77656== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==77656== Command: ./a.out
==77656==
==77656== Invalid write of size 4
==77656==    at 0x109157: f (in /tmp/demo/a.out)
==77656==    by 0x109168: main (in /tmp/demo/a.out)
==77656==  Address 0x4a70068 is 0 bytes after a block of size 40 alloc'd
==77656==    at 0x4841828: malloc (vg_replace_malloc.c:442)
==77656==    by 0x10914A: f (in /tmp/demo/a.out)
==77656==    by 0x109168: main (in /tmp/demo/a.out)
==77656==
==77656==
==77656== HEAP SUMMARY:
==77656==     in use at exit: 40 bytes in 1 blocks
==77656==   total heap usage: 1 allocs, 0 frees, 40 bytes allocated
==77656==
==77656== 40 bytes in 1 blocks are definitely lost in loss record 1 of 1
==77656==    at 0x4841828: malloc (vg_replace_malloc.c:442)
==77656==    by 0x10914A: f (in /tmp/demo/a.out)
==77656==    by 0x109168: main (in /tmp/demo/a.out)
==77656==
==77656== LEAK SUMMARY:
==77656==    definitely lost: 40 bytes in 1 blocks
==77656==    indirectly lost: 0 bytes in 0 blocks
==77656==      possibly lost: 0 bytes in 0 blocks
==77656==    still reachable: 0 bytes in 0 blocks
==77656==         suppressed: 0 bytes in 0 blocks
==77656==
==77656== For lists of detected and suppressed errors, rerun with: -s
==77656== ERROR SUMMARY: 2 errors from 2 contexts (suppressed: 0 from 0)
```

如果看不懂，这里给出[错误含义](https://valgrind.org/docs/manual/mc-manual.html#mc-manual.errormsgs)以及[手册](https://valgrind.org/docs/manual/manual.html)地址



## bork例子

```c
/* This program translates words to Bork, a language that is very similar to English.
   To translate a word to Bork, you take the English word and add an 'f' after every 
   vowel in the word. */

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char *alloc_str(int len) {
    return malloc(len*sizeof(char));
}

/* Str helper functions */
typedef struct Str {
    char *data;
    int len;
} Str;

Str make_Str(char *str) {
    /* Below is a designated initializer. It creates a Str struct and initializes
       its data field to str and its len field to strlen(str) */
    return (Str){.data=str,.len=strlen(str)};
}

void free_Str(Str str) {
    free(str.data);
}

/* concatinates two strings together */
Str concat(Str a, Str b) {
    int new_len = a.len + b.len;
    char *new_str = alloc_str(new_len);
    for (int i = 0; i < a.len; ++i) {
        new_str[i] = a.data[i];
    }
    for (int i = 0; i < b.len; ++i) {
        new_str[i+a.len] = b.data[i];
    }
    free(a.data);
    free(b.data);
    return (Str){.data=new_str, .len=new_len};
}

/* translates a letter to Bork */
Str translate_to_bork(char c) {
    switch(c) {
    case 'a': case 'e': case 'i': case 'o': case 'u': {
        char *res = alloc_str(2);
        res[0] = c;
        res[1] = 'f';
        return make_Str(res);
    }
    }
    char *res = alloc_str(1);
    res[0] = c;
    return make_Str(res);
}

int main(int argc, char*argv[]) {
    if (argc == 1) {
        printf("Remember to give me a string to translate to Bork!\n");
        return 1;
    }

    Str dest_str={}; // Fancy syntax to zero initialize struct
    Str src_str = make_Str(argv[1]);
    for (int i = 0; i < src_str.len; ++i) {
        Str bork_substr = translate_to_bork(src_str.data[i]);
        dest_str = concat(dest_str, bork_substr);
    }

    printf("Input string: \"%s\"\n", src_str.data);
    printf("Length of translated string: %d\n", dest_str.len);
    printf("Translate to Bork: \"%s\"\n", dest_str.data);
    return 0;
}
```

命令行演示以下：
```sh
$ gcc -g -o bork bork.c
$ ./bork hello
Input string: "hello"
Length of translated string: 21
Translate to Bork: "hefl2?^?Ul2?^?Uof?^?U" # <--这里乱码了
```


