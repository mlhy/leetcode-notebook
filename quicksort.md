---
description: 快速排序是所有内部排序算法中平均性能最优的算法。
---

# 快速排序

### 算法思想

快速排序是基于[分治法](https://zh.wikipedia.org/wiki/%E5%88%86%E6%B2%BB%E6%B3%95)的，就是把一个复杂的问题分成两个或更多的相同或相似的子问题，直到最后子问题可以直接简单地求解，原问题的解即子问题的解的合并。

快速排序的特点，每一趟排序后，都会将一个元素（基准元素 `pivot`） 放到指定位置。算法主要包含两个部分，**划分**和**递归**，即`Partition()`和`QuickSort()`。

1. `Partition()`每次选择一个元素 `pivot` 作为基准，通过一趟排序将比`pivot`小的元素全部移动到左边，比`pivot`大的元素全部移动大右边，从而确定 `pivot` 的最终位置。
2. `QuickSort()`对 `pivot` 左边和右边的部分通过递归算法重复上述过程，直至每个部分只有一个元素或者空。

### C语言实现（递增）

{% code-tabs %}
{% code-tabs-item title="QuickSort" %}
```c
int Partition(int* a, int left, int right){
    int pivot = a[left];
    while(left<right){
        while(left<right&&a[right]<=pivot)right--;
        a[left] = a[right];
        while(left<right&&a[left]>=pivot)left++;
        a[right] = a[left];
    }
    a[left] = pivot;
    return left;
}

void QuickSort(int* a, int left, int right){
    if(left<right){
        int pivot_pos = Partition(a, left, right);
        QuickSort(a, left, pivot_pos-1);
        QuickSort(a, pivot_pos+1, right);
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 时间复杂度

| 平均情况 | 最坏情况 | 最好情况 |
| :---: | :---: | :---: |
|  $$O(nlog_{2}n)$$ |  $$O(n^2)$$  | $$O(nlog_{2}n)$$  |

快速排序的时间复杂度与 `Partition()` 的划分是否对称有关，**最理想的情况**是 `Partition()` 做到最平衡的划分，最终划分到每个部分只有 1 个或 0 个元素。设`QuickSort()`递归的最大深度为 $$x$$ ，那么 $$2^x=n$$， 此时递归的深度为$$x=log_{2}n$$ ，相同深度的 `Partition()` 的总复杂度是 $$O(n)$$ ，所以整个排序算法的复杂度是 $$O(nlog_{2}n)$$ 。**最坏的情况**是两个子问题分别有  $$n-1$$ 个元素和 $$0$$ 个元素，比如，完全顺序或完全逆序的序列，并且每次选择第一个或最后一个数作为 `pivot`，此时的相同深度的 `Partition()` 的总算法复杂度为 $$O(n)$$ ，所以整个排序算法的复杂度是 $$O(n^2)$$ 。

### 空间复杂度

| 平均情况 | 最坏情况 | 最好情况 |
| :---: | :---: | :---: |
|  $$O(log_{2}n)$$  |  $$O(n)$$  | $$O(log_{2}(n+1))$$  |

快速排序需要用栈来保存每一次递归调用的信息，栈的容量与递归的最大深度一致，最坏的情况下要进行 $$n-1$$次递归。

### 稳定性

在递增有序序列中，如果在右边的部分有两个相同的元素小于 `pivot`，那么这两个元素的位置换到左边之后相对位置会发生变化。因此快速排序是**不稳定**的算法。

### 进一步优化算法

1. 选取更优的 `pivot` 值，通常情况下取序列的第一个值，进行优化时可以从序列中随机选择，或者从序列的首尾和中间进行选择，然后取值在中间的那一个。
2. 当递归划分的子序列较小时，不再继续使用快速排序，而是使用直接插入排序。

