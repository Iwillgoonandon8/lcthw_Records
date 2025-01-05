Learn C the hard way

## makefile

- 学习

```c
	video:/*             
	https://www.bilibili.com/video/BV1Mx411m7fm/?spm_id_from=333.337.search-card.all.click&vd_source=9a1b02760e5d5176081d05f5537d605e
  */
	up:/*
	https://space.bilibili.com/24014925
	*/
之后可以看（还没有看：
  /*  南方科技大学计算机系于仕琪副教授 
--->makefile
	https://www.bilibili.com/video/BV188411L7d2?spm_id_from=333.788.recommend_more_video.-1&vd_source=9a1b02760e5d5176081d05f5537d605e
	
-->Cmake
	https://www.bilibili.com/video/BV1bg411p7oS/?spm_id_from=333.337.search-card.all.click&vd_source=9a1b02760e5d5176081d05f5537d605e
	*/
```

### 基本操作

当前目录下：

```shell
$ touch makefile
$ vim makefile #打开文件
```

makefile 内：

```shell
target:dependencies
[tab]command
#例子
1 test: test.c
2 	gcc test.c -o test
3 
```

### 打包多个文件

- foo.c , bar.c , tool.c   ,    main.c

​	------>foo.o , bar.o , tool.o ===>main.c=>main

```shell
#例子
	1 
	2 main: main.c tool.o
	3 	gcc main.c tool.o -o main #把前一行中的2个dependencies合并成一个可执行文件 .main
	4
	5 tool.o: tool.c
	6		gcc -c tool.c
	7
	8 clean:
	9 	rm *.o main
	10
#
-c 表示把'.h'直接编译成'.o'
```

保存，返回：

```shell
$ make
gcc -c tool.c
gcc main.c tool.o -o main
$ ls
main main.c makefile makefile~ tool.c tool.h tool.o
$ ./main
[main的输出]
#在这一步之后再:在makefile加入 clean那一行？还是一开始就在makefile中都写到？
#应该一开始就写好，可以的
$ make clean
rm *.o main
```

### 更改makefile 里多次出现的命令：

```shell
 1 CC = gcc       //
 2
 3 main: main.c tool.o
 4 	$(CC) main.c tool.o -o main      //
 5
 6 tool.o: tool.c
 7 	$(CC) -c tool.c      //
 8
 9 clean:
10 	rm *.o main
11
```



```shell
 1 CC = gcc
 2 CFLAGS = -lm -Wall -g      //
 3 
 4 main: main.c foo.o bar.o
 5 	$(CC) $(CFLAGS) main.c foo.o -o main
 6 
 7 foo.o: foo.c
 8 	$(CC) $(CFLAGS) -c foo.c
 9 
10 bar.o: bar.c
11 	$(CC) $(CFLAGS) -c bar.c
12
13 clean:
14 	rm *.o main
```

### 项目有多个主函数，要合成多个可执行文件

```shell
 1 CC = gcc
 2 CFLAGS = -lm -Wall -g
 3 
 4 all: main_max main_min  #
 5
 6 main_max: main_max.c foo.o bar.o
 7 	$(CC) $(CFLAGS) main_max.c foo.o bar.o -o main_max
 8
 9 main_min: main_min.c foo.o bar.o
10 	$(CC) $(CFLAGS) main_min.c foo.o bar.o -o main_min
11
12 foo.o: foo.c
13 	$(CC) $(CFLAGS) -c foo.c
14
15 bar.o: bar.c
16 	$(CC) $(CFLAGS) -c bar.c
17
18 clean:
19 rm *.o main_max main_min
```



## 练习的部分解答以及疑惑

### 1 *

```shell
?: 按照文档中的方式直接*make file*似乎不可以，于是我用的是上面学到的makefile。这样学习c可以吗(ex01是本来没有的文件)
```

