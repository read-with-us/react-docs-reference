# useId

**목차**

- [레퍼런스](#레퍼런스)
- [사용법](#사용법)
- [문제 해결](#문제-해결)

## 레퍼런스

### useId
1. > useId는 접근성 어트리뷰트에 전달할 수 있는 고유 ID를 생성하기 위한 React Hook입니다.
    - 접근성을 향상시키기 위한 훅을 따로 만들 줄 몰랐어요. 얼마전에 깃허브도 링크에 밑줄을 추가한다고 하던데 확실히 해외는 접근성에 신경을 많이 쓰는 것 같습니다.

### useId()
1. > useId를 컴포넌트의 최상위에서 호출하여 고유 ID
    - 부득이하게 직접 dom조작을 해야할 일이 있을 때 유용하게 사용할 수 있을 것 같네요. id 네이밍하는 것도 참 괴로웠는데… 좋은 것 같습니다.. ㅎㅎ
    - 오오 그러게요 보통 저러면 prop으로 id 뚫고 컴포넌트 외부에서 넣어주는 식으로 썼던것 같은데 useId로 대체하면 훨씬 깔끔해지겠네요! 외부에서 저런 컴포넌트 내부 동작을 알 필요가 없어져서 컴포넌트 관점에서도 고급(?)져지는 느낌..

## 사용법

### 접근성 어트리뷰트를 위한 고유 ID 생성하기
1. > aria-describedby와 같은 HTML 접근성 어트리뷰트를 사용하면 두 개의 태그가 서로 연관되어 있다는 것을 명시할 수 있습니다.
    - 어떤 속성인지 궁금했는데 바로 아래 설명을 달아두었네요 ㅎㅎ

2. > React에서 ID를 직접 코드에 입력하는 것은 좋은 사례가 아닙니다.
    - useId를 사용하면 ID가 고유하게끔 리액트가 관리해주지만 만약 이런것 없이 사람이 직접 입력하게 되면 이 ID가 다른 곳에서도 쓰였는지 아닌지 관리하기 어렵기 때문이겠죠..?
    - 넵 저도 동일하게 생각했습니다. 같은 코드 내에서야 어떻게든 관리할 수 있지만 라이브러리에서 예상치 못하게 같은 id를 쓰면 정말 난감할 것 같아요
    (추가) 그래서 uuid를 쓰기도 하지만 useId로 전달된 값 보면 ':r1:' 이런 형태라서 무작위가 아닌 점이 장점인 것 같습니다.
    - id를 직접 코드에 입력하는게 좋은 사례가 아니라고 딱히 생각해본적이 없어서 신선하네요… 말씀해주신 이유가 맞는 것 같아요! 작업중에 id 중복을 피하려고 중요하지도 않은 네이밍에 머리를 굴렸던 경험이 있어서 공감이..
    (+) 오 이수님 적어주신 내용처럼 라이브러리에서 같은 id 쓰는 케이스도 충분히 있을 수 있겠군요..!

3. > 영상을 통해 보조 기술을 활용했을 때 사용자 경험의 차이점을 확인할 수 있습니다.
    - 영상이 1분 30초 내외라 잠깐 볼 시간 됩니다. 보이스 오버로 읽는 건 처음 들어봤는데 너무 신기해요!

### useId를 사용하는 것이 카운터를 증가하는 것보다 나은 이유는 무엇일까요? 

1. > React에서 useId는 호출한 컴포넌트의 “부모 경로”에서 생성됩니다. 클라이언트와 서버 트리가 동일한 경우 렌더링 순서와 관계없이 “부모 경로”가 일치하는 이유입니다.
    - [질문] 부모 경로에서 생성된다는 게 무슨 뜻일까요?

### 여러 개의 연관된 엘리먼트의 ID 생성하기

### 생성된 모든 ID에 대해 공유 접두사 지정하기

1. > 여러 개의 독립된 React 애플리케이션을 하나의 페이지에서 렌더링
    - 이런 방식으로 개발한 적이 없는 것 같다고 생각했다가 라이브러리에서 추가하는 모달이 이런 케이스 아닐까 하는 생각이 들었어요.
