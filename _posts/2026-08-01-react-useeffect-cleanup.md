---
title: "React useEffect와 클린업 함수 제대로 이해하기"
date: 2026-08-01 10:00:00 +0900
categories: [기술, React]
tags: [react, javascript, frontend]
---

## 왜 정리하는가

부트캠프에서 Velog에 "3일차 학습 일지"로 짧게 남겼던 내용을, 나중에 다시 찾아보기 쉽도록
**주제 중심**으로 다시 정리한 글입니다. 일자별 기록 대신, `useEffect`라는 하나의 개념을 완결된 형태로 남깁니다.

## 핵심 개념

`useEffect`는 컴포넌트가 렌더링된 후 실행되는 부수 효과(side effect)를 다루기 위한 훅입니다.

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("tick");
  }, 1000);

  // 클린업 함수: 컴포넌트가 언마운트되거나
  // 의존성이 바뀌어 effect가 재실행되기 직전에 호출된다
  return () => clearInterval(timer);
}, []);
```

## 클린업 함수가 필요한 이유

- 이벤트 리스너, 타이머, 구독(subscription) 등을 정리하지 않으면 메모리 누수가 발생한다.
- 의존성 배열이 바뀌어 effect가 다시 실행될 때마다, 이전 effect의 클린업이 먼저 호출된다.
- 언마운트 시점에도 마지막으로 한 번 호출되어 리소스를 정리한다.

## 트러블슈팅 기록

문제 상황: `setInterval`을 정리하지 않아 컴포넌트를 여러 번 마운트/언마운트할 때 콘솔에 로그가 중복으로 찍히는 버그가 있었다.

원인: 클린업 함수를 반환하지 않아서 이전 interval이 계속 살아있었다.

해결: `return () => clearInterval(timer)`를 추가해서 해결했다.

## 참고

- [React 공식 문서 - useEffect](https://react.dev/reference/react/useEffect)
