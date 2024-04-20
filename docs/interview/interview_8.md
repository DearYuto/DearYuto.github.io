---
sidebar_position: 9
---

# Zustand 상태관리 라이브러리

> Context Provider 없이도 사용할 수 있는 이유

React에서 제공하는 `useSyncExternalStore`를 사용하여 `tearing`을 방지해요.

:::tip tearing이란?

상태 업데이트가 비동기적으로 발생할 경우 일어날 수 있는 현상이에요.

예를 들어, 동일한 state를 바라보고 있는 컴포넌트가 존재하지만 각자 다른 값을 출력하고 있는 현상을 말해요.

업데이트 도중에 컴포넌트가 일관성 없는 상태를 읽게될 수 있는 문제를 방지하고자 `useSyncExternalStore`를 사용할 수 있어요.

:::

`useSyncExternalStore`는 외부 스토어와 싱크를 맞추는 React hooks에요.

`useSyncExternalStore`를 사용하게 되면 `useState`나 `useReducer`와 같이 react에서 상태를 변경 시 사용되는 API를 사용하지 않고도 상태 변화를 감지할 수 있어요.

Zustand는 클로저를 통해 상태를 관리하고 있는데, 클로저만 사용해서는 컴포넌트가 상태 변화를 감지하지 못하기 때문에 `subscribeToStore` 함수를 사용하여 클로저를 리스너에 추가하는 방법으로 동작하고 있어요.
상태가 변경되면 리스너에게 알려서 그때마다 컴포넌트가 적절히 반응을 할 거예요.

:::tip 그렇다면 `useState`나 `useReducer`를 사용하지 않아도 되는 거 아닌가요?

리액트 공식문서에 의하면 useSyncExternalStore API는 React 코드가 아닌 코드와 통합해야 할 때 사용하는 것을 권장하고 있어요.
따라서, **가능하면 내장된 useState, useReducer와 함께 사용하는 것**이 좋아요.

:::

## Zustand, Redux, Recoil의 차이점

#### 1. Zustand

Redux와 달리 복잡한 보일러 플레이트 없이 사용할 수 있어요.

간결한 설정으로 러닝커브가 작고, 라이브러리 크기도 작아요.

#### 2. Redux

상태 관리 라이브러리 중에 가장 널리 알려져 있고, 중앙 집중식 저장소(스토어)를 사용해요.

Redux DevTools를 사용하여 상태 변화를 시각화하거나 디버깅 할 수 있어요.

보일러 플레이트로 인해 초기 설정이 복잡하여 러닝커브가 높아요.

#### 3. Recoil

원자(atom)과 선택자(selector)를 기반으로 해요.

세밀한 업데이트가 가능하다는 장점이 있지만, 이로 인해 복잡해질 수 있어요.

---

[상태 관리 라이브러리 Zustand](https://www.nextree.io/zustand/)

[useSyncExternalStore](https://ko.react.dev/reference/react/useSyncExternalStore)
