# 반복문

</br>

## 반복문을 파이프라인으로 바꾸기
객체 컬렉션을 순회할 때 반복문을 사용하라고 배워왔다. 하지만 언어는 계속해서 더 나은 구조를 자공하는 쪽으로 발전해왔다. 컬렉션 파이프라인을 이용하면 처리 과정을 일련의 연사으로 표현할 수 있다.

### before
```javascript
const names = [];
for (const i of input) {
  if (i.job === "programmer") names.push(i.name);
}
```

### after
```javascript
const names = input
  .filter(i => i.job === "programmer")
  .map(i => i.name)
```

### before
```javascript
function acquireData(input) {
  const lines = input.split("\n");
  const firstLine = true;
  const result = [];
  for (const line of lines) {
    if (firstLine) {
      firstLine = false;
      continue;
    }
    if (line.trim() === "") continue;
    const record = line.split(",");
    if (record[1].trim() === "India") {
      result.push({city: record[0].trim(), phone: record[2].trim()});
    }
  }
  return result;
}
```

### after
```javascript
funtion acquireData(input) {
  const lines = input.split("\n");
  const result = lines
    .slice(1);
    .filter(line => line.trim() !== "")
    .map(line => line.split(","))
    .filter(record => record[1].trim() === "India")
    .map(record => ({city: record[0].trim(), phone: record[2].trim()}));
  return result;
}

funtion acquireData(input) {
  const lines = input.split("\n")
  return lines
        .slice  (1);
        .filter (line => line.trim() !== "")
        .map    (line => line.split(","))
        .filter (fields => fields[1].trim() === "India")
        .map    (fields => ({city: fields[0].trim(), phone: fields[2].trim()}))
        ;
}
```