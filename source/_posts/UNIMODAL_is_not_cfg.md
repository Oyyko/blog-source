---
title: The language UNIMODAL is not context free
date: 2021-10-06
tag: 
- Computation_Theory
- Context_Free_Grammar
category: Computation_Theory
mathjax: true
---
In this article, we will prove that: the language 'UNIMODAL' is not cfg. Also, we will prove that cfg can not be used to compare binary number.
<!--more-->
## proposition one:"BINARY EQUAL"：
Alphabet：{S,0,1}
language BINARY_EQUAL：string such as $\{(0|1)^* S (0|1)^*\}$，AND the binary number before '#' is equal to the the latter one.
statement： BINARY_EQUAL is not context free grammar

Prove: Assume that BINARY_EQUAL is cfl(context free language), then use the pump lemma. For a string s="xxxx...xxxx#yyyy...yyyy", $\exists uvwxy, \text{such that} \ s=uvwxy\ ,\text{and}\ \ \forall i\ge 0, uv^iwx^iy\in \mathbb{BINARY\_EQUAL} $. 

We choose a special string: $s=1^k0^kS0^k1^k$, so when k is large enough, the v and x must be different. Hence, the pumped string is not BINARY_EQUAL. So the proposition is right.

## proposition two:"BINARY BIGGER":
Alphabet：{S,0,1}
language BINARY_EQUAL：string such as $\{(0|1)^* S (0|1)^*\}$，AND the binary number before '#' is **BIGGER** to the the latter one.
statement： BINARY_BIGGER is not context free grammar

Prove: Assume that BINARY_BIGGER is cfl, use the pump lemma for the string $1^k0^{k-1}1S1^k0^k$. 

If v and x are both in the left number, the pump down it.(pump down means that we choose the $i$ in the pump lemma as 0. a.k.a $i=0$) then the digits of left num is 2k-2, while the right is 2k, so the right number is bigger than the left.

If v and x are both in the right number, just choose i as much as you need. Then the right is obvious larger than the left.

If v and x are in different sides:
(1) If v is '1' and x is '1',pump down it. Then two num should be equal
(2) If v is '00...1', and x is '11...1' then pump up it and the right num should be larger.

So, the proposition is true.

## proposition three:"UNIMODAL":
Alphabet：{S,0,1}
language UNIMODAL：string such as $\{(0|1)^* S (0|1)^*(S(0|1)^*)^*\}$. This string will be considered as an array of binary number. And when and only when the array is UNIMODAL. The string belongs to the language.

For example: 1 3 5 3 1 is a unimodal array. The form in binary is 1#11#101#11#1. So the string '1#11#101#11#1' is unimodal.
statement： UNIMODAL is not context free grammar

Prove: use pump lemma. s=uvwxy. Let s be '$1^K0^KS1^K0^KS1^K0^KS1^K0^KS1^K0^K$', Actually, we need $1^K0^KS1^K0^K(S1^K0^K)^k$. Because every num in this array is equal. It's a UNIMODAL string. And if v and x belongs to one num, then pump down it. We get a decrease then increase, hence a controdiction.

And if v and x belongs to different num. Then v is '000...', x is '111...',(because we let the k be large enough). Hence,we pump down, we we will get a decrease and then increase and then decrease. Controdiction!



