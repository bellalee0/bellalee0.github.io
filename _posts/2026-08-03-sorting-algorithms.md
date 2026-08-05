---
title: "정렬 알고리즘 비교 정리 (버블 / 퀵 / 병합)"
date: 2026-08-03 09:30:00 +0900
categories: [학업, 알고리즘]
tags: [algorithm, cs, sorting]
---

## 왜 정리하는가

부트캠프 CS 커리큘럼에서 배운 정렬 알고리즘들을 흩어진 일지 대신 하나의 비교 노트로 모았습니다.
시험이나 코딩테스트 대비용으로 다시 찾아보기 좋게 표로 정리합니다.

## 비교표

| 알고리즘 | 평균 시간복잡도 | 최악 시간복잡도 | 공간복잡도 | 안정 정렬 |
| --- | --- | --- | --- | --- |
| 버블 정렬 | O(n²) | O(n²) | O(1) | O |
| 퀵 정렬 | O(n log n) | O(n²) | O(log n) | X |
| 병합 정렬 | O(n log n) | O(n log n) | O(n) | O |

## 병합 정렬 구현 예시

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    return merge(left, right)


def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

## 배운 점

- 퀵 정렬은 평균적으로 빠르지만, 이미 정렬된 배열 등 최악의 경우 피벗 선택에 따라 O(n²)까지 느려질 수 있다.
- 안정 정렬이 필요한 경우(동일한 값의 원래 순서를 유지해야 할 때)는 병합 정렬을 선택해야 한다.
- 실무에서는 언어 내장 정렬 함수(Timsort 등)를 쓰는 경우가 대부분이지만, 원리를 이해하고 있어야 코딩테스트에서 응용할 수 있다.
