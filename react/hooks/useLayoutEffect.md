# useLayoutEffect

**목차**

## 레퍼런스

### useLayoutEffect(setup, dependencies?)

1. > Before your component is added to the DOM,

   - 앞에서 얘기한대로 이 부분 설명이 useEffect와 다릅니다.
   - useEffect는 After인데 useLayoutEffect는 Before라는 차이가 있어요.

2. > The code inside useLayoutEffect and all state updates scheduled from it block the browser from repainting the screen.

   - 브라우저 리페인트와 Effect 훅 실행 순서를 도식화한 참고 자료 올려봅니다! https://github.com/donavon/hook-flow
   - 업데이트 시일은 좀 오래되었는데 공식 문서 설명과 다르지는 않은 것 같습니다.
   - 감사합니다👏

## 사용법

### Measuring layout before the browser repaints the screen

1. > If there’s enough space, the tooltip should appear above the element, but if it doesn’t fit, it should appear below.

   - 이런 경우에 useLayoutEffect 훅이 유용하겠구나 싶네요! 저는 이 훅이 처음인데 생각해보니 useLayoutEffect를 써야할때도 useEffect로 얼레벌레 때우지 않았을까 싶어여..
   - 저도 이 훅을 사용해본 적이 없는데, 최근에 어떻게 풀지 고민했던 ui 깜빡임 문제가 useLayoutEffect 대신 useEffect를 써서 생긴 문제였을것 같다는 생각이 드네요! 월요일에 확인해봐야겠어요 ㅎㅎㅎ
   - 블로그 개발하면서 gatsby를 2에서 5로 마이그레이션하고 있는데 화면이 깜빡이는 이슈가 있었어요. 이게 혹시 useLayoutEffect를 대신 사용해야 하는 부분이 아닌가 싶네요.

2. > 예제 2 of 2: useEffect does not block the browser
   >
   > performance.now()

   - https://developer.mozilla.org/en-US/docs/Web/API/Performance/now
   - 뭔지 궁금해서 찾아보았는데 높은 정확도의 밀리초를 반환한다고 합니다.

## 문제 해결

### I’m getting an error: ”useLayoutEffect does nothing on the server”

1. > The problem is that on the server, there is no layout information.

   - 실무에서 아직 서버 컴포넌트를 사용하지 않다보니 이런 문제를 맞닥뜨린 적이 없는데 서버 사이드 렌더링으로 사용자 경험을 극대화하면서 고민할 지점이 정말 많을 것 같다는 생각이 듭니다.
   - Next.js 도입하긴 했는데 아직 초기라 까다로운 이슈는 맞닥뜨리지 않았습니다.
   - 회사에서 다음에 들어갈 큰 프로젝트에 Next.js를 도입한다고 해서 스터디 중인데 예전에 한 번 써봤을 때보다 개념이 엄청 확장되고 복잡해졌어요.
   - Next.js로 코딩 테스트하는 분들이 hydration 문제 때문에 고생한다고 들었어요.
   - SEO나 서버 사이드 렌더링이 정말 필요한 경우가 아니면 그냥 클라이언트 사이드 렌더링으로 충분할 수도 있어요.
   - 회사에서 인프라를 관리할 때 웹 서비스로 제공하고 싶어하는 니즈가 있어서 마이크로 프론트엔드 경험이 있는 분들과 인터뷰하고 있는데 그 분들 중에도 서버 사이드 렌더링의 필요성을 못 느껴서 vite를 사용해 개발하는 방향으로 되돌아간 분들도 있다고 합니다.
   - 러닝 커브도 좀 높은 것 같고요.

2. > useLayoutEffect for different reasons than measuring layout, consider useSyncExternalStore instead

   - 이 말은 즉, 화면이 채워지기 전에 무언가를 하고 싶을때, 그 뭔가가 레이아웃 측정과 관련이 있는 경우 이 훅을 사용하는게 맞다는 뜻이겠군요. 이 훅의 역할을 명확히 정의해주는것 같아서 표시해봤습니다!
   - 말씀하신 대로 이 부분을 보니 훅 이름을 정말 잘 지었다는 생각이 들어요 ㅎㅎ
   - 훅이 어디서 쓰이는지 잘 알고 써야 겠어요.
   - 저도 이 훅을 써보지 않고 이름만 들었을 때 레이아웃 관련 이슈를 해결하기 위한 훅인가보다 생각했어요.
