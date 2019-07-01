# 归并排序

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

