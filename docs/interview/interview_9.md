---
sidebar_position: 10
---

# Next.js Route 설정하기 (서버, 클라이언트)

Route 설정은 서버와 클라이언트에서 모두 할 수 있어요.
리디렉션은 서버 컴포넌트, 라우트 핸들러, 서버 액션에서 사용할 수 있어요.

| API                          | 목적                                     | 위치                                    | Status Code  |
| :--------------------------- | :--------------------------------------- | :-------------------------------------- | :----------- |
| redirect                     | 이벤트 또는 mutation후 사용자 리다이렉트 | 서버 컴포넌트, 서버 액션, 라우트 핸들러 | 307 또는 303 |
| permanentRedirect            | 이벤트 또는 mutation후 사용자 리다이렉트 | 서버 컴포넌트, 서버 액션, 라우트 핸들러 | 308          |
| useRouter                    | 클라이언트 측 네비게이션 수행            | 클라이언트 컴포넌트의 이벤트 핸들러     | -            |
| next.config 파일에 redirects | 경로 기반 요청 리디렉션                  | next.config 파일                        | 307 또는 308 |
| NextResponse.redirect        | 조건에 따라 리퀘스트 리디렉션            | 미들웨어                                | Any          |

<br/>

> ### **1. 서버 컴포넌트에서 리디렉션하기**

```tsx
redirect(path, type);
```

<br/>

첫 번째 인수 `path`로 리디렉션할 `상대 경로`나 `절대 경로`를 입력해요.

두 번째 인수인 `type`은 수행할 리디렉션 유형을 작성하면 되는데, 서버 컴포넌트에서 사용 시 의미가 없어요. (replace 또는 push를 사용할 수 있어요.)

### 서버 액션은 어떤 응답을 제공할까요?

서버 액션에서 사용할 경우 303 HTTP 리디렉션 응답을 제공해요.
그렇지 않은 경우엔 307 HTTP 리디렉션 응답을 제공해요.

서버 액션, 경로 핸들러에서 리디렉션은 try/catch 블록 이후에 호출해야 하는데 307(임시) 대신 308(영구) HTTP 리디렉션을 반환할 경우, 영구 리디렉션 함수를 대신 사용할 수 있어요.

:::tip `303(See Other)`과 `307(Temporary Redirect)` 차이점

요청을 리다이렉트 할 때 메소드를 GET으로 바꾸려면 `303`을 사용해요.
예를 들어, 사용자가 폼으로 데이터를 전송했을 때 서버에서 데이터를 처리하고 사용자를
결과 페이지로 안내하고 싶다면 303 리다이렉트를 사용해볼 수 있어요.
폼 다중 제출을 방지할 수 있고, 사용자가 새로고침할 때 같은 데이터가 재전송 되는 것을 막을 수 있어요.

`307`은 최초 요청이 `POST`라면 `POST`를 유지해야 해요.
메소드를 변경하지 않고, 리소스 위치를 변경할 때 사용할 수 있어요.
예를 들어, 로드 밸런싱 등으로 인해 다른 서버로 요청을 전달해야 할 때 사용할 수 있어요.

:::

<br/>

> ### **2. 클라이언트 컴포넌트에서 리디렉션하기**

서버 액션을 통해 클라이언트 컴포넌트에서 리디렉션을 사용할 수 있어요.

이벤트 핸들러를 사용해서 사용자를 리디렉션 시킬 때 `useRouter`를 사용해요.

다음은 useRouter를 사용하지 않고 리디렉션 하는 방법인데 `navigate` 함수를 서버 컴포넌트로 만든 후, 클라언트 컴포넌트에서 import 해서 form action으로 넣어주는 예시에요. (Next.js 공식문서 발췌)

```tsx
// actions.ts
'use server';

import { redirect } from 'next/navigation';

export async function navigate(data: FormData) {
  redirect(`/posts/${data.get('id')}`);
}
```

```tsx
// client-redirect.tsx
'use client'

import { navigate } from './actions'

export function ClientRedirect() {
  return (
    <form action={navigate}>
    // 이하 생략
```

