# cache

**목차**

- [레퍼런스](#레퍼런스)
  - [cache(fn)](#cachefn)
- [사용법](#사용법)
  - [Cache an expensive computation](#cache-an-expensive-computation)
  - [Share a snapshot of data](#share-a-snapshot-of-data)
  - [Preload data](#preload-data)
- [문제 해결](#문제-해결)
  - [My memoized function still runs even though I’ve called it with the same arguments](#my-memoized-function-still-runs-even-though-ive-called-it-with-the-same-arguments)

1. > cache lets you cache the result of a data fetch or computation.

   - 캐싱까지 리액트에서 지원하면 전에 곰곰님 말씀처럼 외부 프레임워크 없이도 SSR을 구현하는 게 가능할 수도 있을 것 같아요

## 레퍼런스

### cache(fn)

1. > cachedFn will also cache errors. If fn throws an error for certain arguments, it will be cached, and the same error is re-thrown when cachedFn is called with those same arguments.

   - 에러까지 캐싱한다니.. 차후에 cache를 실제 프로젝트에서 사용하게 되면 이 부분에 유의해야겠네요🤔
   - 같은 인자로 요청을 보내도 서버 상태에 따라 서버 응답이 다를 수도 있을 것 같은데
   - 그러네요. 이 부분은 사용할 때 좀 더 조사해보는 게 좋을 것 같아요.

## 사용법

### Cache an expensive computation

1. > Instead, define the memoized function in a dedicated module that can be import-ed across components.

   - 안 그래도 두 컴포넌트에서 어떻게 캐시된 함수를 공유하나 했는데 구체적으로 알려줘서 좋습니다!

### Share a snapshot of data

1.  > ```js
    > const getTemperature = cache(async (city) => {
    >   return await fetchTemperature(city);
    > });
    > ```

    - 비동기 함수 호출할 때 그냥 Promise를 리턴할 줄 알았는데 await으로 기다렸다가 값을 반환하길래 기억해둡니다.

2.  > Asynchronous rendering is only supported for Server Components.
    >
    > ```js
    > async function AnimatedWeatherCard({ city }) {
    >   const temperature = await getTemperature(city);
    >   // ...
    > }
    > ```

    - 이미 cache 안에서 await했는데 호출할 때 await을 왜 또 하는 건지 의아했었는데 서버 컴포넌트를 비동기적으로 렌더링하는 문법이라고 하네요.

### Preload data

1. > When rendering Page, the component calls getUser but note that it doesn’t use the returned data. This early getUser call kicks off the asynchronous database query that occurs while Page is doing other computational work and rendering children.

   - Profile을 렌더링하기 전에 Page 에서 미리 getUser 비동기 요청을 해서 캐시하는 거라고 이해하면 될까요? 뭔가 신기하네요
   - 저는 return하고 DOM에 Profile 그리기 전에 미리 요청을 시작하는 거라고 생각했어요. 정작 필요한 건 Profile이라 user에 응답받은 값이 들어오면 다시 리렌더링될 것 같긴 합니다. 그래도 조금 아리송하네요 ㅎㅎ

2. > If the initial data request hasn’t been completed, preloading data in this pattern reduces delay in data-fetching.

   - [질문] 여기서 initial data request란 Page에서 getUser 호출을 말하는 걸까요 아니면 Profile에서의 getUser 호출을 말하는 걸까요..?
   - 하이라이트 색상으로 구분하시면 될 것 같아요. Page의 getUser입니다!

3. > ```js
   > async function MyComponent() {
   >   getData();
   >   // ... some computational work
   >   await getData();
   >   // ...
   > }
   > ```
   >
   > Notice that the first getData call does not await whereas the second does. await is a JavaScript operator that will wait and return the settled result of the promise. The first getData call simply initiates the fetch to cache the promise for the second getData to look-up.

   - 이전 예시랑 결국 같은 거군요. 위에서는 부모 컴포넌트와 자식 컴포넌트에 나뉘어져 있어서 전혀 어색함을 못 느꼈는데 여기서는 한 컴포넌트 내에서 연달아 사용하니까 코드가 이상하다는 느낌이 들어요 ㅋㅋ

4. > The first getData call simply initiates the fetch to cache the promise for the second getData to look-up.

   - 위 설명이 잘 이해가 안돼서 질문을 달았는데 이 부분이 더욱 이해가 잘 되네요! initial call은 promise를 캐시하고 두번째 call은 promise의 settled된 데이터를 조회하는 거군요

5. > This is because cache access is provided through a context which is only accessibile from a component.

   - 생각해보니 리액트에서 props말고 값을 공유하는 건 Context뿐인데 캐시를 컨텍스트로 공유할 거라고 미처 생각 못했습니다
   - 컴포넌트 밖에서는 접근할 수 없다는 유의점에 대한 이유를 설명해주는 부분
   - 캐싱값 접근은 컴포넌트 내에서만 된다

6. > When should I use cache, memo, or useMemo?

   - useMemo: 클라이언트 컴포넌트에서 비용이 많이 드는 계산을 캐시하고 싶을 때 사용
   - cache: 서버 컴포넌트에서 메모이제이션이 필요할 떄 사용
   - memo: 컴포넌트를 props가 바뀔 때만 리렌더링하도록 하고 싶을 때 사용
   - 요약해주신 거 정말 깔끔하고 좋습니다!

7. > All mentioned APIs offer memoization but the difference is what they’re intended to memoize, who can access the cache, and when their cache is invalidated.

   - 안 그래도 기존의 메모이제이션과 cache의 다른 점이 무엇인지 궁금했는데 요약해서 정리해주니 좋네요!

## 문제 해결

### My memoized function still runs even though I’ve called it with the same arguments
