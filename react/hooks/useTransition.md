# useTransition

**목차**

- [레퍼런스](#레퍼런스)
  - [주의사항](#주의사항)
- [사용법](#사용법)
  - [state 업데이트를 non-blocking transition으로 표시](#state-업데이트를-non-blocking-transition으로-표시)
  - [Transition은 이미 표시된 콘텐츠(예시: 탭 컨테이너)를 숨기지 않을 만큼만 “대기”합니다.](<#transition은-이미-표시된-콘텐츠(예시-탭-컨테이너)를-숨기지-않을-만큼만-“대기”합니다>)
  - [Suspense-enabled 라우터 구축](#suspense-enabled-라우터-구축)
- [문제 해결](#문제-해결)
  - [Transition에서 입력 업데이트가 작동하지 않습니다](#transition에서-입력-업데이트가-작동하지-않습니다)
  - [startTransition에 전달한 함수는 즉시 실행됩니다](#starttransition에-전달한-함수는-즉시-실행됩니다)

## 레퍼런스

### 주의사항

1. > 해당 state의 set 함수에 액세스할 수 있는 경우에만 업데이트를 transition으로 래핑할 수 있습니다. 일부 prop이나 커스텀 Hook 값에 대한 응답으로 transition을 시작하려면 useDeferredValue를 사용해 보세요.
   - 이 두개는 같은 목적을 가지고 있지만 사용하는 상황이 서로 다른 훅이로군요.
2. > Transition으로 표시된 state 업데이트는 다른 state 업데이트에 의해 중단됩니다.
   - 상태 업데이트 = 렌더링 = blocking인데 transition으로 표시하면 non-bloking으로 바뀌면서 렌더링을 취소하고 재시작하도록 도와주는 것 같습니다.
3. > 진행 중인 transition이 여러 개 있는 경우, React는 현재 transition을 함께 일괄 처리합니다. 이는 향후 릴리즈에서 제거될 가능성이 높은 제한 사항입니다.
   - Q: 일괄 처리되지 않고 개별 처리할 수 있도록 바뀔 예정이라는 뜻 맞을까요? 아니면 다른 의미일까요?
   - A: 말씀해주신 대로 개별 처리할 수 있도록 바뀔 예정이라는 걸로 이해했어요!
     (원문: If there are multiple ongoing transitions, React currently batches them together. This is a limitation that will likely be removed in a future release.)

## 사용법

### state 업데이트를 non-blocking transition으로 표시

1. > Transition을 사용하면 리렌더링 도중에도 UI가 반응성을 유지합니다. 예를 들어 사용자가 탭을 클릭했다가 마음이 바뀌어 다른 탭을 클릭하면 첫 번째 리렌더링이 완료될 때까지 기다릴 필요 없이 다른 탭을 클릭할 수 있습니다.
   - 이거 보니까 필터링할 때도 유용할 것 같다는 생각이 들어요. 사용자가 여러 필터를 거의 동시에 클릭하는 경우 최적화할 수 있지 않을까
   - 이 부분 진짜 신기하고 유용하네요 잘 사용하면 FID를 줄일 수 있을 것 같아요.
     - 최초 입력 반응 시간 (FID): 상호작용을 측정합니다. 우수한 사용자 환경을 제공하려면 페이지의 FID가 100밀리초 이하여야 합니다. (https://web.dev/articles/vitals?hl=ko)
     - Q: 보통 이런 web vitals 지표 측정을 작업할 때 하시는 편이신가요?
       - 버그 해결을 위해 지표 확인 / 일정 수준 이상 돌아가는지 검증하는 로직을 넣어둠
       - lighthouse에서 core web vital 항목들이 대부분 나오는 것으로 알고 있음. 보통 lighthouse로 측정
       - qa파트에서 외주사 작업 qa 진행 시 light house 점수 기준을 두고 진행하는 케이스도...
2. > useTransition과 일반 state 업데이트의 차이점
   - 설명만 봐서는 이해가 잘 안됐는데 예시를 보니까 좀 이해가 되네요😅 Transition이 없을 경우에는 Posts 탭을 렌더링하는 동안 다른 버튼을 클릭하면 바로 다른 탭으로 넘어가지 않는데, Transition을 사용하면 바로바로 넘어가지는 차이가 있군요.

### Transition은 이미 표시된 콘텐츠(예시: 탭 컨테이너)를 숨기지 않을 만큼만 “대기”합니다.

1. > Transition은 이미 표시된 콘텐츠(예시: 탭 컨테이너)를 숨기지 않을 만큼만 “대기”합니다. 만약 Posts 탭에 중첩된 <Suspense> 경계가 있는 경우 transition은 이를 “대기”하지 않습니다.
   - 아직 Suspense를 안 써보긴 했는데 아래 예제 보니까 루트의 Suspense 폴백은 나타나지 않지만 중첩된 Suspense의 폴백은 나타나는 것에 대한 설명인 것 같아요

### Suspense-enabled 라우터 구축

1. > 여태 읽으면서 이 훅은 왜 등장하게 된걸까…? 싶었는데 요런 경우에 유용하겠어요

## 문제 해결

### Transition에서 입력 업데이트가 작동하지 않습니다

1. > 입력을 제어하는 state 변수에는 transition을 사용할 수 없습니다.
   - Q: 아래 설명을 봐도 잘 모르겠는데 혹시 이해하신 분 있을까요?
   - A: 저는 input 값을 state와 동기화 시키는 등의 동작은 항상 동기적으로 업데이트 되어야 하기 때문에 transition을 사용해선 안되고, 만약 이 값에 대해 transition이 필요하다면 다른 state를 선언해서 그걸 사용하라는 걸로 이해했어요!

### startTransition에 전달한 함수는 즉시 실행됩니다

- 이 페이지를 다 읽고나서도 뭔가 선명하게 이해되지 않은 느낌이 조금 들어서 잠깐 검색을 해봤는데요, [이 아티클](https://medium.com/@ahsan-ali-mansoor/usetransition-hook-explained-885e87414b8)에서는 usetransition 훅을 사용하는 아주 간단한 예제를 들어서 설명하는데 저는 개인적으로 가볍게 쓱 훑어보기 좋았기에 공유합니다! (useTransition 훅을 아래처럼 정의해놓았네요)
