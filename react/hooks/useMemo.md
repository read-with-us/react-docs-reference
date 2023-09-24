# useMemo

**목차**

- [Reference](#reference)
  - [useMemo(calculateValue, dependencies)](#usememocalculatevalue-dependencies)
- [Usage](#usage)
  - [Skipping expensive recalculations](#skipping-expensive-recalculations)
  - [Skipping re-rendering of components](#skipping-re-rendering-of-components)
  - [Memoizing a dependency of another Hook](#memoizing-a-dependency-of-another-hook)
  - [Memoizing a function](#memoizing-a-function)
- [Troubleshooting](#troubleshooting)
  - [My calculation runs twice on every re-render](#my-calculation-runs-twice-on-every-re-render)
  - [My useMemo call is supposed to return an object, but returns undefined](#my-usememo-call-is-supposed-to-return-an-object-but-returns-undefined)
  - [Every time my component renders, the calculation in useMemo re-runs](#every-time-my-component-renders-the-calculation-in-usememo-re-runs)
  - [I need to call useMemo for each list item in a loop, but it’s not allowed](#i-need-to-call-usememo-for-each-list-item-in-a-loop-but-its-not-allowed)

1. > cache

   - 메모이제이션도 어떤 의미인지 알고 있지만 캐시라는 표현이 더 역할을 이해하기 쉽게 해주는 것 같아요.

## Reference

### useMemo(calculateValue, dependencies)

1. > This should be fine if you rely on useMemo solely as a performance optimization. Otherwise, a state variable or a ref may be more appropriate.

   - 어떨 때 useMemo를 사용하는게 적절할지, 어떨 땐 사용하는게 투머치한지 판단하기가 개인적으로 어렵더라구요.. 라고 썼는데 아래에 어떨 때 사용하는게 적절할지도 나와있군요 ㅎㅎ 친절한 리액트 문서
   - 예시를 보면 다 이해되는 것 같은데 막상 실무에서는 여기서 써도되나 하는 생각이 들고 머릿속이 복잡해집니다.

## Usage

### Skipping expensive recalculations

1.  > How to tell if a calculation is expensive?
    >
    > ```js
    > console.time('filter array');
    > const visibleTodos = filterTodos(todos, tab);
    > console.timeEnd('filter array');
    > ```

    - 이 본문 내의 예시에서 소개된 console.time('..'), console.timeEnd('..') 으로 소요 시간을 측정하는 방법이 유용해 보이네요
    - 보통 log, warn, error 정도 메서드만 써봐서 문득 궁금해서 찾아봤습니다. 생각보다 메서드 종류가 많네요!
    - https://developer.mozilla.org/ko/docs/Web/API/console#%EB%A9%94%EC%84%9C%EB%93%9C

2.  > If the overall logged time adds up to a significant amount (say, 1ms or more), it might make sense to memoize that calculation.

    - 대략적인 기준을 제시해줘서 좋네요. 리액트 문서는 참 친절한 거 같아요…^^
    - 맞아요. 모호한 부분을 거의 안 남겨서 좋습니다!
    - 동의합니다! 1ms가 소요되면 useMemo를 사용하면 된다! 라는 기준을 딱 제시해주니 좋아요 (물론 예외도 있겠지만요)

3.  > useMemo won’t make the first render faster. It only helps you skip unnecessary work on updates.

    - useMemo의 목적을 다시 한번 상기시켜주어서 좋네요! useMemo하면 막연히 성능 최적화…라는 생각이 드는데 첫번째 렌더링을 빠르게 하는 것이 아니라 불필요한 업데이트를 막는 hook이라는 걸 기억해야겠습니다
    - 최초 연산을 빠르게 하는 것은 아닌만큼, 별도의 최적화가 필요 없는 값까지 전부 useMemo로 감싸버리는 것은 오히려 성능 저하를 일으킬 수 있겠다는 생각이 드네요. 정확한 case에 명확한 목적으로 사용하는게 중요한 hook 같습니다

4.  > Keep in mind that your machine is probably faster than your users’ so it’s a good idea to test the performance with an artificial slowdown. For example, Chrome offers a CPU Throttling option for this.

    - 예시로 크롬 브라우저 기능도 알려주고,, 좋네요. 안써봤는데 성능 측정할 때 한번 써봐야겠어요

5.  > To get the most accurate timings, build your app for production and test it on a device like your users have.

    - 이런 점도 유의를 해서 테스트해야겠네요!
    - 모바일 앱 개발하는 회사들은 실제 기기와 개발환경에서 차이가 발생하는 문제 때문에 종류별로 폰을 구비해 놓는다고 들었어요.
    - 모바일 앱 개발하는 경우 기종에 따라 버전에 따라 대응해줘야하는 조합이 너무 많음
    - 예전 버전은 최신 버전으로 업그레이드하시라 권고할 수 있는데 최신 버전은 답이 없음

6.  > Should you add useMemo everywhere?

    - 저도 useMemo가 공짜로 최적화를 해주는건 아니라고 어디서 들었어서 useMemo를 신중하게 사용하려는 편이에요. 참고 하실만한 관련 아티클(번역본)을 공유합니다.
    - https://velog.io/@lky5697/stop-using-usememo-now
    - 앗 저도 이 글을 봤었는데..! 예전엔 값이 좀 복잡하다 싶으면 무조건 memo로 둘렀는데 이 글을 읽고 난 이후에 memo 사용하는 것이 조심스러워지더라구요.
    - 블로그 글 썸네일에서 작성자의 진심이 묻어나오네요ㅋㅋㅋㅋ

7.  > Optimizing with useMemo is only valuable in a few cases:

    - 사용할 필요가 있는지 고려할 때 여기 나오는 케이스를 참고하려고 리마인드용으로 남겨둡니다.

8.  > The downside of this approach is that code becomes less readable. Also, not all memoization is effective: a single value that’s “always new” is enough to break memoization for an entire component.

    - 너무 남용하면 메모리 관련해서 큰 문제가 생길 것 같았는데 의외로 가독성 혹은 무효화되는 문제밖에 없다고 되어 있네요 🧐
    - 메모 때문에 추가 연산이 발생해도 이슈가 될 정도는 아니다라는 느낌

9.  > When a component visually wraps other components, let it accept JSX as children. This way, when the wrapper component updates its own state, React knows that its children don’t need to re-render.

    ```js
    <Card>
      <Avatar />
    </Card>
    ```

    - 요런걸 말하는거네요! useMemo 없이도 최적화 할 수 있는 좋은 방법이라고 생각해요
    - [질문]하단에 "기본적으로 컴포넌트가 리렌더링 되면 React는 모든 자식 컴포넌트를 재귀적으로 리렌더링합니다." 라는 부분이 있는데, 이 경우 자식 컴포넌트를 memo로 감싸서 props가 변하지 않으면 리렌더링을 건너뛰도록 해결한다고 되어있습니다. 위 설명과 배치되는 내용이라고 생각하는데 별개의 얘기인걸까요?
    - 별개의 얘기라고 생각해요. 대충 예시를 만들어봤는데.. https://codesandbox.io/s/youthful-hellman-qtppkp?file=/Demo.tsx
    - 컴포넌트 합성에 이런 식으로 컴포넌트 자체를 넘겨주는 방식이 소개되었던 기억이 납니다. https://ko.legacy.reactjs.org/docs/composition-vs-inheritance.html
    - 재사용의 이점만 생각했는데 최적화 관점에서도 이점이 있을 수 있겠네요 👍
    - https://fe-developers.kakaoent.com/2022/220731-composition-component/ 합성컴포넌트 작성에 대해 소개하는 글인데 참고가 될 것 같아요!

10. > Most performance problems in React apps are caused by chains of updates originating from Effects that cause your components to render over and over.

    - useEffect를 무분별하게 쓰지 말아야겠다는 다짐을 또 한번 하게되는 부분이네요…😅

11. > React Developer Tools profiler

    - [질문] React 프로파일러를 사용하거나 최적화해보신 분 있나요?
    - 에디터 작업을 할 때 메모리 문제가 생겨서 프로파일링 툴을 사용해봤습니다.
    - 렌더링 하이라이트 기능은 종종 사용해요.

12. > In the long term, we’re researching doing granular memoization automatically to solve this once and for all.

    - 메모이제이션, 최적화가 자동으로 이루어진다면 정말 편할 것 같네요

13. > This is because JSX nodes are immutable. A JSX node object could not have changed over time, so React knows it’s safe to skip a re-render.

    - [질문] 왜 JSX node object는 불변성을 띄는 걸까요?
    - React 자유도가 너무 높아서 쓰는 사람에 따라 이슈가 많이 터지는데 JSX 노드까지 건드릴 수 있게 했으면 문제가 많이 생길 것 같다
    - 이전 것을 그대로 쓰지 않는다는 점에서 불변한다고 하는 것 같다

### Skipping re-rendering of components

1. > Normally, this wouldn’t be a problem, but it means that List props will never be the same, and your memo optimization won’t work. This is where useMemo comes in handy:

   - [질문]컴포넌트 자체에 memo를 적용했을 때, 이전 렌더링과 동일한 prop이 있는 경우 리렌더링을 건너뛰도록 한다고 했는데, 왜 이 경우에 memo 최적화는 작동하지 않느다고 하는 걸까요..?
   - 필터링하면서 새롭게 생성된 배열은 Object.is로 비교했을 때 동일하지 않으니까 props 변경이 일어났다고 봐서 그런 것 같습니다!

2. > Manually wrapping JSX nodes into useMemo is not convenient. For example, you can’t do this conditionally. This is usually why you would wrap components with memo instead of wrapping JSX nodes.

   - useMemo를 이용해 컴포넌트를 메모이제이션 하는 것, memo를 이용해 컴포넌트를 메모이제이션 하는 것 사이에 성능상의 차이는 없는 모양이네요. 작업 편리성때문에 memo를 권장할 뿐인 것 같군요
   - 앗 그런데 useMemo가 훅이니까 memo를 사용하는 것과 달리 조건부로 렌더링할 수 없다는 차이가 있다고 말하는 것 같아요.
   - 🚨 이 부분 달아주신 설명을 잘못 이해하고 댓글 달았어요. 말씀해주신 작업 편리성에 대한 부연설명으로 이해해주시면 될 것 같습니다!

### Memoizing a dependency of another Hook

1. > To fix this, you could memoize the searchOptions object itself before passing it as a dependency:

   - useEffect 의존성으로 object를 사용하면 안된다는 내용을 보고 이후에 의존성으로 들어갈 object 처리에 애먹었었는데 이런 식으로 처리하면 되는거였네요…. 👁️

### Memoizing a function

1. > Memoizing functions is common enough that React has a built-in Hook specifically for that. Wrap your functions into useCallback instead of useMemo to avoid having to write an extra nested function

   - useMemo와 useCallback 모두 캐싱을 위한 hook이지만 useCallback은 함수 캐싱을 위한 hook이라는 것이 확 와닿네요
     - useMemo: 함수를 호출하고 그 결과를 캐싱
     - useCallback: 함수 자체를 캐싱

## Troubleshooting

### My calculation runs twice on every re-render

### My useMemo call is supposed to return an object, but returns undefined

### Every time my component renders, the calculation in useMemo re-runs

### I need to call useMemo for each list item in a loop, but it’s not allowed
