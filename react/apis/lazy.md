# lazy

**목차**

- [레퍼런스](#레퍼런스)
  - [lazy(load)](#lazyload)
  - [load 함수](#load-함수)
- [사용법](#사용법)
  - [Suspense와 Lazy-loading 컴포넌트](#suspense와-lazy-loading-컴포넌트)
- [문제 해결](#문제-해결)
  - [lazy 컴포넌트의 상태가 의도치 않게 재설정됩니다.](#lazy-컴포넌트의-상태가-의도치-않게-재설정됩니다)

1.  > lazy는 로딩 중인 컴포넌트 코드가 처음으로 렌더링 될 때까지 연기할 수 있습니다.

    - 저는 최근에 react-router-dom 셋팅하면서 처음 사용해본 것 같아요.
    - Q: 혹시 그 외의 케이스에서 사용해보신 분 계신가요?
    - 저는 제가 쓴 적은 없고 본 적만 있어요! react-router-dom 세팅할 때 왜 lazy가 필요하셨나요? 페이지 전환 시에 필요하셨던 건가요?
    - 넵넵 리액트 보일러플레이트 코드에 있길래 사용해본거였는데 이렇게 쓰면 사용자가 필요할때에만(router 이동 요청 시에만) 해당 코드를 불러오게 되어서 불필요하게 처음부터 코드를 불러오지 않게 되어 성능향상 이점이 있다고 하더라구요!
    - 저도 제가 쓴 적은 없는데 본적이 있어서 찾아보고 왔네요 ㅎㅎㅎ 모달이나 바텀시트 등 바로 보이지 않는 컴포넌트들에 쓰는 것 같아요! 라우트에도 쓰고요
    - 오 모달, 바텀시트 예시 들으니까 이해가 잘 됩니다.
    - https://reactrouter.com/en/main/route/lazy

## 레퍼런스

### lazy(load)

1. > React는 먼저 load를 실행한 후 load가 이행될 때까지 기다렸다가 이행된 값의 .default를 React 컴포넌트로 렌더링합니다.

   - 오타인가 싶어서 원문 봤는데 번역에는 문제 없더라구요. 아래 반환값 읽다보니까 default 프로퍼티가 실제로 있다고 합니다.
   - 실제 코드에서 사용된 예를 보니 다음과 같이 쓰고 있어요.

     ```js
     const LazyComponent = lazy(() =>
       import('^/components/fileName').then((module) => ({
         default: module.secondExportedElement,
       })),
     );
     ```

   - 보니까 한 파일 내에서 여러가지를 export하는 경우에 default 지정해주는 것 같아요
   - 안 그래도 궁금했는데 감사합니다. 👍

### load 함수

## 사용법

### Suspense와 Lazy-loading 컴포넌트

1. > 위의 코드는 동적 import()에 의존하므로 번들러 또는 프레임워크의 지원이 필요할 수 있습니다.

   - Next.js와 같은 프레임워크가 필요해 보이는데 그냥 React에서 쓸 때는 웹팩 설정 같은 걸 해야 하는 거겠죠?
   - 번들러나 프레임워크의 지원이 필요하다고 써 있어서 Next.js 안 쓰는 경우에 React에서 어떻게 쓰는 건지 궁금했어요
   - 그런데 프레임워크나 번들러 없는 리액트 환경에서 개발하는 경우가 없을 것 같아요 ㅋㅋ

2. > 이 패턴을 사용하려면 임포트하려는 lazy 컴포넌트가 default 내보내기로 내보내져 있어야 합니다.

   - 원문 보니까 'export default'로 내보내야 한다는 뜻입니다. 혹시 팀 컨벤션이 named export인 분들은 좀 고민이 있을 것 같아요.
   - [질문] 왜 default export로 내보내기 되어야하는 걸까요..?
   - 관련해서 이런 블로그 글이 있는데 아직 읽어보진 못했어요!
   - https://colton68.medium.com/understanding-the-importance-of-default-export-when-using-react-lazy-ae215dc6bc5e
   - 💡default export를 통해 하나의 값을 내보내야 React.lazy()가 컴포넌트 함수 자체를 찾을 수 있고 named export를 통해 여러 값을 내보냈을 때는 컴포넌트 이름을 알아야 함수를 찾을 수 있기 때문인 것 같습니다.
   - 아래는 블로그 원문입니다.
     > So, when you use the default export syntax like export default MovieDetails, the exported value is the MovieDetails component function itself, which is exactly what React.lazy() is looking for.
     > If you were to use a named export instead, like export const MovieDetails = () => {}, the value returned by the import statement would be an object containing all named exports, including MovieDetails. To access the MovieDetails component, you would need to use MovieDetails.MovieDetails.
     > In short, using default export allows you to easily return a single value, which is exactly what you need when lazy loading components with React.lazy().

## 문제 해결

### lazy 컴포넌트의 상태가 의도치 않게 재설정됩니다.
