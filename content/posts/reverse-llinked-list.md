---
title: "연결 리스트 뒤집기 (Reverse Linked List)"
date: 2024-01-01
draft: false
tags: ["python", "linked-list", "pointer", "leetcode"]
categories: ["coding"]
---

## 예제 문제

> **"연결 리스트를 뒤집어라"**

```
1 → 2 → 3 → 4 → 5 → None
↓
5 → 4 → 3 → 2 → 1 → None
```

---

## 연결 리스트란?

일반 리스트와 다르게, 각 노드가 다음 노드를 가리키는 구조예요:

```
일반 리스트: [1, 2, 3, 4, 5]   → 인덱스로 바로 접근
연결 리스트: 1 → 2 → 3 → 4 → 5 → None  → next를 따라 접근
```

각 노드는 두 가지 정보를 가져요:

```
[ val | next ]
[  1  |  →2 ]
[  2  |  →3 ]
[  3  | None ]  ← 마지막은 None
```

---

## ListNode 클래스

```python
class ListNode:
    def __init__(self, val=0):  # 생성자 — 객체를 만들 때 자동 실행
        self.val = val          # 값
        self.next = None        # 다음 노드 (처음엔 연결 전이라 None)
```

**클래스 = 설계도, 객체 = 설계도에 값을 투입해서 만든 실체**

```python
node1 = ListNode(1)   # val=1 인 노드 생성
node2 = ListNode(2)   # val=2 인 노드 생성
node3 = ListNode(3)   # val=3 인 노드 생성

node1.next = node2    # 연결
node2.next = node3    # 연결
```

연결 후:

```
node1       node2       node3
[ 1 | →] → [ 2 | →] → [ 3 | None ]
```

**`next = None` 은 "아직 연결 안 됨" 을 의미. 연결은 객체를 만든 후에 따로 해요.**

---

## head 란?

`head` 는 연결 리스트의 **첫 번째 노드** 예요:

```
head
 ↓
 1 → 2 → 3 → None
```

첫 노드만 알면 `next` 를 따라 전체에 접근할 수 있어요:

```python
head.val             # → 1
head.next.val        # → 2
head.next.next.val   # → 3
```

---

## 1단계 — 입출력 계약

```
입력: 연결 리스트의 첫 번째 노드 (head)
출력: 뒤집힌 연결 리스트의 첫 번째 노드
```

함수 뼈대:

```python
def reverse_list(head: ListNode) -> ListNode:

    return
```

---

## 2단계 — 핵심 아이디어

> **"→ 를 ← 로 바꾸는 것"**

```
뒤집기 전:   None    1 → 2 → 3 → None
                     ↑
                    head

뒤집기 후:   None ← 1 ← 2 ← 3
                               ↑
                            새 head
```

---

## 3단계 — 세 개의 포인터

방향을 바꾸려면 세 포인터가 필요해요:

```
prev      curr    next_node
None       1    →    2    →    3
```

- `prev` — 이전 노드 (처음엔 None)
- `curr` — 지금 처리중인 노드
- `next_node` — 다음에 처리할 노드 (미리 저장)

**`prev = None` 으로 시작하는 이유:**

```
뒤집기 후 1은 마지막 노드가 됨
마지막 노드는 None을 가리켜야 함
그래서 prev = None 으로 준비
```

---

## 4단계 — 한 루프에서 하는 일

매 루프마다 하는 일은 딱 하나:

> **"→ 를 ← 로 바꾸는 것"**

```
step1: next_node = curr.next   # 다음 노드 미리 저장 (잃어버리지 않으려고)
step2: curr.next = prev        # 화살표 방향 바꾸기
step3: prev = curr             # prev 한 칸 오른쪽으로 이동
step4: curr = next_node        # curr 한 칸 오른쪽으로 이동
```

---

## 5단계 — 샘플로 직관적으로 보기

**막막할 때는 샘플 값으로 먼저 써보고, 코드로 바꾸세요:**

```
prev=1, curr=2, next_node=3 일 때

샘플값              코드
next_node = 3  →   next_node = curr.next
2.next = 1     →   curr.next = prev
prev = 2       →   prev = curr
curr = 3       →   curr = next_node
```

---

## 6단계 — 루프 추적

```
시작: prev=None, curr=1

1회:
  next_node=2, 1→None, prev=1, curr=2
  None ← 1    2 → 3

2회:
  next_node=3, 2→1, prev=2, curr=3
  None ← 1 ← 2    3

3회:
  next_node=None, 3→2, prev=3, curr=None
  None ← 1 ← 2 ← 3

curr=None → 종료!
return prev=3  ✓
```

---

## 7단계 — 완성 코드

```python
def reverse_list(head: ListNode) -> ListNode:
    # head: 연결 리스트의 첫 번째 노드

    prev = None
    curr = head

    while curr:                    # curr가 None이 될 때까지
        next_node = curr.next      # 다음 노드 미리 저장
        curr.next = prev           # 방향 뒤집기
        prev = curr                # prev 이동
        curr = next_node           # curr 이동

    return prev                    # 새로운 head 반환
```

---

## 시간복잡도 & 공간복잡도

```
시간복잡도: O(n)   → 모든 노드를 한 번씩 방문
공간복잡도: O(1)   → prev, curr, next_node 변수 3개만 사용
```

---

## 사고 흐름 요약

```
핵심 아이디어        → "→ 를 ← 로 바꾸는 것"
        ↓
세 포인터 필요       → prev, curr, next_node
        ↓
순서가 중요          → 미리 저장 → 방향 바꾸기 → 이동
        ↓
막막하면             → 샘플 값으로 먼저 써보고 코드로 바꾸기
        ↓
완성                 → O(n), O(1)
```

> **연결 리스트는 눈에 보이지 않아서 어려워요. 샘플 값으로 직접 추적해보는 습관이 핵심이에요.**