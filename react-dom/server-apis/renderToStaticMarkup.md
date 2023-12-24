# 목차

- [목차](#목차)
- [renderToStaticMarkup](#rendertostaticmarkup)
  - [사용법](#사용법)

# renderToStaticMarkup

## 사용법

1. > renderToStaticMarkup을 서버 응답과 함께 보낼 수 있는 HTML 문자열로 앱에 렌더링하기 위해 호출하세요:
   - 정리해보면 renderToStaticMarkup과 renderToString은 이 정도의 차이가 있는 것 같은데.. 솔직히 두 api 다 어떤 경우에 사용할지 감이 정확하게는 안오네요 🤔
     - renderToStaticMarkup -> 상호작용하지 않음 (hydrate 불가능) / Suspense는 제한적으로 지원
     - renderToString -> 상호작용 가능 (hydrate 가능) / Suspense 지원하지 않음 (정확히는 렌더가 중단될때 항상 폴백을 반환..?)
   - 정리 감사합니다! 저도 당장은 어떤 때 써야 하는지 크게 와닿지는 않네요..!! 😅
