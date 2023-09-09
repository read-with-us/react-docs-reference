# useReducer

**목차**

- 레퍼런스
  - useReducer(reducer, initialArg, init?)
  - dispatch function
- 사용법
  - 컴포넌트에 reducer 추가하기
  - reducer 함수 작성하기
  - 초기 state 재생성 방지하기
- 문제 해결
  - action을 dispatch했지만 로깅하면 이전 state값이 표시됩니다
  - action을 dispatch 했지만 화면은 업데이트 되지 않습니다
  - dispatch하면 reducer state의 일부분이 undefined가 됩니다
  - dispatch하면 모든 reducer state가 undefined가 됩니다
  - “리렌더링이 너무 많습니다” 라는 오류가 발생합니다
  - reducer 또는 초기화 함수가 두 번 실행됩니다

> 1. useReducer

- 아직 useReducer를 써본적이 없네요.. 존재는 알지만 손 안가는 훅 중에 하나가 아닐까😇
- 공감입니닷…저도 useReducer는 써본 적이 없어요
- 숙련도의 차이일수도 있겠다는 생각도 듭니다.

## 레퍼런스

## 사용법

### Adding a reducer to a component

1. > useReducer is very similar to useState, but it lets you move the state update logic from event handlers into a single function outside of your component.
   - 실무에서 useReducer를 잘 활용하지 않았는데 테스트를 작성하지 않기 때문에 로직을 완전히 분리하는 것이 우선순위가 높지 않아서 일까요? [질문] 실무에서 테스트 코드 작성하시는 분 있나요?
   - 아니요...아니요...
   - 정말 중요한 함수 아니면 테스트 코드를 작성하지 않게 돼요.
2. > choosing between useState and useReducer
   - useReducer는 useState보다 코드가 길어지지만 여러 state를 비슷한 방식으로 업데이트할 때 유용하고 하네요. 이런 경우 useReducer가 디버깅하기도 더 쉽고요.

### Writing the reducer function

1. > 예제 코드 중
   ```javascript
   const initialState = { name: "Taylor", age: 42 };
   export default function Form() {
     //
   }
   ```
   - 이런 부분도 렌더링할 때마다 객체를 새롭게 생성하지 않으려고 밖으로 뺀 것 같은데 숙달되기 전에는 이런 디테일 챙기기가 어려운 것 같아요.

## 문제 해결

### My entire reducer state becomes undefined after dispatching

