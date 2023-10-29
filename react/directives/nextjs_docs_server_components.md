https://nextjs.org/docs/app/building-your-application/rendering/server-components

# Server Components

## 목차

- Benefits of Server Rendering
- Using Server Components in Next.js
- How are Server Components rendered?
- Server Rendering Strategies
  - Static Rendering (Default)
  - Dynamic Rendering
    - Switching to Dynamic Rendering
    - Dynamic Functions
  - Streaming

## Benefits of Server Rendering

1. > Benefits of Server Rendering
   - 대충 SEO에 좋다… 화면이 빨리 그려진다… 정도로만 이해하고 있었는데 구체적으로 설명되어서 좋네요 👍

### Using Server Components in Next.js

1. > By default, Next.js uses Server Components.
   - 보통 흐름이 서버 -> 클라이언트니까 기본값이 서버 컴포넌트인게 당연한 것 같아요

### How are Server Components rendered?

1. > The React Server Components Payload is used to reconcile the Client and Server Component trees, and update the DOM.
   - 저번에 보았던 [서버 컴포넌트 트리와 클라이언트 컴포넌트 트리가 섞여있는 모습](https://velog.velcdn.com/images/2ast/post/465f8024-69c3-47ee-8b6f-1ee3d75b4fda/image.png)이 이 과정을 통해서 도출되는 거려나요…?
2. > What is the React Server Component Payload (RSC)?
   - 갑자기 처음 보는 개념이 등장해서 당황했는데 간략한 설명이 있어서 다행입니다. 여전히 어렵긴 하지만요 ㅎㅎ
   - 트리 중간중간 구멍이 있을테니 그걸 채우기 위한 placeholder도 포함되어 있네요

## Server Rendering Strategies

1. > Server Rendering Strategies
   - [질문] 렌더링 전략 세가지를 설명해주고 있는데, Next.js에서 알아서 적절한 렌더링 전략을 사용해서 렌더링을 해준다는 걸까요?
2. > In Next.js, you can have dynamically rendered routes that have both cached and uncached data. This is because the RSC Payload and data are cached separately. This allows you to opt into dynamic rendering without worrying about the performance impact of fetching all the data at request time.
   - [질문] 이 부분 보면 Next.js에서는 정적 페이지든 동적 페이지든 무조건 동적 렌더링을 선택하는 게 좋은 것 같은데 직접 써보신 분들 입장에서는 어떠셨나요?
3. > During rendering, if a dynamic function or uncached data request is discovered, Next.js will switch to dynamically rendering the whole route.
   - Next.js에서는 직접 지정하지 않아도 정적 렌더링과 동적 렌더링을 알아서 스위칭해주는 걸까요?
4. > As a developer, you do not need to choose between static and dynamic rendering as Next.js will automatically choose the best rendering strategy for each route based on the features and APIs used. Instead, you choose when to cache or revalidate specific data, and you may choose to stream parts of your UI.
   - 정적 렌더링과 동적 렌더링 방식을 지정할 필요없이 자동으로 해주네요.
