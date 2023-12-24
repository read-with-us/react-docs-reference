# 목차

- [목차](#목차)
- [renderToString](#rendertostring)
  - [레퍼런스](#레퍼런스)
  - [대안](#대안)
  - [클라이언트 코드에서 renderToString 제거하기](#클라이언트-코드에서-rendertostring-제거하기)

# renderToString

## 레퍼런스

1. > renderToString은 브라우저에서 동작하지만, 클라이언트 코드에서 사용하는 것은 권장하지 않습니다.
   - 앗 그러면 왜 동작하도록 허용해둔 걸까요? 아니면 쓰려고 할 때 에러 메시지를 줄지 🤔
   - 그러게요 권장하지 않으면 애초에 막아두면 되었을텐데 🤔

## 대안

1. > Deno와 최신 엣지 런타임에서 [Web Streams](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API)을 사용하는 경우 [renderToReadableStream](https://ko.react.dev/reference/react-dom/server/renderToReadableStream) 을 사용하세요
   - 안 그래도 node stream과 web stream을 위한 API가 왜 따로 존재하나 궁금했는데, Node.js 환경이 아닌 자바스크립트 런타임 환경 (Deno나 Bun, Next.js를 Edge 런타임으로 사용하는 경우 등등..) 에서 stream을 사용하고 싶은 경우에 필요한거군요
   - Edge 런타임이 뭔지 궁금해서 찾아보았습니다. https://edge-runtime.vercel.app/
   - 오 감사합니다!! 찾아보니 링크해주신 문서 하단에도 이런식으로 'Edge 런타임은 Node.js 가 아니라 JS V8 engine을 사용한다. 그래서 Node.js API들은 사용할 수 없다.'고 명기하고 있네요ㅎㅎ
     > In production, the Edge Runtime uses the JavaScript V8 engine, not Node.js, so there is no access to Node.js APIs.
   - 내용 읽다보니까 생각났는데 알파인 리눅스처럼 경량화된 웹 API 환경과 비슷한 느낌인 것 같습니다.  
     참고 링크: https://www.alpinelinux.org/about/
2. > 서버 환경에서 스트림을 지원하지 않는 경우에도 renderToString을 계속 사용할 수 있습니다.
   - Next.js 코드에서 renderToString을 검색해보니까, 크게 요 두가지 상황에서 사용되는 것 같아요! (사용 케이스가 더 있을수도 있지만 일단 저는 이렇게만 이해했습니다)
     - Next Image 태그 테스트 코드  
       경로 예시: test/unit/next-image-new.test.ts
     - 통합 테스트 코드  
       경로 예시: test/integration/app-tree/pages/\_app.tsx
   - 테스트 환경에서는 스트림을 지원하지 않을테니 요걸 썼나..? 싶네요

## 클라이언트 코드에서 renderToString 제거하기

1. > 클라이언트에서 react-dom/server를 가져오면 불필요하게 번들 크기가 커지므로 피해야 합니다.
   - 아 이런 이유로 피하는 거였네요. 그런데 클라이언트에서 서버 API를 사용하거나 서버에서 클라이언트 API를 사용하는 건 보통 기피할 것 같습니다
