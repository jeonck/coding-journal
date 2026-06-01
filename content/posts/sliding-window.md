---
title: "코딩 사고 흐름 — 슬라이딩 윈도우 (Longest Substring Without Repeating Characters)"
date: 2024-01-01
draft: false
tags: ["python", "sliding-window", "string", "set", "leetcode"]
categories: ["coding"]
---

## 예제 문제

> **"문자열에서 중복 없는 가장 긴 부분 문자열의 길이를 반환하라"**

```
"abcabcbb" → 3  ("abc")
"bbbbb"    → 1  ("b")
```

---

## 1단계 — 입출력 계약

```
입력: 문자열 "abcabcbb"
출력: 중복 없는 가장 긴 부분 문자열 길이 3
```

함수 뼈대:

```python
def length_of_longest_substring(s):

    return
```

---

## 2단계 — 손으로 풀기

`"abcabcbb"` 를 손으로 보면:

```
a       → 중복 없음 → 길이 1
ab      → 중복 없음 → 길이 2
abc     → 중복 없음 → 길이 3
abca    → a 중복!   → 왼쪽을 줄임
bca     → 중복 없음 → 길이 3
bcab    → b 중복!   → 왼쪽을 줄임
...
```

---

## 3단계 — 브루트 포스

모든 구간을 다 확인하는 방법:

```python
for i in range(len(s)):
    for j in range(i, len(s)):   # 모든 구간을 처음부터 다시 확인
        # i~j 구간에 중복이 있는지 확인
```

낭비가 보여요:

> **"이미 확인한 구간을 버리고 매번 처음부터 다시 확인하고 있다"** → O(n²)

---

## 4단계 — 최적화 아이디어

**낭비를 없애는 질문:**

> **"처음부터 다시 시작하지 않고, 왼쪽만 줄이면 되지 않나?"**

```
[abc]abcbb   → 중복 없으면 오른쪽 확장
[abca]bcbb   → a 중복! → 왼쪽 줄임
 [bca]bcbb   → 중복 없음 → 오른쪽 확장
```

이게 바로 **슬라이딩 윈도우** 예요.

---

## 문제 키워드 → 알고리즘 선택

> **"가장 긴 부분 문자열"** → 슬라이딩 윈도우
> **"중복 없는"** → 중복이 생기면 왼쪽을 줄이는 로직 추가

```
문제 조건이 알고리즘을 선택하게 해주고
제약이 세부 구현을 결정해요
```

---

## 5단계 — 로직을 쓰다가 변수 발견

초기화 변수를 먼저 생각하는 게 아니에요.
**로직을 쓰다가 필요한 것을 발견하고, 위로 올라가서 추가하는 게 자연스러운 흐름이에요.**

```
오른쪽으로 이동하면서...
→ for right 가 필요하네

중복이 있으면 왼쪽을 줄이고...
→ "왼쪽 위치를 기억해야 하네" → left = 0 발견

창문 안 문자를 확인해야 하고...
→ "문자들을 기억해야 하네" → seen = set() 발견

가장 긴 길이를 기억해야 하고...
→ "최댓값을 기억해야 하네" → best = 0 발견
```

---

## 6단계 — 중복이면 왜 앞의 것도 제거하나?

`"abcb"` 에서 `b` 가 중복일 때:

```
[a b c b]
 ↑       ↑
left   right
```

창문은 항상 **연속된 구간** 이라서 중간을 건너뛸 수 없어요:

```
while 1회: "a" 제거, left=1 → [b c b] → 아직 b 중복!
while 2회: "b" 제거, left=2 → [c b]   → 중복 없음 ✓
```

> **"중복이 없어질 때까지 왼쪽을 계속 줄이는 것"**

그래서 `if` 가 아니라 `while` 을 써요:

```python
if    → 한 번만 확인
while → 중복이 없어질 때까지 계속 확인
```

---

## 7단계 — 창문 크기 구하기

