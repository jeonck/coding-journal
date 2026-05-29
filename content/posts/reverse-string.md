---
title: "코딩 사고 흐름 — 문자열 뒤집기 (Reverse String)"
date: 2024-01-01
draft: false
tags: ["python", "stack", "string", "leetcode"]
categories: ["coding"]
---

## 예제 문제

> **"문자열을 뒤집어라"**

```
"hello" → "olleh"
```

---

## 1단계 — 입출력 계약

```
입력: 문자열 "hello"
출력: 뒤집힌 문자열 "olleh"
```

함수 뼈대:

```python
def reverse_string(s):

    return
```

---

## 2단계 — 손으로 풀기

`"hello"` 를 손으로 뒤집는다면?

```
h e l l o
→ 뒤에서부터 읽으면
o l l e h
```

문자열을 리스트로 보면:

```python
["h", "e", "l", "l", "o"]
```

> **"스택에 넣고 하나씩 꺼내면 자연스럽게 뒤집힌다"**

스택은 가장 나중에 넣은 것이 가장 먼저 나오기 때문이에요.

---

## 3단계 — 스택으로 풀기

```
"hello" 를 하나씩 스택에 넣으면:

h → stack = ["h"]
e → stack = ["h", "e"]
l → stack = ["h", "e", "l"]
l → stack = ["h", "e", "l", "l"]
o → stack = ["h", "e", "l", "l", "o"]

하나씩 꺼내면:
pop → "o"
pop → "l"
pop → "l"
pop → "e"
pop → "h"
→ "olleh" ✓
```

---

## 4단계 — 완성 코드

```python
def reverse_string(s):

    stack = []

    # 1. 문자열을 하나씩 스택에 넣기
    for char in s:
        stack.append(char)

    # 2. 스택에서 하나씩 꺼내서 결과에 붙이기
    result = ""
    while stack:            # 스택이 빌 때까지
        result += stack.pop()

    return result
```

---

## 5단계 — 머릿속 추적

```
s = "hello"

넣기:
stack = ["h", "e", "l", "l", "o"]

꺼내기:
result = "" + "o" = "o"
result = "o" + "l" = "ol"
result = "ol" + "l" = "oll"
result = "oll" + "e" = "olle"
result = "olle" + "h" = "olleh"

→ "olleh" ✓
```

---

## 코드 핵심 포인트

**`stack.append(char)`**  
문자를 하나씩 스택에 추가.

**`while stack:`**  
스택이 빌 때까지 반복. 스택에 뭔가 남아있으면 True, 비었으면 False.

**`result += stack.pop()`**  
스택에서 마지막 것을 꺼내서 result 뒤에 붙임.

---

## Python 다양한 풀이 비교

세 가지 방법 모두 정답이에요:

```python
# 방법 1 - 스택 (사고 흐름이 가장 명확)
def reverse_string(s):
    stack = []
    for char in s:
        stack.append(char)
    result = ""
    while stack:
        result += stack.pop()
    return result

# 방법 2 - 슬라이싱
def reverse_string(s):
    return s[::-1]

# 방법 3 - 내장함수
def reverse_string(s):
    return "".join(reversed(s))
```

---

## 슬라이싱 `s[::-1]` 이란?

슬라이싱 문법:

```python
s[ 시작 : 끝 : 간격 ]
```

시작과 끝을 비워두면 전체를 의미하고, 간격이 `-1` 이면 뒤에서부터를 의미해요:

```python
s = "hello"

s[::1]   # 처음부터 끝까지, 1칸씩 앞으로  → "hello"
s[::-1]  # 처음부터 끝까지, 1칸씩 뒤로    → "olleh"
s[::2]   # 1칸씩 건너뜀                   → "hlo"
```

> **`s[::-1]` 은 "문자열 전체를 뒤에서부터 1칸씩 가져와라"**

---

## 사고 흐름 요약

```
손으로 풀기        → "뒤에서부터 읽으면 된다"
        ↓
자료구조 선택      → 스택 (나중에 넣은 것이 먼저 나옴)
        ↓
넣기 → 꺼내기      → 자연스럽게 뒤집힘
        ↓
완성
```

> 방법 1로 직접 사고하고 구현할 수 있으면, 방법 2, 3은 자연스럽게 따라와요.  
> 면접에서는 **사고 흐름을 설명할 수 있는 것** 이 훨씬 중요해요.