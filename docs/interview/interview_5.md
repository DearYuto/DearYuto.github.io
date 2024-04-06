---
sidebar_position: 6
---

# CSS z-index 속성 이해하기

#### CSS `position`에 따라 z-index는 어떻게 달라질까요?

:::danger
`z-index`를 많이 사용하게 되면 스타일 유지보수가 힘들어 질 수 있다는 단점이 있어요.
:::

position이 static(기본 값)외에 `relative(현재 자기 자신 위치를 기준)`, `absolute(relative 속성을 가진 부모 기준)`, `fixed(뷰포트 기준)`, `sticky(스크롤 영역 기준)`인 경우 z-index를 사용했을 때 겹침 순서가 변경될 수 있어요.

### 요소의 겹침 순서

요소는 HTML의 순서가 나중에 오는 것이 더 위로 깔린다는 특징이 있어요.

예를 들어 아래 이미지의 첫 번째 카드 묶음을 보면, 임의로 하트 카드를 margin 속성을 사용하여 별 카드 위로 배치한 경우를 보면 알 수 있어요.

![z-index 사용 전 예시](/img/z-index_0.png)

`첫 번째 카드 묶음`은 `하트 카드`에 margin을 넣어 겹쳐진 상태로
position이 `모두 static`인 경우에요.

`별 카드` 이후에 작성된 `하트 카드`가 더 위에 올라가 있음을 확인할 수 있어요.
하지만, 특이하게도 `★ 텍스트`가 하트 카드 앞에 얹어져 보이네요. 😮😮

브라우저 해석 방식에 따라 차이가 있는지 확인해보았는데, 크롬, 파이어폭스, 엣지에서 모두 동일하게 텍스트가 앞에 보여지고 있었어요. (신기한데 이 부분은 정확히 어떻게 동작하는 건지 모르겠네요....!)

`두 번째 카드 묶음`은 `별 카드`에 `position : relative;`을 설정한 상태입니다.
`하트 카드`가 코드 위치상 별 카드보다 나중에 작성되어 있지만, `relative`로 설정해주어 별 카드가 더 앞에

`세 번째 카드 묶음`은 는 두 카드 모두에 `position : relative;`을 설정했어요.
첫 번째 카드 묶음과 약간의 차이점이라 하면 `★ 텍스트`가 `하트 카드`의 뒤로 깔렸다는 점이 있겠네요.

### z-index를 적용할 경우

브라우저에서는 z-index가 낮은 속성을 먼저 그리고, 후에 z-index가 높은 요소를 먼저 그려요.

:::danger
position이 `static`인 경우 z-index 속성이 0(auto)으로 고정되므로 z-index 속성을 설정해주어도 제대로 동작하지 않아요.
:::

### z-index가 적용되는 범위 (stacking context)

`z-index`는 전체 범위에 적용되지 않아요.

부모 요소에 z-index가 설정되어있다면, 자식 z-index가 아무리 크더라도 부모 바깥에 있는 다른 z-index가 설정된 요소와 비교될 수 없어요.

예를 들어, 다음과 같은 코드가 있다고 가정해볼게요.

```html
<div style="z-index:1">
  <div style="z-index:9999" class="box box1">★</div>
</div>
<div style="z-index:10" class="box box2">♥</div>
```

![z-index 적용 범위 예시](/img/z-index_1.svg)

---

> **참고 자료**

[CSS의 z-index 속성 이해하기](https://www.daleseo.com/css-z-index/)
