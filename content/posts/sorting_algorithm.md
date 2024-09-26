---
title: Sorting Algorithm
description: 排序算法整理
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

顺便整理了经典排序算法，测试一下博客的代码功能。

冒泡排序：

![](bubble.png)

```c
void bubble_sort(int arr[], int len)
{
    for (int i = 0; i < len - 1; i++)
        for (int j = 0; j < len - 1 - i; j++)
            if (arr[j] > arr[j + 1])
                swap(&arr[j], &arr[j + 1]);
}
```

快速排序：

![](quick.png)

```c
void partition(int arr[], int l, int r)
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
    partition(arr, l, j);
    partition(arr, i, r);
}

void quick_sort(int arr[], int len)
{
    partition(arr, 0, len - 1);
}
```

插入排序：

![](insertion.png)

```c
void insertion_sort(int arr[], int len)
{
    for (int i = 1; i < len; i++)
    {
        int key = arr[i], j = i - 1;
        while (j >= 0 && arr[j] > key)
        {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

希尔排序：

![](shell.png)

```c
void shell_sort(int arr[], int len)
{
    int gap[] = {1, 3, 5};
    int n = sizeof(gap) / sizeof(int);
    while (n)
    {
        int h = gap[--n];
        for (int i = h; i < len; i++)
            for (int j = i; j >= h && arr[j - h] > arr[j]; j -= h)
                swap(&arr[j - h], &arr[j]);
    }
}
```

选择排序：

![](selection.png)

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

堆排序：

![](heap.png)

```c
void down(int arr[], int dad, int end)
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
        down(arr, i, len - 1);
    for (int i = len - 1; i > 0; i--)
        swap(&arr[0], &arr[i]), down(arr, 0, i - 1);
}
```
