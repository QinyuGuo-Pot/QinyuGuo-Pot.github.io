﻿---
layout: post
title: matlab的mandist函数
category: others
date: 2019-07-20
---
* 目录
{:toc #markdown-toc}

mandist(A,B)函数是用来求A中的每个行向量与B中的每个列向量的绝对距离

# EG 1 ：
a = [1,2,3]    b = [-1,5,6]    c = [1,0,1]\
mandist(a,b') = 8

解释：\
（1）b' 表示 b矩阵的转置\
（2）要求：mandist两个参数表示两个矩阵，第一个矩阵的列数 = 第二个矩阵的行数\
（3）计算：|1-（-1）| + |2-5| + |3-6| = 8\
（4）维数：行数 = 第一个矩阵的行数，列数 = 第二个矩阵的列数

练习 ：\
mandist(a,c') = |1-1| + |2-0| + |3-1| = 4\
mandist(c,b') = |1-(-1)| + |0-5| + |1-6| = 12

# EG 2 ：
A = \
[1,2,3\
4,5,6]\
mandist(A,A') =\
[0,9\
9,0]

解释：\
（1）维数：A(2,3) , A'(3,2) 

A' = \
[1,4\
2,5\
3,6]

（2）令x = mandist(A,A')\
（3）计算：\
x(1,1) = |1-1| + |2-2| +|3-3| = 0\
x(1,2) = |1-4| + |2-5| +|3-6| = 9\
x(2,1) = |4-1| + |5-2| +|6-3| = 9\
x(2,2) = |4-4| + |5-5| +|6-6| = 0

练习：\
y = mandist(A',A) = \
[0,2,4\
2,0,2\
4,2,0]\
计算：\
y(1,1) = |1-1| + |4-4| = 0\
y(1,2) = |1-2| + |4-5| = 2\
y(1,3) = |1-3| + |4-6| = 4\
y(2,1) = ....

补充\
mandist(A) = mandist(A',A)\
mandist(A') = mandist(A,A')