![直接输入*make ex01*](https://fan67.oss-cn-beijing.aliyuncs.com/2991733535621_.pic.jpg)

- 或许我应该先 *STFW* 一下（一会儿叭 :D

### 3

ex3的崩溃集😈

![](https://fan67.oss-cn-beijing.aliyuncs.com/3001733538061_.pic.jpg)

![](https://fan67.oss-cn-beijing.aliyuncs.com/3021733538782_.pic.jpg)

![](https://fan67.oss-cn-beijing.aliyuncs.com/3011733538683_.pic.jpg)

### 4 *

遇到问题：

![](https://fan67.oss-cn-beijing.aliyuncs.com/3031733540191_.pic.jpg)

由ai帮助，去看为什么会这样：

![](https://fan67.oss-cn-beijing.aliyuncs.com/3041733540320_.pic.jpg)

```shell
curl -O https://sourceware.org/pub/valgrind/valgrind-3.24.0.tar.bz2
#换成这个
```

#### * 读这个？？？  (一会儿叭 :D

```shell
:~/valgrind-3.24.0$ cat Makefile
```

#### * 还有这个要干：STFW about Valgrind 

### 6 *

#### * printf格式化

```c
%o //输出八进制整数
%x //输出十六进制整数
%e //输出科学计数法表示的浮点数
%m.nf //输出
  %-m.nf
```

### 7

```shell
#long
ex7.c:10:47: warning: integer overflow in expression of type ‘long int’ results in ‘2057039091198853120’ [-Woverflow]
   10 |         long universe_of_defects = 100000000L * 102400000000000L * 1024L *1024L;
#unsigned long
ex7.c:10:79: warning: integer overflow in expression of type ‘long int’ results in ‘4071254063142928384’ [-Woverflow]
   10 | ong universe_of_defects =1024 * 1024 * 1024 * 1024L * 1024L * 20000*200;
      |                                                             ^
```

- unsigned

  ```shell
  ‌unsigned long 和 long 的主要区别在于它们的符号表示。‌
  ‌符号表示‌：unsigned long 表示无符号长整型，只能存储非负数；而 long 可以是有符号长整型，可以存储正数、负数和零。
  ‌取值范围‌：unsigned long 的取值范围是 0 到 4294967295（2^32 - 1）；而 long 的取值范围取决于具体的编译器和平台，通常为 -2147483648 到 2147483647（即 -2^31 到 2^31 - 1）。
  ‌内存占用‌：两者通常都占用 4 个字节的内存空间，但具体的内存占用情况可能会因编译器和平台的不同而有所变化。
  ```



### 9

```shell
"如果一个字符数组占四个字节，一个整数也占4个字节，你可以像整数一样使用整个name吗？你如何用黑魔法实现它？"
printf("%s",name); 应该是这个意思？
```

- "将`name`转换成`another`的形式，看看代码是否能正常工作。" :不能

在自己的编译器上又试了一下

![](https://fan67.oss-cn-beijing.aliyuncs.com/3281733663384_.pic.jpg)

原本在Linux里面的异常：**Segmentation fault (core dumped)**

```shell
numbers: 0 0 0 0
name each: 1 2 3 4
name: 1234
Segmentation fault (core dumped)
```

### 10 *

#### * 命令行参数

```shell
https://baike.baidu.com/item/命令行参数/3206082?anchor=1#1
```

#### 字符串数组

```shell
- "在C语言中通过混合char *str = "blah"和char str[] ={'b','l','a','h'}语法构建二维数组来构建字符串数组"
每个字符串都是数组的一个元素，字符串的每个字符又是字符串的一个元素。
```

```shell
strcpy(states[1],argv[0]);
-->Segmentation fault (core dumped)
strcpy(argv[0],states[1]);
-->
arg 0: Oregon
states 0: California
states 1: Oregon
states 2: Washington
states 3: Texas
```

### 11 ?

```c
#include<stdio.h>
#include<string.h>

int main( int argc, char *argv[] )
{
	int i=argc-1;
	while( i >= 0 )
	{
		printf("arg %d: %s\n",i,argv[i]);
		i--;
	}

	char *states[4]={"California","Oregon","Washington","Texas"};
	
	int num_states = 4;
	i = 0;
	while( i < num_states )
	{
		strcpy(states[i],argv[i]);
	
		printf("state %d: %s\n",i,states[i]);
		printf("%d: %s\n",i,argv[i]); 
		
		i++;
	}
	return 0;
}
```

![](https://fan67.oss-cn-beijing.aliyuncs.com/3361733820732_.pic.jpg)

AI的分析：

```
叮！快来看看我和文心一言的奇妙对话～点击链接 https://yiyan.baidu.com/share/QpjEvlGZbP?utm_invite_code=eo6kD7hgoaeaMWomAskMHw%3D%3D&utm_name=6I6r6ZqQ5q6H6Zuq&utm_fission_type=common -- 文心一言，既能写文案、读文档，又能绘画聊天、写诗做表，你的全能伙伴！
```

而：

```shell
clf67@clf67:~/Cysyx$ make ex11
gcc -Wall -g ex11.c -o ex11
ex11.c: In function ‘main’:
ex11.c:19:24: warning: passing argument 1 of ‘strcpy’ from incompatible pointer type [-Wincompatible-pointer-types]
   19 |                 strcpy(&states[i],&argv[i]);
      |                        ^~~~~~~~~~
      |                        |
      |                        char **
In file included from ex11.c:2:
/usr/include/string.h:141:39: note: expected ‘char * restrict’ but argument is of type ‘char **’
  141 | extern char *strcpy (char *__restrict __dest, const char *__restrict __src)
      |                      ~~~~~~~~~~~~~~~~~^~~~~~
ex11.c:19:35: warning: passing argument 2 of ‘strcpy’ from incompatible pointer type [-Wincompatible-pointer-types]
   19 |                 strcpy(&states[i],&argv[i]);
      |                                   ^~~~~~~~
      |                                   |
      |                                   char **
In file included from ex11.c:2:
/usr/include/string.h:141:70: note: expected ‘const char * restrict’ but argument is of type ‘char **’
  141 | rn char *strcpy (char *__restrict __dest, const char *__restrict __src)
      |                                           ~~~~~~~~~~~~~~~~~~~~~~~^~~~~

clf67@clf67:~/Cysyx$ ./ex11
arg 1: (null)
state 0: ./ex11
0: ./ex11
state 1: 
1: (null)
state 2: SHELL=/bin/bash
2: SHELL=/bin/bash
state 3: PWD=/home/clf67/Cysyx
3: PWD=/home/clf67/Cysyx
clf67@clf67:~/Cysyx$ cat ex11.c
#include<stdio.h>
#include<string.h>

int main( int argc, char *argv[] )
{
	int i=argc;
	while( i > 0 )
	{
		printf("arg %d: %s\n",i,argv[i]);
		i--;
	}

	char *states[] = {"California","Oregon","Washington","Texas"};
	
	int num_states = 4;
	i = 0;
	while( i < num_states )
	{
		strcpy(&states[i],&argv[i]);
	
		printf("state %d: %s\n",i,states[i]);
		printf("%d: %s\n",i,argv[i]); 
		
		i++;
	}
	return 0;
}
```

### 12

### 13 * ✅

#### * 使用另一个`for`循环来让它处理你传入的所有命令行参数。✅

#### 附加题 5（相关的执行结果：把 break 放入 if结构 里面之后

![](https://fan67.oss-cn-beijing.aliyuncs.com/2091733905046_.pic.jpg)



### 14

#### 附加1

```c
【1】删去 can_print_it
 15 void print_letters(char arg[])
 16 {
 17         int i=0;
 18 
 19         for( i=0;arg[i]!='\0';i++ )                          
 20         {
 21                 char ch = arg[i];  
 22                 if( ( ch>='a' && ch<='z' ) || ( ch>='A' && ch<='Z' )|| ch=='     ' ){
 23                         printf("'%c' == %d ",ch,ch);
 24                 }
 25         }
 26         printf("\n");
 27 }
【2】只留 print_arguments
  4    void print_arguments(int argc, char *argv[])
  5   {
  6           int i=0,j=0;
  7           for( i=0; i<argc; i++ )
  8           {
  9                   for( j=0; argv[i][j]!='\0' ; j++ ){
 10                         char ch = argv[i][j];
 11                         if( ( ch>='a' && ch<='z' ) || ( ch>='A' && ch<='Z' )|| ch==' ' ){
 12                           printf("'%c' == %d ",ch,ch);
 13                          }
 14                 }
 15           }
 16           printf("\n");
 17   }
```

#### 附加2 ( ' // ' 标记有改动的地方 )

```c
void print_letters(char arg[],int length);
void print_arguments(int argc, char *argv[])
{
	int i=0,l; //
	for( i=0; i<argc; i++ )
	{	
		l=strlen(argv[i]); //
		print_letters(argv[i],l); //
	}
}
//第一题[仅删去 can_print_it，更改print_letters]【2】(第二题在此基础上)
void print_letters(char arg[],int length)
{
	int i=0;

	for( i=0;i<length;i++ ) //
	{
		char ch = arg[i];
		if( ( ch>='a' && ch<='z' ) || ( ch>='A' && ch<='Z' )|| ch==' ' ){
			printf("'%c' == %d ",ch,ch);
		}
	}
	printf("\n");
}

int main(int argc,char *argv[]){
	print_arguments(argc,argv);
	return 0;
}
```

### 15 指针

实用的指针用法：

- 向OS申请一块内存，并且用指针处理它。这包括字符串，和一些你从来没见过的东西，比如结构体。
- 通过指针向函数传递大块的内存（比如很大的结构体），这样不必把全部数据都传递进去。
- 获取函数的地址用于动态调用。❎
- 对一块内存做复杂的搜索，比如，转换网络套接字中的字节，或者解析文件。❎

*试着将`cur_age`指向`names`。可以需要C风格转换来强制执行，试着查阅相关资料把它弄明白。【不确定】

![0f1217fd827c924415df269fb5d0d08a](https://fan67.oss-cn-beijing.aliyuncs.com/0f1217fd827c924415df269fb5d0d08a.png)

*

```c
for( cur_name=names, cur_age=ages; (cur_age-ages)<count; cur_name++,cur_age++ ){
		printf("%s lived %d years so far.\n",*cur_name,*cur_age);
	}//这里怎么用“访问数组的方式呢”？
```

### 16

#### <assert.h> 

是 C 语言标准库中的一个头文件，它提供了一种用于调试程序的工具——断言（assertion）。断言是一种在程序运行时检查条件是否为真的机制，如果条件为假，则程序会终止执行。

要使用断言工具，需确保C程序包含了**<assert.h>头文件**

assert 宏 的定义如下：

```c
void assert(scalar expression);
```

其中 expression 是一个标量表达式（即可以转换为 int 类型的表达式）。如果 expression 的值为真（非零），则 assert 什么都不做，程序继续执行。如果 expression 的值为假（零），则 assert 会调用 abort 函数，终止程序的执行，并通常会在标准错误输出上打印一条错误消息，包括表达式、源文件名和行号。
————————————————

```
                        版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
```

原文链接：https://blog.csdn.net/csbysj2020/article/details/140393169

#### valgrind 排查内存泄露

![](https://fan67.oss-cn-beijing.aliyuncs.com/33bc63f6678af8ab2eb5777cc72a68df.png)

> 我们主要讨论内存错误检测器(Memcheck)，用于检测C/C ++程序中常见问题：
>
> - 访问不应该使用的内存，例如溢出，堆溢出，溢出栈顶，在访问释放了的内存。
> - 使用未定义的值，即尚未初始化的值或从其他未定义值派生的值。
> - 错误地释放堆内存，如重复释放堆，或者不匹配的使用malloc/new/new []与free/delete/delete []
> - 在memcpy和相关函数中重叠src和dst指针。
> - 在调用内存分配函数时，给size参数传递一个负值。
> - 内存泄漏。
>
> **valgrind的使用-命令格式**
>
> `valgrind [valgrind-options] your-prog [your-prog options] `
>
> 一些常用的options:
>
> **-h --help**   显示帮助信息
>
> **-version**   显示valgrind内核的版本，每个工具都有各自的版本
>
> **-q --quiet**   安静地运行，只打印错误信息
>
> **-v --verbose**   打印更详细的信息
>
> **-tool=[default: memcheck]**   最常用的选项。运行valgrind中名为 toolname 的工具。如果省略工具名，默认运行memcheck
>
> **db-attach=[default: no]**   绑定到调试器上，便于调试错误
>
> 🤔  😯  **-leak-check=<no|summary|yes|full>[default: summary]**   
>
> 当这个选项打开时，在客户程序结束时查找内存泄漏。内存泄漏意味着有用malloc分配内存块，但是没有用free释放，而且没有指针指向这块内存。这样的内存块永远不能被程序释放，因为没有指针指向它们。如果设置为summary，Valgrind会报告有多少内存泄漏发生了。如果设置为full或yes，Valgrind给出每一个独立的泄漏的详细信息。
> 
>
> ————————————————
>
>                             版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。
>
> 原文链接：https://blog.csdn.net/u010523811/article/details/129141596

......一个有点好笑的错误：

![image-20241216200803757](../../Library/Application Support/typora-user-images/image-20241216200803757.png)

谢谢**文小言（ai）** [大声！！！] :

> 在使用 `valgrind` 命令时，你遇到的错误主要是由于命令行格式不正确导致的。`valgrind` 命令中的选项和参数应该被正确地解析，但是在你的命令中，由于格式问题，shell 尝试将 `<no|summary|yes|full>` 中的各个选项当作独立的命令来执行，从而导致了错误。
>
> 正确的 `valgrind` 命令格式应该如下：
>
> ```bash
> valgrind --leak-check=<选项> ./ex16
> ```
>
> **其中 `<选项>` 可以是 `no`、`summary`、`yes` 或 `full`😓。**例如，如果你想检查内存泄漏的详细信息，你应该使用：
>
> ```bash
> valgrind --leak-check=full ./ex16
> ```
>
> 关于你遇到的 `std::logic_error` 异常，这通常是由于你的程序 `ex16` 在运行时尝试进行某种不合法的操作，比如构造一个包含 `nullptr` 的 `std::string`。这个错误与 `valgrind` 命令本身无关，而是你的程序内部逻辑问题。你需要检查 `ex16` 程序中是否有尝试用 `nullptr` 来初始化 `std::string` 的代码，并修正它。
>
> 总结一下，你应该：
>
> 1. 使用正确的 `valgrind` 命令格式。
> 2. 检查并修正 `ex16` 程序中的逻辑错误，特别是与 `std::string` 初始化相关的部分。
>
> 希望这些信息能帮助你解决问题！

​	好的！
​	![](https://fan67.oss-cn-beijing.aliyuncs.com/0f6a9417233f180457ae9db6f0376318.png)

#### 附加题 👍     【代码学习模版】

准备--了解“栈” 

https://baijiahao.baidu.com/s?id=1804271246966730285 ✅

https://www.bilibili.com/video/BV1WJ4m187cp/?vd_source=9a1b02760e5d5176081d05f5537d605e ✅

https://blog.csdn.net/Sugar_goat/article/details/136635655 ❎

这个练习的附加题中我想让你尝试一些有难度的东西：将这个程序改为不用指针和`malloc`的版本。这可能很困难，所以你需要研究下面这些东西：

- 如何在栈上创建结构体，就像你创建任何其它变量那样。
- 如何使用`x.y`而不是`x->y`来初始化结构体。
- 如何不使用指针来将结构体传给其它函数。

------

/// 来自ai --文小言

要实现一个真正的栈数据结构，我们需要确保元素只能从一个端（顶部）被添加（压栈）和移除（弹栈）。以下是一个简化的栈实现，使用动态内存分配来存储`person`结构体，并管理一个指向栈顶的指针。

这里，我将修改您的代码以创建一个栈，该栈可以存储`person`实例，并提供基本的栈操作：压栈（push）、弹栈（pop）和查看栈顶元素（peek）。由于栈是后进先出的，我们不需要`bottom`节点，只需要一个指向栈顶的指针和可能的栈大小或元素计数器（在这个简化示例中，我将省略计数器）。

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct person {
    char *name;
    int age;
    int height;
    int weight;
    struct person *next;
} person;

typedef struct stack {
    person *top;
} stack;

// 创建person实例
person* create_person(const char *name, int age, int height, int weight) {
    person *new_person = (person*)malloc(sizeof(person));
    new_person->name = (char*)malloc((strlen(name) + 1) * sizeof(char));
    strcpy(new_person->name, name);
    new_person->age = age;
    new_person->height = height;
    new_person->weight = weight;
    new_person->next = NULL;
    return new_person;
}

// 压栈操作
void push(stack *s, person *p) {
    p->next = s->top;
    s->top = p;
}

// 弹栈操作，返回栈顶元素并移除它
person* pop(stack *s) {
    if (s->top == NULL) {
        return NULL; // 栈空，返回NULL
    }
    person *temp = s->top;
    s->top = s->top->next;
    return temp;
}

// 查看栈顶元素但不移除它
person* peek(stack *s) {
    return s->top;
}

// 打印栈内容（从栈顶到栈底）
void print_stack(stack *s) {
    person *temp = s->top;
    while (temp != NULL) {
        printf("Name: %s\n\tAge: %d\n\tHeight: %d\n\tWeight: %d\n", temp->name, temp->age, temp->height, temp->weight);
        temp = temp->next;
    }
}

// 释放栈中所有元素占用的内存
void free_stack(stack *s) {
    person *temp;
    while ((temp = pop(s)) != NULL) {
        free(temp->name);
        free(temp);
    }
}

int main() {
    stack people_stack;
    people_stack.top = NULL;

    // 创建person实例并压栈
    push(&people_stack, create_person("Joe Alex", 32, 64, 140));
    push(&people_stack, create_person("Frank Blank", 20, 72, 180));

    // 打印栈内容
    print_stack(&people_stack);

    // 弹栈并打印每个弹出的元素
    person *popped;
    while ((popped = pop(&people_stack)) != NULL) {
        printf("Popped:\n");
        printf("Name: %s\n\tAge: %d\n\tHeight: %d\n\tWeight: %d\n", popped->name, popped->age, popped->height, popped->weight);
        free(popped->name);
        free(popped);
    }

    // 此时栈应为空，再次打印应无输出
    print_stack(&people_stack);

    return 0;
}
```

在这个实现中，`create_person`函数用于创建并初始化一个新的`person`实例。`push`函数将一个新的`person`实例压入栈顶。`pop`函数移除并返回栈顶元素。`peek`函数返回栈顶元素但不移除它。`print_stack`函数用于打印栈中的所有元素。最后，`free_stack`函数用于释放栈中所有元素占用的内存（注意：在`main`函数中，我已经在弹栈时释放了每个元素的内存，所以在这个特定示例中没有调用`free_stack`）。

请注意，在实际应用中，您应该始终检查动态内存分配是否成功（即检查`malloc`的返回值是否为`NULL`），并相应地处理错误情况。此外，为了简化示例，我没有包含错误处理代码。在生产代码中，您应该添加适当的错误处理逻辑。

```c
这是一份四不像代码，待后续完善
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct person {
    char *name;
    int age;
    int height;
    int weight;
    struct person *next;
} person;

typedef struct stack {
    person *top;
} stack;

// 创建person实例
person* create_person(const char *name, int age, int height, int weight) {
    person *new_person = (person*)malloc(sizeof(person));
    new_person->name = (char*)malloc((strlen(name) + 1) * sizeof(char));
    strcpy(new_person->name, name);
    new_person->age = age;
    new_person->height = height;
    new_person->weight = weight;
    new_person->next = NULL;
    return new_person;
}

// 压栈操作
void push(stack *s, person *p) {
    p->next = s->top;
    s->top = p;
}

// 弹栈操作，返回栈顶元素并移除它
person* pop(stack *s) {
    if (s->top == NULL) {
        return NULL; // 栈空，返回NULL
    }
    person *temp = s->top;
    s->top = s->top->next;
    return temp;
}

// 查看栈顶元素但不移除它
person* peek(stack *s) {
    return s->top;
}

// 打印栈内容（从栈顶到栈底）
void print_stack(stack *s) {
    person *temp = s->top;
    while (temp != NULL) {
        printf("Name: %s\n\tAge: %d\n\tHeight: %d\n\tWeight: %d\n", temp->name, temp->age, temp->height, temp->weight);
        temp = temp->next;
    }
}

// 释放栈中所有元素占用的内存
void free_stack(stack *s) {
    person *temp;
    while ((temp = pop(s)) != NULL) {
        free(temp->name);
        free(temp);
    }
}

int main() {
    stack people_stack;
    people_stack.top = NULL;

    // 创建person实例并压栈
    push(&people_stack, create_person("Joe Alex", 32, 64, 140));
    push(&people_stack, create_person("Frank Blank", 20, 72, 180));

    // 打印栈内容
    print_stack(&people_stack);

    // 弹栈并打印每个弹出的元素
    person *popped;
    while ((popped = pop(&people_stack)) != NULL) {
        printf("Popped:\n");
        printf("Name: %s\n\tAge: %d\n\tHeight: %d\n\tWeight: %d\n", popped->name, popped->age, popped->height, popped->weight);
        free(popped->name);
        free(popped);
    }

    // 此时栈应为空，再次打印应无输出
    print_stack(&people_stack);

    return 0;
}
```



### 17

#### <errno.h>

定义了通过错误码来回报错误资讯的宏。

#### atoi

```c
int atoi(const char *str);   //主要功能是将字符串转换为整数
```

它首先会跳过字符串前面的空白字符（如空格、制表符等），然后检查字符串中的第一个非空白字符。

如果第一个非空白字符是一个有效的整数符号（正号或负号），则继续读取后续的数字字符，直到遇到一个非数字字符为止。

转换过程中，字符串中的其他字符会被忽略。

如果转换成功，函数返回转换后的整数；如果转换失败（例如，输入字符串为空或仅包含空白字符），则返回0‌

---**理清内存的最简单的方式是遵守这条原则：如果你的变量并不是从`malloc`中获取的，也不是从一个从`malloc`获取的函数中获取的，那么它在栈上。**

#### "三个值得关注的关于栈和堆的主要问题："

- 如果你从`malloc`获取了一块内存，并且把指针放在了栈上，那么当函数退出时，指针会被弹出而丢失。
- 如果你在栈上存放了大量数据（比如大结构体和数组），那么会产生“栈溢出”并且程序会中止。这种情况下应该通过`malloc`放在堆上。
- 如果你获取了指向栈上变量的指针，并且将它用于传参或从函数返回，接收它的函数会产生“段错误”。因为实际的数据被弹出而消失，指针也会指向被释放的内存。

#### `strncpy`有设计缺陷

`strncpy`函数不会自动在目标字符串末尾添加空字符，除非源字符串长度小于指定的复制长度。所以有时需要手动再目标数组中添加'\0'

#### 待办

> [!NOTE]
>
> ```c
> //失败版，先往后看
> ex17_0.c
> #include<stdio.h>
> #include<assert.h>
> #include<stdlib.h>
> #include<errno.h>
> #include<string.h>
> 
> int MAX_DATA=0,MAX_ROWS=0;
> 
> struct Address {
> 
> 	int id;
> 	int set;
> //	char name[MAX_DATA];
> //	char email[MAX_DATA];
> 	char *name;
> 	char *email;
> 
> };
> //定义了一个Address结构体
> 
> 
> struct Database {
> 
> //	struct Address rows[MAX_ROWS];
> 
> 	struct Address *rows;
> 	int maxdata;
> 	int maxrows;
> //结构体数组
> 
> };
> 
> 
> struct Connection {
> 
> 	FILE *file;
> 	struct Database *db;
> 
> };
> //是要利用结构体数组和文件的内容做交互吧？
> 
> void Database_close(struct Connection *conn);
> 
> void die(const char *message, struct Connection *conn)
> {
> 	if(errno) {
> 		perror(message);
> 		Database_close(conn);
> 
> 	} else {
> 		printf("ERROR: %s\n",message);
> 		Database_close(conn);
> 
> 	}
> 
> 	exit(1);
> }
> 
> 
> void Address_print(struct Address *addr)
> {
> 
> 	printf("%d %s %s\n",addr->id,addr->name,addr->email);
> 
> }
> //打印Address中的信息
> 
> 
> void Database_load(struct Connection *conn)
> {
> 
> 	int rc = fread(conn->db, sizeof(struct Database), 1, conn->file);
> 	if( rc!=1 ) die("Failed to load database.",conn);
> 
> }
> //将文件里的信息存在结构体数组中
> 
> 
> struct Connection *Database_open(const char *filename, char mode, int maxdata, int maxrows)
> {
> 
> 	//动态分配内存给conn
> 	struct Connection *conn = malloc(sizeof(struct Connection));
> 	if(!conn) die("Memory error",conn);
> 	
> 	//动态分配内存给db
> 	conn->db = malloc(sizeof(struct Database));
> 	if(!conn->db) die("Memory error",conn);
> 	
> 	conn->db->maxdata = maxdata;
> 	conn->db->maxrows = maxrows;
> 	conn->db->rows = malloc(maxrows*sizeof(struct Address));
> 	
> 	for(int i=0; i<maxrows; i++){
> 		struct Address addr = {.name = malloc( maxdata * sizeof(char) ),.email = malloc( maxdata * sizeof(char) )};
> 		conn->db->rows[i] = addr;
> 	}
> 
> 	if(mode == 'c'){
> 	//c=create
> 		conn->file = fopen(filename, "w");
> 	
> 
> 	} else {
> 	
> 		conn->file = fopen(filename, "r+");
> 
> 		if(conn->file){
> 		
> 			//加载文件内容到数组中
> 			Database_load(conn);
> 		
> 		}
> 	}
> 
> 	if(!conn->file) die("Failed to open the file",conn);
> 
> 	return conn;
> }
> 
> 
> void Database_close(struct Connection *conn)
> {
> 	if(conn){
> 
> 		if(conn->file) fclose(conn->file);
> 		
> 		//把每一个分配到内存都释放
> 		//if(conn->db) free(conn->db);
> 		for(int i=0; i<MAX_ROWS; i++)
> 		{
> 			free(conn->db->rows+i->name);
> 			free(conn->db->rows+i->email);
> 
> 		}
> 		free(conn->db->rows);
> 		if(conn->db) free(conn->db); 
> 		free(conn);
> 	}
> }
> 
> 
> void Database_write(struct Connection *conn)
> {
> 
> 	//让文件指针的位置回到文件的起始位置
> 	rewind(conn->file);
> 
> 
> 	//把结构体中的信息写到文件中
> 	int rc = fwrite(conn->db, sizeof(struct Database), 1, conn->file);
> 	if(rc != 1) die("Failed to write database.",conn);
> 
> 
> 	//fflush:
> 	//它的主要作用是清除读写缓冲区，确保缓冲区内的数据被写入文件中
> 	//fflush(FILE *stream)
> 	rc = fflush(conn->file);
> 	if(rc == -1) die("Cannot flush database.",conn);
> 
> }
> 
> 
> void Database_create(struct Connection *conn)
> {
> 	int i=0;
> 
> 	for(i=0; i<MAX_ROWS; i++)
> 	{
> 
> 		struct Address addr ={.id=i, .set=0}; //
> 		conn->db->rows[i] = addr;
> 		//结构体之间的赋值
> 	}
> }
> 
> 
> void Database_set(struct Connection *conn, int id, const char *name, const char *email)
> {
> 
> 	struct Address *addr = &conn->db->rows[id];
> 	if(addr->set) die("Already set, delete it first",conn);
> 
> 	addr->set = 1;
> 	//WARNING
> 	int x=strlen(name); //
> 	char *res = strncpy(addr->name, name, MAX_DATA);
> 	
> 	if( x>= MAX_DATA ) addr->name[MAX_DATA-1] = '\0';
> 
> 	if(!res) die("Name copy failed",conn);
> 
> 	res = strncpy(addr->email, email, MAX_DATA);
> 	if(!res) die("Email copy failed",conn);
> 
> }
> 
> void Database_get(struct Connection *conn, int id)
> {
> 	struct Address *addr = &conn->db->rows[id];
> 
> 	if(addr->set){
> 		Address_print(addr);
> 	} else {
> 		die("ID is not set",conn);
> 	}
> }
> 
> void Database_delete(struct Connection *conn, int id)
> {
> 	
> 	struct Address addr = {.id=id, .set=0};
> 	conn->db->rows[id] = addr;
> 
> }
> 
> 
> void Database_list(struct Connection *conn)
> {
> 	int i=0;
> 	struct Database *db = conn->db;
> 
> 	for(i=0; i<MAX_ROWS; i++)
> 	{
> 		struct Address *cur = &db->rows[i];
> 
> 		if(cur->set){
> 			Address_print(cur);
> 		}
> 	}
> }
> 
> int main(int argc, char *argv[])
> {
> 	if(argc < 3) die("USAGE: ex17 <dbfile> <action> [action params]",NULL);
> 
> 	char *filename = argv[1];
> 	char action = argv[2][0];
> 	struct Connection *conn;
> 	/*用户输入的数字存储在argv里，是字符型，而需要使用int型的；
> 	  那么将两个‘数字’另写入一个文件里，再另设int型变量从文件中存入2个整数*/
> 	/*但是函数 atoi 有同样作用，为什么还要存到文件里？*/
> 	/*因为再次执行其他命令还要用到。。。*/
> 	if( action=='c' ){
> 		FILE *pf = fopen("convert.txt","w+");//打开准备写入
> 		if(pf == NULL){
> 			die("Failed to deposit the MAX_DATA&MAX_ROWS",conn);
> 		}	
> 		//把字符串转换成整型
> 		MAX_DATA = atoi(argv[3]);
> 		MAX_ROWS = atoi(argv[4]);
> 		//写入
> 		fprintf(pf, "%d %d", MAX_DATA, MAX_ROWS);
> 		//关掉
> 		fclose(pf);
> 
> //	printf("%d %d",MAX_DATA,MAX_ROWS);	
> 	}else{
> 		//从已经存在的文件中读
> 		FILE *pf0 = fopen("convert.txt","r");
> 		if(pf0 == NULL){
> 			//提醒用户还未创建数据库
> 			die("You haven't created a database yet.",conn);
> 		}
> 
> 		fscanf(pf0, "%d %d", &MAX_DATA, &MAX_ROWS);
> 		//关掉
> 		fclose(pf0);
> 	
> 	}	
> 	/* 接受参数maxdata,maxrows */
> 	conn = Database_open(filename, action, MAX_DATA, MAX_ROWS);
> 	int id = 0;
> 
> 	if(argc >5) id = atoi(argv[3]);
> 	//
> 
> 	if(id >= MAX_ROWS ) die("There's not that many records.",conn);
> 
> 	switch(action) {
> 		case 'c':
> 			Database_create(conn);
> 			Database_write(conn);
> 			break;
> 
> 		case 'g':
> 			if(argc != 4) die("Need an id to get",conn);
> 
> 			Database_get(conn,id);
> 			break;
> 
> 		case 's':
> 			if(argc !=6) die("Need id,name,email to set",conn);
> 
> 			Database_set(conn,id,argv[4],argv[5]);
> 			Database_write(conn);
> 			break;
> 
> 		case 'd':
> 			if(argc != 4) die("Need id to delete",conn);
> 
> 			Database_delete(conn,id);
> 			Database_write(conn);
> 			break;
> 
> 		case 'l':
> 			Database_list(conn);
> 			break;
> 
> 		default:
> 			die("Invalid action, only : c=create, g=get, s=set, d=del,l=list",conn);
> 	}
> 
> 	Database_close(conn);
> 	
> 	return 0;
> }
> ```
>
> 

### 18 函数指针

‌**函数指针是指向函数的指针变量**‌

#### 相关的基本知识

1. 格式 `int (*POINTER_NAME)(int a, int b)`

编写它的方法：

- 先 编写一个普通的函数声明 `int callme(int a, int b);`
- 再 将函数用指针语法包装 `int (*callme)(int a, int b);`
- 将名称改成指针名称 `int (*compare_cb)(int a, int b);`

```c
int (*tester)(int a, int b) = sorted_order;
printf("TEST: %d is same as %d\n", tester(2,3), sorted_order(2,3));
```

> [!IMPORTANT]
>
> 为 **函数返回类型为指针**的函数编写一个函数指针 也是可以的，步骤仍然是：
>
> - 编写：`char *make_coolness(int awesome_levels)`
> - 包装：`char *(*make_coolness)(int awesome_levels)`
> - 重命名：`char *(*coolness_cb)(int awesome_levels)`

#### memcpy

是C和C++标准库中的一个内存拷贝函数，用于将一段内存区域的内容复制到另一段内存区域中。

```c
void *memcpy(void *destin, void *source, unsigned n);
- destin:指向用于存储复制内容的目标数组的指针
- source:指向要复制的数据源的指针
- n:要被复制的字节数
```

即：**从源内存地址的起始位置开始拷贝n个字节到目标内存地址的起始位置中*

####  *strange_order?

就是一个 strange 的 order 吗？

#### “如何使它崩溃？”

> 我打算让你做一些奇怪的事情来使它崩溃，这些函数指针都是类似于其它指针的指针，他们都指向内存的一块区域。C中可以将一种指针的指针转换为另一种，以便以不同方式处理数据。这些通常是不必要的，但是为了想你展示如何侵入你的电脑，我希望你把这段代码添加在`test_sorting`下面：
>
> ```c
> unsigned char *data = (unsigned char *)cmp;
> 
> for(i = 0; i < 25; i++) {
>     printf("%02x:", data[i]);
> }
> 
> printf("\n");
> ```
>
> 这个循环将你的函数转换成字符串，并且打印出来它的内容。这并不会中断你的程序，除非CPU和OS在执行过程中遇到了问题。在它打印排序过的数组之后，你所看到的是一个十六进制数字的字符串：
>
> ```
> 55:48:89:e5:89:7d:fc:89:75:f8:8b:55:fc:8b:45:f8:29:d0:c9:c3:55:48:89:e5:89:
> ```
>
> 这就应该是函数的原始的汇编字节码了，你应该能看到它们有相同的起始和不同的结尾。也有可能这个循环并没有获得函数的全部，或者获得了过多的代码而跑到程序的另外一片空间。这些不通过更多分析是不可能知道的。

#### 十六进制编辑器--hexedit

常用命令：

[hexedit](https://blog.csdn.net/qq_22418329/article/details/108872444)

- 附加题：

  - 用“如何使它崩溃？”中的代码打印( 放在了main里面 ， 三个排序之后 )

    ```c
    				 void (*die_0)(const char *message);
             die_0 = die;
             unsigned char *data = (unsigned char *)die_0;
     
             for(i = 0; i < 25; i++) {
                 printf("%02x:", data[i]);
             }
     
             printf("\n");
    ```

    结果:

    <img src="https://fan67.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-12-29%2022.20.09.png" style="zoom:70%;" />
    
    
    
    ```shell
    :~/C$ ./ex18 1 5 2
    1 2 5 
    5 2 1 
    5 2 1 
    fd:7b:be:a9:fd:03:00:91:e0:0f:00:f9:a4:ff:ff:97:00:00:40:b9:1f:00:00:71:80:
    ```
    
    ![](https://fan67.oss-cn-beijing.aliyuncs.com/202412272004611.png)
    
    如上，红框之后的部分和程序打印出来的序列不一样，为什么呢？
    
    

































