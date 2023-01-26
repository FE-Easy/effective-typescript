# 230126
## 이펙티브 타입스크립트 14 15 16 17
## 14. 타입 연산과 제너릭 사용으로 반복 줄이기

## 15. 동적 데이터에 인덱스 시그니처 사용하기
### 인덱스 시그니처(Index Signature)란?
> 인덱스 시그니처(Index Signature)는 { [Key: T]: U } 형식으로 객체가 여러 Key를 가질 수 있으며, Key와 매핑되는 Value를 가지는 경우 사용합니다.
### 문제) 인덱스 시그니처에 대한 아래 문제를 읽고 물음에 답해보세요 - 은빈
### 1. `[property: string]: string`이 인덱스 시그니처이며, 다음 세 가지 의미를 담고 있습니다. 빈칸을 채워주세요.
```tsx
type Rocket = { [property: string]: string };
const rocket: Rocket = {
  name: "Falcon 9",
  variant: "v1.0",
  thrust: "4,940 kN",
};
```

- 키의 이름: 키의 (__)만 표시하는 용도입니다. (____)에서는 사용하지 않기 때문에 무시할 수 있는 참고 정보라고 생각해도 됩니다.
- 키의 타입: string이나 number 또는 symbol의 조합이어야 하지만, 보통은 (______)을 사용합니다.
- 값의 타입: 어떤 것이든 될 수 있습니다.

### 2. 이렇게 타입 체크가 수행되면 네 가지 단점이 드러나게 됩니다. 단점 네가지를 얘기해주세요.

1. 
2. 
3. 
4. 

### 3. 어떤 타입에 가능한 필드가 제안되어 있을 경우 인덱스 시그니처보다 정확한 타입을 사용해야합니다. 선택적 필드 또는 유니온 타입의 예시 타입을 작성해주세요.

```tsx
// Bad 너무 광범위
interface Row1 {
  [column: string]: number;
}

// 아래에 작성해주세요
```

## 16. number 인덱스 시그니처 보다는 Array, 튜플, ArrayLike를 사용하기
## 17. 변경 관련된 오류 방지를 위해 readonly 사용하기
### 문제) `readonly`에 대한 아래 문제를 읽고 물음에 답해보세요 - 종한
### 1. 아래는 readonly 와 const 를 비교한 표입니다. 빈칸을 채워주세요
<table>
  <tr>
    <th>구분</th>
    <th>readonly</th>
    <th>const</th>
  </tr>
  <tr>
    <th>재할당</th>
    <th> </th>
    <th>불가</th>
  </tr>
  <tr>
    <th>배열일 때 원소 추가, 수정, 삭제</th>
    <th> </th>
    <th>가능</th>
  </tr>
  <tr>
    <th>원본 배열을 변경하는 메서드(pop, push 등) 사용 가능?</th>
    <th> </th>
    <th>가능</th>
  </tr>
</table>

### 2. 아래 코드를 읽고 물음에 답해주세요
> 단락으로 구분된 문자열이 주어질 때, 이를 \n 으로 구분하는 프로그램을 만들려고 합니다. 아래 코드를 수정하여 올바르게 동작하게 수정해주세요
> (타입 에러를 해결해야 합니다)

https://bit.ly/3XJ6yoy

```ts
const text = 
`Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Quisque ex sapien, consequat eget vulputate ut, lobortis in augue.

Integer vel aliquet purus. Suspendisse hendrerit nisi ac hendrerit cursus.
Vestibulum sed metus dapibus, congue ante sed, luctus orci.
Pellentesque urna sapien, lobortis bibendum vehicula quis, hendrerit sed augue.

Nulla non massa non est luctus accumsan. Phasellus iaculis tortor sem,
sed tincidunt tortor pulvinar ac. Integer pharetra ante sit amet mi pellentesque consequat.
Nam nisl sapien, euismod nec metus at, tempor maximus justo.`;

// 현재 출력
console.log(parseTaggedText(text.split("\n"))); // [[], [], []]

// 올바른 출력
console.log(parseTaggedText(text.split("\n"))); // [[Lorem... ], [Integer... ], [Nulla ...]]

// 수정할 함수
function parseTaggedText(lines: string[]): string[][] {
  const paragraphs: string[][] = [];
  const currPara: readonly string[] = [];

  const addParagraph = () => {
    if (currPara.length) {
      paragraphs.push(currPara); // 이 부분을 고쳐주세요
      currPara.length = 0; // 이 부분을 고쳐주세요
    }
  };

  for (const line of lines) {
    if (!line) {
      addParagraph();
    } else {
      currPara.push(line); // 이 부분을 고쳐주세요
    }
  }
  addParagraph();
  return paragraphs;
}
```