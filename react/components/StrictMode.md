# StrictMode

## 목차

- [StrictMode](#strictmode)
  - [목차](#목차)
  - [사용 방법](#사용-방법)
    - [전체 앱에 대해 Strict Mode 활성화](#전체-앱에-대해-strict-mode-활성화)
    - [개발 중 이중 렌더링으로 발견한 버그 수정](#개발-중-이중-렌더링으로-발견한-버그-수정)

## 사용 방법

### 전체 앱에 대해 Strict Mode 활성화

1. 전체 앱을 (특히 새로 생성된 앱의 경우) Strict Mode로 래핑하는 것을 권장합니다.
   - 컴포넌트 태그로 되어 있는 strict mode가 익숙하지 않았는데, nextJs는 기본적으로 strict mode가 on이 되어 있어서 그런 거였어요. 옵션을 끄려면 next.config.js 파일에서 아래와 같이 설정해주어야 했던 걸로 기억합니다.
     ```ts
     module.exports = {
       reactStrictMode: false,
     };
     ```
   - 오 자체적으로 옵션을 제공하는군요. 이거 몰랐으면 StrictMode를 중복해서 쓸 뻔 했네요 😅

### 개발 중 이중 렌더링으로 발견한 버그 수정

1. Strict Mode는 실수로 작성된 순수하지 않은 코드를 찾아내기 위해 몇 가지 함수(순수 함수여야 하는 것만)를 개발 환경에서 두 번 호출합니다. 이에는 다음이 포함됩니다.  
   ...(중략)... [set 함수](https://ko.react.dev/reference/react/useState#setstate) ...(후략)
   - useState와 별개로 불변성 확인을 위해 setState 함수를 두 번 호출한다고 이해했습니다.
2. 두 번째 렌더링 호출 중 console.log 호출이 약간 흐리게 표시됩니다.
   - 헉.. 그랬던가요…? 다음에 한 번 확인해봐야겠네요 ㅋㅋㅋ
   - 오 맞아요 어떤 건 흐리게 표시되더라구요… 이것 때문이었군여 ㅋㅋㅋ
