# Fragment

## 목차

- [Fragment](#fragment)
  - [목차](#목차)
  - [레퍼런스](#레퍼런스)
    - [`<Fragment>`](#fragment-1)
    - [주의사항](#주의사항)
  - [사용법](#사용법)
    - [Fragment 리스트 렌더링](#fragment-리스트-렌더링)

## 레퍼런스

### `<Fragment>`

1. > Fragment 안에서 그룹화된 엘리먼트는 DOM 결과물에 영향을 주지 않습니다.
   - Vue2를 사용할 때는 Fragment 같은 빈(?) 엘리먼트가 없어서 항상 의미없는 div로 감싸서 불편했는데 좋은 것 같습니다. (Vue3부터는 Fragment를 지원한다고 봤던 것 같기도 합니다..!)

### 주의사항

1. > React는 <><Child /></>에서 [<Child />]로 렌더링하거나 (또는 반대의 경우), 혹은 <><Child /></> 에서 <Child /> 렌더링하거나 (또는 반대의 경우) [state를 초기화](https://ko.react.dev/learn/preserving-and-resetting-state)하지 않습니다. 이는 오직 single level까지 적용됩니다. 예를 들어 <><><Child /></></> 에서 <Child />로 렌더링하는 것은 state가 초기화됩니다. 정확한 semantics는 [여기](https://gist.github.com/clemmy/b3ef00f9507909429d8aa0d3ee4f986b)서 참조하실 수 있습니다.
   - **[질문]**
   - Q1. [ ]로 감싸는 것에도 의미가 있는건가요?
   - Q2. Fragment로 이중중첩 이상부터는 Fragment안의 Child의 state가 보존되지 않는다고 이해해도 될까요?
   - **[답변]**
   - A1. 저도 이런 형태로 렌더링하는 방식은 처음봐서 배열로 감싸는 게 어떤 의미인지 모르겠네요. 아래 코드샌드박스에서 하나 감싸보면 배열이니까 key를 설정하라고 경고가 뜨긴 합니다. 두 번째 질문에 대해서는 저도 같은 의미로 이해했습니다. (수정) 링크 들어가서 보니까 정확히는 렌더링 사이에 구조가 바뀌면 상태가 보존되지 않는거네요!  
     UI 트리에서 위치가 같으면 상태를 보존한다는 것과 관련된 링크입니다. https://ko.react.dev/learn/preserving-and-resetting-state#same-component-at-the-same-position-preserves-state
   - A2. 여러 엘리먼트를 감쌀 때, <>로 해도 되지만, [ ]로 감싸도 되는 것 같아요. 예제에서 아래처럼 수정해보시면 이해가 되실 것 같아요.
     ```
     <>
       <PostTitle title={title} />
       <PostBody body={body} />
     </>
     ```
     ```
     [ <PostTitletitle=title/>, <PostBodybody=body/> ]
     ```
     아래처럼 해도 렌더링이 됩니다.
   - A3. 오 이건 처음 알았네요 신기합니다

## 사용법

### Fragment 리스트 렌더링

1. > [반복을 통해 여러 엘리먼트를 렌더링할 때](https://ko.react.dev/learn/rendering-lists) 각 요소에 key를 할당해야 합니다.
   - key 할당하라는 warning을 자주 봤는데 Fragment로 감싸줘야겠네요..!! 맨날 <>로만 써서 생각을 못하고 있었어요
