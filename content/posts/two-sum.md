---
title: "Two Sum (두 수의 합)"
date: 2024-01-01
draft: false
tags: ["python", "dictionary", "hash-map", "leetcode"]
categories: ["coding"]
---

## 예제 문제

> **"숫자 리스트에서 두 수의 합이 target이 되는 인덱스 쌍을 반환하라"**

```
nums = [2, 7, 11, 15], target = 9 → [0, 1]
```

---

## 1단계 — 문제 어구 → 코드 연결

막막할 때는 문제 어구를 주석으로 먼저 써보세요.

```python
def two_sum(nums, target):

    # "이미 나온 숫자를 기억"

    # "숫자를 하나씩 보면서"

        # "두 수의 합이 target" → 필요한 나머지 계산

        # "나머지가 이미 있으면" → 인덱스 쌍을 반환

        # "없으면 현재 숫자 기억"

    return []
```

> **주석으로 흐름을 먼저 잡고, 코드로 채워나가는 것**

---

## 2단계 — 어구에서 코드 실마리 찾기

```
"숫자 리스트에서"
→ def two_sum(nums, target):

"두 수의 합이 target"
→ num1 + num2 == target
→ num2 = target - num1   (역산)

"인덱스 쌍을 반환"
→ return [index1, index2]

"이미 나온 숫자를 기억"
→ seen = {}  # 숫자:인덱스
```

---

## 3단계 — 브루트 포스

직관 그대로 모든 쌍을 다 더해보는 방법:

```python
def two_sum(nums, target):
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):   # 모든 쌍을 다 확인
            if nums[i] + nums[j] == target:
                return [i, j]
    return []
```

낭비가 보여요:

> **"매번 전체를 훑으며 짝을 찾고 있다"** → O(n²)

---

## 4단계 — 최적화 아이디어

**낭비를 없애는 질문:**

> **"전체를 훑지 않고, 필요한 값이 있는지 바로 알 수 없나?"**

```
num=2 를 보고: "9가 되려면 7이 필요해. 이미 나왔나?"
→ 지나온 숫자를 딕셔너리에 저장해두면 바로 확인 가능!
```

---

## 5단계 — 딕셔너리 구조

```
"어떤 숫자가" → "어떤 인덱스에 있는지"

seen = { 숫자(key) : 인덱스(value) }
seen = { 2:0, 7:1, 11:2 }
```

> **두 정보를 연결해서 기억해야 할 때 → 딕셔너리**

---

## 6단계 — enumerate() 란?

인덱스와 값을 동시에 꺼내주는 함수예요:

```python
nums = [2, 7, 11, 15]

# enumerate 없이
for i in range(len(nums)):
    num = nums[i]        # 인덱스로 값을 꺼냄

# enumerate 있이
for i, num in enumerate(nums):
    # i   = 인덱스
    # num = 값
```

인덱스를 저장하고 반환해야 하니까 `enumerate` 가 필요해요.

---

## 7단계 — 완성 코드

```python
def two_sum(nums, target):

    # "이미 나온 숫자를 기억"
    seen = {}   # { 숫자(key): 인덱스(value) }

    for i, num in enumerate(nums):

        # "두 수의 합이 target" → 역산
        need = target - num

        # "나머지가 이미 있으면"
        if need in seen:

            # "인덱스 쌍을 반환"
            return [seen[need], i]

        # "없으면 현재 숫자 기억"
        seen[num] = i

    return []
```

---

## 8단계 — 머릿속 추적

```
nums = [2, 7, 11, 15], target = 9

i=0, num=2: need=7,  seen={}      → 없음 → seen={2:0}
i=1, num=7: need=2,  seen={2:0}   → 있음! → return [0, 1] ✓
```

---

## 시간복잡도 & 공간복잡도

```
시간복잡도: O(n)   → 리스트를 한 번만 훑음
공간복잡도: O(n)   → seen에 최대 n개 저장
```

브루트 포스와 비교:

| | 시간복잡도 | 공간복잡도 |
|---|---|---|
| 브루트 포스 | O(n²) | O(1) |
| 딕셔너리 | O(n) | O(n) |

---

## 코딩 연습

### 1차 시도 — 주석 힌트

```python
def two_sum(nums, target):

    # 숫자:인덱스 를 기억할 딕셔너리
    seen = ___________

    for i, num in enumerate(nums):

        # 필요한 나머지
        need = ___________

        # 나머지가 이미 있으면
        if need in seen:
            return ___________

        # 현재 숫자 저장
        seen[___________] = ___________

    return []
```

---

### 2차 시도 — 힌트 줄임

```python
def two_sum(nums, target):

    seen = ___________

    for ___________, ___________ in enumerate(nums):
        need = ___________

        if ___________ in seen:
            return ___________

        seen[___________] = ___________

    return []
```

---

### 3차 시도 — 백지에서 작성

문제 어구를 먼저 주석으로 쓰고, 코드로 채워보세요:

```python
def two_sum(nums, target):

    # 이미 나온 숫자를 기억

    # 숫자를 하나씩 보면서

        # 필요한 나머지 계산

        # 나머지가 이미 있으면 반환

        # 없으면 현재 숫자 기억

    return []
```

---

## 사고 흐름 요약

```
문제 어구 읽기
        ↓
어구를 주석으로 먼저 쓰기
        ↓
브루트 포스 → "매번 전체를 훑음" 낭비 발견
        ↓
"지나온 숫자를 기억해두자"
        ↓
딕셔너리 { 숫자: 인덱스 }
        ↓
need = target - num  역산
        ↓
완성 → O(n)
```

> **막막할 때는 문제 어구를 주석으로 먼저 써보세요.**
> **주석이 코드의 뼈대가 돼요.**
