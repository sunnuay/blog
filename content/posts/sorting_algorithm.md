---
title: Sorting Algorithm
description: 排序算法
---

排序算法作为最基本的算法之一，其重要性不言而喻，时至今日仍在被优化改进。

2023 年 6 月 8 日，Google DeepMind 新作 AlphaDev 发现了更快的排序算法，同时相关文章 [Faster sorting algorithms discovered using deep reinforcement learning](https://www.nature.com/articles/s41586-023-06004-9) 在 Nature 发表。

有趣的是不久后 Dimitris Papailiopoulos 发推 [GPT-4 "discovered" the same sorting algorithm as AlphaDev by removing "mov S P"](https://twitter.com/DimitrisPapail/status/1666843952824168465) 表示通过合适的提示词，GPT-4 也能得到同样结果，还引得马斯克围观。

C++ STL 中 std::sort 采用的是 David Musser 提出的混合排序算法，主要使用快速排序，同时记录递归深度，看情况转为堆排序，此外对短数据采用其他排序算法。

LLVM 作为被广泛使用的 C++ 编译后端，其 libcxx 对于短数据采用手写 sort3 sort4 sort5 的形式，AlphaDev 对此进行了优化，相关 [Commit](https://github.com/llvm/llvm-project/commit/194d1965d2c841fa81e107d19e27fae1467e7f11) 已被合并，这也是排序算法库十多年来第一次更新。

AlphaDev 从汇编出发，以发现更快的排序和散列算法为目标，通过强化学习解组合优化问题，根据计算耗时和结果来评估奖励，不断迭代，最后在底层发现了可优化的部分。

图为 sort3 汇编代码的前后对比，难以相信被优化了几十年，每天被调用数万亿次的排序算法，还存在着如此简单的优化方法。

![](assembly.webp)

- mov: move
- cmp: compare
- cmovg: conditional move if greater
- cmovl: conditional move if less

大模型的推理主要是模式识别，知识整合等，而非真正的逻辑推理。但经过验证，在指定范围的情况下，最新的 GPT-4 的确可以对上述代码进行优化，得到与论文相同的结果。此外，大模型能否作为通解代替特定程序，去优化更多微小细节，还有待研究。

附十大排序算法，其中 r 表示范围，基数，桶数量，d 表示位数。

| name      | stable | time   | space | place |
| --------- | ------ | ------ | ----- | ----- |
| insertion | yes    | n^2    | 1     | in    |
| selection | no     | n^2    | 1     | in    |
| bubble    | yes    | n^2    | 1     | in    |
| shell     | no     | varies | 1     | in    |
| heap      | no     | nlog n | 1     | in    |
| quick     | no     | nlog n | log n | in    |
| merge     | yes    | nlog n | n     | out   |
| counting  | yes    | n+r    | n+r   | out   |
| radix     | yes    | d(n+r) | n+r   | out   |
| bucket    | yes    | n+r    | n+r   | out   |

插入排序：
```c
void insertion_sort(int arr[], int len)
{
    for (int i = 1; i < len; i++)
    {
        int j, key = arr[i];
        for (j = i - 1; j >= 0 && arr[j] > key; j--)
            arr[j + 1] = arr[j];
        arr[j + 1] = key;
    }
}
```
![](insertion.png)


选择排序：
```c
void selection_sort(int arr[], int len)
{
    for (int i = len - 1; i > 0; i--)
    {
        int max = i;
        for (int j = 0; j < i; j++)
            if (arr[j] > arr[max])
                max = j;
        swap(&arr[max], &arr[i]);
    }
}
```
![](selection.png)


冒泡排序：
```c
void bubble_sort(int arr[], int len)
{
    for (int i = 0; i < len - 1; i++)
        for (int j = 0; j < len - 1 - i; j++)
            if (arr[j] > arr[j + 1])
                swap(&arr[j], &arr[j + 1]);
}
```
![](bubble.png)

希尔排序：
```c
void shell_sort(int arr[], int len)
{
    int gap[] = {1, 3, 5};
    int n = sizeof(gap) / sizeof(int);
    while (n--)
        for (int h = gap[n], i = h; i < len; i++)
            for (int j = i; j >= h; j -= h)
                if (arr[j - h] > arr[j])
                    swap(&arr[j - h], &arr[j]);
                else
                    break;
}
```
![](shell.png)

堆排序：
```c
void heap(int arr[], int dad, int end)
{
    int son = dad * 2 + 1;
    while (son <= end)
    {
        if (son < end && arr[son] < arr[son + 1])
            son++;
        if (arr[dad] < arr[son])
        {
            swap(&arr[dad], &arr[son]);
            dad = son;
            son = dad * 2 + 1;
        }
        else
            return;
    }
}

void heap_sort(int arr[], int len)
{
    for (int i = len / 2 - 1; i >= 0; i--)
        heap(arr, i, len - 1);
    for (int i = len - 1; i > 0; i--)
        swap(&arr[0], &arr[i]), heap(arr, 0, i - 1);
}
```
![](heap.png)

快速排序：
```c
void quick(int arr[], int l, int r)
{
    if (l >= r)
        return;
    int mid = arr[(l + r) / 2], i = l, j = r;
    while (i <= j)
    {
        while (arr[i] < mid)
            i++;
        while (arr[j] > mid)
            j--;
        if (i <= j)
            swap(&arr[i], &arr[j]), i++, j--;
    }
    quick(arr, l, j);
    quick(arr, i, r);
}

void quick_sort(int arr[], int len)
{
    quick(arr, 0, len - 1);
}
```
![](quick.png)

归并排序：
```c
void merge(int arr[], int tmp[], int l, int r)
{
    if (l >= r)
        return;
    int m = (l + r) / 2;
    merge(arr, tmp, l, m);
    merge(arr, tmp, m + 1, r);
    int i = l, j = m + 1, k = l;
    while (i <= m && j <= r)
        tmp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
    while (i <= m)
        tmp[k++] = arr[i++];
    while (j <= r)
        tmp[k++] = arr[j++];
    for (i = l; i <= r; i++)
        arr[i] = tmp[i];
}

void merge_sort(int arr[], int len)
{
    int tmp[len];
    merge(arr, tmp, 0, len - 1);
}
```

计数排序：
```c
void counting_sort(int arr[], int len)
{
    int max = arr[0], min = arr[0];
    for (int i = 1; i < len; i++)
        if (arr[i] > max)
            max = arr[i];
        else if (arr[i] < min)
            min = arr[i];
    int ran = max - min + 1;
    int tmp[ran];
    for (int i = 0; i < ran; i++)
        tmp[i] = 0;
    for (int i = 0; i < len; i++)
        tmp[arr[i] - min]++;
    for (int i = 0, j = 0; i < ran; i++)
        while (tmp[i]--)
            arr[j++] = i + min;
}
```

基数排序：
```c
void radix_sort(int arr[], int len)
{
    int max = arr[0];
    for (int i = 1; i < len; i++)
        if (arr[i] > max)
            max = arr[i];
    int tmp[len];
    for (int exp = 1; max / exp > 0; exp *= 10)
    {
        int cnt[10] = {0};
        for (int i = 0; i < len; i++)
            cnt[(arr[i] / exp) % 10]++;
        for (int i = 1; i < 10; i++)
            cnt[i] += cnt[i - 1];
        for (int i = len - 1; i >= 0; i--)
            tmp[--cnt[(arr[i] / exp) % 10]] = arr[i];
        for (int i = 0; i < len; i++)
            arr[i] = tmp[i];
    }
}
```

桶排序：
```c
void bucket_sort(int arr[], int len)
{
    int max = arr[0], min = arr[0];
    for (int i = 1; i < len; i++)
        if (arr[i] > max)
            max = arr[i];
        else if (arr[i] < min)
            min = arr[i];
    const int num = 10;
    int gap = (max - min) / num + 1;
    int bkt[num][len], cnt[num] = {0};
    for (int i = 0; i < len; i++)
    {
        int n = (arr[i] - min) / gap;
        bkt[n][cnt[n]++] = arr[i];
    }
    for (int k = 0, i = 0; i < num; i++)
    {
        insertion_sort(bkt[i], cnt[i]);
        for (int j = 0; j < cnt[i]; j++)
            arr[k++] = bkt[i][j];
    }
}
```
