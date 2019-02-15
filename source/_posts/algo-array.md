---
title: 05 | 数组：为什么很多编程语言中数组都是从0开始编号？
date: 2019-02-15 11:11:05
tags: [algo]
categories: [数据结构与算法之美学习笔记]
---

## 1. 什么是数组？
数组(Array)是一种**线性表**数据结构。它是一组**连续的内存空间**，来存储一组具有**相同类型的数据**。

关键词：线性表，连续的内存空间，相同类型的数据

### 连续的内存空间和相同类型的数据

1. 优点：随机访问

2. 缺点：低效的“插入”和“删除”
- 为什么低效？  
- 有什么措施可以防止低效吗？  

## 2. 警惕数组的访问越界问题

```
int main(int argc, char* argv[]){
    int i = 0;
    int arr[3] = {0};
    for(; i<=3; i++){
        arr[i] = 0;
        printf("hello world\n");
    }
    return 0;
}
```

```
gcc -fno-stack-protector test.c
```

## 3. 容器能否完全替代数组？

## 4. 代码实现
### Array.java
```
package array;

/**
 * 1）数组的插入，删除，按照下标随机访问;
 * 2）数组中的数据是int类型的;
 *
 * @Author <Xuebin Zhang>
 */
public class Array
{
    public int[] data;
    private int n;
    private int count;

    // 构造方法，定义数组大小
    public Array(int capacity) {
        this.data = new int[capacity];
        this.n = capacity;
        this.count = 0;
    }

    // 根据索引，找到数据中的元素并返回
    public int find(int index) {
        if (index < 0 || index >= count) return -1;
        return data[index];
    }

    // 插入元素
    public boolean insert(int index, int value) {
        // 数组已满
        if (count == n) {
            System.out.println("没有可插入的位置");
            return false;
        }
        // 如果count还没满，那么就可以插入数据到数组中
        // 位置非法
        if (index < 0 || index > count) {
            System.out.println("位置非法");
            return false;
        }
        // 位置合法
        for (int i = count; i > index; --i) {
            data[i-1] = data[i];
        }
        data[index] = value;
        ++count;
        return true;
    }

    // 根据索引，删除数组中的元素
    public boolean delete(int index) {
        if (index < 0 || index > count) return false;
        for (int i = index + 1; i < count; i++) {
            data[i-1] = data[i];
        }
        --count;
        return true;
    }

    public void printAll() {
        for (int i = 0; i < count; i++) {
            System.out.print(data[i] + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        Array array = new Array(5);
        array.printAll();
        array.insert(0, 3);
        array.insert(0, 4);
        array.insert(1, 5);
        array.insert(3, 9);
        array.insert(3, 10);
        array.printAll();
    }
}
```
### GenericArray.java
```
public class GenericArray<T>
{
    private T[] data;
    private int size;

    // 根据传入容量，构造Array
    public GenericArray(int capacity) {
        data = (T[]) new Object[capacity];
        size = 0;
    }

    // 无参构造方法，默认数组容量为10
    public GenericArray() {
        this(10);
    }

    // 获取数组容量
    public int getCapacity() {
        return data.length;
    }

    // 获取当前元素个数
    public int count() {
        return size;
    }

    // 判断数组是否为空
    public boolean isEmpty() {
        return size == 0;
    }

    // 修改index位置的元素
    public void set(int index, T e) {
        checkIndex(index);
        data[index] = e;
    }

    // 获取对应的index位置的元素
    public T get(int index) {
        checkIndex(index);
        return data[index];
    }

    // 查看元素是否包含元素e
    public boolean contains(T e) {
        for (int i = 0; i < size; i++) {
            if (data[i].equals(e)) {
                return true;
            }
        }
        return false;
    }

    // 获取对应元素的下标，未找到，返回-1
    public int find(T e) {
        for (int i = 0; i < size; i++) {
            if (data[i].equals(e)) {
                return i;
            }
        }
        return -1;
    }

    // 在index位置，插入元素e，时间复杂度O(m+n)
    public void add(int index, T e) {
        checkIndex(index);
        // 如果当前元素个数等于数组容量，则将数组扩容为原来的2倍
        if (size == data.length) {
            resize(2 * data.length);
        }

        for (int i = size; i > index; i--) {
            data[i] = data[i-1];
        }
        data[index] = e;
        size++;
    }

    // 向数组头插入数据
    public void addFirst(T e) {
        add(0, e);
    }

    // 删除index位置的元素并返回
    public T remove(int index) {
        checkIndexForRemove(index);

        T ret = data[index];
        for (int i = index+1; i < size; i++) {
            data[i-1] = data[i];
        }
        size--;
        data[size] = null;

        // 缩容
        if (size == data.length / 4 && data.length / 2 != 0) {
            resize(data.length / 2);
        }

        return ret;
    }

    // 删除第一个元素
    public T removeFirst() {
        return remove(0);
    }

    // 删除末尾元素
    public T removeLast() {
        return remove(size-1);
    }

    // 从数组中删除指定元素
    public void removeElement(T e) {
        int index = find(e);
        if (index != -1) {
            remove(index);
        }
    }

    @Override
    public String toString() {
        StringBuffer builder = new StringBuffer();
        builder.append(String.format("Array size = %d, capacity = %d \n", size, data.length));
        builder.append('[');
        for (int i = 0; i < size; i++) {
            builder.append(data[i]);
            if (i != size-1) {
                builder.append(", ");
            }
        }
        builder.append(']');
        return builder.toString();
    } 

    // 扩容方法，时间复杂度是O(n)
    private void resize(int capacity) {
        T[] newData = (T[]) new Object[capacity];
        for (int i = 0; i < size; i++) {
            newData[i] = data[i];
        }
        data = newData;
    }
    
    private void checkIndex(int index) {
        if (index < 0 || index > size) {
            throw new IllegalArgumentException("Add failed! Require index>=0 and index<=size");
        }
    }

    private void checkIndexForRemove(int index) {
        if (index < 0 || index >= size) {
            throw new IllegalArgumentException("Remove failed! Require index>=0 and index<=size");
        }
    }

    public void printAll() {
        for (int i = 0; i < data.length; i++) {
            System.out.print(data[i] + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        GenericArray<Integer> array = new GenericArray<Integer>(5);
        array.printAll();
        array.add(0, 3);
        array.add(0, 4);
        array.add(1, 5);
        array.add(3, 9);
        array.add(3, 10);
        array.printAll();
    }
}
```

参考链接：
[GCC 中的编译器堆栈保护技术](https://www.ibm.com/developerworks/cn/linux/l-cn-gccstack/index.html)
