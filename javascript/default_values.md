js에서 Default 값을 주는 두 가지 방법

### 첫 번째

함수 내부에서 값이 들어왔는지 확인하는 방식

```typescript
function createElement(type, height, width) {
  const element = document.createElement(type || "div");

  element.style.height = height || 100;
  element.style.width = width || 100;

  return element;
}
```

### 두 번째

props에서 설정하는 방식

```typescript
function createElement(type = "div", height = 100, width = 100) {
  const element = document.createElement(type);

  element.style.height = height;
  element.style.width = width;

  return element;
}
```

두 번째 방식이 낫다고 판단됨.
