---
title: 파이썬 코딩테스트시 유용한 코드 템플릿
date: 2022-02-08 20:50:00 +0900
categories: 알고리즘
---


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=true} -->

<!-- code_chunk_output -->

1. [소개](#소개)
2. [deque 한번에 선언하기](#deque-한번에-선언하기)
3. [defaultdict : dict에 KeyError 대신 딕셔너리 아이템 생성](#defaultdict-dict에-keyerror-대신-딕셔너리-아이템-생성)
4. [Counter : 키에는 아이템의 값, 값에는 아이템의 개수인 딕셔너리 출력](#counter-키에는-아이템의-값-값에는-아이템의-개수인-딕셔너리-출력)
5. [Counter.most_common(i) : 가장 빈도수가 높은 요소 i개 출력](#countermost_commoni-가장-빈도수가-높은-요소-i개-출력)
6. [isalnum: 영문, 숫자 판별](#isalnum-영문-숫자-판별)
7. [정규표현식으로 영문 숫자만 잘라내기](#정규표현식으로-영문-숫자만-잘라내기)
8. [문자 뒤집기](#문자-뒤집기)
9. [람다로 정렬](#람다로-정렬)
10. [이중 리스트 컴프리헨션](#이중-리스트-컴프리헨션)
11. [애너그램 구하기](#애너그램-구하기)
12. [key에 함수 넣기](#key에-함수-넣기)

<!-- /code_chunk_output -->

---

# 소개

 [파이썬 알고리즘 인터뷰](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189909178) 에 대한 내용 중 구현하는 문제에서 유용하게 사용할 수 있는 코드 템플릿을 정리하였습니다.

---

# deque 한번에 선언하기

```python
bridge = deque(0 for _ in range(bridge_length))
```

# defaultdict : dict에 KeyError 대신 딕셔너리 아이템 생성

```python
import collections

a = collections.defaultdict(int)

a['A'] = 5
a['B'] = 4

a['C'] += 1

print(a)

# OUTPUT
defaultdict(<class 'int'>, {'A': 5, 'B': 4, 'C': 1})
```

# Counter : 키에는 아이템의 값, 값에는 아이템의 개수인 딕셔너리 출력

```python

import collections

a = [1, 2, 3, 4, 5, 5, 5, 6, 6]
b = collections.Counter(a)

print(b)

# OUTPUT
Counter({5: 3, 6: 2, 1: 1, 2: 1, 3: 1, 4: 1})
```

# Counter.most_common(i) : 가장 빈도수가 높은 요소 i개 출력

```python
import collections

a = [1, 2, 3, 4, 5, 5, 5, 6, 6]
b = collections.Counter(a)

print(b.most_common(3))

# OUTPUT
[(5, 3), (6, 2), (1, 1)]

```

# isalnum: 영문, 숫자 판별

```python
for char in s:
    if char.isalnum():
        print(char)
```

# 정규표현식으로 영문 숫자만 잘라내기

```python
s = re.sub('[^a-z0-9]', '', s)
```

# 문자 뒤집기

```python
s == s[::-1]
```

# 람다로 정렬

```python
letters.sort(key=lambda x: (x.split()[1:], x.split()[0]))
```

x를 공백 기준으로 split 해서 `[1:]`로 정렬하고, 동일하면 후 순위로 `[0]`으로 정렬

# 이중 리스트 컴프리헨션

```python
import re

paragraph = 'Bob hit a ball, the hit Ball flew far after it was hit'

banned = ['hit']

words = [word for word in re.sub(r'[^\w]', ' ', paragraph)
         .lower().split() if word not in banned]

print(words)

#OUTPUT
['bob', 'a', 'ball', 'the', 'ball', 'flew', 'far', 'after', 'it', 'was']

```

# 애너그램 구하기

```python
import collections

strs = ['eat', 'tea', 'tan', 'ate', 'nat', 'bat']

anagrams = collections.defaultdict(list)

for word in strs:
    # 정렬하여 딕셔너리에 추가
    anagrams[''.join(sorted(word))].append(word)

print(list(anagrams.values()))

# OUTPUT
[['eat', 'tea', 'ate'], ['tan', 'nat'], ['bat']]
```

# key에 함수 넣기

```python
a = ['cae', 'cfc', 'abc']


def fn(s):
    return s[1], s[-1]


print(sorted(a, key=fn))

```
