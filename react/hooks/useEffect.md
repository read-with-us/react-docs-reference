# useEffect

**목차**

- [레퍼런스](#레퍼런스)
  - [useEffect(setup, dependencies?)](#useeffectsetup-dependencies)
- [사용법](#사용법)
  - [Connecting to an external system](#connecting-to-an-external-system)
  - [Wrapping Effects in custom Hooks](#wrapping-effects-in-custom-hooks)
  - [Controlling a non-React widget](#controlling-a-non-react-widget)
  - [Fetching data with Effects](#fetching-data-with-effects)
  - [Specifying reactive dependencies](#specifying-reactive-dependencies)
  - [Updating state based on previous state from an Effect](#updating-state-based-on-previous-state-from-an-effect)
  - [Removing unnecessary object dependencies](#removing-unnecessary-object-dependencies)
  - [Removing unnecessary function dependencies](#removing-unnecessary-function-dependencies)
  - [Reading the latest props and state from an Effect](#reading-the-latest-props-and-state-from-an-effect)
  - [Displaying different content on the server and the client](#displaying-different-content-on-the-server-and-the-client)
- [문제 해결](#문제-해결)
  - [My Effect runs twice when the component mounts](#my-effect-runs-twice-when-the-component-mounts)
  - [My Effect runs after every re-render](#my-effect-runs-after-every-re-render)
  - [My Effect keeps re-running in an infinite cycle](#my-effect-keeps-re-running-in-an-infinite-cycle)
  - [My cleanup logic runs even though my component didn’t unmount](#my-cleanup-logic-runs-even-though-my-component-didnt-unmount)
  - [My Effect does something visual, and I see a flicker before it runs](#my-effect-does-something-visual-and-i-see-a-flicker-before-it-runs)

## 레퍼런스

### useEffect(setup, dependencies?)

1. > After every re-render with changed dependencies, React will first run the cleanup function (if you provided it) with the old values, and then run your setup function with the new values.

   - 이전 값으로 cleanup 함수를 실행하고 새로운 값으로 setup 함수를 실행한다는 설명 덕분에 역할이 더 명확하게 이해됩니다.
   - 네! 저도요! 안 그래도 이 부분이 useEffect의 실행 단계를 잘 알려주어서 표시하려 했어요

2. > If you’re not trying to synchronize with some external system, you probably don’t need an Effect.

   - Learn 파트에서 you-might-not-need-an-effect 페이지는 정말 꼭 읽어보시는 걸 추천드려요!
   - 저도 추천드립니다. useEffect를 지금 써도 되나…? 할 때 많이 들어가보는 페이지네요🧐
   - 맞아요 이 파트 읽고 지금 페이지를 읽으니까 이해가 훨씬 수월하네요..!

3. > You can also extract state updates and non-reactive logic outside of your Effect.

   - 최근에 이런 상황에서 개선시도를 해보려다가 잘 안되어서 json string으로 변환해서 넣어주고 effect 내부에서 파싱해서 사용하는 식으로 했었어요 ㅠㅠ
   - 비슷하게 함수가 의존성 배열에 포함되는 경우 린트를 따르다 보면 useCallback 훅을 사용하고 거기다 useMemo 훅까지 사용하게 되어서 코드가 장황해지는 것 같아요.

## 사용법

### Connecting to an external system

1. > These systems aren’t controlled by React, so they are called external.

   - 리액트가 컨트롤 하는게 아니면 '외부'의 것이라고 본다!

2. > Try to write every Effect as an independent process and think about a single setup/cleanup cycle at a time.

   - 의존성이 같더라도 프로세스가 다르다면 Effect를 독립적인 로직으로 작성해야겠군요
   - 클래스 컴포넌트는 라이프사이클 메서드가 있어서 componentDidMount()와 같은 특정 메서드 안에 모든 로직을 작성해야 했다. 그래서 로직을 분리하기가 어려웠던 반면 훅을 사용하면 로직을 분리하기 쉬운 것 같다.
   - 단일 책임 원칙이 생각납니다.

3. > Here, external system means any piece of code that’s not controlled by React, such as:
   >
   > - A timer managed with setInterval() and clearInterval().
   > - An event subscription using window.addEventListener() and window.removeEventListener().
   > - A third-party animation library with an API like animation.start() and animation.reset().

   - useEffect가 필요한 대표적인 예들을 알려주어 좋네요
   - 맞아요. 외부 시스템과 동기화한다고 할 때 어디까지 외부인지 헷갈리는데 구체적인 예를 들어서 좋았어요.
   - 최근에 Next.js 문서를 회사 분들이랑 스터디하고 있는데 개정된 React 문서는 Next.js 문서에 비해 정말 친절하고 잘 만들었다고 생각합니다.

4. > 예제 4 of 5: Controlling a modal dialog
   >
   > <dialog>

   - 4-5 예제에서 소개한 dialog 태그는 처음 보네요..!
   - https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog
   - 저도 처음봤어요. (x3)

### Wrapping Effects in custom Hooks

1. > it’s usually a sign that you need to extract some custom Hooks for common behaviors your components rely on.

   - 재사용 가능해보이는 useEffect가 두번 이상 반복되거나 코드가 번잡하게 느껴질 때 커스텀 훅으로 말아서 사용하는지라 공감되네요..!

2. > 예제 5 of 5: Tracking element visibility
   >
   > IntersectionObserver

   - https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API
   - 예전에 공부할 때 이 API 사용해서 무한스크롤 구현했던 기억이 나네요. observe 중인 엘리먼트가 화면에 보이면 다음 데이터를 불러오는 방식이었어요.

### Controlling a non-React widget

1. > After the Map React component is removed from the tree, both the DOM node and the MapWidget class instance will be automatically garbage-collected by the browser JavaScript engine.

   - cleanup 함수가 필요한지 아닌지를 판단하려면 외부 라이브러리 예를 들어, Web API에 대한 이해가 필수겠네요!

### Fetching data with Effects

1. > Note the ignore variable which is initialized to false, and is set to true during cleanup. This ensures your code doesn’t suffer from “race conditions”: network responses may arrive in a different order than you sent them.

   - 이 훅에서 ignore 변수가 왜 필요한지 잘 기억나지 않았는데 다시 한 번 리마인드 되어서 좋습니다.
   - ignore라는 변수명을 딱 봤을 때 뭐를 무시한다는 건지 와닿지 않는 것 같아요. 더 좋은 변수명이 없을지 🤔

2. > Writing data fetching directly in Effects gets repetitive and makes it difficult to add optimizations like caching and server rendering later. It’s easier to use a custom Hook—either your own or maintained by the community.

   - 데이터 가져올 때마다 useEffect를 쓰는 게 아니라 커스텀 훅으로 추출해야 하는 이유가 단지 재사용 때문이 아니라 공통 로직을 적용하기 위한 이유도 있는 걸 간과했던 것 같아요.

3. > Otherwise, consider using or building a client-side cache. Popular open source solutions include React Query, useSWR, and React Router 6.4+.

   - 저희 회사는 레거시 코드는 모두 useEffect로 직접 fetching하는 코드로 작성되어 있고 최근에 react query를 도입해서 막 사용 중입니다.
   - [질문] 다른 분들은 어떤 라이브러리를 사용하시나요? 혹은 Next.js와 같은 프레임워크에서 서버사이드 렌더링 하시나요?
   - React Query 사용합니다. (x3)
   - 그래도 여전히 페이지나 데이터 구조가 단순한 경우에는 직접 fetching하는 방식을 종종 사용하는 것 같아요.

### Specifying reactive dependencies

1. > Notice that you can’t “choose” the dependencies of your Effect.

   - 이전에는 의존성 배열 값을 적당히 선택 가능하다고 생각하고 있었는데, 최근에 useEffect 사용할 때 아예 이 점을 염두에 두고 작업해보니 effect가 사용하기 조심스러운 친구라는 생각이 새삼 들어서 관점의 차이가 신기했습니다 ㅎㅎ

### Updating state based on previous state from an Effect

### Removing unnecessary object dependencies

### Removing unnecessary function dependencies

### Reading the latest props and state from an Effect

1. > However, sometimes you’ll want to read the latest props and state from an Effect without “reacting” to them.

   - 최근에도 이런 케이스가 있었는데 업데이트 시 반응하길 원하는 값에만 조건을 달아서(이전값이랑 비교했을 때 변동 없으면 패스) 처리하는 식으로 하면서 코드가 더러워지더라구요.. useEffectEvent가 빨리 안정적으로 출시되면 좋겠군요 흐흐
   - 앗 저도 얼마 전에 이전 값이랑 비교하는 방식으로 구현한 적 있어요 ㅎㅎ 동감입니다!

### Displaying different content on the server and the client

## 문제 해결

### My Effect runs twice when the component mounts

### My Effect runs after every re-render

### My Effect keeps re-running in an infinite cycle

### My cleanup logic runs even though my component didn’t unmount

### My Effect does something visual, and I see a flicker before it runs
