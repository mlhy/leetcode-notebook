# 插入排序

## 直接插入排序

### 算法思想（增序）



### C语言实现

{% code-tabs %}
{% code-tabs-item title="InsertSort" %}
```c
void InsertSort(int* a, int n){
    int i, j, temp;
    for(i=1;i<n;i++){
        temp = a[i];
        for(j=i-1;j>=0;j--){
            if(temp<a[j])
                a[j+1] = a[j];
            else
                break;
        }
        a[j+1] = temp;
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 折半插入排序

{% code-tabs %}
{% code-tabs-item title="HalfInsertSort" %}
```c
void HalfInsertSort(int* a, int n){
    for(int i=1;i<n;i++){
        int temp = a[i];
        int left = 0;
        int right = i-1;
        while(left<=right){
            int mid = (left+right)/2;
            if(temp<a[mid])right = mid-1;
            else left = mid+1;
        }
        for(int j=i-1;j>=right+1;j--)
            a[j+1] = a[j];
        a[right+1] = temp;
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 希尔排序

又名缩小增量排序。

### 算法思想（增序）

基本思想是分组，各组进行直接插入排序，然后对所有元素进行直接插入排序。

目前尚未解出最优的增量序列，希尔提出的增量序列是 $$d_1=n/2$$， $$d_{i+1}=d_i/2$$，最后一个增量 $$d$$ 为 1。

### 算法示例

如增量序列为  $$d=$$ {5，3，1} 的 {50，26，38，80，70，90，8，30，40，20} 的希尔排序过程为：

第一趟 $$d=5$$ ：{50，8，30，40，20，90，26，38，80，70}

第二趟 $$d=3$$ ：{26，8，30，40，20，80，50，38，90，70}

第三趟 $$d=1$$ ：{8，20，26，30，38，40，50，70，80，90}

### 空间复杂度

$$O(1)$$ 。

### 时间复杂度

数学上还没有确切的解，当 n 在一定范围时为 $$O(n^{1.3})$$ ，最坏情况的时间复杂度为 $$O(n^2)$$ 。

### 稳定性

当相同的元素被分到不同的分组当中时，可能会改变它们的相对位置，因此它是**不稳定**的。

### 适用性

仅适用于**顺序存储的线性表**。

