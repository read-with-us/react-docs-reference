# useRef

**목차**

- [레퍼런스](#레퍼런스)
  - [useRef(initialValue)](#userefinitialvalue)
- [사용법](#사용법)
  - [ref로 값 참조하기](#ref로-값-참조하기)
  - [ref로 DOM 조작하기](#ref로-dom-조작하기)
  - [ref 콘텐츠 재생성 피하기](#ref-콘텐츠-재생성-피하기)
- [문제 해결](#문제-해결)
  - [커스텀 컴포넌트에 대한 ref를 얻을 수 없습니다](#커스텀-컴포넌트에-대한-ref를-얻을-수-없습니다)

## 레퍼런스

### useRef(initialValue)

1. > ref 객체를 JSX 노드의 ref어트리뷰트로 React에 전달하면 React는 current프로퍼티를 설정합니다.
   - 이 부분은 막연하게 React가 자동으로 설정해주는 건가 생각했는데 예상이 맞았어요!

## 사용법

### ref로 값 참조하기

1. > ref는 컴포넌트의 시각적 출력에 영향을 미치지 않는 정보를 저장하는 데 적합

   - ref의 용도에 대해 설명할 때 횡설수설했었는데 명확한 설명이네요..!

2. > ref를 사용하면 다음을 보장합니다:
   >
   > - (렌더링할 때마다 재설정되는 일반 변수와 달리) 리렌더링 사이에 정보를 저장할 수 있습니다.
   > - (리렌더링을 촉발하는 state 변수와 달리) 변경해도 리렌더링을 촉발하지 않습니다.
   > - (정보가 공유되는 외부 변수와 달리) 각각의 컴포넌트에 로컬로 저장됩니다.

   - useRef를 사용하는 경우를 판별하는 기준이 되는 특징인 것 같아, 리마인드 목적으로 어노테이션 달아둡니다!

### ref로 DOM 조작하기

1. > 예제 2 of 4: 이미지 스크롤하기
   >
   > scrollIntoView

   - React는 아니고 Web API이긴 한데 처음 보고 신기해서 남겨봅니다! 엘리먼트가 사용자에게 보이도록 상위 컨테이너를 스크롤해준다고 하네요. https://developer.mozilla.org/ko/docs/Web/API/Element/scrollIntoView
   - 지금 사용 중인 hypothesis 어노테이션 카드 클릭했을 때 이동하는 것도 scrollintoview 메서드 사용 중일 것 같아요.

### ref 콘텐츠 재생성 피하기

1.  > ```js
    > const playerRef = useRef(null);
    > if (playerRef.current === null) {
    >   playerRef.current = new VideoPlayer();
    > }
    > ```

    - 이런 최적화 방법이 있군요. state나 ref나 초기값이 한 번만 쓰이니까 호출도 한 번만 된다고 무의식적으로 생각하고 있었어요

2.  > 일반적으로 렌더링 중에 ref.current를 쓰거나 읽는 것은 허용되지 않습니다. 하지만 이 경우에는 결과가 항상 동일하고 초기화 중에만 조건이 실행되므로 충분히 예측할 수 있으므로 괜찮습니다.

    - 생각보다 관대하군요.. 결과적으로 '초기화중에만 ref를 렌더링한다' 는 동작만 보장이 되면 괜찮은가보네요.
    - state와 다르게 렌더링 사이에 값이 저장되니까 ref 초기화가 저런 식으로 처리가 가능한 것 같습니다.

## 문제 해결

### 커스텀 컴포넌트에 대한 ref를 얻을 수 없습니다

1.  > 기본적으로 컴포넌트는 내부의 DOM 노드에 대한 ref를 외부로 노출하지 않습니다. 이 문제를 해결하려면 ref를 가져오고자 하는 컴포넌트를 찾으세요:
    >
    > ```js
    > export default function MyInput({ value, onChange }) {
    >   return <input value={value} onChange={onChange} />;
    > }
    > ```
    >
    > 그런 다음 아래와 같이 forwardRef로 감싸세요:
    >
    > ```js
    > const MyInput = forwardRef(({ value, onChange }, ref) => {
    >   return <input value={value} onChange={onChange} ref={ref} />;
    > });
    > ```

    - fowardRef는 커스텀 컴포넌트의 ref를 가져오기위해서 사용하는군요! 리마인드 차원에서 남깁니다.
    - 최근에 회사에서 디자인시스템 컴포넌트에 ref를 prop으로 뚫어놨는데 적용이 안된다고 문의가 들어왔는데, forwardRef로 감싸지 않아 적용이 안되고 있었더라구요..
    - 오 그래도 사용하면서 문의가 들어온다니 부럽습니다. 저희는 잘 쓰시지도 않고 피드백도 안 들어와서 고민이에요 ㅎㅎ 그대로 쓰지 않고 조금씩 디자인이 달라지다 보니 개발자분들도 결국 스타일을 오버라이드하게 되면서 각자 편한대로 쓰게 되는 것 같아요.
    - 저희도 작년에 만들어두었는데 그동안 잠잠하다가 큰 프로젝트에 디자인 시스템을 적용하기로 하면서 사용자 피드백이 좀 들어오고 있어요. 근데 커스텀 엄청 하는 건 결국 똑같아요 ㅎㅎ
