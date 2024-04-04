---
sidebar_position: 2
---

# 서버 컴포넌트, 클라이언트 컴포넌트는 언제 사용할까요?

### 📄 요약

> **서버 컴포넌트를 사용할 때**

1. 데이터를 fetch하는 경우
2. 백엔드 리소스에 직접 액세스하는 경우
3. 액세스 토큰, API 키와 같은 민감한 정보를 보관하는 경우
4. 클라이언트 측 자바스크립트 리소스를 감소 시키고 서버에 종속성을 높이는 경우

> **클라이언트 컴포넌트를 사용할 때**

1. 사용자와 상호 작용(이벤트 등)을 해야 하는 경우
2. 상태 및 수명주기(useState, useEffect 등)를 사용하는 경우
3. 브라우저 전용 API를 사용하는 경우
4. 사용자 정의 훅이 state, effect, 브라우저 API에 의존하는 경우

---

리액트 애플리케이션을 빌드할 때 서버, 클라이언트 어디에서 렌더링할지 고려해야 해요.

서버 컴포넌트에서는 클라이언트 측 렌더링이 수행되기 전,
데이터를 가져오거나 데이터베이스 등 백엔드 서비스에 액세스 될 때 작업을 수행할 수 있어요.

Next에서는 `env`파일에 접근할 때 접두사에 `NEXT_PUBLIC`을 붙이면 클라이언트에서 액세스 할 수 있지만,
env의 key에 `NEXT_PUBLIC`를 붙이지 않으면 서버에서만 액세스 할 수 있는 비공개 변수가 돼요.

다음은 Next.js의 공식 문서 예시코드에요.

여기서는 `NEXT_PUBLIC` 접두사가 없기 때문에 서버에서만 액세스 할 수 있을 거예요.

```tsx
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      // highlight-next-line
      authorization: process.env.API_KEY,
    },
  });

  return res.json();
}
```

### 왜 이렇게 구분을 두었을까요?

보안상 이유 등 의도치 않은 서버 코드 사용을 방지하기 위해서에요.

`server-only` 패키지를 설치해서 사용한다면, 해당 패키지는 서버 사이드에서만 작동되도록 설계되어 있기 때문에 클라이언트 사이드에서 아래 API를 호출한다면 에러가 발생해요.

```tsx
// highlight-next-line
import 'server-only';

export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  });

  return res.json();
}
```

### 클라이언트 컴포넌트

Next.js에서는 `use client;` 지시어를 상단에 선언하여 클라이언트 컴포넌트임을 명시할 수 있어요.

만약, 사용자와 상호작용 하는 등 클라이언트 전용 기능을 사용하고 있는 컴포넌트를 서버 사이드에서 사용한다면 에러가 발생할 수 있기 때문에 클라이언트 컴포넌트로 래핑하여 사용하는 방법으로 해결할 수 있을 거예요.

컨텍스트 프로바이더는 서버 컴포넌트에서 지원되지 않기 때문에 마찬가지로 프로바이더를 다른 파일로 분리하여 래핑시키는 방법으로 사용할 수 있어요.

자바스크립트 번들 크기를 줄이기 위해서는 클라이언트 컴포넌트를 [트리 아래로 이동](https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns#moving-client-components-down-the-tree)하는 것이 좋다고 해요. (Next.js 공식문서 피셜)

:::danger CHECK!

서버 컴포넌트를 클라이언트 컴포넌트로 가져오는 패턴은 지원되지 않아요.

:::

:::tip
서버 컴포넌트를 클라이언트 컴포넌트의 프롭스로 전달할 수 있어요. (children 활용하기)
:::

서버 컴포넌트를 클라이언트 컴포넌트의 children으로 넣어주는 방법을 사용하면, 클라이언트 컴포넌트와 서버 컴포넌트가 분리되어 독립적으로 렌더링이 가능해요.

---

> 참고 자료

[Server and Client Composition Patterns](https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns#when-to-use-server-and-client-components)

```;

```
