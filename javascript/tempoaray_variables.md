## Title

임시변수를 사용하지 말자.

## Content

임시 변수를 사용하면 안 좋은 점들

1. 코드보다는 명령에 가까운 코드들이 생김
2. 디버깅이 어려워짐
3. 코드가 변경될 가능성이 높아짐 (즉, 함수가 하나 이상의 역할을 하게 될 위험이 높아짐)

현재 기간계 프로젝트에서도 임시 변수가 많이 쓰이고 있는데, 대표적인 예시가 post, patch 등의 요청 시 body에 담을 객체를 생성하는 함수이다.

```typescript
const preProcessData = (data: any) => {
  const ret: any[] = [];

  data.forEach((el: any) => {
    ret.push({
      title: el.title,
      id: el.id,
      options: el.options.map((option: any) => {
        return {
          name: option.name,
          id: option.id,
        };
      }),
    });
  });

  return ret;
};
```

임시 변수 `ret` 을 이용해 데이터를 처리하고 있는데, data.map을 통해 해결 가능할 것으로 보인다.

## Todo

- preprocessData 함수에서 임시 변수 제거하기