1. > To find why, throw an error outside the switch:
   ```javascript
   function reducer(state, action) {
     switch (action.type) {
       case "incremented_age": {
         // ...
       }
       case "edited_name": {
         // ...
       }
     }
     throw Error("Unknown action: " + action.type);
   }
   ```
   - switch문이면 default를 쓸 수도 있겠지만 상황에 따라 이렇게 명확하게 에러를 던지는 것도 좋은 방법인듯하네요
   - 맞습니다. 그런데 default문 안 썼다고 lint에서 뭐라고 하지 않나 궁금하네요. (뭐라고 하지 않는걸로..[eslint playground](https://eslint.org/play/#eyJ0ZXh0IjoiZnVuY3Rpb24gcmVkdWNlcihzdGF0ZSwgYWN0aW9uKSB7XG4gIHN3aXRjaCAoYWN0aW9uLnR5cGUpIHtcbiAgICBjYXNlICdpbmNyZW1lbnRlZF9hZ2UnOiB7XG4gICAgICByZXR1cm4gJzEnXG4gICAgfVxuICAgIGNhc2UgJ2VkaXRlZF9uYW1lJzoge1xuICAgICAgcmV0dXJuICcxJ1xuICAgIH1cbiAgfVxuICB0aHJvdyBFcnJvcignVW5rbm93biBhY3Rpb246ICcgKyBhY3Rpb24udHlwZSk7XG59XG5cbnJlZHVjZXIoKSIsIm9wdGlvbnMiOnsiZW52Ijp7ImVzNiI6dHJ1ZX0sInBhcnNlck9wdGlvbnMiOnsiZWNtYUZlYXR1cmVzIjp7fSwiZWNtYVZlcnNpb24iOiJsYXRlc3QiLCJzb3VyY2VUeXBlIjoic2NyaXB0In0sInJ1bGVzIjp7ImNvbnN0cnVjdG9yLXN1cGVyIjpbImVycm9yIl0sImZvci1kaXJlY3Rpb24iOlsiZXJyb3IiXSwiZ2V0dGVyLXJldHVybiI6WyJlcnJvciJdLCJuby1hc3luYy1wcm9taXNlLWV4ZWN1dG9yIjpbImVycm9yIl0sIm5vLWNhc2UtZGVjbGFyYXRpb25zIjpbImVycm9yIl0sIm5vLWNsYXNzLWFzc2lnbiI6WyJlcnJvciJdLCJuby1jb21wYXJlLW5lZy16ZXJvIjpbImVycm9yIl0sIm5vLWNvbmQtYXNzaWduIjpbImVycm9yIl0sIm5vLWNvbnN0LWFzc2lnbiI6WyJlcnJvciJdLCJuby1jb25zdGFudC1jb25kaXRpb24iOlsiZXJyb3IiXSwibm8tY29udHJvbC1yZWdleCI6WyJlcnJvciJdLCJuby1kZWJ1Z2dlciI6WyJlcnJvciJdLCJuby1kZWxldGUtdmFyIjpbImVycm9yIl0sIm5vLWR1cGUtYXJncyI6WyJlcnJvciJdLCJuby1kdXBlLWNsYXNzLW1lbWJlcnMiOlsiZXJyb3IiXSwibm8tZHVwZS1lbHNlLWlmIjpbImVycm9yIl0sIm5vLWR1cGUta2V5cyI6WyJlcnJvciJdLCJuby1kdXBsaWNhdGUtY2FzZSI6WyJlcnJvciJdLCJuby1lbXB0eSI6WyJlcnJvciJdLCJuby1lbXB0eS1jaGFyYWN0ZXItY2xhc3MiOlsiZXJyb3IiXSwibm8tZW1wdHktcGF0dGVybiI6WyJlcnJvciJdLCJuby1leC1hc3NpZ24iOlsiZXJyb3IiXSwibm8tZXh0cmEtYm9vbGVhbi1jYXN0IjpbImVycm9yIl0sIm5vLWV4dHJhLXNlbWkiOlsiZXJyb3IiXSwibm8tZmFsbHRocm91Z2giOlsiZXJyb3IiXSwibm8tZnVuYy1hc3NpZ24iOlsiZXJyb3IiXSwibm8tZ2xvYmFsLWFzc2lnbiI6WyJlcnJvciJdLCJuby1pbXBvcnQtYXNzaWduIjpbImVycm9yIl0sIm5vLWlubmVyLWRlY2xhcmF0aW9ucyI6WyJlcnJvciJdLCJuby1pbnZhbGlkLXJlZ2V4cCI6WyJlcnJvciJdLCJuby1pcnJlZ3VsYXItd2hpdGVzcGFjZSI6WyJlcnJvciJdLCJuby1sb3NzLW9mLXByZWNpc2lvbiI6WyJlcnJvciJdLCJuby1taXNsZWFkaW5nLWNoYXJhY3Rlci1jbGFzcyI6WyJlcnJvciJdLCJuby1taXhlZC1zcGFjZXMtYW5kLXRhYnMiOlsiZXJyb3IiXSwibm8tbmV3LXN5bWJvbCI6WyJlcnJvciJdLCJuby1ub25vY3RhbC1kZWNpbWFsLWVzY2FwZSI6WyJlcnJvciJdLCJuby1vYmotY2FsbHMiOlsiZXJyb3IiXSwibm8tb2N0YWwiOlsiZXJyb3IiXSwibm8tcHJvdG90eXBlLWJ1aWx0aW5zIjpbImVycm9yIl0sIm5vLXJlZGVjbGFyZSI6WyJlcnJvciJdLCJuby1yZWdleC1zcGFjZXMiOlsiZXJyb3IiXSwibm8tc2VsZi1hc3NpZ24iOlsiZXJyb3IiXSwibm8tc2V0dGVyLXJldHVybiI6WyJlcnJvciJdLCJuby1zaGFkb3ctcmVzdHJpY3RlZC1uYW1lcyI6WyJlcnJvciJdLCJuby1zcGFyc2UtYXJyYXlzIjpbImVycm9yIl0sIm5vLXRoaXMtYmVmb3JlLXN1cGVyIjpbImVycm9yIl0sIm5vLXVuZGVmIjpbImVycm9yIl0sIm5vLXVuZXhwZWN0ZWQtbXVsdGlsaW5lIjpbImVycm9yIl0sIm5vLXVucmVhY2hhYmxlIjpbImVycm9yIl0sIm5vLXVuc2FmZS1maW5hbGx5IjpbImVycm9yIl0sIm5vLXVuc2FmZS1uZWdhdGlvbiI6WyJlcnJvciJdLCJuby11bnNhZmUtb3B0aW9uYWwtY2hhaW5pbmciOlsiZXJyb3IiXSwibm8tdW51c2VkLWxhYmVscyI6WyJlcnJvciJdLCJuby11bnVzZWQtdmFycyI6WyJlcnJvciJdLCJuby11c2VsZXNzLWJhY2tyZWZlcmVuY2UiOlsiZXJyb3IiXSwibm8tdXNlbGVzcy1jYXRjaCI6WyJlcnJvciJdLCJuby11c2VsZXNzLWVzY2FwZSI6WyJlcnJvciJdLCJuby13aXRoIjpbImVycm9yIl0sInJlcXVpcmUteWllbGQiOlsiZXJyb3IiXSwidXNlLWlzbmFuIjpbImVycm9yIl0sInZhbGlkLXR5cGVvZiI6WyJlcnJvciJdfX19))

### I’m getting an error: “Too many re-renders”

1. > You might get an error that says: Too many re-renders. React limits the number of renders to prevent an infinite loop.
   - 갑자기 useEffect 훅을 잘못 작성해서 개발 서버 터뜨렸던 기억이 나네요 ㅎㅎ
