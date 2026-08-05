---
title: "[프로젝트 회고] 투두리스트 앱 - Velog 일지를 프로젝트 글로 재구성하기"
date: 2026-08-05 20:00:00 +0900
categories: [기술, 투두리스트-프로젝트]
tags: [project, retrospective, migration]
pin: true
---

이 글은 실제 프로젝트 회고인 동시에, **Velog의 "N일차" 일기 형식 글을 어떻게 재구성하면 좋을지 보여주는 예시**입니다.
아래 형식을 참고해서 기존 글들을 옮겨보세요.

## Before (Velog 원본 - 일기 형식)

> **12일차**
> 오늘은 투두리스트 프로젝트를 시작했다. 아침에 컴포넌트 구조를 짜다가 상태 관리가 꼬여서 좀 헤맸다.
> useState를 여러 개 쓰다가 결국 useReducer로 바꿨더니 나아졌다. 내일은 로컬스토리지 연동을 해야 한다.

이런 글은 "그날 무엇을 했는지"는 알 수 있지만, 나중에 "이 프로젝트에서 상태 관리를 어떻게 설계했는지"를 찾아보기는 어렵습니다.

## After (재구성 - 프로젝트 중심)

### 프로젝트 개요

- **기간**: 2026.07.20 ~ 2026.07.28
- **목표**: React로 할 일 목록(CRUD) 앱 구현, 로컬스토리지로 데이터 유지
- **기술 스택**: React, useReducer, LocalStorage API

### 기술적 의사결정: useState 대신 useReducer

초기에는 할 일 목록, 입력값, 필터 상태를 각각 `useState`로 관리했지만, 상태들이 서로 연관되어
업데이트될 때마다 여러 `setState`를 순차 호출해야 했고 상태 불일치가 발생했습니다.

`useReducer`로 전환해 액션 기반으로 상태 변경 로직을 한 곳에 모았습니다.

```jsx
function todoReducer(state, action) {
  switch (action.type) {
    case "ADD":
      return [...state, { id: Date.now(), text: action.text, done: false }];
    case "TOGGLE":
      return state.map((todo) =>
        todo.id === action.id ? { ...todo, done: !todo.done } : todo
      );
    case "REMOVE":
      return state.filter((todo) => todo.id !== action.id);
    default:
      return state;
  }
}
```

### 트러블슈팅

**문제**: 새로고침하면 할 일 목록이 사라짐.

**원인**: 상태를 메모리에만 저장하고 있었음.

**해결**: `useEffect`로 상태가 바뀔 때마다 `localStorage.setItem`으로 저장하고,
초기 상태를 `localStorage.getItem`에서 불러오도록 수정.

### 배운 점

- 상태 간 연관성이 커지면 `useState` 여러 개보다 `useReducer`가 관리하기 쉬웠다.
- 브라우저 새로고침 시 상태 유지가 필요하면 초기 렌더링 시점에 값을 복원하는 로직을 반드시 고려해야 한다.

---

## 마이그레이션 체크리스트

기존 Velog 글을 옮길 때 아래 순서를 참고하세요.

1. 같은 프로젝트/주제로 묶인 일지 글들을 모은다.
2. "무엇을 했는가"가 아니라 "무엇을 배웠는가/어떤 문제를 어떻게 풀었는가" 중심으로 재구성한다.
3. `categories`는 `[기술, 프로젝트명]` 또는 `[학업, 과목명]`처럼 2단계로 지정한다.
4. 코드 스니펫은 실제 커밋에서 발췌해 정확성을 확인한다.
5. 날짜(`date`)는 원래 작성일이 아니라 재구성해 발행하는 날짜로 넣어도 무방하다 (원문 링크를 남기고 싶다면 본문 하단에 출처로 남긴다).
