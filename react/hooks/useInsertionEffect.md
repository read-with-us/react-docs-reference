# useInsertionEffect

**목차**

- [레퍼런스](#레퍼런스)
  - [useInsertionEffect(setup, dependencies?)](#useinsertioneffectsetup-dependencies)
- [사용법](#사용법)
  - [CSS-in-JS 라이브러리에서 동적 스타일 주입하기](#css-in-js-라이브러리에서-동적-스타일-주입하기)

1. > useInsertionEffect는 CSS-in-JS 라이브러리 작성자를 위한 것입니다.

   - 공식에서 CSS-in-JS 사용이라는 명확한 타겟을 둔 hook을 개발하다니 신기하네요.

2. > CSS-in-JS 라이브러리 작업 중에 스타일을 주입할 위치가 필요한 것이 아니라면, useEffect 또는 useLayoutEffect를 사용하세요.

   - 한 번도 짚고 넘어간 적이 없었는데 CSS를 JS로 주입하는 게 CSS in JS라서 라이프사이클에 포함되려면 이런 특별한 훅이 필요한가봐요.
   - css in js 라이브러리를 메인으로 사용하고 있는데도 이런 훅의 존재를 첨 알았어요 ㅎㅎㅎ
   - useInsertionHook은 라이브러리 개발을 위한 훅이라 쓸 일이 없다고 본다.
   - 예를 들어, Next.js에서 이런 부분을 지원해주고 있을 것이고 직접 쓸 일이 없을 것 같다.

## 레퍼런스

### useInsertionEffect(setup, dependencies?)

1. > DOM에 추가되기 전에, layout effects 가 실행되기 전에, React는 setup 함수를 실행합니다.

   - 지난 시간에 본 플로우에서는 InsertionEffect는 나오지 않는데 'React updates DOM' 앞에 실행될 것 같아요.
   - https://github.com/donavon/hook-flow
   - 🚨 영어 원문으로 읽어보니까 DOM에 추가되기 전이 아니라 추가된 이후입니다. 그러면 플로우 상에서 'React updates DOM'과 'Cleanun LayoutEffects' 사이에 실행되는 것이 아닐까 싶습니다.

     > When your component is added to the DOM, but before any layout effects fire, React will run your setup function.

2. > useInsertionEffect 내부에서는 상태를 업데이트할 수 없습니다.
   > useInsertionEffect가 실행되는 시점에 ref는 아직 연결되지 않습니다.

   - 이 두 가지 특성은 독특하네요. 정말 상태랑 아무 관련이 없는 훅이네요.

3. > 매번 모든 cleanup 을 실행하고 setup 하는 다른 Effects 와 달리, useInsertionEffect 는 하나의 컴포넌트에 대해 cleanup 과 setup 을 모두 실행합니다. 그 결과 cleanup 과 setup 이 ‘interleaving’ 됩니다.

   - 인터리빙은 찾아보니까 끼워넣기라는 뜻이라고 하는데 그래도 이 문장의 의미를 잘 모르겠어요.
   - ➕ 스터디 끝나고 좀 더 찾아봤는데 이런 의미인 것 같습니다.
     - useEffect1의 cleanup 실행 -> useEffect2의 cleanup 실행 -> useEffect1의 setup 실행 -> useEffect2의 setup 실행
     - useInsertionEffect1의 cleanup 실행 -> useInsertionEffect1의 setup 실행 -> useInsertionEffect2의 cleanup 실행 -> useInsertionEffect2의 setup 실행
   - https://github.com/reactjs/react.dev/pull/6172

## 사용법

### CSS-in-JS 라이브러리에서 동적 스타일 주입하기

1. > 런타임 <style> 태그 주입은 다음 두 가지 이유로 권장하지 않습니다.
   >
   > 1. 런타임 주입은 브라우저에서 스타일을 훨씬 더 자주 다시 계산하도록 합니다.
   > 2. 런타임 주입이 React 생명주기 중에 잘못된 시점에 발생하면 속도가 매우 느려질 수 있습니다.

   - body 내의 style 태그는 CSSOM을 다시 그리게 해서 그런것 같네요?! https://stackoverflow.com/questions/44371227/when-and-how-do-browsers-render-style-tag-in-body
   - 오 이래서 브라우저 렌더링의 원리를 알아야 되는 거였군요. 예전에 그냥 면접 대비한다고 그냥 외웠는데 🤣

2. > 첫 번째 문제는 해결할 수 없지만 useInsertionEffect를 사용하면 두 번째 문제를 해결할 수 있습니다.

   - 요약하면 다음 두가지 조건이 만족되는 경우 사용하는 목적으로 개발된 hook이군용
   - 1. CSS-in-JS를 이용하여 스타일링
   - 2. 런타임에 <style> 태그를 직접 주입
   - [질문] 그런데 저는 런타임에 style 태그를 직접 주입하는 방식으로 개발을 해본 적이 아예 없어서요. 혹시 이런 경험이 있는 분이 계신지 궁금합니다
   - 뭔가 아무렇지 않게 쓰는 것 중에 사실은 런타임에 스타일을 주입하는 게 있지 않을까 하고 찾아봤는데 styled components가 대표적인 예시인 것 같습니다. 저도 후다닥 찾아본 거라 틀릴 수도 있어요 ㅎㅎ https://bepyan.github.io/blog/2022/css-in-js
   - 일반적으로 이미 배포되어 있는 css-in-js 라이브러리를 사용하기 때문에 이런 방식으로 굳이 개발할 필요가 없었을 것 같기도 합니다.
   - 요새 이런 문제 때문에 zero runtime이나 tailwindcss처럼 유틸리티 퍼스트로 넘어가는 것 같다
   - emotion 쓰다보니까 느려지는 게 체감이 된다
