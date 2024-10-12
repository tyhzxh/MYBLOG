+++
title = 'CSAPP第二章'
date = 2024-04-07T23:31:21+08:00
draft = true
+++

呃呃，开个新坑（虽然算法被搁置很久了）

著名的大黑书 CSAPP 《深入理解计算机系统》，总结一下3月的学习历程

耗费许多心血，题目较多

从第二章（信息的表示与处理）开始，因为第一章属于导论，之后有机会的话就补充一下

## 整数

从这一节开始，本书将我们引入了计算机最底层的数据表示领域（梦开始的地方）

首先强调一点，计算机一切的二进制序列是没有实际意义的，重要的是看你怎么去解释它

下面开始细说

### 有符号数

以int类型举例（当然我就不列出所有的有符号类型了）

程序员编程时面对的 是一个 内存空间（一种抽象）

这个内存空间是一个很长的**字节**数组（注意是字节，这就代表着字节是内存的最小单位）

内存模型就是这么回事 当然每个字节时候 8 个位组成 ，这里的位就是二进制的位

所以本质上是一堆二进制序列

每一个字节里的8位二进制数习惯用16进制表示，例如11111111表示位ff

看多了自然就明白，16进制的两位分别由二进制的4位组成

上面的两个f对应4个1 每一位分别是8 4 2 1（数电里会讲）

接着回到有符号数

int类型有4个字节 一共32位 

灰常的简单啊（等你学了浮点数也会这么觉得）

第一位是符号位 0 就是正数 1 就是负数

这种存储方式叫补码，我认为补码是一种对二进制代码的解释方式，因为相同的二进制在不同的类型下意义不同

负数的补码的真值就是取反再加1

当然，补码的一个重要的点就是数的相反数就是取反再加1

### 无符号数

无符号数最简单，32位二进制代码就是真值

到这里就没了？不，这本书太深了，它向读者展现了大量的类型转换，溢出，等等

计算机的底层简单却又复杂，简单是它的实现是如此的优美，如此的巧妙

大一学c语言时学过补码，觉得非常麻烦，但是现在看来，真的是相当巧妙的一种存储方式，正数和负数都通过取反再加1实现了取反

这里做了三个小作业，深入地进入了二进制代码的世界

#### 大小端机器

书里展现了滑稽的大小端之争，所谓大小端，指的是计算机存储变量字节的顺序，是先存储小的位数（小端机器），还是先存储大的位数（大端机器）

好在现在PC大多都是小端机器

实现一个检测机器是大端小端的程序

```c
#include <stdio.h>

char get_first_bytes(char *a)
{
 //printf("first byte is %.2x\n", a[0]);
 return a[0];
}

void islittle_endian()
{
int a = 1;
if (get_first_bytes((char *)&a))
    printf("is little_endian");
else
    printf("is large_endian");
}

int main()
{
    islittle_endian();
}
```

#### show_byte()

```c++
#include <stdio.h>

typedef unsigned char *byte_pointer; // 无符号char指针

void show_bytes(byte_pointer start, int len)
{
    int i;
    for (int i = 0; i < len; i++)
    {
        printf("%.2x", start[i]); 
    }
    printf("\n");
}

void show_int(int x)
{
    show_bytes((byte_pointer)&x, sizeof(int));
} 

void show_float(float x)
{
    show_bytes((byte_pointer)&x, sizeof(float));
}

void show_pointer(void *x)
{
    show_bytes((byte_pointer)&x, sizeof(void *));
}

// void test_show_bytes1(int val){
//     int ival = val;
//     float fval = (float)val;
//     int * pval = &ival;
//     show_int(ival);
//     show_float(fval);
//     show_pointer(pval);
// }


void show_short(short x){
    show_bytes((byte_pointer)&x,sizeof(short));
}

void show_long(long  x){
    show_bytes((byte_pointer)&x,sizeof(long ));
}

void show_double(double x){
    show_bytes((byte_pointer)&x,sizeof(double));
}


void test_show_bytes2(short val){
    short sval = val;
    long lval = (long)val;
    double dval = (double)val;
    show_short(sval);
    show_long(lval);
    show_double(dval); 
}

int main()
{
    //test_show_bytes1(12345);
    test_show_bytes2(12345);
    return 0;
}
```

#### |&运算初探

```c
#include <stdio.h>

int f1(int x){
    return (x|(0xffffffff));
}

int f2(int x){
    return (x&(0x00000000));
}

int f3(int x){
    return (x|(0xff000000));
}

int f4(int x){
    return (x&(0xffffff00));
}

void show_16bit(int x){
    printf("%x\n",x);
}

int main(){
    int x ;
    scanf("%x",&x);
    show_16bit(f1(x));
    show_16bit(f2(x));
    show_16bit(f3(x));
    show_16bit(f4(x));

}
```

**无符号类型名字** 就是在*有符号类型名字*前面加上usigned

注意：`unsigned` 单独等价于 `unsigned int`

## 浮点数（难点）

符号位s 阶码exp 尾码franc

非规格化 规格化 无穷大 不存在数

浮点数和补码一样精妙，做到了最后可以按整数排序那样从大到小进行排序

当你理解了它的设计时，你就不会再厌烦它的复杂！

## 运算（难点也是重点）

lab的难度也是暴大（之后出一节专门总结）
