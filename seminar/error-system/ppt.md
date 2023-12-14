---
marp: true
title: Error System
paginate: true
theme: olive
footer: "에러 시스템 도입기"
---

# 에러 시스템 도입기

FT2-1 김상호, 김해람, 오현석

<!-- paginate: true -->

---

# 에러, 어떻게 처리하고 계신가요?

---

<!-- header: "에러, 어떻게 처리하고 계신가요?" -->

## try-catch의 도배

```javascript
const getList = async () => {
  try {
    const response = await axios.get("path");
  } catch (error) {
    console.error(error); ?
    window.alert(error); ?
  }
};
```

---

<!-- header: "에러, 어떻게 처리하고 계신가요?" -->

## 무작정 throw 하기

```javascript
const getList = async () => {
  try {
    const response = await axios.get("path");
  } catch (error) {
    throw new Error(error);
  }
};
```

\# 어디로 던지는 걸까요...?

---

<!-- header: '' -->

# 어떤 경우에 발생할까?

---

<!-- header: "어떤 경우에 발생할까?" -->

## api 호출 실패 시

```javascript
// api.js
const getList = async () => {
  const response = await axios.get("path");
};
```

---

<!-- header: "어떤 경우에 발생할까?" -->

## 잘못된 data에 접근할 경우

```javascript
// api.js
const getList = async () => {
  const response = await axios.get("path");
};

// component.js
const data = await getList();
```

---

# 어떻게 처리할까

---

<!-- header: "어떻게 처리할까" -->

### Error boundary

- 페이지에서 런타임에 발생할 수 있는 에러들

- 페이지 데이터와 관련된 에러

```
const data = await get();

...
<view>
  {data.map}
</view>
```

- 개발자가 막 던진 에러들

---

<!-- header: "" -->

# 에러의 기준점 정하기

- 나도 모르게 런타임에 발생한 에러

- 막 던져도 받을 수 있는 환경

---

<!-- header: "에러의 기준점 정하기" -->

## RootBoundary

- 전역에서 처리

- 주로 런타임상에서 예측하지 못하게 발생한 에러들이나,

<br />
<br />

## PageBoundary

- 페이지 별로 에러가 발생했을 때 동작이 특정되는 경우

- 기본적으로는 에러가 Root로 전파되지 않음 (데이터 보존)

- 로깅을 선택할 수 있도록 props로

---

<!-- header: "에러의 기준점 정하기 / RootBoundary" -->

RootBoundary 코드 넣기

---

<!-- header: "에러의 기준점 정하기 / PageBoundary" -->

PageBoundary 코드 넣기

---

<!-- header: "" -->

# 추가 : 슬랙에 로깅하기

- sentry와 같은 로깅 툴을 쓰면 좋지만, 너무 오버스펙

- 간단히 에러만 알려주는 용도로 슬랙을 활용

- 별도의 프록시 서버 (로컬에서는 안돌리고, 스테이징이나 qa 용도)

- 메세지 찍은거 캡쳐

---

# 제안사항

도메인별로 코드 분리하기

- 상팩 600
- 계약 700
- 고객 800

이건 어디다 넣을까요..
