# createPortal

## createPortal
1. >  자식은 부모 트리가 제공하는 context에 액세스할 수 있으며 이벤트는 여전히 React 트리에 따라 자식에서 부모로 버블링됩니다.
   - 스타일의 영향은 받지 않지만, React 트리를 따라 context에 엑세스할 수 있다는 점이 좋다
2. > 이 예시에서 두 컨테이너에는 모달 대화 상자에 영향을 주는 스타일이 있지만, portal에 렌더링된 스타일은 영향을 받지 않는데, 그 이유는 DOM에서 모달이 상위 JSX 요소에 포함되지 않기 때문입니다.
   - 아 스타일의 영향에서 자유롭다는 장점은 미처 생각을 못했었네요. 예전에 shadow DOM 나왔던 것도 그렇고 생각보다 스타일 충돌을 피하려고 분리하는 경우가 많은 것 같아요.
   - 해당 예제 링크 : (https://hyp.is/GF96XIv5Ee60QqdhnPOC4Q/ko.react.dev/reference/react-dom/createPortal)
3. > 모달을 만들 때는 WAI-ARIA 모달 제작 관행을 따르세요.
   - 링크가 바뀐 것 같아서 달아봅니다. (https://www.w3.org/WAI/ARIA/apg/patterns/) 
   - 모달 패턴 링크 : (https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/)
4. > 지도 예시를 통한 createPortal의 다른 사용법
   - 해당 예제 링크 : (https://hyp.is/hrfq1Iv5Ee6h8VfkJYXYfw/ko.react.dev/reference/react-dom/createPortal)

### 🤷‍♀️ 질문
1. > createPortal을 사용하면 일부 자식을 DOM의 다른 부분으로 렌더링할 수 있습니다.
   - Q: createPortal를 많이 사용하시는 편인가요? 저는 사실 사용해본 적이 잘 없어서 궁금하네요!
   - A: 실무에서 사용을 하는지 의문이 든다.
   - A: 모달을 만들어 사용할경우에는 라이브러리 또는 스타일을 통해 띄운것처럼 보이게 하는 방식을 사용해서 만든다.
   - 결론적으로 아무도 사용해보지 않음 ㅠ
2. > 이로 인해 문제가 발생하면 portal 내부에서 이벤트 전파를 중지하거나 portal 자체를 React 트리에서 위로 이동하세요.
   - Q: React 트리로 이벤트가 전파되면 어떤 문제가 발생할 수 있을까요? 오히려 전파가 안 되면 연결하느라 더 힘들 것 같은데 사용하시다가 문제 겪으신 분 있나요?
   - A: 실제로 랜더링 되는 돔트리는인 경우에 portal에서 발생하는 이벤트가 App컴포넌트로 전달되어 App의 이벤트들이 예상치 못하게 트리거되는 경우를 말하는 것 같다.
   - 관련링크 : (https://eun-jee.com/post/front-end/react/02-createPortal/#portal%EC%9D%84-%ED%86%B5%ED%95%9C-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B2%84%EB%B8%94%EB%A7%81)
