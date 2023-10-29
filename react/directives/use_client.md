# 'use client'

## 목차

- [Reference](#reference)
  - ['use client'](#use-client)
- Usage

## Reference

### `'use client'`

1. > When a file marked 'use client' is imported from a server component, compatible bundlers will treat the import as the “cut-off point” between server-only code and client code. Components at or below this point in the module graph can use client-only React features like useState.
   - 지난 시간에 공유된 블로그에서 [아래 이미지](https://velog.velcdn.com/images/2ast/post/465f8024-69c3-47ee-8b6f-1ee3d75b4fda/image.png)처럼 클라이언트 컴포넌트와 서버 컴포넌트가 번갈아 나타나는 그림을 봤는데 use client와 use server 지시문을 사용해서 그렇게 할 수 있는 거였네요.

### caveats

1. > It’s not necessary to add 'use client' to every file that uses client-only React features, only the files that are imported from server component files. 'use client' denotes the boundary between server-only and client code; any components further down the tree will automatically be executed on the client.
   - 서버 컴포넌트 사용 시 해당 프로젝트의 모든 client 컴포넌트에는 'use client'를 표기해줘야 하나 싶어 번거롭다고 생각했는데, 다행히 그건 아닌 모양이네요
2. > When a 'use client' file is imported from another client file, the directive has no effect. This allows you to write client-only components that are simultaneously usable from server and client components.
   - 동일한 지시문은 아무 영향이 없을 거라고 생각하긴 했는데 이로 인한 장점도 설명해줘서 좋았습니다!
3. > To reduce client bundle size and take full advantage of the server, move state (and the 'use client' directives) lower in the tree when possible, and pass rendered server components as children to client components.
   - 트리 상에서 클라이언트 컴포넌트 위치가 낮을 수록 좋다. 기억해두겠습니다
4. > Because props are serialized across the server–client boundary, note that the placement of these directives can affect the amount of data sent to the client; avoid data structures that are larger than necessary.
   - props는 서버와 클라이언트를 넘나들며 네트워크를 통해 전송된다. [질문] 직렬화 관련 얘기는 json으로 파싱할 수 있게 전달한다는 의미가 맞겠죠?
   - [의견1] 백엔드 프론트면 나눠어진 느낌인데 하나의 프레임웍 안에서 일어나는 일이다 보니 헷갈리는 부분이 있음
   - [의견2] next js의 app router 환경에서 서버 컴포넌트를 사용하는데 만약 프레임웍 없이 리액트에서만 사용한다면 어떻게 셋팅되는걸까?
   - [의견3] 리액트는 ui 라이브러리라서 그런거 하려면 프레임웍 사용을 권장 하는것 같음
   - [의견4] 회사 프로젝트를 app router로 했는데 배포 후 문제가 생겨서 (버전 문제, 라이브러리 문제) 로 page router로 돌아갔다는 이야기를 들은 적 있음
   - [의견5] vite + 리액트 조합도 많이 쓰는걸 봄
5. > That way, they can render exclusively on the server when used from a server component, but they’ll be added to the client bundle when used from a client component.
   - 클라이언트 컴포넌트로 선언한다는 건 번들에 포함된다는 뜻이므로 항상 번들 사이즈를 염두에 두자!
6. > Otherwise, users will need to wrap library components in their own 'use client' files which can be cumbersome and prevents the library from moving logic to the server later.
   - 제가 해석을 제대로 한 건지 모르겠어서 ㅎㅎ [질문] 사용자들이 use client로 감싸서 임의로 사용하게 하면 나중에 라이브러리가 use server로 바꾸고 싶어도 어렵다는 뜻 맞을까요?
7. > (The 'use xyz' directive format somewhat resembles the useXyz() Hook naming convention, but the similarity is coincidental.)
   - 의도적으로 네이밍을 맞춘 줄 알았는데 우연이라니 신기하네요ㅎㅎ (그걸 굳이 언급하는 것도 좀 귀여운 포인트군용)
   - javascript의 'use strict' 선언 방식에서 따오지 않았나 하는 생각이 듭니다! (추측)
   - 맞아요 뭔가 변명문장 같은 늬낌이라 귀엽… ㅎㅎㅎ
