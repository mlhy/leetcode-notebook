---
description: 归并排序是将两个或两个以上的有序序列组合成一个新的有序序列。在这篇笔记中介绍的是 2-路归并排序。
---

# 归并排序

### 算法思想

归并排序算法主要包含两个部分，一个是**合并**，由`Merge()`实现；一个是**递归**，由 `MergeR()` 实现。代码中的`MergeSort()`函数可以方便调用以及管理申请的辅助空间 `b[n]`，避免内存泄漏。

### C语言实现

{% code-tabs %}
{% code-tabs-item title="MergeSort" %}
```c
void Merge(int* a, int* b, int start,  int end){
    int len = end - start;

    for(int i=start;i<=end;i++)
        b[i] = a[i];

    int mid = len/2 + start;
    int start_1 = start, end_1 = mid;
    int start_2 = mid+1, end_2 = end;
    int k = start;

    while(start_1<=end_1&&start_2<=end_2){
        if(b[start_1]<b[start_2])
            a[k++] = b[start_1++];
        else
            a[k++] = b[start_2++];
    }

    while(start_1<=end_1)
        a[k++] = b[start_1++];

    while(start_2<=end_2)
        a[k++] = b[start_2++];
}

void MergeR(int* a, int* b, int start, int end){
    if(start>=end)
        return ;
    int len = end - start;
    int mid = len/2 + start;
    if(start<end){
        MergeR(a, b, start, mid);
        MergeR(a, b, mid+1, end);
    }
    Merge(a, b, start, end);
}

void MergeSort(int* a, int len){
    int * b = (int*)malloc(sizeof(int)*len);
    MergeR(a, b, 0, len-1);
    free(b);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 时间复杂度

| 平均情况 | 最坏情况 | 最好情况 |
| :---: | :---: | :---: |
|  $$O(nlog_{2}n)$$ |  $$O(nlog_2n)$$  | $$O(nlog_{2}n)$$  |

每一趟归并的时间复杂度为 $$O(n)$$ ，递归的深度为 $$O(log_2n)$$ ，因此算法的时间复杂度为 $$O(nlog_2n)$$ 。

### 空间复杂度

在`Merge()`操作中，需要申请长度为`n`的辅助序列`b[n]`，因此空间复杂度为 $$O(n)$$ 。

### 稳定性

`Merge()`操作不会改变相同元素的相对顺序，因此是**稳定的**。

