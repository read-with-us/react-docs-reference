## use

**목차**

- [레퍼런스](#레퍼런스)
- [사용법](#사용법)
- [문제 해결](#문제-해결)

## 레퍼런스

### use

1. > use Hook은 현재 React의 canary 채널과 실험 채널에서만 사용할 수 있습니다.
	  - 아직 정식 버전으로 출시되지는 않았군요. 아래 블로그나 RFC 들어가보면 반응이 엄청 뜨거운데 서버 컴포넌트 사용하시는 분들은 얼른 출시되길 바랄 것 같아요. [https://github.com/reactjs/rfcs/pull/229](https://github.com/reactjs/rfcs/pull/229) [https://yceffort.kr/2023/06/react-use-hook](https://yceffort.kr/2023/06/react-use-hook)

### use(resource)

1. > 서버 컴포넌트
	  - 일단 서버 컴포넌트에 대한 이해가 부족해서 이 문서에 대한 이해가 잘 되지 않는 것 같아서 서버 컴포넌트를 먼저 찾아보았어요. 
    [https://tech.kakaopay.com/post/react-server-components/](https://tech.kakaopay.com/post/react-server-components/)
    [https://velog.io/@2ast/React-%EC%84%9C%EB%B2%84-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8React-Server-Component%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0](https://velog.io/@2ast/React-%EC%84%9C%EB%B2%84-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8React-Server-Component%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0)
2. > async 및 await은 await이 호출된 시점부터 렌더링을 시작하는 반면 use는 데이터가 리졸브된 후 컴포넌트를 리렌더링합니다.
	  - 일반적인 비동기 처리와의 차이점이 뭐지 싶었는데 딱 이야기해주는군요! 그런데 사실 개인적으로, 그럼에도 불구하고 async/await 대신 use를 사용해야 하는 이유는 잘 모르겠네요. 🤔
3. > 서버 컴포넌트에서 클라이언트 컴포넌트로 전달된 Promise는 리렌더링 전반에 걸쳐 안정적입니다.
    - 오.. 이건 좀 좋은 것 같습니다. use + 서버 컴포넌트를 잘 활용하면 얻을 수 있는 장점은 "비동기 요청으로 인해 발생하는 불필요한 리렌더링을 줄일 수 있다는 것" 이라고 이야기 하는 것 같은데, 다른 분들도 비슷하게 이해하셨는지 아니면 다른 포인트로 이해하셨는지 궁금해요.
    - 저는 약간 다르게 이해했는데요, "비동기 요청으로 인해 발생하는 리렌더링" 보다는… 리렌더링으로 인해 발생하는 불필요한 비동기 요청을 줄일 수 있다..로 이해했어요😵‍💫
    클라이언트 컴포넌트는 리렌더링 될 일이 많은데 (state가 업데이트 된다던지, useEffect에 뭐가 걸려있다던지.. 서버 컴포넌트에서는 둘 다 사용하지 못한다고 하더라구요) 이런 클라이언트 컴포넌트에서 비동기 작업도 하면 해당 컴포넌트가 리렌더링 될 때마다 불필요한 비동기 작업도 수행된다..! 하는 뜻으로 이해했어요. 그래서 가능하면 서버 컴포넌트에서 비동기 처리하여 데이터를 클라이언트 컴포넌트에 전달하는 방식을 추천하는거 아닐까 하고 생각했습니당
    - 서버 컴포넌트 관련하여 아래 두 글(하나는 원문 하나는 번역본)을 어제 한 번 후루룩 읽었는데 무슨 말인지 하나도 못 알아들었습니다.. 다른 분들은 이해하실거라 믿으며 남겨봅니다…
    [https://www.joshwcomeau.com/react/server-components/](https://www.joshwcomeau.com/react/server-components/)
    [https://yozm.wishket.com/magazine/detail/2271/](https://yozm.wishket.com/magazine/detail/2271/)

## 사용법

### use를 사용하여 context 참조하기

1. > use를 사용하여 context 참조하기
	  - 학습하기쪽에서 스포일러(?) 텍스트만 봤을 땐 서버와 통신할 때에만 사용되는 줄 알았는데 사용법이 다양해서 재밌네요 !
2. > useContext는 컴포넌트의 최상위 수준에서 호출해야 하지만 use는 if와 같은 조건문이나 for과 같은 반복문 내부에서 호출할 수 있습니다
    - 이런 차이가 있었군요..
    - [질문] Context를 조건부로 호출하면 어떤 장점이 있을까요? useContext를 사용해도 컴포넌트를 분리하면 조건부로 호출할 수 있을 것 같아서 뚜렷한 차이를 잘 모르겠습니다.
    - 그것도 방법이겠네요..🤔 컨텍스트를 거의 사용하지 않다보니.. 개인적으로는 use로 컨텍스트를 좀 더 유연하게 사용할 수 있도록 한 것 아닐까 싶어욥
3. > `useContext`와 마찬가지로 `use(context)`는 호출 컴포넌트의 **위에서** 가장 가까운 context provider를 찾습니다. 위쪽으로 검색하며 `use(context)`를 호출 컴포넌트 내부의 context provider는 고려하지 **않습니다**.
    - 번역이 너무 이상해서 🥲 원문 옮겨놓습니다.
      > Like `useContext`, `use(context)` always looks for the closest context provider _above_ the component that calls it. It searches upwards and **does not** consider context providers in the component from which you’re calling `use(context)`.
    - 👏
    
### 서버에서 클라이언트로 데이터 스트리밍하기

1. > 서버 컴포넌트에서 클라이언트 컴포넌트로 Promise prop을 전달하여 서버에서 클라이언트로 데이터를 스트리밍할 수 있습니다.
    - 진짜 흥미롭네용 Next.js같은 메타프레임워크 없이 리액트만으로도 SSR 구현이 어렵지 않게 가능해질듯..?! 🤔
    - 서버에서 클라이언트로 데이터를 스트리밍한다는 문구가 인상 깊습니다!
2. > `<Message messagePromise={messagePromise} />`
    - 여기서 fetchMessage()가 리졸브 되어야 컴포넌트가 렌더링 된다. 정리하는 겸 적어둡니다…!
    - 👏
3. > 클라이언트 컴포넌트는 prop으로 받은 Promise를 use Hook에 전달합니다.
    - 컬러 하이라이팅.. 리액트 문서 너무 친절해서 매번 놀라네요 ㅎㅎㅎ👍
    - ㅎㅎ 맞습니다. 코드랑 설명이랑 번갈아 보다 보면 보기 어려울 때 있는데 하이라이팅해줘서 좋아요!
    - 오..맞아요 저도 색깔로 구분해주니까 한 눈에 들어와서 좋다고 생각하고 있었어요
4. > 서버 컴포넌트에서 클라이언트 컴포넌트로 Promise를 전달할 때 리졸브된 값이 직렬화 가능해야 합니다. 함수는 직렬화할 수 없으므로 Promise의 리졸브 값이 될 수 없습니다.
    - 나중에 따로 찾아보려고 킵해둡니다
5. > 직렬화
    - 컴퓨터 메모리 상에 존재하는 객체(Object) -> 문자열(string) 로 변환하는 것
    - JS 객체를 JSON 문자열로 변경 (Serialization)
    - 오! 마침 찾아보려고 했는데 감사합니다. 자주 보는 용어인데도 돌아서면 까먹었던 것 같아요 🤣
    - 직렬화라는 말은 정말..한글임에도 무슨 말인지 모르겠어요ㅠㅠ
6. > 서버 컴포넌트에서 클라이언트 컴포넌트로 Promise를 전달할 때 리졸브된 값이 직렬화 가능해야 합니다.
    - 서버 컴포넌트는 서버에서 해석되어 JSON 형식으로 직렬화되고, 클라이언트 컴포넌트에서 스트림 형식으로 구독하기 때문에 직렬화되는 값만 가능하다고 하네요.
7. > Promise는 서버 컴포넌트에서 클라이언트 컴포넌트로 전달될 수 있으며 use Hook을 통해 클라이언트 컴포넌트에서 리졸브됩니다. 또한 서버 컴포넌트에서 await을 사용하여 Promise를 리졸브하고 데이터를 클라이언트 컴포넌트에 prop으로 전달하는 방법도 존재합니다.
    - 여기까지 쭉 보면서 드는 생각을 정리해보았습니다.
      - 클라이언트 렌더링: async/await을 모두 클라이언트에서 실행
      - 서버사이드 렌더링: async/await이 모두 서버에서 실행
      - use 훅: 서버에서 async가 실행되고 클라이언트에서 await이 실행
	    이렇게 이해했는데 맞을까요?
8. > 하지만 서버 컴포넌트에서 await을 사용하면 await 문이 완료될 때까지 렌더링이 차단됩니다. 
    - use 훅이 나온 배경인 것 같아요. 나중에 직접 써보면 차이점을 느낄 수 있을 것 같기도 하고 🤔

### 거부된 Promise 처리하기

### promise.catch를 사용하여 대체 값 제공하기

## 문제-해결

1. 
   > “Suspense Exception: This is not a real error!”
   
   - 에러 메시지가 약간 명확하지 않은 느낌? 어떻게 조치해야할지 바로 떠오르지 않을 것 같아요.