[useRouter](https://nextjs.org/docs/app/api-reference/functions/use-router#userouter)를 사용한다면 다음과 같이 사용해볼 수 있어요.

```tsx
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()
  // 이하 생략
```

:::warning 페이지 라우터에서 앱 라우터로 마이그레이션 할 경우
useRouter를 import할 때 `next/navigation`에서 가져와야 해요.

:::

<br/>

> ### 3. **영구 리디렉션**

사용자를 다른 URL로 영구적으로 리디렉션할 때 사용할 수 있어요.
서버 컴포넌트, 라우트 핸들러, 서버 액션에서 영구 리디렉션 호출이 가능해요.

사용자가 아이디를 변경했거나, 프로필 URL을 업데이트 하는 등의 엔티티 정식 URL을 변경하는 이벤트 이후 등에 주로 사용돼요.

영구 리디렉션의 상태코드는 기본적으로 `308`을 반환해요.
절대 URL도 허용하고, 외부 링크로 리디렉션 하는데 사용할 수도 있어요.

만약, 렌더링 프로세스 전에 리디렉션을 해야 한다면 `next.config` 세팅을 하거나, `미들웨어`를 사용하는 방법으로 리디렉션을 수행할 수 있어요.

<br/>

> ### **4. next.config 파일에서 redirects 설정하기**

path, header, cookie, 쿼리 매칭을 지원하고 있어서 수신 요청에 따라 사용자를 유연하게 리디렉션 할 수 있어요.

```ts
// next.config.js
module.exports = {
  async redirects() {
    return [
      // Basic redirect
      {
        source: '/about',
        destination: '/',
        permanent: true,
      },
      // Wildcard path matching
      {
        source: '/blog/:slug',
        destination: '/news/:slug',
        permanent: true,
      },
    ];
  },
};
```

<br/>

> ### **5. 미들웨어 NextResponse.redirect**

미들웨어를 사용하면, 요청이 완료되기 전에 코드를 실행할 수 있어요.

프로젝트의 루트에서 `middleware.ts(또는 js)` 파일을 생성하여 정의하면 돼요.
프로젝트당 하나의 미들웨어 파일만 지원되지만, 미들웨어 로직을 모듈로 구성할 수는 있어요. (여러 개의 파일을 분리하고 middleware로 import하는 방식)

사용자 인증이나 세션 관리 등 사용자를 리디렉션할 때, 리디렉션 횟수가 많을 때 유용해요. (예를 들면, 비로그인 회원이 로그인 페이지에 진입했을 때)

특정 경로를 정확하게 타게팅하거나 제외할 때는 [매처](https://nextjs.org/docs/app/building-your-application/routing/middleware#matcher)를 사용하면 돼요.

```ts
// middleware.ts
import { NextResponse, NextRequest } from 'next/server';
import { authenticate } from 'auth-provider';

export function middleware(request: NextRequest) {
  const isAuthenticated = authenticate(request);

  // If the user is authenticated, continue as normal
  if (isAuthenticated) {
    return NextResponse.next();
  }

  // Redirect to login page if not authenticated
  return NextResponse.redirect(new URL('/login', request.url));
}

export const config = {
  matcher: '/dashboard/:path*',
};
```

미들웨어는 next.config파일의 리디렉션 후, 렌더링 전에 실행돼요.

또, 캐시된 콘텐츠와 경로가 일치하기 전에 미들웨어가 실행돼요.

:::tip 서버에서 반드시 리다이렉트를 수행해야 하는 경우

- 사용자 인증 후 특정 페이지로 자동 이동할 때

- 페이지나 리소스의 URL이 변경됐을 때 오래된 URL에서 새로운 URL로 사용자를 리다이렉트시켜 404 오류 방지 및 SEO 최적화

- 사용자가 접근 권한이 없는 페이지에 접근 시

- 폼 제출 작업 완료 후 성공 페이지나 다른 페이지로 리다이렉트 시킬 때. (중복 제출 방지 및 사용자에게 작업 완료를 알릴 수 있어요.)

:::

---

> 참고 자료

[redirect](https://nextjs.org/docs/app/api-reference/functions/redirect)
[Redirecting](https://nextjs.org/docs/app/building-your-application/routing/redirecting#managing-redirects-at-scale-advanced)
