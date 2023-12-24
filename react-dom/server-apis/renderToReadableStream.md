# 목차

- [목차](#목차)
- [renderToReadableStream](#rendertoreadablestream)
  - [shell 외부의 에러로부터 회복하기](#shell-외부의-에러로부터-회복하기)
  - [각기 다른 방식으로 다른 종류의 에러를 처리하기](#각기-다른-방식으로-다른-종류의-에러를-처리하기)
  - [서버 렌더링 멈추기](#서버-렌더링-멈추기)

# renderToReadableStream

## shell 외부의 에러로부터 회복하기

1. > Posts 컴포넌트 혹은 그 내부 어딘가에서 에러가 발생했을 경우, React는 [에러로 부터 회복하려고 할 것입니다:](https://ko.react.dev/reference/react/Suspense#providing-a-fallback-for-server-errors-and-client-only-content)
   - 지난 주 API에서 이 부분 읽을 때 1, 2, 3이 잘 이해가 안 되었는데 연결되어있는 [Suspense 링크](https://ko.react.dev/reference/react/Suspense#providing-a-fallback-for-server-errors-and-client-only-content) 들어가서 읽어보니까 오히려 좀 이해가 됩니다!

## 각기 다른 방식으로 다른 종류의 에러를 처리하기

1. > Error 서브클래스를 직접 만들 수 있고,

   - Q. 문득 궁금한데 에러 클래스를 직접 만드시는 분들은 어떤 이유 때문에 만드시나요?
   - A. 요청 분기에 따라 에러 케이스를 구분해야 하는 일이 생기면 가끔 만듭니다! 서버가 아니라 클라이언트단에서 만들어야 하는 경우는 드문 것 같지만요.. 🤔  
     예를 들어서, 에디터에 파일을 업로드하기 위해 이런 처리가 필요한 상황이고 1, 2, 3 단계를 클라이언트에서 처리해야 했는데, 이 때 각각 어떤 단계에서 에러가 났는지 확인하고 핸들링하기 위해 사용했던 적이 있어요.
     1. 파일을 브라우저에 업로드
     2. 업로드한 파일 소팅
     3. 소팅한 파일을 에디터에 노출하기 위한 형식으로 변경 처리
     4. 서버에 실제 파일 저장 (업로드) 요청

## 서버 렌더링 멈추기

1. > ```tsx
   > const controller = new AbortController();
   > setTimeout(() => {
   >   controller.abort();
   > }, 10000);
   > ```
   - Q. AbortController랑 signal 써 보신 분 있나요? API를 가져오던 중 언마운트될 때 호출을 중단하도록 써 본 적이 있는데 이럴 때 쓰는 게 맞았을까 싶어서요.
   - A1. 아쉽게도 써본적은 없습니다 😢
   - A2. 저도요
