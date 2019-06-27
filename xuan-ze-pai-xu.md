---
description: 选择排序的典型算法有简单选择排序和堆排序。简单选择排序是最简单的排序算法。堆排序
---

# 选择排序（堆排序）

## 简单选择排序

这是最简单的排序算法。

### 算法思想

每一趟排序中选择最小的元素与有序序列的最后一个交换位置。简单选择排序每一趟排序也至少能确定一个元素的位置。

### C语言实现

{% code-tabs %}
{% code-tabs-item title="SelectSort" %}
```c
void SelectSort(int* a, int n){
    for(int i=0;i<n;i++){
        int min = i;
        for(int j=i;j<n;j++){
            if(a[min]>a[j])
                min = j;
        }
        int temp = a[i];
        a[i] = a[min];
        a[min] = temp;
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 时间复杂度

简单选择排序的时间复杂度始终是 $$O(n^2)$$ 。

### 空间复杂度

$$O(1)$$ 。

### 稳定性

不稳定。例如{2，**2**，1}经过一趟排序之后第一个 2 会和最后一个 1 交换位置，序列变为{1，**2**，2}，此时值相同的不同元素的相对顺序发生了改变。

## 堆排序

