---
title: "이진 트리 레벨 순서 탐색 (BFS)"
date: 2024-01-01
draft: false
tags: ["python", "tree", "bfs", "queue", "leetcode"]
categories: ["coding"]
---

## 예제 문제

> **"이진 트리를 레벨 순서로 탐색하라"**

```
        1
       / \
      2   3
     / \
    4   5

결과: [[1], [2, 3], [4, 5]]
```

---

## 이진 트리란?

각 노드가 자식을 최대 2개까지 가지는 구조예요:

```
        1          ← 루트 (root): 가장 위
       / \
      2   3        ← 1의 자식
     / \
    4   5          ← 2의 자식
```

용어 정리:

```
루트 (root)  = 가장 위의 노드 (1)
부모         = 자식을 가진 노드
자식         = 부모 아래 노드
리프 (leaf)  = 자식이 없는 노드 (3, 4, 5)
레벨         = 루트부터의 깊이
```

---

## TreeNode 클래스

```python
# 1단계 — 설계도 (클래스 정의)
class TreeNode:
    def __init__(self, val=0):   # 생성자 — 객체 만들 때 자동 실행
        self.val = val           # 값 1개
        self.left = None         # 왼쪽 자식 노드
        self.right = None        # 오른쪽 자식 노드

# 2단계 — 자료구조 형성 (객체 생성 + 연결)
root  = TreeNode(1)
node2 = TreeNode(2)
node3 = TreeNode(3)
node4 = TreeNode(4)
node5 = TreeNode(5)

root.left   = node2    # 연결
root.right  = node3
node2.left  = node4
node2.right = node5

# 3단계 — 알고리즘 수행
level_order(root)
```

> **클래스로 설계도를 만들고 → 자료구조를 형성하고 → 그 위에서 알고리즘이 동작해요**

LeetCode 에서는 1, 2단계가 이미 주어져 있어요. 우리는 3단계 함수 작성에만 집중하면 돼요.

---

## BFS 란?

> **"레벨 순서대로 탐색"**

```
레벨 1: [1]
레벨 2: [2, 3]
레벨 3: [4, 5]
```

---

## 핵심 아이디어

> **"먼저 들어온 것이 먼저 나간다"**

```
1이 나오면서 말함: "내 자식 2, 3이 다음이야"
2가 나오면서 말함: "내 자식 4, 5가 다음이야"
3이 나오면서 말함: "나는 자식이 없어"
4, 5가 나오면서 말함: "나는 자식이 없어" → 끝
```

이 특성을 가진 자료구조 → **큐(Queue)**

```
스택: 나중에 넣은 것이 먼저 나옴 (LIFO) → DFS
큐:   먼저 넣은 것이 먼저 나옴  (FIFO) → BFS
```

> **자료구조 선택 하나가 탐색 방식을 완전히 바꿔요**

---

## 큐를 Python 에서는?

```python
from collections import deque

queue = deque()

queue.append(1)    # 넣기    → deque([1])
queue.append(2)    # 넣기    → deque([1, 2])
queue.popleft()    # 꺼내기  → 1  (먼저 넣은 것부터)
```

---

## level 이란?

결과를 레벨별로 묶어서 저장하려면 임시 바구니가 필요해요:

```python
level = []                    # 빈 바구니 꺼내기
level.append(node.val)        # 값 담기
result.append(level)          # result에 통째로 담기
```

```
level 없이:  [1, 2, 3, 4, 5]          ← 레벨 구분 없음
level 있이:  [[1], [2, 3], [4, 5]]    ← 레벨별로 묶임
```

> **`level` 은 이번 레벨의 값들을 임시로 모아두는 바구니**

---

## 완성 코드

```python
from collections import deque

def level_order(root):

    # 엣지케이스 먼저
    if not root:                          # 트리가 비었으면
        return []

    result = []
    queue = deque([root])                 # 루트부터 시작

    while queue:                          # 큐가 빌 때까지
        level = []                        # 빈 바구니
        size = len(queue)                 # 이번 레벨 노드 수 미리 저장

        for _ in range(size):
            node = queue.popleft()        # 큐에서 꺼내기
            level.append(node.val)        # 값 바구니에 담기

            if node.left:
                queue.append(node.left)   # 왼쪽 자식 큐에 넣기
            if node.right:
                queue.append(node.right)  # 오른쪽 자식 큐에 넣기

        result.append(level)              # 바구니 통째로 담기

    return result
```

---

## 전체 흐름 추적

```
시작: queue = [1]

1회: size=1
  1 꺼냄 → level=[1] → 2,3 큐에 넣음
  queue = [2, 3]
  result = [[1]]

2회: size=2
  2 꺼냄 → level=[2] → 4,5 큐에 넣음
  3 꺼냄 → level=[2,3] → 자식 없음
  queue = [4, 5]
  result = [[1], [2, 3]]

3회: size=2
  4 꺼냄 → level=[4] → 자식 없음
  5 꺼냄 → level=[4,5] → 자식 없음
  queue = []
  result = [[1], [2, 3], [4, 5]]  ✓
```

---

## 시간복잡도 & 공간복잡도

```
시간복잡도: O(n)   → 모든 노드를 한 번씩 방문
공간복잡도: O(n)   → 큐에 최대 n개 노드 저장
```

---

## 사고 흐름 요약

```
레벨 순서 탐색
      ↓
"먼저 들어온 것이 먼저 나간다"
      ↓
큐(Queue) 선택
      ↓
엣지케이스 먼저 처리
      ↓
노드 꺼내면서 → 값은 level에, 자식은 큐에
      ↓
큐가 빌 때까지 반복
```

> **큐에 넣었다 빼면서, 값은 level에 저장하고, 자식을 다시 큐에 넣는 과정을 반복하는 것이 BFS의 전부예요.**