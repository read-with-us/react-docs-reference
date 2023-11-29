# flushSync 

## flushSync
1. >  일부 브라우저 API는 콜백 내부의 결과가 DOM에서 동기적으로 사용될 것으로 예상하므로 콜백이 완료될 때까지 렌더링된 DOM을 사용해서 브라우저가 작업할 수 있습니다.
   - 원문과 비교해서 참고시 이해에 도움!
   - 원문 : Some browser APIs expect results inside of callbacks to be written to the DOM synchronously, by the end of the callback, so the browser can do something with the rendered DOM.
   - 번역 : 일부 브라우저 API는 콜백이 끝날 때까지 콜백 내부의 결과가 DOM에 동기적으로 기록되어 브라우저가 렌더링된 DOM으로 무언가를 할 수 있기를 기대합니다.
2. > onbeforeprint 브라우저 API를 사용하면 프린트 다이얼로그가 열리기 직전에 페이지를 변경할 수 있습니다. 문서를 더 잘 표시하기 위해 사용자가 정의한 프린트 스타일을 적용하는 데 유용합니다.
   - 프린트 API 로 출력 기능 넣을 때 원하는대로 스타일하기에 정말 좋을 것 같음.
   - onbeforeprint: (https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeprint_event)
   - onafterprint: (https://developer.mozilla.org/en-US/docs/Web/API/Window/afterprint_event)

### 🤷‍♀️ 질문
1. >대부분의 경우 flushSync를 사용하지 않을 수 있으므로 최후의 수단으로 사용하세요.
   - Q: flushSync를 사용할만한 다른 경우가 있을까요? 아니면 사용해보신 분이 계신가요?
   - A: 학습하기에 예제, 아이템을 추가하는 시점이랑 스크롤하는 시점을 맞추기 위해 사용하는 케이스 관련링크 : (https://ko.react.dev/learn/manipulating-the-dom-with-refs#flushing-state-updates-synchronously-with-flush-sync)
   - A: 상당히 조심히 사용해야할것같다 계속 최후의 수단으로 사용하라고 하는걸 보니...
