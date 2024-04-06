---
sidebar_position: 4
---

# Next.js의 DataFetching 전략에 따른 SSR 구현 방식 차이점

- Streaming with Suspense
- Static Data
- Dynamic Data
- Incremental Static Regeneration

### Streaming with Suspense

스트리밍을 사용하여 서버에서 클라이언트로 UI를 `점진적으로 렌더링`하여 전송해요.

페이지의 가장 중요한 부분을 먼저 상호작용을 할 수 있다는 점에서 전체 페이지가 모두 로드 되기를 기다리지 않아도 돼요.

스트리밍은 `엣지 런타임`, `노드 런타임` 모두에서 동작해요.

:::info Edge Runtime, Node Runtime이란?

Next.js에서 런타임은 코드가 실행되는 동안 사용할 수 있는 라이브러리, API, 일반 기능의 집합을 의미해요.

서버에는 애플리케이션 코드의 일부를 렌더링 할 수 있는 2가지 런타임이 존재해요.

`엣지 런타임`: 웹 API를 기반으로 하며, 작고 간단한 기능으로 잛은 지연시간으로 동적이고 개인화된 콘텐츠를 제공할 때 사용해볼 수 있어요.
모든 Node.js API를 지원하지 않고 있어 일부 npm 패키지가 동작하지 않을 수 있기 때문에 API나 패키지를 사용해야한다면, 노드 런타임을 사용하는 게 좋아요.

`노드 런타임`: 모든 Node.js API와 에코시스템 호환 패키지에 액세스 할 수 있어요.
엣지 런타임만큼 빠르지는 않지만, 모든 npm 패키지에 액세스 할 수 있어요.

:::

### Static Data

기본적으로 Next.js에서 데이터를 가져오는 것은 정적으로 캐시가 된다는 특징이 있어요.

### Dynamic Data

동적 데이터, 서버 렌더링 데이터는 요청할 때마다 새로 가져오는 데이터에요.

### Incremental Static Regeneration(ISR)

`Revalidating Data`: 데이터 캐시를 지우고 최신 데이터를 다시 가져오는 프로세스를 의미해요. 데이터가 변경될 경우 최신 정보를 표시할 수 있어요.

캐시된 데이터는 2가지 방법으로 재검증 할 수 있어요.

**1. Time-based revalidation**

일정 시간이 지나면 데이터를 자동으로 재검증하는 방법이에요.

자주 변경되지 않고 즉시 최신 데이터가 보여지지 않아도 괜찮을 때 사용해볼 수 있어요.

Next.js에서는 `fetch`에 `next.revaldate` 옵션을 사용하면, 리소스를 캐싱할 시간을 설정할 수 있어요.

```ts
fetch('https://...', { next: { revalidate: 3600 } });
```

위 방법 외에도 [Route Segment Config](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)를 사용하는 방법도 있어요.

```ts
// layout 파일에서 세팅
export const revalidate = 3600; // revalidate at most every hour
```

**2. [On-demand revalidation](https://nextjs.org/docs/app/building-your-application/caching#on-demand-revalidation)**

이벤트를 기반으로 데이터를 수동으로 재검증해요.

서버 액션이나 revalidatePath, revalidateTag로 데이터를 온디맨드 방식으로 재검증 할 수 있어요.

가능한 한 빨리 최신 데이터를 보여줘야 할 때 사용할 수 있어요. (예를 들면, headless CMS 콘텐츠 업데이트를 해야 하는 경우)

온디맨드 재검증이 트리거 될 때 캐시에서 해당 항목을 제거해요.

`Time-based revalidation`과 달리 새 데이터를 가져올 때까지 오래된 데이터를 보관하지 않고, 이후에 요청이 발생하면 캐시 미스가 발생하고, 외부 데이터 소스에서 데이터를 가져온 후 데이터 캐시에 저장해요.

```tsx
export default async function Page() {
  // highlight-next-line
  const res = await fetch('https://...', { next: { tags: ['collection'] } });
  const data = await res.json();
  // ...
}
```

---

### **데이터를 불러오는 방법 4가지**

**1. `fetch` 사용하기**

Next.js에서는 웹 API인 `fetch`를 확장하여 요청에 대한 캐싱과 재검증 동작을 구성할 수 있도록 해요.

React는 리액트 컴포넌트 트리를 렌더링하는 동안 `fetch`를 자동으로 메모화돼요.

동일한 데이터를 여러 번 요청하더라도 `fetch`를 사용하거나 `리액트 캐시`를 사용할 수 있어요.

**2. 서브파티 라이브러리를 사용하기**

다른 라이브러리를 사용(CMS, ORM, DB 등)하는 경우, 리액트 캐시 기능을 사용하여 요청을 캐싱하거나 요청을 재검증할 수 있어요.

**3. 클라이언트에서 라우트 핸들러 사용하기**

라우트 핸들러는 서버에서 실행되고, 데이터를 클라이언트로 반환해요.

API 토큰과 같은 민감한 정보를 클라이언트에 노출하고 싶지 않을 때 사용할 수 있어요.

**4. 서드파티 라이브러리로 클라이언트 데이터 가져오기**

`SWR`이나 `TanStack Query(React Query)`를 사용하여 클라이언트에서 데이터를 가져올 수 있어요.

서드파티 라이브러리 자체에서 메모화, 캐싱, 재검증 등 자체 API를 제공한다는 특징이 있어요.

---

> **참고 자료**

[Streaming with Suspense](https://app-router.vercel.app/streaming)

[Edge and Node.js Runtimes](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes)

[Data Fetching, Caching, and Revalidating](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#static-data-fetching)
