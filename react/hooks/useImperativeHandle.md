# useImperativeHandle

**목차**

- [레퍼런스](#레퍼런스)
  - [useImperativeHandle(ref, createHandle, dependencies?)](#useimperativehandleref-createhandle-dependencies)
- [사용법](#사용법)
  - [부모 컴포넌트에 커스텀 ref핸들 노출](#부모-컴포넌트에-커스텀-ref핸들-노출)
  - [사용자 정의 imperative 메서드 노출](#사용자-정의-imperative-메서드-노출)

1. > useImperativeHandle은 ref로 노출되는 핸들을 사용자가 직접 정의할 수 있게 해주는 React 훅입니다.

   - 이 친구는 완전 초면이라 언제 나온 hook이지? 하고 찾아봤는데 생각보다 꽤 됐네요. 아마 hook 처음 나왔던 16.8v부터 있었던 것 같아요..! (추측) https://ko.legacy.reactjs.org/docs/hooks-reference.html
   - 오오 저도 이번에 처음 보는 친구네요..!

## 레퍼런스

### useImperativeHandle(ref, createHandle, dependencies?)

## 사용법

### 부모 컴포넌트에 커스텀 ref핸들 노출

1. > ```js
   > useImperativeHandle(
   >   ref,
   >   () => {
   >     return {
   >       focus() {
   >         inputRef.current.focus();
   >       },
   >       scrollIntoView() {
   >         inputRef.current.scrollIntoView();
   >       },
   >     };
   >   },
   >   [],
   > );
   > ```

   - 이럴 때 사용한다고 들었던 기억이 나요. 예시 코드를 보니까 어떻게 쓰는지 확 와닿네요!

2. > 이제 부모 컴포넌트가 MyInput에 대한 ref를 가져오면 focus 및 scrollIntoView 메서드를 호출할 수 있습니다. 그러나 기본 `<input>` DOM 노드의 전체 엑세스 권한은 없습니다.

   - 공통 컴포넌트나 디자인 시스템 개발하는 경우 필요에 따라 사용하면 좋겠네요! 대규모 서비스에서는 컴포넌트에 대한 사용자의 사용례를 한정하는 것도 관리 포인트를 줄이는 방법이 될 수 있으니까요. 🤔
   - 개발하면서 항상 느끼는 게 사용자를 절대 믿으면 안 된다. 사용례를 한정하는게 확실히 좋을 때가 있다는 거에요.

### 사용자 정의 imperative 메서드 노출

1. > 이렇게 하면 부모 Page에서 버튼을 클릭할 때 댓글 목록을 스크롤하고 입력 필드에 초점을 맞출 수 있습니다.

   - 여러 기능을 묶어서 더 추상화된 기능을 제공하고 싶을 때에도 적절해 보입니다.

2. > prop으로 표현할 수 있는 것은 ref를 사용하지 마세요. 예를 들어 Modal 컴포넌트에서 { open, close } 와 같은 imperative handle을 노출하는 대신 `<Modal isOpen={isOpen} />`과 같은 isOpen prop을 사용하는 것이 더 좋습니다. Effects를 사용하면 prop을 통해 명령형 동작(imperative behavior)을 노출할 수 있습니다.

   - 어디까지나 ref는 escape hatches일 뿐이다.. 꼭 기억하기✏️
   - Hooks 소개 페이지에서도 이 훅은 사용 빈도가 낮다고 나와 있어요. https://ko.react.dev/reference/react#ref-hooks
