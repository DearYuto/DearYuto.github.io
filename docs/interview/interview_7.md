---
sidebar_position: 8
---

# 언제 전역상태를 써야 할까?

클라이언트 상태관리를 하기 위해 로컬과 전역을 고민해볼 수 있어요.

**1. 여러 컴포넌트에서 데이터를 공유할 때**

- 사용자 세션 정보, 로그인 상태와 같은 정보 등 여러 부분에서 지속적으로 참조해야 할 수 있기 때문에 전역 상태를 사용하면 코드의 가독성과 유지보수성을 향상 시킬 수 있어요.

**2. 상태 업데이트 로직을 중앙화할 때**

- 상태 업데이트 관련 로직을 한 군데에서 관리하는 방법으로도 사용해볼 수 있어요.

**3. 불필요한 렌더링을 방지하고 싶을 때**

- ContextAPI의 경우 프로바이더의 단위를 잘게 쪼개서 사용하거나 리액트의 메모이제이션 API를 사용해 어느정도 방지할 수 있지만, 다른 전역상태 라이브러리를 사용한다면 ContextAPI를 사용해 발생할 수 있는 불필요한 렌더링까지도 최적화 할 수 있어요.

**4. 프롭스 드릴링을 최소화 하고 싶을 때**

- 컴포넌트 트리에서 많은 수준을 거쳐 props를 전달해야 할 경우 전역 상태를 사용하여 이를 방지할 수 있고, 코드의 가독성과 유지보수성을 향상시킬 수 있어요.

### 전역 상태 관리 선택지

전역 상태를 관리한다면 ContextAPI + useState 또는 useReducer를 사용하거나, Redux, Zustand와 같은 라이브러리를 사용해볼 수 있을 거예요.

> 그런데 왜 내장 API가 있음에도 Redux, Zustand와 같은 상태관리 라이브러리를 사용하는 걸까요?

### ContextAPI도 전역 상태 관리가 가능한데 라이브러리를 사용하는 이유

전역 상태 관리 측면에서는 차이점이 거의 없지만, `ContextAPI`는 빈번한 상태 변화가 발생한다면, 최적화되어 있지 않고 프로바이더로 감싸놓은 요소에서 변경이 발생하면 프로바이더 범위 내의 모든 컴포넌트가 리렌더링되어 불필요한 성능 이슈가 발생하게 돼요.

이러한 성능 이슈는 Provider를 더 작은 단위로 쪼개는 방법이 있겠지만, 가독성이나 유지보수 측면에서 다른 라이브러리와 비교했을 때 아쉬움이 있을 수 있어요.

Redux와 Zustand와 같은 라이브러리는 성능 최적화가 잘 되어 있어 불필요한 리렌더링이 발생되지 않는다는 특징이 있는데요,

특히 Zustand의 경우 Provider를 따로 세팅하지 않아도 되고, 보일러플레이트가 간결하다는 큰 장점이 있어요.

Zustand는 리액트 환경에서 상태 관리를 하기 위해 사용한다고 잘 알려져 있지만, 바닐라 자바스크립트 환경에서도 사용할 수 있어요.
