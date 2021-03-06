---
description: LeetCode | Medium | 215. Kth Largest Element in an Array
---

# 215. Kth Largest Element in an Array

### 题目：数组中第 K 个最大的数

Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

**Example 1:**

```text
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Note:**   
You may assume k is always valid, 1 ≤ k ≤ array's length.

### 算法思路

最简单的思路是使用快速排序算法，然后取第 K 个值，此时平均算法复杂度与快速排序相同是 $$O(nlog_{2}n)$$。但是如果针对这个问题对快速排序进行优化就可以得到复杂度 $$O(n)$$ 的算法。

将每一次`Partition()` 之后得到的`pivotpos` 与 `k`进行比较，然后只对包含 k 的子问题进行递归，那么随着递归深入，复杂度指数下降。计算方法为 $$n+n/2+n/4+n/8+……<2n$$ ，最终**平均算法复杂度**为 $$O(n)$$ 。

### C 语言实现

因为题目是找第 k 个最大的数，所以这里的排序是降序的。

{% code-tabs %}
{% code-tabs-item title=" QuickSortkth" %}
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

void QuickSortK(int* a, int left, int right, int k){
    if(left<right){
        int pivotpos = Partition(a, left, right);
        if(pivotpos>=k)
            QuickSortK(a, left, pivotpos-1, k);
        else
            QuickSortK(a, pivotpos+1, right, k);  
    }
}

int findKthLargest(int* nums, int numsSize, int k){
    QuickSortK(nums, 0, numsSize-1, k);
    return nums[k-1];
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



