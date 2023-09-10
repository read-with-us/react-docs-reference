# useContext

**목차**

- [레퍼런스](#레퍼런스)
  - [useContext(SomeContext)](#usecontextsomecontext)
- [사용법](#사용법)
  - [트리의 깊은 곳에 데이터 전달하기](#트리의-깊은-곳에-데이터-전달하기)
  - [Context를 통해 전달된 데이터 업데이트하기](#context를-통해-전달된-데이터-업데이트하기)
  - [fallback 기본값 지정](#fallback-기본값-지정)
  - [트리의 일부 context 오버라이드 하기](#트리의-일부-context-오버라이드-하기)
  - [객체와 함수를 전달할 때 리렌더링 최적화하기](#객체와-함수를-전달할-때-리렌더링-최적화하기)
- [문제 해결](#문제-해결)
  - [컴포넌트에 provider 값이 보이지 않습니다.](#컴포넌트에-provider-값이-보이지-않습니다)
  - [기본값이 다른데도 context가 undefined이 됩니다.](#기본값이-다른데도-context가-undefined이-됩니다)

1. > context를 읽고 구독할 수 있는

   - 구독한다고 하니까 역할이 더 잘 이해되네요. useSubscribe로 이름 지을 수도 있지 않았을까요?

## 레퍼런스

### useContext(SomeContext)

1. > 이 값은 트리에서 호출하는 컴포넌트 위의 가장 가까운 SomeContext.Provider 에 전달된 value로 결정됩니다.

   - 동일한 context를 다중으로 감싸본 적은 없어서 몰랐는데, 그런 경우에는 가장 가까운 부모 프로바이더의 value로 결정되는군요?

2. > provider가 없으면 반환된 값은 해당 컨텍스트에 대해 createContext 에 전달한 defaultValue 가 됩니다.

   - 그동안 프로바이더가 없으면 오류가 발생하는 줄 알고 있었어요!

3. > React는 다른 value을 받는 provider로부터 시작해서 특정 context를 사용하는 모든 자식들을 자동으로 리렌더링합니다.

   - 이 특징(혹은 단점?)이 참 context를 사용하기 조심스럽게 만드는 것 같아요..
   - 상태 라이브러리를 쓰게 만드는 결정적인 이유네요 ㅠㅜ
   - [질문] 다른 분들은 상태 라이브러리 어떤 거 써보셨나요? 그리고 어떤 라이브러리 경험이 가장 좋으셨나요? 저희 팀은 Redux 정말 가볍게 다뤄본 경험밖에 없답니다. 기존 코드에서는 Context로 auth 같은 전역 상태만 다루고 있어요.

     - Recoil과 Zustand를 메인으로 쓰는데 Recoil이 React state와 문법적으로 가장 유사해서 개발 경험이 가장 좋았다. Zustand도 나쁘지 않지만 공식 문서가 좀 불친절해서 사용례를 찾아봐야 한다.
     - Recoil이 아직 베타 딱지를 안 떼서 도입하기 망설여진다.
     - 찾아보니 버전이 0.7.7이네요 ㅎㅎ
     - 취준할 때 Redux를 써 본 적이 있지만 그동안 라이브러리를 쓸 정도로 복잡한 화면을 다루지 않았고 Context만 사용해왔다.
     - 새로운 프로젝트에서 Redux Toolkit을 사용할 예정이다. 보일러 플레이트를 확 줄여준다고 한다.
     - https://npmtrends.com/@reduxjs/toolkit-vs-jotai-vs-react-redux-vs-recoil-vs-redux-vs-zustand
     - React Query로 일종의 상태 관리 라이브러리라고 볼 수 있다. 혹은 GraphQL을 사용한다면 Apollo도 선택지 중 하나다.

   - 프로바이더로 감싸고 있는 하위 컴포넌트는 모두 리렌더링하는 줄 알았는데 useContext로 context를 읽는 컴포넌트만 리렌더링했었네요. 그런데 다시 생각해보니 context를 직접 읽지 않더라도 자식 컴포넌트는 props가 바뀌면서 결국 리렌더링하게 되겠군요.
   - 하단 예시처럼 provider에 전달하는 value가 바뀔때마다 provider로 감싼 모든 컴포넌트들이 다시 리렌더링 되는것 같네요…! https://ko.react.dev/reference/react/useContext#updating-a-value-via-context

## 사용법

### 트리의 깊은 곳에 데이터 전달하기

### Context를 통해 전달된 데이터 업데이트하기

1. > 예제 5 of 5: context 와 reducer를 통한 확장
   >
   > 규모가 큰 앱에서는 컨텍스트와 reducer를 결합하여 컴포넌트에서 특정 상태와 관련된 로직을 분리하는 것이 일반적입니다. 이 예제에서는 모든 “wiring”이 reducer와 두 개의 개별 contexts가 포함된 TasksContext.js에 숨겨져 있습니다.

   - 이 예제 좋네요! 개인 토이 프로젝트에서 지금 context 두개로 app을 감싸 상태 관리하는 부분이 있는데, useReducer 사용하는 방향으로 리팩토링 한 번 해봐야겠습니당. (후기도 공유할게요!)

### fallback 기본값 지정

### 트리의 일부 context 오버라이드 하기

1.  > 트리의 일부분을 다른 값의 provider로 감싸서 해당 부분에 대한 context를 오버라이드 할 수 있습니다.

    - context를 오버라이드 할 수도 있군요! 처음 알았네요

### 객체와 함수를 전달할 때 리렌더링 최적화하기

1. > React가 이 사실을 활용할 수 있도록 login 함수를 useCallback으로 감싸고 객체 생성을 useMemo로 감싸면 됩니다. 이것이 성능 최적화입니다.

   - context 사용시에는 특히나 memoization에 신경써야겠네요..!

## 문제 해결

- 예전에 Context에서 문제 생겼을 때 트러블슈팅에 적혀 있는 게 원인 중 하나 아니었을까. 문제 있으면 한 번 와서 훑어보면 좋을 것 같아요.

### 컴포넌트에 provider 값이 보이지 않습니다.

1. > 이를 확인하려면 window.SomeContext1와 window.SomeContext2을 전역에 할당하고 콘솔에서 window.SomeContext1 === window.SomeContext2인지 확인하면 됩니다.

   - 아까 빌드 모듈에서 문제가 발생할 수 있다는 게 어떤 상황인지 잘 파악이 안 되었는데 디버깅 방법이 나와 있어서 좋습니다.

### 기본값이 다른데도 context가 undefined이 됩니다.
