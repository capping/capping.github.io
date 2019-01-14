---
title: LeetCode c twoSum
date: 2019-01-14 16:44:16
tags: [LeetCode, c]
categories: [LeetCode-c]
---

本文是刷LeetCode学c系列文章。

1. Two Sum
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.
**Example:**
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
Solution1:
```
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* nums, int numsSize, int target) {
    int *a = (int*) malloc(2*sizeof(int));
    if(a==NULL) exit(1);  // 判断是否分配成功
    for (int i=0; i<numsSize; i++) {
        for (int j=i+1; j<numsSize; j++) {
           if (nums[i] + nums[j] == target) {
               a[0] = i;
               a[1] = j;
           }
        }
    }
    return a;
}

```
解析：
来源：http://c.biancheng.net/cpp/html/137.html  
malloc函数用来动态分配内存，其原型为：`void* malloc(size_t size);`  
【参数说明】size为需要分配的内存空间大小，以字节(Byte)计。  
【函数说明】malloc()在堆区分配一块指定大小的内存空间，用来存放数据。这块内存空间在函数执行完成后不会被初始化，它们的值是未知的。如果希望在分配内存的同时
进行初始化，请使用calloc()函数。
【返回值】分配成功返回指向该内存的地址，失败则返回NULL。  

由于申请内存空间时可能有也可能没有，所以需要自行判断是否申请成功，再进行后续操作。  

如果size的值为0，那么返回值会因标准库实现的不同而不同，可能是NULL，也可能不是，但返回的指针不应该再次被引用。  

**注意:函数的返回值类型是void\*，void并不是说没有返回值或者返回空指针，而是返回的指针类型未知。所以在使用malloc时通常需要进行强制类型转换，将void指针
转换成我们希望的类型，例如：**
```
char *ptr = (char *)malloc(10);  // 分配10个字节的内存空间，用来存放字符串
```
动态内存分配举例：
```
#include <stdio.h>  /* printf, scanf, NULL */
#include <stdlib.h>  /* malloc, free, rand, system */

int main ()
{
    int i,n;
    char * buffer;

    printf ("输入字符串的长度：");
    scanf ("%d", &i);

    buffer = (char*)malloc(i+1);  // 字符串最后包含 \0
    if(buffer==NULL) exit(1);  // 判断是否分配成功

    // 随机生成字符串
    for(n=0; n<i; n++)
        buffer[n] = rand()%26+'a';
    buffer[i]='\0';

    printf ("随机生成的字符串为：%s\n",buffer);
    free(buffer);  // 释放内存空间

    system("pause");
    return 0;
}
```
运行结果：
输入字符串的长度：20
随机生成的字符串为：lrfkqyuqfjkxyqvnrtys
