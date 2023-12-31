# useState

**목차**

- 레퍼런스
  - useState(initialState)
  - setSomething(nextState)과 같은 set 함수
- 사용법
  - 컴포넌트에 state 추가하기
  - 이전 state를 기반으로 state 업데이트하기
  - 객체 및 배열 state 업데이트하기
  - 초기 state 다시 생성하지 않기
  - key로 state 초기화하기
  - 이전 렌더링에서 얻은 정보 저장하기
- 문제 해결
  - state를 업데이트했지만 로그에는 계속 이전 값이 표시됩니다
  - state를 업데이트해도 화면이 바뀌지 않습니다
  - 에러가 발생했습니다: “리렌더링 횟수가 너무 많습니다”
  - 초기화 함수 또는 업데이터 함수가 두 번 실행됩니다
  - state의 값으로 함수를 설정하려고 하면 설정 대신 호출됩니다

## 레퍼런스

## 사용법

### 이전 state를 기반으로 state 업데이트하기

1. > 규칙상 대기 중인 state 인수의 이름을 age의 a와 같이 state 변수 이름의 첫 글자로 지정하는 것이 일반적입니다. 그러나 prevAge 또는 더 명확하다고 생각하는 다른 이름으로 지정해도 됩니다.
   - 저는 이거나 prev를 더 선호해요😅
   - 첫 글자로 해도 된다고 하니 어느게 더 낫나 고민되네요

### 항상 업데이터를 사용하는 것이 더 좋은가요?

1. > 친절한 문법보다 일관성을 더 선호한다면 설정하려는 state가 이전 state에서 계산되는 경우 항상 업데이터를 작성하는 것이 합리적일 것입니다.
   - 무조건 이전 값 사용할 때 updater function 쓰다가 지난 번에 공식 문서 읽고 이벤트 핸들러 안에서 그냥 state를 사용해도 상관없다는 걸 깨달은 적이 있어요. 그런데 일관성 생각하면 다 써도 괜찮았던 거군요!

### 초기 state 다시 생성하지 않기

1. > createInitialTodos()의 결과는 초기 렌더링에만 사용되지만, 여전히 모든 렌더링에서 이 함수를 호출합니다. 이는 큰 배열을 생성하거나 값비싼 계산을 수행하는 경우 낭비일 수 있습니다. 이 문제를 해결하려면, useState에 초기화 함수로 전달하세요.
   - useState에 함수를 전달한 적이 별로 없는 것 같은데…! 이 부분은 당장 적용해봐야겠어요
2. > 함수를 호출한 결과인 createInitialTodos()가 아니라 함수 자체인 createInitialTodos를 전달하고 있다는 것에 주목하세요

   - createInitialTodos() 처럼 함수가 실행되게 해놓으면 매번 렌더링 될 때마다 이 함수도 실행되니 비효율적이겠네요..! 초반에 많이 했던 onClick 관련 실수가 생각나네요

     ```javascript
     onClick={handleClick} // (o)

     onClick={handleClick()} // (x)
     ```

   - 그러고보니 useState에 한 번도 함수를 전달해본 적이 없는 것 같아요. 그런데 말씀하신 것처럼 렌더링할 때마다 호출하더라도 그 리턴값으로 다시 초기화되지 않으니까 동작에 이상을 못 느꼈을 것 같아요.

### key로 state 초기화하기

1. > 컴포넌트에 다른 key를 전달하여 컴포넌트의 state를 초기화할 수 있습니다.
   - 다른 Key를 넘기면 state가 초기화되는 군요… 처음 알았네요

### 이전 렌더링에서 얻은 정보 저장하기

1. > 렌더링하는 동안 set 함수를 호출하는 경우, 그 set 함수는 prevCount !== count와 같은 조건 안에 있어야 하며, 조건 내부에 setPrevCount(count)와 같은 호출이 있어야 한다는 점에 유의하세요. 그렇지 않으면 리렌더링을 반복하다가 결국 깨질 것입니다.
   - Too many re-renders. React limits the number of renders to prevent an infinite loop. 에러를 뿜으면서 깨지네요
     (크래쉬가 나는걸 '깨진다'고 표현했군요😁)
2. > 렌더링 도중 set 함수를 호출하면 React는 컴포넌트가 return문으로 종료된 직후, 자식을 렌더링하기 전에 해당 컴포넌트를 리렌더링 합니다. 이렇게 하면 자식 컴포넌트를 두 번 렌더링할 필요가 없습니다. 나머지 컴포넌트 함수는 계속 실행되고 결과는 버려집니다. 조건이 모든 Hook 호출보다 아래에 있으면 이른(early) return;을 통해 렌더링을 더 일찍 다시 시작할 수 있습니다.
   - 제가 이해한 게 맞나 싶어서 정리해 봅니다. [질문] set 함수를 호출하더라도 early return 하지 않으면 return 문의 라인이 모두 실행 되고 다만, React가 실제로 화면에 렌더링하지는 않는다는 의미가 맞을까요?

## 문제 해결

### 에러가 발생했습니다: “리렌더링 횟수가 너무 많습니다”

1.  > // 🚩 잘못된 방법: 렌더링 동안 핸들러 요청
        ```javascript
        return <button onClick={handleClick()}>Click me</button>
        ```
    - 이런 실수 많이 저질렀었던…. 렌더링 동안에 핸들러를 요청하지 말고 이벤트 핸들러나 인라인 함수로 전달!
2.  > 콘솔에서 에러 옆에 있는 화살표를 클릭한 뒤 JavaScript 스택에서 에러의 원인이 되는 특정 set 함수 호출을 찾아보세요.
    - 좋은 디버깅 팁입니다

### 초기화 함수 또는 업데이터 함수가 두 번 실행됩니다

1. > 컴포넌트, 초기화 함수, 업데이터 함수는 순수해야 합니다. 이벤트 핸들러는 순수할 필요가 없으므로 React는 이벤트 핸들러를 두 번 호출하지 않습니다.
   - Strict Mode에서 두 번 호출하는 건 알고 있는데 이벤트 핸들러는 두 번 호출하지 않는다는 건 왜 기억이 나지 않는 걸까요 🤣 공식 문서에서 언급되었을 텐데 전에는 놓쳤나 봐요.

### state의 값으로 함수를 설정하려고 하면 설정 대신 호출됩니다

1. > 정말로 함수를 저장하길 원한다면, 두 경우 모두 함수 앞에 () =>를 넣어야 합니다. 그러면 React는 전달한 함수를 값으로 저장합니다.
   - [질문] 함수를 state로 저장해보신 분 있나요? 어떤 경우에 함수를 저장할 필요가 있을까요?
   - 제가 스터디 후에 정리하면서 생각해보니 이렇게 함수를 저장하는게 가능하긴 하겠지만 말 그대로 '가능'한 것이지 필요한 경우가 굉장히 드물 것 같아요.🤔
