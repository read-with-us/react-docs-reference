# hydrateRoot

## hydrateRoot
1. >  hydrateRoot
   - next나 remix 같은 프레임워크 없이 ssr을 구현하고 싶을때 사용하는 기능일것 같다
   - 개인도 사용할 수 있지만 프레임워크에서도 내부적으로 이 API를 써서 SSR을 구현할 것 같다.
   - Next.js 깃허브에서 검색해보니까 실제로 사용중 관련링크: (https://github.com/search?q=repo%3Avercel%2Fnext.js%20hydrateRoot&type=code)
   - 프레임워크 배제하고 사용한다면 고려할게 많은것같다...
   - react dom 쪽 API는 프레임워크 만드는 분들이 쓰는 것 같다...
     
2. > hydrateRoot(domNode, reactNode, options?)
   - hydrate는 reactNode, domNode, callback? 순서입니다.
     
3. > 서버 환경에서 React로 앞서 만들어진 HTML에 후에 만들어진 React를 hydrateRoot를 호출해 “붙입니다”.
   - 원문참조시 해석에 더 도움 ^_^
   - Call hydrateRoot to “attach” React to existing HTML that was already rendered by React in a server environment.
  
4. > root.unmount를 호출해 React에게 삭제된 컨텐츠들을 “그만” 다루라고 알려주어야 합니다. 그렇지 않으면 삭제되어버린 React root 내부의 컴포넌트들은 삭제되지 않을 것이며, “구독”처럼 컴퓨팅 자원을 자유롭게 놓아주지 못하게 됩니다.
   - 원문참조시 해석에 더 도움 ^_^
   - Call hydrateRoot to “attach” React to existing HTML that was already rendered by React in a server environment.

5. > 주로 아래와 같은 원인으로 hydration 에러가 일어납니다.
   > - React를 통해 만들어진 HTML의 root node안에 새 줄같은 추가적인 공백.
   > - -typeof window !== 'undefined'과 같은 조건을 렌더링 로직에서 사용함.
   > - window.matchMedia같은 브라우저에서만 사용가능한 API를 렌더링 로직에 사용함.
   > - 서버와 클라이언트에서 서로 다른 데이터를 렌더링함.
   - 하이드레이션을 쓸 때는 이런 지점에서 주의해야 한다 리마인드!!
   - 서버에서 내려준 html이 있고, hydrateRoot를 통해서 트리를 비교 후 리액트 컴포넌트 로직을 붙이는 것이라서 구조가 다르면 엉뚱한 곳에 이벤트 핸들러가 붙을 수 있는것 같다.
   - Q: 서버 html과 react 컴포넌트 트리를 비교하는 과정이 궁금해지네요.
   - A: 리액트 github 관련링크 : (https://github.com/facebook/react/blob/6c7b41da3de12be2d95c60181b3fe896f824f13a/packages/react-dom/src/client/ReactDOMRoot.js#L257C17-L257C28)


### 🤷‍♀️ 질문
1. >대부분의 경우 flushSync를 사용하지 않을 수 있으므로 최후의 수단으로 사용하세요.
   - Q: 서버 html과 react 컴포넌트 트리를 비교하는 과정이 궁금해지네요.
   - A: 학습하기에 예제, 아이템을 추가하는 시점이랑 스크롤하는 시점을 맞추기 위해 사용하는 케이스 관련링크 : (https://ko.react.dev/learn/manipulating-the-dom-with-refs#flushing-state-updates-synchronously-with-flush-sync)
