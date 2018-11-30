---
title: Java数组操作
date: 2018-10-30 14:04:01
tags:
---
1. 声明数组
```
String[] arr1;
String arr2[];
String arr3[] = new String[5];
String[] arr4 = new String[5]
```
2. 初始化数组
```
// 静态初始化
String[] arr1 = {"1", "2", "3", "4", "5"};
String[] arr1 = new String[]{"1", "2", "3", "4", "5"};
String arr2[] = new String[]{"1", "2", "3", "4", "5"};
String arr2[] = {"1", "2", "3", "4", "5"};
// 动态初始化
int score[] = new int[3];
for (int i = 0; i < score.length; i++) {
    score[i] = i + 1;
}
```
3. 查看数组的长度
```
int length = arr.length;
```
4. 遍历数组
```
for (int i = 0; i< arr.length; i++) {

}
```
5. int 数组转成string数组
```
int[] arr = {1, 2, 3, 4, 5};
String arrStrings = Arrays.toString(arr);
System.out.println(arrStrings);
```
6. 从array中创建ArrayList
```

```