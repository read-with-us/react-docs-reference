# Hooks

## State Hooks

## Context Hooks

## Ref Hooks

## Effect Hooks

1. > `useLayoutEffect`는 브라우저가 화면을 다시 그리기 전에 실행되며, 레이아웃을 계산할 수 있습니다.
   - 영어 원문에는 repaint라고 되어 있는데 브라우저 동작 원리 얘기할 때 나오는 리페인트 개념과 연관이 있는 것 같아서 [링크](https://d2.naver.com/helloworld/59361) 첨부합니다!

## Performance Hooks

1. > 렌더링 우선순위를 설정하기 위해 다음 중 하나의 Hook을 사용하세요. `useTransition`은 state 전환을 논블로킹으로 표시하고 다른 업데이트가 이를 중단시킬 수 있습니다. `useDeferredValue`는 UI의 덜 중요한 부분의 업데이트를 미루고, 다른 부분이 먼저 업데이트되도록 허용합니다.
   - 렌더링의 우선순위를 설정한다는 이야기가 좀 생소해요. [질문] 둘 다 처음보는 훅인데 혹시 써보신 분 있나요?
   - [pattern.dev](https://www.patterns.dev/posts) 중간에 rendering pattern 부분에 ssr이 소개되어 있는데, ssr에서 최적화의 기반이 되는 훅들이지 않을까 하는 생각입니다.
   - 정리하면서 [관련 아티클](https://vroomfan.tistory.com/45)을 잠깐 찾아 봤는데요! `useDeferredValue`를 설명하고 실제로 적용시킨 예시까지 나온 글이라 소개합니다

## 추가로 제공되는 Hooks

## 사용자 정의 Hooks
