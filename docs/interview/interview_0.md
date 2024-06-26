---
sidebar_position: 1
---

# 브라우저의 렌더링 과정

> 📔 **간단 요약**

- **1. 자원 다운로드** - 사용자가 웹 사이트에 접속 시 필요한 리소스(HTML, CSS 등)을 다운로드 받는다.
- **2. DOM 트리 구축** - HTML을 파싱하여 DOM 트리를 만든다.
- **3. CSSOM 트리 구축** - CSS를 처리하고 CSSOM 트리를 만든다.
- **4. 렌더 트리 구축** - DOM 트리와 CSSOM를 합쳐 렌더 트리를 만든다.
- **5. 레이아웃 단계** - 렌더 트리를 기반으로 레이아웃을 실행하여 각 객체의 위치와 크기를 계산한다.
- **6. 페인트 단계** - 레이아웃 단계에서 계산된 각 박스를 실제 화면의 픽셀로 변환한다.
- **7. 합성 단계** - 올바른 순서로 화면에 그려지기 위해 합성 단계가 진행된다.

---

> ### 💫 렌더링 과정

#### 1. DOM 트리 생성

```
변환 ▶ 토큰화 ▶ 렉싱 ▶ DOM 생성
```

- 바이트를 문자로 변환 : 브라우저가 HTML 원시 바이트를 정의한 인코딩(ex: UTF-8)에 따라 문자로 변환하여 원시 텍스트를 구성해요.

- 토큰 변환 : 문자열을 읽고 HTML 구문 분석하여 브라우저가 문자열을 W3C 표준에 지정된 고유 토큰으로 변환해요.

- 노드 변환 : 각 토큰을 특정 타입의 노드로 변환해요.
  예를 들어, div는 Element 노드, 텍스트는 Text 노드, 속성은 Attribute 노드가 될 거예요.

- DOM 트리 구축 : 각 노드들은 HTML 문서 구조에 따라 트리 형태로 조직돼요.
  이때 만들어진 트리가 DOM 트리가 되고, 각 노드는 HTML 문서의 태그에 해당해요.

#### 2. CSSOM 트리 생성

브라우저가 CSS 규칙을 이해할 수 있도록 스타일 맵으로 변환해요.

브라우저는 CSS의 규칙을 읽고, 트리 노드를 만들어요.

이때, CSS 선택기에 따라 부모, 자식, 형제 관계 노드가 만들어 질 거예요.

브라우저는 노드에 적용할 수 있는 일반적인 규칙을 적용하고, 재귀적으로 더 구체적인 규칙으로 작성된 스타일을 캐스캐이딩해요.

#### 3. 렌더링 트리 생성

DOM 트리와 CSSOM 트리를 결합해 렌더링 트리를 만들어요.
이때 보이지 않는 요소는 포함되지 않아요.

예를 들면, display : none으로 설정된 요소, HTML의 head 태그 내의 요소는 렌더링 트리에서 제외돼요.

#### 4. 레이아웃 단계

렌더 트리를 기반으로 모든 노드의 너비, 위치, 높이를 결정하고, 각 객체의 크기와 위치를 계산해요.

#### 5. 페인트 단계

레이아웃 단계에서 계산된 객체를 실제 화면의 픽셀로 변환해요.

텍스트, 색상, 그림자 등 모든 시각적인 요소를 화면에 그리는 작업이 포함돼요.

#### 6. 합성 단계

레이아웃과 페인트 단계에서 준비된 요소를 합쳐 사용자에게 보여줄 최종 화면을 만들어요.

렌더링된 레이어를 최종적으로 하나로 합성하는 과정이에요.

---

> ### **참고 자료**

[브라우저의 렌더링 과정](https://medium.com/%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98%ED%92%88%EA%B2%A9/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95-5c01c4158ce)

[브라우저는 어떻게 동작하는가](https://developer.mozilla.org/ko/docs/Web/Performance/How_browsers_work)
