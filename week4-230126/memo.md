# memo
## 📍 `for-in` loop 는 느리다
배열을 순회할 때는 `for-in` loop 대신 다른 loop를 사용하는 것이 권장되는데 그 이유는 아래와 같다
- `for-in` loop 가 느리기 때문에
  - `for-in` loop는 프로토타입 체인에 있는 모든 프로퍼티를 순회하기 때문에 느릴 수 밖에 없다
- 순회 순서가 일정하지 않아서
  - 순회 순서는 브라우저에 따라 다르게 구현되어 있어서 늘 index 순서대로 순회하지 않을 수 있다