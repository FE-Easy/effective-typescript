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

// 문제 2. '매핑된 타입'을 활용하여 해당 객체의 타입을 명시해주세요
const REQUIRES_UPDATE = {
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
    if (oldProps[k] !== newProps[k]) { // 문제3: if 조건을 추가하여 익명함수는 체크하지 않도록 해주세요
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
console.log(shouldUpdate(data1, data2)); 
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

### 2. 다른 타입에는 별도의 변수를 사용할때의 장점을 얘기해주세요.