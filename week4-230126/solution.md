# 230126
## 이펙티브 타입스크립트 14 15 16 17
## 14. 타입 연산과 제너릭 사용으로 반복 줄이기

## 15. 동적 데이터에 인덱스 시그니처 사용하기
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
    <th>가능</th>
    <th>불가</th>
  </tr>
  <tr>
    <th>배열일 때 원소 추가, 수정, 삭제</th>
    <th>불가</th>
    <th>가능</th>
  </tr>
  <tr>
    <th>원본 배열을 변경하는 메서드(pop, push 등) 사용 가능?</th>
    <th>불가</th>
    <th>가능</th>
  </tr>
</table>

### 2. 아래 코드를 읽고 물음에 답해주세요
> 단락으로 구분된 문자열이 주어질 때, 이를 \n 으로 구분하는 프로그램을 만들려고 합니다. 아래 코드를 수정하여 올바르게 동작하게 수정해주세요
> (타입 에러를 해결해야 합니다)

https://bit.ly/3Hw7mYa

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
  let currPara: readonly string[] = [];

  const addParagraph = () => {
    if (currPara.length) {
      paragraphs.push([...currPara]);
      currPara = [];
    }
  };

  for (const line of lines) {
    if (!line) {
      addParagraph();
    } else {
      currPara = [...currPara, line];
    }
  }
  addParagraph();
  return paragraphs;
}
```