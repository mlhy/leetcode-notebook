---
description: 选择排序的典型算法有简单选择排序和堆排序。简单选择排序是最简单的排序算法。堆排序在最好、最坏和平均情况下的算法复杂度都是稳定的。
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

堆排序是一种树形的选择排序方法。把一个序列 L 看作是一个完全二叉树的结构。堆的定义为 $$L(i) \leq L(2i+1)$$ 且 $$L(i) \leq L(2i+2)$$ 或 $$L(i) \geq L(2i+1)$$ 且 $$L(i) \geq L(2i+2)$$ ， $$i$$ 从 0 开始。根结点为最小值时是**小根堆**，为最大值时是**大根堆**。

### 算法思想



### C语言实现（大根堆）

#### 调整的递归实现

```c
void AdjustHeap(int* a, int index, int n){
    if(index>=n/2)
        return ;

    int left = index*2 + 1;
    int right = index*2 + 2;
    
    int big = index;
    if(a[left]>a[big])
        big = left;
    if(right<n&&a[right]>a[big])
        big = right;
    swap(&a[big], &a[index]);
        
    if(big!=index)
        AdjustHeap(a, big, n);
}
```

#### 调整的非递归实现

```c
void AdjustHeap(int* a, int index, int n){
    while(index<n/2){
        int left = index*2 + 1;
        int right = index*2 + 2;

        int big = index;
        if(a[left]>a[big])
            big = left;
        if(right<n&&a[right]>a[big])
            big = right;
        if(big != index){
            swap(&a[big], &a[index]);
            index = big;
        }else{
            break;
        }
    }
}
```

#### 建堆

```c
void BuildHeap(int* a, int n){
    for(int i=n/2;i>=0;i--)
        AdjustHeap(a, i, n);
}
```

#### 堆排序

```c
void HeapSort(int *a, int n){
    BuildHeap(a, n);
    for(int i=n-1;i>=0;i--){
        swap(&a[0], &a[i]);
        AdjustHeap(a, 0, i);
    }
}
```

### 时间复杂度

### 空间复杂度

### 稳定性