```
"abcabcbb"
 ↑   ↑
left right

right - left + 1 = 2 - 0 + 1 = 3  ("abc")
```

`+1` 이 필요한 이유:

```
left=0, right=2
인덱스: 0, 1, 2 → 3개 → 2 - 0 + 1 = 3
```

---

## 8단계 — 완성 코드

```python
def length_of_longest_substring(s):

    # 로직을 쓰다가 발견한 변수들
    seen = set()    # 창문 안의 문자들 (중복 확인용)
    left = 0        # 왼쪽 포인터 (창문 시작)
    best = 0        # 가장 긴 길이

    for right in range(len(s)):        # right 는 계속 오른쪽으로
        while s[right] in seen:        # 중복이 있으면
            seen.remove(s[left])       # 왼쪽 제거
            left += 1                  # 왼쪽 이동

        seen.add(s[right])             # 현재 문자 추가
        best = max(best, right - left + 1)  # 창문 크기 업데이트

    return best
```

---

## 9단계 — 머릿속 추적

```
s = "abcabcbb"

right=0: "a" → seen={"a"},           left=0, best=1
right=1: "b" → seen={"a","b"},       left=0, best=2
right=2: "c" → seen={"a","b","c"},   left=0, best=3
right=3: "a" 중복!
  → "a" 제거, left=1 → seen={"b","c"}
  → "a" 추가 → seen={"b","c","a"},   left=1, best=3
right=4: "b" 중복!
  → "b" 제거, left=2 → seen={"c","a"}
  → "b" 추가 → seen={"c","a","b"},   left=2, best=3
...
→ return 3  ✓
```

---

## 시간복잡도 & 공간복잡도

```
시간복잡도: O(n)   → right, left 모두 최대 n번 이동
공간복잡도: O(n)   → seen에 최대 n개 문자 저장
```

브루트 포스와 비교:

| | 시간복잡도 | 공간복잡도 |
|---|---|---|
| 브루트 포스 | O(n²) | O(1) |
| 슬라이딩 윈도우 | O(n) | O(n) |

---

## 코딩 연습

### 1차 시도 — 힌트 많음

```python
def length_of_longest_substring(s):

    seen = ___________     # 창문 안의 문자들
    left = ___________     # 왼쪽 포인터 시작
    best = ___________     # 가장 긴 길이

    for right in range(len(s)):
        while s[right] in seen:      # 중복이 있으면
            seen.remove(s[left])     # 왼쪽 제거
            left += 1                # 왼쪽 이동

        seen.add(s[right])           # 현재 문자 추가
        best = max(best, ___________)  # 창문 크기

    return best
```

---

### 2차 시도 — 힌트 줄임

```python
def length_of_longest_substring(s):

    seen = ___________
    left = ___________
    best = ___________

    for right in range(len(s)):
        while ___________:
            ___________
            ___________

        ___________
        best = max(___________, ___________)

    return ___________
```

---

### 3차 시도 — 백지에서 작성

말로 먼저 읽어보고 코드로 옮겨보세요:

```
1. 창문 안 문자를 기억할 공간
2. 왼쪽 포인터, 최댓값 초기화
3. right 를 오른쪽으로 이동하면서:
   - 중복이 없어질 때까지 왼쪽 줄이기
   - 현재 문자 추가
   - 창문 크기 업데이트
4. 최댓값 반환
```

---

## 사고 흐름 요약

```
브루트 포스          → 모든 구간을 처음부터 다시 확인   O(n²)
        ↓
낭비 발견            → "처음부터 다시 시작하지 않아도 됨"
        ↓
슬라이딩 윈도우      → 왼쪽, 오른쪽 두 포인터
        ↓
로직을 쓰다가        → seen, left, best 변수 발견
        ↓
중복 조건            → while 로 없어질 때까지 제거
        ↓
완성                 → O(n)
```

> **초기화 변수를 먼저 생각하는 게 아니라, 로직을 쓰다가 필요한 것을 발견하고 위로 올라가서 추가하는 게 자연스러운 흐름이에요.**