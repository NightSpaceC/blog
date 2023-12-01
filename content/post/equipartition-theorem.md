---
author: "Night Space"
title: 能均分原理
date: 2023-11-30T23:20:00+08:00
lastmod: 2023-12-01T22:40:00+08:00
tags:
    - 物理
    - 统计物理学
categories:
    - 物理
    - 统计物理学
---
令 $q_i$ 为 $n$ 个简正坐标及相应的广义动量，则

$$ \epsilon = \sum _ {i = 1} ^ {n} a_i ^ 2 q_i ^ 2 $$

显然，当 $a_i = 0$ ， $\left< a_i ^ 2 q_i ^ 2 \right> = 0$

以下假设 $a_i \neq 0$

由玻尔兹曼系统最概然分布

$$ f(\epsilon) = \frac {C ^ \epsilon g(\epsilon)} {\int _ 0 ^ {+ \infin} C ^ {\epsilon ^ \prime} g(\epsilon ^ \prime) \mathrm {d} \epsilon ^ \prime} $$

其中

$$ C = C(g ; \overline \epsilon) $$

设 $n - 1$ 维球体

$$ \sum _ {j \in \left[ 1 , n \right] \cap \mathbb {N} 且 j \neq i} a_j ^ 2 q_j ^ 2 = R ^ 2 $$

体积为

$$ V = f_V R ^ {n - 1} $$

$$ f_V = f_V(a_1 , ... , a_{i - 1}, a_{i + 1}, ... , a_n) $$

所以

$$ g(\epsilon) = \int _ {- \frac {\sqrt {\epsilon}} {a_i}} ^ {\frac {\sqrt {\epsilon}} {a_i}} f_V \sqrt {\epsilon - a_i ^ 2 q_i ^ 2} ^ {n - 1} \mathrm {d} q_i $$

化简得

$$ g(\epsilon) = \frac {2 f_V} {a_i} \int _ 0 ^ {\sqrt {\epsilon}} \sqrt {\epsilon - t ^ 2} ^ {n - 1} \mathrm {d} t $$

其中进行了第一类换元积分

$$ t = a_i q_i $$

所以

$$ f(\epsilon) = \frac{C ^ \epsilon \int _ 0 ^ {\sqrt {\epsilon}} \sqrt {\epsilon - t ^ 2} ^ {n - 1} \mathrm {d} t}{\int _ 0 ^ {+ \infin} C ^ {\epsilon ^ \prime} \int _ 0 ^ {\sqrt {\epsilon ^ \prime}} \sqrt {{\epsilon ^ \prime} - t ^ 2} ^ {n - 1} \mathrm {d} t \mathrm {d} \epsilon ^ \prime} $$

由贝叶斯公式

$$ f(\epsilon, q_i) = f(\epsilon) \cdot \frac {f_V \sqrt {\epsilon - a_i ^ 2 q_i ^ 2} ^ {n - 1}} {g(\epsilon)} $$

得

$$ f(\epsilon, q_i) = \frac{a_i C ^ \epsilon \sqrt {\epsilon - a_i ^ 2 q_i ^ 2} ^ {n - 1}}{2 \int _ 0 ^ {+ \infin} C ^ {\epsilon ^ \prime} \int _ 0 ^ {\sqrt {\epsilon ^ \prime}} \sqrt {{\epsilon ^ \prime} - t ^ 2} ^ {n - 1} \mathrm {d} t \mathrm {d} \epsilon ^ \prime} $$

由全概率公式

$$ f(q_i) = \int _ {a_i ^ 2 q_i ^ 2} ^ {+ \infin}f(\epsilon, q_i) f(\epsilon) \mathrm {d} \epsilon $$

得

$$ f(q_i) = \frac {a_i} {2 (\int _ 0 ^ {+ \infin} C ^ {\epsilon ^ \prime} \int _ 0 ^ {\sqrt {\epsilon ^ \prime}} \sqrt {{\epsilon ^ \prime} - t ^ 2} ^ {n - 1} \mathrm {d} t \mathrm {d} \epsilon ^ \prime) ^ 2 } \int _ {a_i ^ 2 q_i ^ 2} ^ {+ \infin} C ^ {2 \epsilon} \sqrt {\epsilon - a_i ^ 2 q_i ^ 2} ^ {n - 1} \int _ 0 ^ {\sqrt {\epsilon}} \sqrt {\epsilon - t ^ 2} ^ {n - 1} \mathrm {d} t \mathrm {d} \epsilon $$

由数学期望定义

$$ \left< a_i ^ 2 q_i ^ 2 \right> = \int _ {- \infin} ^ {+ \infin} a_i ^ 2 q_i ^ 2 f(q_i) \mathrm {d} q_i $$

所以

$$ \left< a_i ^ 2 q_i ^ 2 \right> = \frac {1} {(\int _ 0 ^ {+ \infin} C ^ {\epsilon ^ \prime} \int _ 0 ^ {\sqrt {\epsilon ^ \prime}} \sqrt {{\epsilon ^ \prime} - t ^ 2} ^ {n - 1} \mathrm {d} t \mathrm {d} \epsilon ^ \prime) ^ 2 } \int _ 0 ^ {+ \infin} {t ^ \prime} ^ 2 \int _ {{t ^ \prime} ^ 2} ^ {+ \infin} C ^ {2 \epsilon} \sqrt {\epsilon - a_i ^ 2 q_i ^ 2} ^ {n - 1} \int _ 0 ^ {\sqrt {\epsilon}} \sqrt {\epsilon - t ^ 2} ^ {n - 1} \mathrm {d} t \mathrm {d} \epsilon \mathrm {d} {t ^ \prime} $$

其中进行了第一类换元积分

$$ t ^ \prime = a_i q_i $$

所以

$$ \forall a_i \neq 0 , \left< a_i ^ 2 q_i ^ 2 \right> = \mathrm {const} $$
