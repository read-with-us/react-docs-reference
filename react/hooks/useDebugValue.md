# useDebugValue

**목차**

- [레퍼런스](#레퍼런스)
- [사용법](#사용법)

## 레퍼런스

### useDebugValue
1. > lets you add a label to a custom Hook in React DevTools.
    - 개발자 도구에서만 사용하는 훅이라 사용빈도가 많진 않을 것 같네요. 개발자도구에서 사용할 수 있는 훅을 제공하는 것도 신기하구요 ㅎㅎ 매번 콘솔에 찍어서 확인했는데 이런 방법을 써봐도 괜찮을 것 같아요


## 사용법

### Adding a label to a custom Hook
1. > Call useDebugValue at the top level of your custom Hook to display a readable debug value for React DevTools.
    - 통상적인 개발에서는 디버거에 label까지 붙일 일은 거의 없는 것 같아요. 혹시 이 hook 실제로 사용해 본 적 있는 분이 계신지 궁금하네용
    - 뭔가 리액트개발자도구를 적극적으로 사용하는 편이 아니어서 그런지 용도가 크게 와닿지는 않네요..! 이수님께서 말씀하신것처럼 정말 라이브러리 개발자들을 위한 훅 같은 느낌…

2. > This gives components calling useOnlineStatus a label like OnlineStatus: "Online" when you inspect them:
    - 리액트 개발자 도구에서 커스텀 훅이 어떻게 보이는지 한 번도 생각 못해봤네요 ㅎㅎ
    예제 fork 해서 확인했더니 useDebugValue 훅을 주석처리하면 사진에서 가리키는 부분이 사라집니다. 정말 디버깅할 때만 필요한 훅이네요.

### Deferring formatting of a debug value 

1. > When your component is inspected, React DevTools will call this function and display its result.
    - 포맷팅 함수를 굳이 왜 넣었을까 생각했는데 inspect됐을 때만 호출해서 비싼 계산을 피하도록 하는 장점이 있었군요! 역시 라이브러리 개발하려면 이정도 섬세함이 필요한 것 같습니다
