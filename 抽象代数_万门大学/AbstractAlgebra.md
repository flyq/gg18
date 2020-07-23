[TOC]

# 抽象代数 Abstract Algebra


## Contents
- [抽象代数 Abstract Algebra](#抽象代数-abstract-algebra)
  - [Contents](#contents)
- [1. 集合的定义](#1-集合的定义)
  - [定义1：包含$\subseteq$](#定义1包含math-xmlnshttpwwww3org1998mathmathmlsemanticsmrowmomomrowannotation-encodingapplicationx-texsubseteqannotationsemanticsmath)
  - [定义2：相等$=$](#定义2相等math-xmlnshttpwwww3org1998mathmathmlsemanticsmrowmomomrowannotation-encodingapplicationx-texannotationsemanticsmath)
  - [定理1：](#定理1)
  - [定理2：](#定理2)
- [2. 集合的运算](#2-集合的运算)
  - [定义3：并集$\cup$](#定义3并集math-xmlnshttpwwww3org1998mathmathmlsemanticsmrowmomomrowannotation-encodingapplicationx-texcupannotationsemanticsmath)
  - [定义4：交集$\cap$](#定义4交集math-xmlnshttpwwww3org1998mathmathmlsemanticsmrowmomomrowannotation-encodingapplicationx-texcapannotationsemanticsmath)
  - [定理3：](#定理3)
  - [定理4：](#定理4)
  - [定理5：](#定理5)
  - [定理6：](#定理6)
  - [定义5：索引集](#定义5索引集)
  - [定理7：](#定理7)
  - [定义6：卡式积](#定义6卡式积)
  - [定义7：不交并](#定义7不交并)
  - [定义8：幂集](#定义8幂集)


# 1. 集合的定义

| 中文 | 符号 | 英文 |
| :---: | :---: | :---: |
|朴素集合论||Naive Set Theory|
|空集|$\emptyset$|Empty Set|
|空虚的真|| Vacuous Truth|
|子集| $\subseteq$ | Subset|
|属于|$\in$||
|对于所有|$\forall$||


给定一个对象，能否判断其是否属于集合？

元素，集合里面的个体

$x \in A$

整数集：$\mathbb{Z}$   
自然数集：$\mathbb N$


**所有对空集进行的全称命题都是真命题**：
$\forall x \in \varnothing, \ldots$

## 定义1：包含$\subseteq$

$$
A \subseteq B, 当且仅当 \forall x \in A, x \in B.
$$

## 定义2：相等$=$
$$
A = B, 当且仅当 A \subseteq B 且 B \subseteq A.
$$

## 定理1：
$$
\emptyset \subseteq A, A 是任意集合
$$

## 定理2：
$$
只有一个\emptyset
$$


# 2. 集合的运算

| 中文 | 符号 | 英文 |
| :---: | :---: | :---: |
| 并集 | $\cup$ ||
| 交集 | $\cap$ ||
| 补集 | $A^c$ ||
| 索引集 ||index|

## 定义3：并集$\cup$

$$
A \cup B = \{ x \in A 或者 x \in B \}
$$

## 定义4：交集$\cap$

$$
A \cap B = \{x \in A 并且 x \in B \}
$$

## 定理3：
$$
(A \cup B) \cup C = A \cup (B \cup C)
$$

## 定理4：
$$
(A \cap B) \cap C = A \cap (B \cap C)
$$

## 定理5：
$$
A \cup B = B \cup A \\
A \cap B = B \cap A
$$

## 定理6：
$$
(A \cup B) \cap C = (A \cap C) \cup (B \cap C) \\
(A \cap B) \cup C = (A \cup C) \cap (B \cup C)
$$


## 定义5：索引集
$$
\{ A_\alpha | \alpha \in I \} 其中 I 是索引集。\\
对所有的 A_\alpha 并集：\bigcup_{\alpha \in I}A_\alpha \\
\ \\
对所有的 A_\alpha 交集：\bigcap_{\alpha \in I}A_\alpha
$$

## 定理7：
德摩根定律推广：
$$
A_\alpha \subseteq U,\\
\ \\
(\bigcap_{\alpha \in I}A_\alpha)^c = \bigcup_{\alpha \in I} A_{\alpha}^c \\
\ \\
(\bigcup_{\alpha \in I}A_\alpha)^c  = \bigcap_{\alpha \in I}A_{\alpha}^c
$$

## 定义6：卡式积
定义：
$$
A \times B  = \{ (x, y) | x \in A, y \in B \}
$$
示例：
$$
\begin{aligned}
\mathbb{R} \times \mathbb{R} &= \{(x, y)|x, y \in \mathbb{R} \} \\
&\cong \mathbb{R}^2
\end{aligned}
$$

## 定义7：不交并
$$
A \sqcup B = (A \times \{0\}) \cup (B \times \{ 1\})
$$


## 定义8：幂集
定义：
$$
2^A = \{所有的子集\}
$$

示例：
$$
A = \{ 1, 2 \} \\
\ \\
2^A = \{ \varnothing, \{1 \}, \{2\}, \{1, 2 \} \} \\
\ \\
2^\emptyset = \{ \varnothing \}
$$

思考：
$$
\varnothing \times A = ? \\
A \times \varnothing = ? \\
\varnothing \times \varnothing = ?
$$





参考：
1. https://oeis.org/wiki/List_of_LaTeX_mathematical_symbols

2. https://latex.codecogs.com/eqneditor/editor.php