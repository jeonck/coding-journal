---
title: "이진 탐색 (Binary Search)"
date: 2024-01-01
draft: false
tags: ["python", "binary-search", "array", "leetcode"]
categories: ["coding"]
---

## 예제 문제

> **"정렬된 숫자 리스트에서 target 이 있으면 인덱스를, 없으면 -1을 반환하라"**

```
[1, 3, 5, 7, 9, 11], target = 7  → 3
[1, 3, 5, 7, 9, 11], target = 6  → -1
```

---

## 1단계 — 입출력 계약

```
입력: 정렬된 숫자 리스트, target
출력: target의 인덱스 또는 -1
```

함수 뼈대:

```python
def binary_search(nums, target):

    return -1
```

---

## 2단계 — 브루트 포스

처음부터 하나씩 확인하는 방법:

```
1 == 7? No
3 == 7? No
5 == 7? No
7 == 7? Yes → 인덱스 3
```

동작은 맞지만, 리스트가 크면 느려요.

---

## 3단계 — 핵심 조건 발견

> **"정렬되어 있다"** 는 조건 하나가 완전히 다른 접근을 가능하게 해요.

사전에서 단어를 찾을 때처럼:

```
1. 가운데를 펼침 → 5
   7 > 5 → 오른쪽에 있다

2. 오른쪽 가운데 → 9
   7 < 9 → 왼쪽에 있다

3. 남은 것 → 7
   7 == 7 → 찾았다!
```

> **매번 절반을 버리는 것** 이 이진 탐색의 핵심이에요.

---

## 4단계 — 세 개의 포인터

절반을 버리려면 탐색 범위를 관리할 세 포인터가 필요해요:

```
[1, 3, 5, 7, 9, 11]
 ↑        ↑        ↑
left     mid     right
```

- `left` — 탐색 범위의 시작
- `right` — 탐색 범위의 끝
- `mid` — 가운데 → `(left + right) // 2`

**`//` 란?**

```python
5 / 2  = 2.5   # 일반 나누기
5 // 2 = 2     # 소수점 버림
```

인덱스는 항상 정수여야 하기 때문에 `//` 를 사용해요.

---

## 5단계 — 판단 기준

```
nums[mid] == target  → 찾았다!
nums[mid] < target   → target은 오른쪽 → left = mid + 1
nums[mid] > target   → target은 왼쪽  → right = mid - 1
```

---

## 6단계 — 완성 코드

```python
def binary_search(nums, target):

    left = 0
    right = len(nums) - 1

    while left <= right:          # 탐색 범위가 남아있으면
        mid = (left + right) // 2

        if nums[mid] == target:   # 찾았으면
            return mid
        elif nums[mid] < target:  # target이 오른쪽에 있으면
            left = mid + 1        # 왼쪽 절반 버림
        else:                     # target이 왼쪽에 있으면
            right = mid - 1       # 오른쪽 절반 버림

    return -1                     # 못 찾으면
```

---

## 7단계 — 머릿속 추적

```
nums = [1, 3, 5, 7, 9, 11], target = 7

1회: left=0, right=5, mid=2 → nums[2]=5 < 7  → left = 3
2회: left=3, right=5, mid=4 → nums[4]=9 > 7  → right = 3
3회: left=3, right=3, mid=3 → nums[3]=7 == 7 → return 3 ✓

target = 6 일 때:
1회: left=0, right=5, mid=2 → nums[2]=5 < 6  → left = 3
2회: left=3, right=5, mid=4 → nums[4]=9 > 6  → right = 3
3회: left=3, right=3, mid=3 → nums[3]=7 > 6  → right = 2
→ left(3) > right(2) → 탈출 → return -1 ✓
```

---

## 시간복잡도 & 공간복잡도

### 시간복잡도

매번 절반을 버리기 때문에:

```
6개    → 최대 3번
12개   → 최대 4번
100만개 → 최대 20번!
```

> **O(log n)** — 입력이 아무리 커져도 탐색 횟수가 조금만 늘어남

### 공간복잡도

```
left, right, mid 변수 3개만 사용
입력이 커져도 메모리 추가 없음
```

> **O(1)**

### 브루트 포스와 비교

| | 시간복잡도 | 공간복잡도 |
|---|---|---|
| 브루트 포스 | O(n) | O(1) |
| 이진 탐색 | O(log n) | O(1) |

> 100만개 리스트에서 브루트 포스는 최대 100만번, 이진 탐색은 최대 20번만 탐색해요.

---

## 사고 흐름 요약

```
브루트 포스          → 처음부터 하나씩 확인       O(n)
        ↓
핵심 조건 발견       → "정렬되어 있다"
        ↓
절반을 버릴 수 있다  → 매번 탐색 범위가 절반으로 줄어듦
        ↓
세 포인터로 관리     → left, right, mid
        ↓
판단 기준            → 크면 오른쪽, 작으면 왼쪽
        ↓
완성                 → O(log n)
```

> **"정렬되어 있다"** 는 조건을 발견하는 순간, 이진 탐색이 자연스럽게 떠올라요.