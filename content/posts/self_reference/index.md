---
title: Self Reference
date: 2025-04-01
description: From Gödel to philosophy
language: zh
---

![](herta.webp)
*「我」的诞生*

---

## 哥德尔不完备定理

任何自洽的形式系统，只要蕴涵皮亚诺算术公理，就可以在其中构造在体系中不能被证明的真命题，因此通过推理演绎不能得到所有真命题（即体系是不完备的）。

首先给出 12 个基本符号：

| Constant sign | Gödel number |  Usual Meaning   |
| :-----------: | :----------: | :--------------: |
|    $\sim$     |      1       |       not        |
|    $\lor$     |      2       |        or        |
|   $\supset$   |      3       |     if…then…     |
|   $\exists$   |      4       |   there is an…   |
|      $=$      |      5       |      equals      |
|      $0$      |      6       |       zero       |
|      $s$      |      7       | the successor of |
|      $($      |      8       | punctuation mark |
|      $)$      |      9       | punctuation mark |
|      $,$      |      10      | punctuation mark |
|      $+$      |      11      |       plus       |
|   $\times$    |      12      |      times       |

其他符号可以由以上 12 个符号表示，例如 $p\land q$ 可以表示为 $\sim(\sim p \lor \sim q)$

用字母表示变量符号，对应的哥德尔数为大于 12 的素数，例如 x，y，z 的哥德尔数为 13，17，19，以此类推。

举例 $\sim(0=0)$，其表示“零不等于零”，此公式可以写为 $2^1\times 3^8\times 5^6 \times 7^5 \times 11^6 \times 13^9$，对应一个哥德尔数。也可以写为 $sss\dots sss0$，其中的 $s$ 有很多个。

举例 $(\exists x)(x\times ss0=sss\dots sss0)$ $\land$ $\sim(\exists x)(x\times ssss0=sss\dots sss0)$，其表示“公式 $\sim(0=0)$ 的第一个符号是波浪号”，即公式第一个素数 2 的的指数为波浪号 1，即 $2^1\times 3^8\times 5^6 \times 7^5 \times 11^6 \times 13^9$ 的哥德尔数能被 2 整除而不能被 4 整除，以上整体对应一个哥德尔数。

由此任何可以被构造的公式都有自己的哥德尔数。这样的构造妙在可以把公式本身的哥德尔数带入到公式中。

考虑公式 $(\exists x)(x=sy)$，其表示“存在一个变量 $x$ 使得它是 $y$ 的后继”，设 $m$ 为公式的哥德尔数。

用符号 $m$ 替代 $y$，得到新公式 $(\exists x)(x=sm)$，记 $\text{sub}(m,m,17)$ 为新公式的哥德尔数，第一个数 $m$ 指其对应的公式，找到其中符号 $y$ 的位置替换为第二个数 $m$。

考虑“无法证明哥德尔数为 $\text{sub}(y,y,17)$ 的公式”，设 $n$ 为公式的哥德尔数。

再将 $y$ 替换为 $n$ 创建新公式，即“无法证明哥德尔数为 $\text{sub}(n,n,17)$ 的公式”，将公式称为 G，G 的哥德尔数为 $\text{sub}(n,n,17)$。

如果体系具有一致性，假设 G 是假命题，那“哥德尔数为 $\text{sub}(n,n,17)$ 的公式”就是真命题，即 G 是真命题，矛盾，故 G 是真命题。

所以 G 是一个无法被判定的真命题。
