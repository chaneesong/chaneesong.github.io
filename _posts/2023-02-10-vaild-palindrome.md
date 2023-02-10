---
title: Vaild Palindrome
author: chaneesong
date: 2023-02-10 13:34:00 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [125. Vaild Palindrome](https://leetcode.com/problems/valid-palindrome/)

## 문제 설명

모든 대문자를 소문자로 변환하고, 영어와 숫자가 아닌 문자는 제거한 뒤 앞뒤로 읽어도 똑같은 문장은 팰린드롬이다.  
문자열 s가 주어질 때, 팰린드롬이면 true를 아니라면 false를 리턴하라.

## 해결 방법

![light mode only](/posts/valid-palindrome-image/solve.jpg)

1. 대문자를 소문자로 변환한다.
2. 영어와 숫자가 아닌 문자는 제거한다.
3. 팰린드롬인지 확인한다.
