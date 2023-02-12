# 230202
## 이펙티브 타입스크립트 18 19 20 21

## #18 매핑된 타입에 관한 다음  코드를 읽고 물음에 답해주세요 - 종한
```ts
interface ScatterProps {
  xs: number[];
  ys: number[]
  xRange: [number, number];
  yRange: [number, number];
  color: string;
  onClick: (x: number, y: number, index: number) => void;
}

type Answer = {
  [k in keyof ScatterProps]: boolean
}

// 문제 2. '매핑된 타입'을 활용하여 해당 객체의 타입을 명시해주세요
const REQUIRES_UPDATE: Answer = {
  xs: true,
  ys: true,
  xRange: true,
  yRange: true,
  color: true,
  onClick: false,
};

function shouldUpdate(
  oldProps: ScatterProps,
  newProps: ScatterProps
) {
  let k: keyof ScatterProps;
  for (k in oldProps) {
    if (oldProps[k] !== newProps[k] && REQUIRES_UPDATE[k]) { // 문제3: if 조건을 추가하여 익명함수는 체크하지 않도록 해주세요
      return true;
    }
  }
  return false;
}

const data1: ScatterProps =  {
  xs: [1, 2, 3],
  ys: [1, 2, 3],
  xRange: [1, 2],
  yRange: [1, 2],
  color: "red",
  onClick(x, y, index) {
    console.log(x, y, index)
  }
}

// data2 는 data1과 같음
const data2: ScatterProps =  {
  xs: [1, 2, 3],
  ys: [1, 2, 3],
  xRange: [1, 2],
  yRange: [1, 2],
  color: "red",
  onClick(x, y, index) {
    console.log(x, y, index)
  },
}

// 문제1. 아래 결과를 예측해보고 그 이유를 설명해주세요
console.log(shouldUpdate(data1, data2)); // true -> why?? 배열의 메모리 주소가 달라서
```

## #20 다른 타입에는 다른 변수 사용하기 - 은빈

### 1. 아래 코드에서 오류가 발생하는 이유에 대해 설명해주세요.

```tsx
let id = "12-34-56"; // string
fetchProduct(id);

id = 123456;
// ~~ '123456'형식은 'string' 형식에 할당할 수 없습니다.
fetchProductBySerialNumber(id);
// ~~ 'string' 형식의 인수는 'number' 형식의 매개변수에 할당될 수 없습니다.

// let으로 해서 string
```

- 답 : 타입스크립트는 "12-34-56"이라는 값을 보고, id의 타입을 string으로 추론
-> string 타입에 number 타입을 할당할 수 없기 때문에 오류 발생
- 오류해결
1. let id: string | number 로 유니온 타입 확장
-> 그러나, 더 많은 문제가 생길 수 있다.
id를 사용할 때 마다 값이 어떤 타입인지 확인해야하기 때문에 유니온 타입은 string이나 number 같은 간단한 타입에 비해 다루기 더 어려워진다.
2. 별도의 변수를 도입

```tsx
const id = "12-34-56";
fetchProduct(id);

const serial = 123456; // 정상
fetchProductBySerialNumber(id); // 정상
```

### 2. 다른 타입에는 별도의 변수를 사용할때의 장점을 얘기해주세요.
- 서로관련없는 두 값을 분리합니다.
- 변수명을 구체적으로 지을 수 있습니다.
- 타입 추론을 향상시키고, 타입 구문이 불필요해집니다
- let 대신 const를 사용할 수 있습니다.