# 'use server'

## 목차

- [Reference](#reference)
  - ['use server'](#use-server)
- Usage

## Reference

### `'use server'`

1. > Add 'use server'; at the very top of an async function to mark that the function can be executed by the client.

   ```javascript
   async function addToCart(data) {
     "use server";
     // ...
   }

   // <ProductDetailPage addToCart={addToCart} />
   ```

   - 엇 클라이언트 컴포넌트처럼 최상단에서 선언해야 하는 줄 알았는데 잘못 알고 있었네요.
   - 파일 맨 위에 쓰는 건 그렇다 치는데,,, 전 이게 안 이뻐보인다는 생각이 계속 드네요 ㅋㅋㅋㅋㅋ
   - ㅋㅋㅋ공감이에요 안써봐서 그런가 왜 이리 어색해 보이는지…!!
   - 최근 Next js 2023 컨퍼런스가 있었는데 [위 영상](https://youtu.be/HjCbrGRSvoY?t=3992)에 용례가 나와있어요..! 이런 형태구나 하고 가볍게 참고만 하시면 좋을듯해서 가져와봤어요 (이 유튜버가 올린것 말고 컨퍼런스 라이브 영상을 못찾겠네요ㅠㅠ)
   - 오 영상 감사합니다! 영상은 아니고 트위터에서 캡쳐만 봤었는데 구리다고 난리났더라구여,,,ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ
   - 1시간 6분 쯤에 예시가 나오는 것 같아요
   - 요렇게 async 함수 내에서 'use server'를 호출하여 사용하는 방식을 서버 액션이라는 이름으로 부르는 모양이네용! 서버 컴포넌트 & 서버 액션 사이에는 분명히 차이가 있는 것 같아 [참고 링크](https://nextjs.org/docs/app/api-reference/functions/server-actions) 공유합니다~

2. > Alternatively, add 'use server'; at the very top of a file to mark all exports within that file as async server functions that can be used anywhere, including imported in client component files.
   - 서버사이드 함수를 한 파일에 모아두면 좀 깔끔하게 쓸 수 있겠네요
   - 아 이 부분 보니까 최상단에 쓸 수도 있고 특정 컴포넌트만 서버 사이드로 만들 수도 있고 그런 것 같습니다

### caveats

1. > For security, always treat them as untrusted input, making sure to validate and escape the arguments as appropriate.
   - 클라이언트에서 들어오는 값이니까 검증을 잘해야한다…기억해두기!
   - 음 서버에서 항상 이스케이프하는 것처럼 이제는 프론트 코드에서도 서버랑 직접 연결되는 부분이 있으니까 이런 보안 조치가 필요하다는 거겠죠?
   - 그런것 같아요 프론트엔드-백엔드 구조라면 프론트에서 한 번 처리, 백에서 한 번 처리할 수 있었다면(이중잠금장치 처럼) 이제는 프론트에서 바로 서버에 닿을 때가 있는거니까 그럴땐 유효성 검사가 중요하다! 라고 하는 것 같아요
2. > To avoid the confusion that might result from mixing client- and server-side code in the same file, 'use server' can only be used in server-side files
   - 같이 쓸 수 있었으면 정말 헷갈렸을 것 같아요^^;
