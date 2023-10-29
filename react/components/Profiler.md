# Profiler

## 목차

- [Profiler](#profiler)
  - [목차](#목차)
  - [레퍼런스 (Reference)](#레퍼런스-reference)
    - [`<Profiler>`](#profiler-1)
    - [주의사항 (Caveats)](#주의사항-caveats)
    - [`onRender` callback - Parameters](#onrender-callback---parameters)

## 레퍼런스 (Reference)

### `<Profiler>`

1. `<Profiler>`

   - [실제 사용 예시](https://jiyong1.github.io/posts/react-profiler/) 실제로 사용했을 때 어떤지 궁금해서 검색해봤어요..! 그런데 말고 react profiler 도구도 따로 있는 모양이에요.
   - 오 실제 어떻게 나오는지 궁금했는데 감사합니다! 구체적으로 얼마나 최적화되었는지 확인할 수 있어서 좋네요.
   - 안그래도 예시 찾고있었는데 감사합니다!

### 주의사항 (Caveats)

1. To opt into production profiling, you need to enable a [special production build with profiling enabled.](https://gist.github.com/bvaughn/25e6233aeb1b4f0cdb8d8366e54a3977)

   - 프로덕션 프로파일링을 위한 빌드가 있을 줄은 몰랐어요. 운영 서버말고 개발 서버 같은 데서 성능 측정하고 싶으면 유용할 것 같습니다.
   - 웹뷰 환경에서 빌드한 결과물을 Profiling 하고 싶은 경우 유용하게 사용할 수 있겠다는 생각이 드네요..!

### `onRender` callback - Parameters

1. Compare `actualDuration` against it to see if memoization is working.

   - memo나 useMemo를 사용하고 나서도 이게 최적화가 잘 된건가… dev tools에서 렌더링이 덜 깜빡이긴 하는 것 같은데… 이러고 있었는데 Profiler를 샤용하면 정확하게 메모이제이션이 얼마나 되었는지 확인할 수 있겠네요.

     dev tools에 profiler 탭이 있긴 한데 사용법을 잘 모르기도하고 딱 봤을 때 잘 모르겠더라구요.

     +) 찾아보니깐 dev tools로 렌더링이 오래걸리는 컴포넌트를 찾은 후에 컴포넌트로 자세하게 측정해보면 좋을 것 같아요.
