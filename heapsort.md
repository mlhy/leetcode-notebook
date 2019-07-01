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

堆排序代码主要有建堆、调整和排序三个部分。

#### 建堆

对于无序序列 L 需要先进行建堆，使其符合上述堆排序的定义。建堆有两种方法，一种是每次在堆底插入一个元素然后进行调整，这样建堆的时间复杂度为 $$O(nlog_2n)$$ ，不推荐使用；另一种方法是直接对序列 L 中有孩子的结点进行调整，这样建堆的时间复杂度为 $$O(n)$$ 。

#### 调整

调整代码是堆排序的核心。对每一组 $$L(i)$$、$$L(2i+1)$$、 $$L(2i+2)$$ 进行调整，使最大元素在最上面。如果原本最大元素不在最上面的位置，那么交换位置后可能会影响其子树的排列，需要继续对子树进行调整，这时可以使用递归实现。

**Note：**这里 ****$$i$$的范围是 $$0\leq i\leq \frac{n}2 -1$$，同时这里的 $$L(2i+2)$$可能不存在，需要判断 $$2i+2< n$$ 是否成立， $$n\geq 2$$ 。

#### 排序

每一次将堆顶元素取出，将堆底元素放入堆顶，即**交换堆顶和堆底元素的位置**，一旦交换原本的堆被破坏，需要**重新调整堆**，此时堆的长度减 1，最后的序列 L 就是一个有序序列。

### C语言实现（大根堆）

#### 调整的递归实现

{% code-tabs %}
{% code-tabs-item title="AdjustHeap" %}
```c
void swap(int* a,int* b){
    int temp = *a;
    *a = *b;
    *b = temp;
}

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
{% endcode-tabs-item %}
{% endcode-tabs %}

#### 调整的非递归实现

{% code-tabs %}
{% code-tabs-item title="AdjustHeap" %}
```c
void swap(int* a,int* b){
    int temp = *a;
    *a = *b;
    *b = temp;
}

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
{% endcode-tabs-item %}
{% endcode-tabs %}

#### 建堆

{% code-tabs %}
{% code-tabs-item title="BuildHeap" %}
```c
void BuildHeap(int* a, int n){
    for(int i=n/2;i>=0;i--)
        AdjustHeap(a, i, n);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### 堆排序

{% code-tabs %}
{% code-tabs-item title="HeapSort" %}
```c
void HeapSort(int *a, int n){
    BuildHeap(a, n);
    for(int i=n-1;i>=0;i--){
        swap(&a[0], &a[i]);
        AdjustHeap(a, 0, i);
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 时间复杂度

| 平均情况 | 最坏情况 | 最好情况 |
| :---: | :---: | :---: |
|  $$O(nlog_{2}n)$$ |  $$O(nlog_2n)$$  | $$O(nlog_{2}n)$$  |

建堆的时间复杂度为 $$O(n)$$。调整的时间复杂度为 $$O(log_2n)$$ 。堆排序在最好、最坏和平均情况下的算法复杂度都是 $$O(nlog_2n)$$ 。

### 空间复杂度

递归实现的空间复杂度为 $$O(log_2n)$$ ，非递归实现的空间复杂度为 $$O(1)$$ 。

### 稳定性

不稳定。

