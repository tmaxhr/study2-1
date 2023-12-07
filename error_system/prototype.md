## 슬랙 로깅

- 현재 로깅할 때 필요한 정보들

  - 메세지, 에러 타입, status code, 스택, userAgent

- 현재 호스팅 해 놓은 서버는 일정 시간 동안 요청이 없으면 꺼져버림, 따라서 실제 운영 때에는 별도로 서버를 띄워야 함

## 에러 바운더리 구현

### Error boundary가 잡지 못하는 에러들

- 비동기 함수들 관련 에러
- 이벤트 핸들러에서 발생하는 에러

이를 잡기위해 `App.tsx`에 아래와 같은 코드를 입력해 주었다.

```typescript
useEffect(() => {
  ...

  window.addEventListener('error', handleError);
  return () => {
    window.removeEventListener('error', handleError);
  }
}, [...]);
```

위 코드를 통해 error boundary가 기본적으로 잡지 못하는 에러들을 잡도록 설정했다.

### 구현 내용

페이지 별로 처리할 에러들을 위한 `RetryErrorBoundary`, 전역으로 던져진 에러들을 처리할 `RootErrorBoundary`로 나누었다.

또한 alert를 보여줄 것인지, 500에러를 보여줄 페이지로 라우팅 시킬 것인지에 따라 error type을 아래와 같이 설정했다.

```typescript
type ErrorName = "ALERT_ERROR" | "CRITICAL_ERROR";
```

RetryErrorBoundary 에서는

1. AxiosError(400번대 에러) 또는 CRITICAL_ERROR 인 경우 각 경우에 맞는 페이지로 라우팅 시킨다.
2. 이외에 커스텀한 에러 코드들인 경우에는 alert를 보여주고, 화면이 refresh 되지 않도록 resetErrorBoundary()를 통해 에러를 리셋한다.
3. suspense가 필요하다면 suspense로 감싸 로딩 화면을 보여준다.

RootErrorBoundary는 RetryErrorBoundary와 동일하게 동작하지만, RetryErrorBoundary와 다른 점은 Suspense가 없다는 점이다.

### 로깅 전략

RetryErrorBoundary로 잡히는 에러들은 페이지에서 별도로 처리하는 에러들이기 때문에 사용자에게 충분한 안내를 해 줄 것이고, 따라서 슬랙에 로깅을 하지 않아도 된다.

RootErrorBoundary로 잡히는 에러들은 `예측하지 못한 에러들`이다. 따라서 이런 에러들은 슬랙 로깅을 통해서 모니터링이 필요하다.

## 백엔드와 협의해야 할 것들

- 500번대 에러까지는 자주 사용되는 에러 코드들이니 Error boundary에서 처리하면 된다.
- 이 외에 보상, 상품 등 파트 별로 에러를 분리해 어디 파트가 확인해야 할 지 확인하기 위해 600번대, 700번대 등 번호를 나누어서 사용하면 좋을 것 같다.
- 에러를 보여줄 것이라면, 어떤 경우에 사용자에게 어떤 가이드가 제공되어야 할지도 정책으로 정의가 되어야 할 것 같다.
