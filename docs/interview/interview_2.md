---
sidebar_position: 3
---

# Next.js의 SSR 문제점이 있다면 Suspense로 어떻게 해결할 수 있나요?

### 📄 요약

`Streaming Server Rendering`을 사용하면 해결할 수 있어요.

페이지 내 모든 컴포넌트를 로딩할 때까지 기다리지 않고, 먼저 로딩이 끝난 컴포넌트를 우선적으로 보여주어 사용자 경험을 향상시키는 방법을 사용하면 돼요.

:::tip 스트리밍 서버 렌더링이 무엇인가요?
서버에서 클라이언트로 HTML을 순차적으로 렌더링하는 것을 의미해요.

:::

---

Next.js를 사용하여 SSR 방식으로 페이지를 보여주고자 해요.

이때 발생할 수 있는 문제점으로 무엇이 있을까요?

바로 모든 컴포넌트 요소가 전부 로드될 때까지 기다려야 할 수도 있다는 점이에요.

:::info 무슨 의미인가요?

다음 예시 이미지와 함께 설명해볼게요.

![Suspense 예시 이미지](/img/suspense.svg)

:::

예시 이미지의 유토 블로그 메인 페이지는 총 2개의 컴포넌트가 존재하고 있어요.

하나의 컴포넌트는 로드까지 2초가 걸리고, 또 하나의 컴포넌트는 4초가 걸려요.

간단한 코드 예시를 보면 다음과 같아요.

```tsx
export default function Example() {
  return (
    <Suspense
      fallback={
        <div>하위 모든 컴포넌트가 로드 될 때까지 이 컴포넌트가 나타나요.</div>
      }
    >
      <Component1 />
      <Component2 />
    </Suspense>
  );
}

const Component1 = async () =>
  new Promise((res) => {
    setTimeout(() => {
      res(<div>유토의 1번 컴포넌트</div>);
    }, 2_000);
  });

const Component2 = async () =>
  new Promise((res) => {
    setTimeout(() => {
      res(<div>유토의 2번 컴포넌트</div>);
    }, 4_000);
  });
```

`Suspense`를 사용해서 컴포넌트 1번과 2번을 감싸도록 했어요.

이렇게 하면, 두 컴포넌트가 모두 로드가 될 때까지 fallback에 있는 컴포넌트를 먼저 보여주도록 하고, `Suspense`의 하위 컴포넌트가 준비가 완료되면 하위 컴포넌트를 볼 수 있을 거에요.

어떻게 동작하는지 이미지로도 확인해볼게요.

![Suspense 동작 이미지](/img/suspense_example.gif)

가장 로드가 오래 걸리는 2번 컴포넌트(4초)가 로드 완료될 때까지 fallback의 컴포넌트가 보여지고 있네요.

음, 그런데 뭔가 비효율적인 것 같아요. 😶

1번 컴포넌트는 2초만 기다리면 되니까, 1번 컴포넌트는 2초 후에 보여주고, 4초를 기다려야하는 2번 컴포넌트는 추가로 2초 더 기다린 후에 보여지면 더 사용자 경험을 향상 시킬 수 있을 것 같아요.

이를 해결하기 위해 `Suspense`를 추가로 사용해서 앱을 더 세부적으로 독립시켜보면 좋을 것 같아요.

```tsx
export default function Example() {
  return (
    <Suspense fallback={<h2>💎최상위 폴백은 로딩중! ^ㅇ^</h2>}>
      <Component1 />
      // highlight-next-line
      <Suspense fallback={<h2>😓4초 걸리는 컴포넌트.. 기다려주세요.</h2>}>
        <Component2 />
        // highlight-next-line
      </Suspense>
    </Suspense>
  );
}
```

`Suspense` 를 하나 더 추가해보았어요. 이제 어떻게 동작에 차이가 있는지 살펴볼게요!

![Suspense 동작 이미지](/img/suspense_example2.gif)

이번에는 2초 걸리는 1번 컴포넌트는 로딩이 끝나서 사용자가 2초 후에 볼 수 있게 됐어요.

그리고 2초 더 기다려서 4초가 걸리는 2번 컴포넌트도 순차적으로(스트리밍하게) 보여줄 수 있어요.

이렇게 사용자가 페이지 내 모든 컴포넌트가 전부 로드될 때까지 기다리지 않고, 먼저 로드되는 컴포넌트는 우선적으로 볼 수 있도록 SSR의 문제점을 해결할 수 있어요.

:::tip
Next.js에서는 `loading` 파일을 만들어 사용하면 Next.js에서 이 기능을 최적화해서 사용할 수 있어요.
:::

---

### Suspense

React18 버전에 공식 스펙으로 도입된 `Suspense`를 사용하면 앱을 더 작은 독립 단위로 세분화할 수 있어요.

`Suspense`를 사용하면 자식 컴포넌트가 로딩이 완료될 때까지 fallback 프롭스를 사용하여 로딩화면을 표시할 수 있어요.

즉, 렌더링하고자 하는 자식 컴포넌트의 로딩이 완료되지 않았을 때 대체할 수 있는 수단으로 사용할 수 있어요. 대개 로딩 스피너, 스켈레톤과 같은 컴포넌트를 대체해 보여주곤 해요.

Next.js는 데이터 로딩을 위해 `Suspense`를 활성화하는 프레임워크이기 때문에 이를 활용하여 SSR에서 발생할 수 있는 문제점인 모든 컴포넌트가 전부 로드될 때까지 사용자를 대기시키지 않아도 돼요.

:::danger
`Suspense`는 비동기로 동작하는 컴포넌트에서 사용할 수 있어요.

Next.js에서는 이를 지원하고 있기 때문에 비동기로 동작하는 컴포넌트에 바로 사용할 수 있지만, 현재 React환경에서는 lazy load해서 컴포넌트를 지연 로딩시킬 때 사용할 수 있어요.

(React 18 버전 기준 실험적 기능인 `use`를 사용하는 경우에도 쓸 수 있어요.)
:::

### React lazy import와 Suspense 사용

다음은 리액트에서 lazy import를 사용해 Suspense 사용하는 예시 코드에요.

lazy 로드가 제대로 동작할 수 있도록 임의로 setTimeout을 사용해 지연시켰어요.

```tsx
// Example.tsx
import { Suspense, lazy } from 'react';

const Component1 = lazy(
  () =>
    new Promise((resolve) =>
      setTimeout(() => resolve(import('./Component1')), 2000)
    )
);

const Component2 = lazy(
  () =>
    new Promise((resolve) =>
      setTimeout(() => resolve(import('./Component2')), 4000)
    )
);

export default function Example() {
  return (
    <Suspense fallback={<h2>💎최상위 폴백은 로딩중! ^ㅇ^</h2>}>
      <Component1 />
      <Suspense fallback={<h2>😓4초 걸리는 컴포넌트.. 기다려주세요.</h2>}>
        <Component2 />
      </Suspense>
    </Suspense>
  );
}
```

```tsx
// Component1.tsx
function Component1() {
  return <div>Component1 내용</div>;
}

export default Component1;
```

```tsx
// Component2.tsx
function Component2() {
  return <div>Component2 내용</div>;
}

export default Component2;
```

---

> **참고 자료**

[New Suspense SSR Architecture in React 18](https://github.com/reactwg/react-18/discussions/37)

[React Suspens](https://react.dev/reference/react/Suspense)
