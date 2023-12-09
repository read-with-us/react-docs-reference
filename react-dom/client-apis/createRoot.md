# createRoot

**목차**

- [레퍼런스](#레퍼런스)
  - [root.unmount()](#rootunmount)
- [사용법](#사용법)
  - [Updating a root component](#updating-a-root-component)
- [문제 해결](#문제-해결)
  - [“대상 컨테이너가 DOM 엘리먼트가 아닙니다” 라는 오류가 발생합니다.](#대상-컨테이너가-dom-엘리먼트가-아닙니다-라는-오류가-발생합니다)

## 레퍼런스

### root.unmount()

1. > 온전히 React만으로 작성된 앱에는 일반적으로 root.unmount에 대한 호출이 없습니다.
   - 여태껏 일부 페이지만 react로 작성해본 경험이 없고 full react 프로젝트만 작업해와서 그런지 root.render는 익숙한데 root.unmount는 뭔가 생소하네요.
   - 에이전시에서 jQuery로 되어있는 프로젝트에 일부만 React로 넣는 경우가 있었어요. 해당 페이지를 벗어나는 경우에 unmount를 사용할 수 있겠다 생각했습니다!

## 사용법

### Updating a root component

1. > 컴포넌트 트리 구조가 이전 렌더링과 일치하는 한, React는 기존 state를 유지합니다.
   - 지난 주에는 예제랑 이 설명을 보면서 신기하다고 느꼈는데 링크를 따라가서 읽어보고 상태는 컴포넌트가 아니라 리액트가 관리하고 트리 위치에 연결시킨다는 걸 떠올렸더니 전혀 어색하지 않은 문장이라고 다시 생각하게 되었어요!

## 문제 해결

### “대상 컨테이너가 DOM 엘리먼트가 아닙니다” 라는 오류가 발생합니다.

1. > 오타가 있는지 확인하세요!
   - 갑자기 든 생각인데 에러 중에 오타로 인한 에러가 제일 찾기 힘든 것 같아요. 너무 엉뚱하다보니 에러 메시지도 이상하고 ㅎㅎ
   - 맞아요 오타로 인한 에러 고칠 때가 문제 대비 원인 찾는 시간도 오래 걸리고… 다 고치고 나서 가장 허탈한 것 같네유…ㅎㅎ
   - vscode extension인 [code spell checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)를 사용하면 사전에 존재하는 단어가 아닐 시 밑줄을 그어주어 오타 체크하기에 좋아요!
2. > 번들의 &lt;script&gt; 태그는 HTML에서 그보다 뒤에 있는 DOM 노드를 “인식할” 수 없습니다.
   - 그래서 보통 body 끝에서 script를 호출한다고 알고 있는데 리액트를 쓰면서도 이런 문제가 발생할 수 있을까요?
   - react를 사용할 땐 script 태그 내의 이러한 순서를 크게 신경쓰지 않았던 것 같습니다.
   - script를 커스텀하게 넣을 때 주의하라 정도로 이해했어요! 스크립트 cdn 주소를 넣는 경우처럼...
