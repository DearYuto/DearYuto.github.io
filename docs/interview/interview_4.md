---
sidebar_position: 5
---

# 애니메이션 컴포넌트 라이브러리가 동작이 안될 때

`useState`로 애니메이션의 동작을 관리하고 있을 때 SSR 이후 애니메이션이 동작하지 않는다면 어떻게 해결할 수 있을까요?

SSR에서는 클라이언트 사이드에서 사용할 수 있는 `useState`를 사용할 수 없기 때문에

`클라이언트 컴포넌트`로 분리 후, 래핑하는 방법으로 해결할 수 있어요.

**`use client`** 지시어를 붙여주면, 서버사이드 렌더링에 포함되긴 하지만 브라우저에서 하이드레이션될 거예요.

이 방법을 사용하여 사용자 이벤트 등의 동작이 처리될 수 있도록 하여 해결해주면 될 것
같아요.

:::info 결론
`use client`를 선언하자.
SSR로 내린 후 하이드레이션을 거쳐 CSR로 UI를 변경시켜줄 수 있다.
:::

### 시도해볼 다른 방법

- `useEffect`를 사용하여 애니메이션 활성화 로직을 여기에서 실행시키기

- next/dynamic을 사용하여 서버 사이드 렌더링되지 않고 클라이언트 사이드에서 컴포넌트를 로드 시키는 방법

```tsx
import dynamic from 'next/dynamic';

const DynamicAnimatedComponent = dynamic(() => import('./AnimatedComponent'), {
  ssr: false,
});
```
