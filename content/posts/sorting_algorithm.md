---
title: Sorting Algorithm
description: 排序算法
---

排序算法作为最基本的算法之一，其重要性不言而喻，时至今日仍在不断优化改进。

2023 年 6 月 8 日，Google DeepMind 新作 AlphaDev 发现了更快的排序算法，同时相关文章 [Faster sorting algorithms discovered using deep reinforcement learning](https://www.nature.com/articles/s41586-023-06004-9) 在 Nature 发表。

有趣的是不久后 Dimitris Papailiopoulos 发推 [GPT-4 "discovered" the same sorting algorithm as AlphaDev by removing "mov S P"](https://twitter.com/DimitrisPapail/status/1666843952824168465) 表示通过合适的提示词，GPT-4 也能得到同样结果，还引得马斯克围观，经过试验我发现 GPT-4 确实能够发现正确答案。

排序算法的极限时间复杂度为nlogn，C++ STL 中 std::sort 采用的是 David Musser 提出的混合排序算法，主要使用快速排序，同时记录递归深度，看情况转为堆排序，此外对短数据采用其他排序算法。

LLVM 作为被广泛使用的 C++ 编译后端，其 libcxx 对于短数据采用手写 sort3 sort4 sort5 的形式，AlphaDev 就是对它们的汇编代码进行了改进，相关 [Commit](https://github.com/llvm/llvm-project/commit/194d1965d2c841fa81e107d19e27fae1467e7f11) 已被合并。

AlphaDev 方法与 AlphaTensor 的方法 [Discovering faster matrix multiplication algorithms with reinforcement learning](https://www.nature.com/articles/s41586-022-05172-4) 类似，都是用强化学习解组合优化问题，根据计算耗时和结果正确性来评估奖励，进行迭代优化。

附 sort3 排序网络：

![](1.webp)

- mov: move
- cmp: compare
- cmovg: conditional move if greater
- cmovl: conditional move if less

![](2.webp)

附 GPT-4 原回答：

Let's go through the code step by step, analyzing what each instruction does, and making some observations that might be helpful for future optimization. At this stage, I won't make any recommendations for removal or changes, I'll just analyze the purpose of each instruction.

```
Memory[0] = A
Memory[1] = B
Memory[2] = C
```
These lines just indicate that there are three values stored in memory locations 0, 1, and 2. They represent the initial state of the memory, and not instructions in assembly language.

```
mov Memory[0] P
mov Memory[1] Q
mov Memory[2] R
```
These instructions move the values stored in memory locations 0, 1, and 2 into registers P, Q, and R respectively. It's a common practice to load data into registers for faster access and manipulation.

```
mov R S
```
This instruction copies the value of register R into register S. At this point, both R and S contain the value of C (Memory[2]). This might be useful for comparisons that are coming next.

```
cmp P R
```
This instruction compares the values in registers P and R (A and C). It sets the status flags according to the result of the comparison (P - R).

```
cmovg P R
```
This instruction moves the value from P to R if the result of the last comparison was such that P > R (i.e., A > C). This effectively stores the maximum of A and C in R.

```
cmovl P S
```
This instruction moves the value from P to S if the result of the last comparison was such that P < R (i.e., A < C). This effectively stores the minimum of A and C in S.

```
mov S P
```
This instruction moves the value from S to P. Since S holds the minimum of A and C after the previous instruction, P now also holds the minimum of A and C.

```
cmp S Q
```
This instruction compares the values in registers S and Q (min(A, C) and B). It sets the status flags according to the result of the comparison (S - Q).

```
cmovg Q P
```
This instruction moves the value from Q to P if the result of the last comparison was such that S < Q (i.e., min(A, C) < B). This effectively stores the minimum of A, B, and C in P.

```
cmovg S Q
```
This instruction moves the value from S to Q if the result of the last comparison was such that S > Q (i.e., min(A, C) > B). This effectively stores the maximum of min(A, C) and B in Q.

```
mov P Memory[0]
```
This instruction moves the value from register P back to memory location 0. Since P holds the minimum of A, B, and C, Memory[0] will now hold this value.

```
mov Q Memory[1]
```
This instruction moves the value from register Q back to memory location 1. Since Q holds the maximum of min(A, C) and B, Memory[1] will now hold this value.

```
mov R Memory[2]
```
This instruction moves the value from register R back to memory location 2. Since R holds the maximum of A and C, Memory[2] will now hold this value.

At this stage, we've analyzed each instruction and its role in the sorting algorithm. There are potential areas where optimizations might be applied, but we need to be careful as the instructions are interdependent.

Let's move on to step two, focusing on optimization:

```
mov Memory[0] P
mov Memory[1] Q
mov Memory[2] R
```
These lines are essential for loading values from memory.

```
mov R S
```
This line is necessary to have an additional register (S) for comparison and swapping.

```
cmp P R
```
This line is essential for comparing the values in P and R.

```
cmovg P R
```
This line is essential for swapping the values if P > R, ensuring R will hold the max(A, C).

```
cmovl P S
```
This line is essential for swapping the values if P < R, ensuring S will hold the min(A, C).

```
*** mov S P
```
This instruction can be optimized out. We know that after the previous two instructions, S holds the minimum of A and C. Instead of moving it to P, we can directly use S in the following comparisons, saving one instruction.

```
cmp S Q
```
This line is essential for comparing the values in S and Q.

```
cmovg Q P
```
This line is essential for finding the minimum among A, B, and C.

```
cmovg S Q
```
This line is essential for determining the middle value.

```
mov P Memory[0]
mov Q Memory[1]
mov R Memory[2]
```
These lines are essential for storing the sorted values back into memory.

To summarize, we found that the instruction "mov S P" can be removed. The rest of the instructions are necessary for the algorithm to function correctly. However, in the lines following the removal, P should be replaced with S to maintain correctness.
