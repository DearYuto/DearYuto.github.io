---
sidebar_position: 1
---

# Dart 문법 기초 - 변수

## 기본 지식

- 다트는 `main()`함수로 프로그램을 시작해요.

- 다트는 `;`을 생략할 수 없어요.

> **Dart 주석 사용 방식**

```js
// 한줄 주석

/**
 * 여러 줄 주석 사용 방법
 */
```

> **`print()`함수를 사용하여 콘솔 출력을 할 수 있어요.**

```c
void main(){
  print('유토냥이');
}
```

## 🌈 변수

> **변수 선언하기**

### var

```c
void main() {
  var name1 = '유토';
  // name1 = 1; 에러
}
```

`var` 키워드로 선언하면 자동으로 타입을 추론하게 돼요.

컴파일 될 때 `var`는 추론된 타입으로 치환이 되는데, 위 코드에서 name1에 `String`타입의 문자열을 할당했기 때문에 이미 타입이 String으로 추론되어있는 상태에요.

따라서, 이후 다른 타입으로 값을 변경하고자 하면 에러가 발생해요.

위 코드에서는 이미 `String` 타입인 `유토`가 할당되어 있는 상태에서 `int` 타입인 `1`로 값 변경을 시도했기 때문에 에러가 발생했어요.

### dynamic

```c
void main() {
  dynamic name2 = '유토냥이';
  name2 = 1;
}
```

`dynamic` 타입은 변수의 타입이 고정되어 있지 않은 상태로 여러 타입을 재할당 할 수 있어요.

타입스크립트를 사용해봤다면 `any`와 유사하다는 것을 알 수 있어요.

:::tip

**var와 dynamic 차이점**

`var`는 이미 한번 특정 타입으로 값이 할당되면 다른 타입으로 값을 재할당 할 수 없어요.

`dynamic`은 이미 특정 타입으로 값이 할당되어 있더라도 다른 타입으로 재할당할 수 있어요.
:::

### final

```c
void main() {
  final String name3;
  name3 = '유토냐아앙이';
  // 에러 name3 = '444';
}
```

`final`은 `런타임 상수`에요.

한번 할당된 값은 다른 값으로 변경할 수 없어요.

`DateTime`과 같은 값을 사용해서 런타임 시점에 알 수 있는 값을 저장할 때 사용할 수 있어요.

### const

```c
void main() {
  const String name4 = 'REASON';
  // 에러 const String name4;
}
```

`const`는 `빌드타임 상수`에요.

한번 할당된 값은 다른 값으로 변경될 수 없어요.

코드를 실행하지 않더라도 값을 확정할 수 있으면 `const`를 사용하면 돼요.

:::tip

**final과 const 차이점**

`final`은 런타임 시점에 값이 정해지는 상수를 저장할 수 있어요.

코드를 실행해야 값이 확정되는 DateTime과 같은 값을 다룰 때 적합해요.

`const`는 빌드타임 시점에 값이 정해지는 상수를 저장할 수 있어요.

코드를 실행하지 않더라도 값이 확정되는 상수라면 const를 사용해요.

:::

final과 const 모두 상수 타입이기 때문에 재할당은 할 수 없어요.

### 기본 변수 타입 (int, double, bool, String)

```c
void main() {
  String name5 = '이유';
  name5 = '배고픈 유토';

  int a = 4; // 정수

  double b = 4.4; // 실수

  bool isHungry = true; // true, false
}
```

`String` : 문자열

`int` : 정수

`double` : 실수

`bool` : 불리언

### 컬렉션 타입 - List

여러 개의 값을 순서대로 저장할 수 있는 타입이에요.

각 원소의 인덱스는 0부터 시작해요.

```c
void main() {
  List<String> yutoBox = ['배부른 유토', '배고픈 유토', '유토냥이', '유토'];

  yutoBox.add('유토 박스에 🐈 넣기'); // 리스트에 값 추가

  print(yutoBox);
}
```

### 컬렉션 타입 - Map

key와 value를 한 쌍으로 저장할 수 있는 타입이에요.

key를 통해 찾고자하는 데이터를 빠르게 찾을 수 있어요.

```c
void main() {
  Map<String, int> fruitBox = {
    'apple': 10,
    'banana': 2,
    'melon': 4,
  };

  print(fruitBox['apple']);
}

```

`key`로 접근하면 값을 꺼내올 수 있어요.

### 컬렉션 타입 - Set

중복되지 않는 값들만 저장되는 타입이에요.

```c
void main() {
  Set<String> yutoBox = {'유토', '유토', '유토', '유토'};

  print(yutoBox); // {유토}
}

```

초기 값으로 `유토`가 총 4번 중복되어 있지만, 실제로 값을 출력해보면 중복된 값은 모두 제거되고 하나만 남아 있음을 확인할 수 있어요.

:::tip

**Map과 Set을 List 타입으로 변경하는 방법**

.toList() 메서드를 사용하면 List 타입으로 변환할 수 있어요.

```c
yutoBox.toList();
```

:::

> 참고 자료

[코드팩토리의 플러터 프로그래밍](https://search.kyobobook.co.kr/search?keyword=%EC%BD%94%EB%93%9C%ED%8C%A9%ED%86%A0%EB%A6%AC%EC%9D%98%20%ED%94%8C%EB%9F%AC%ED%84%B0%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D&gbCode=TOT&target=total)
