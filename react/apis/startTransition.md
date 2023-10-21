# startTransition

**목차**

- [레퍼런스](#레퍼런스)
  - [주의사항](#주의사항)
- [startTransition](#starttransition)
  - [사용법](#사용법)

## 레퍼런스

### 주의사항

1. > startTransition은 transition이 진행 중인지 추적할 수 있는 방법을 제공하지 않습니다. 진행 중인 transition을 표시하려면 useTransition이 필요합니다.
   - 이 부분만 다른 것 같아서 리마인드용으로 표시해둡니다.

## 사용법

1. > - startTransition은 useTransition과 매우 유사하지만, transition이 진행 중인지 추적하는 isPending 플래그를 제공하지 않습니다. useTransition을 사용할 수 없을 때 startTransition을 호출할 수 있습니다.
   - isPending 상태가 필요 없는 경우에는, 컴포넌트 내부라도 굳이 useTransition을 호출할 필요 없이 startTransition만 사용해도 되겠네요
