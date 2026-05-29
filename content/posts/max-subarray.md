---
title: "코딩 사고 흐름 — 최대 부분 합 (Maximum Subarray)"
date: 2024-01-01
draft: false
tags: ["python", "dynamic-programming", "array", "leetcode"]
categories: ["coding"]
---

## 예제 문제

> **"숫자 리스트에서 연속된 부분의 합 중 가장 큰 값을 반환하라"**

```
[-2, 1, -3, 4, -1, 2, 1, -5, 4] → 6
연속된 부분: [4, -1, 2, 1]
```

---

## 1단계 — 입출력 계약

```
입력: 숫자 리스트 [-2, 1, -3, 4, -1, 2, 1, -5, 4]
출력: 연속 부분합의 최댓값 6
```

함수 뼈대:

```python
def max_subarray(nums):

    return
```

---

## 2단계 — 브루트 포스

모든 연속 구간의 합을 다 구해보는 방법:

```
[-2]            = -2
[-2, 1]         = -1
[1]             =  1
[1, -3]         = -2
[4, -1, 2, 1]   =  6  ← 최대
...
```

코드로:

```python
def max_subarray(nums):
    best = nums[0]

    for i in range(len(nums)):
        total = 0
        for j in range(i, len(nums)):   # 모든 구간을 다 계산
            total += nums[j]
            best = max(best, total)

    return best
```

낭비가 보여요:

> **"이미 계산한 합을 버리고, 매번 처음부터 다시 더하고 있다"**

---

## 3단계 — 최적화 아이디어

**낭비를 없애는 질문:**

> **"이전 합을 재사용할 수 없을까?"**

예를 들어 `[4, -1]` 의 합을 구할 때:

```
처음부터 다시:   4 + (-1) = 3
이전 합 재사용: [4] 의 합(4) + (-1) = 3  ← 똑같음!
```

그런데 문제가 생겨요:

```
이전 합(-2) + 1 = -1
그냥 1에서 새로 시작 = 1  ← 이게 더 큰데?
```

> **"이전 합이 마이너스면, 들고 가봤자 손해다"**

---

## 4단계 — 판단 기준

매 숫자마다 이 질문을 해요:

> **"이전 합을 들고 가는 게 이득인가, 여기서 새로 시작하는 게 이득인가?"**

```python
current = max(num, current + num)
#              ↑         ↑
#          새로 시작   들고 가기
```

더 큰 쪽을 선택하면 돼요.

---

## 5단계 — current vs best

`current` 만으로는 부족해요:

```
current 변화: -2 → 1 → -2 → 4 → 3 → 5 → 6 → 1 → 5
```

`current` 는 매번 바뀌기 때문에, 최댓값을 따로 기억해야 해요:

```
best 변화: -2 → 1 → 1 → 4 → 4 → 5 → 6 → 6 → 6
                                        ↑
                                   여기서 고정!
```

```python
best = max(best, current)  # 최댓값 계속 업데이트
```

> **`current` 는 지금 구간의 합, `best` 는 지금까지 본 최댓값**

---

## 6단계 — 완성 코드

```python
def max_subarray(nums):

    current = nums[0]   # 현재 구간의 합
    best = nums[0]      # 지금까지의 최댓값

    for num in nums[1:]:                    # 두 번째부터 시작
        current = max(num, current + num)   # 새로 시작 vs 들고 가기
        best = max(best, current)           # 최댓값 업데이트

    return best
```

---

## 7단계 — 머릿속 추적

```
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
current = -2,  best = -2

num=-2: current= -2, best= -2
num= 1: current=  1, best=  1   ← 1에서 새로 시작
num=-3: current= -2, best=  1
num= 4: current=  4, best=  4   ← 4에서 새로 시작
num=-1: current=  3, best=  4
num= 2: current=  5, best=  5
num= 1: current=  6, best=  6   ← 최댓값 고정!
num=-5: current=  1, best=  6
num= 4: current=  5, best=  6

→ return 6  ✓
```

---

## 시간복잡도 & 공간복잡도

### 시간복잡도란?

> **"입력이 커질수록 코드가 얼마나 느려지는가"**

```
리스트를 한 번 훑으면  → n개 → n번 반복     → O(n)
리스트를 중첩해서 훑으면 → n개 → n x n번    → O(n²)
```

### 공간복잡도란?

> **"입력이 커질수록 얼마나 많은 메모리를 쓰는가"**

```python
current = 0   # 변수 2개만 사용
best = 0      # 입력이 커져도 메모리 추가 없음  → O(1)

stack = []
for num in nums:
    stack.append(num)  # 입력 크기만큼 메모리 사용  → O(n)
```

### 이번 문제 적용

```python
for num in nums[1:]:        # 리스트를 딱 한 번만 훑음  → O(n)
    current = max(...)      # 변수 2개만 사용           → O(1)
    best = max(...)
```

| | 시간복잡도 | 공간복잡도 |
|---|---|---|
| 브루트 포스 | O(n²) | O(1) |
| 최적화 (Kadane's) | O(n) | O(1) |

> 시간복잡도는 n배 빨라졌고, 공간복잡도는 둘 다 동일해요.

---

## 사고 흐름 요약

```
브루트 포스          → 모든 구간을 다 계산           O(n²)
        ↓
낭비 발견            → "이미 계산한 합을 버리고 매번 새로 더함"
        ↓
최적화 아이디어      → "이전 합을 재사용하자"
        ↓
새로운 문제 발견     → "이전 합이 마이너스면 손해"
        ↓
판단 기준            → 들고 가기 vs 새로 시작, 더 큰 쪽 선택
        ↓
best로 최댓값 추적   → current와 별도로 관리         O(n)
```