## 논의 내용

### 에러 처리 기준

- server와 관련된 에러(ex. api 호출 관련)는 react-query를 통해 해결
- client와 관련된 에러(ex. 화면에 띄울 data가 없는 경우, 라우팅 관련 에러, ...)는 react의 error boundary를 활용

### 로깅 범위

에러 시스템 도입 후 production 레벨에서만 진행할지, 로컬 환경(development)에서도 진행할지 논의 필요

### 에러 코드와 메세지 관리

- 에러코드 500미만의 에러는 클라이언트에서 따로 관리하기
- 500번 대의 에러는 react-query의 query cache 등을 이용해 일괄적으로 관리

## 참고

[react error boundary](https://github.com/bvaughn/react-error-boundary)

https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary

[QueryCache in react query](https://tanstack.com/query/v4/docs/react/reference/QueryCache)

https://fe-developers.kakaoent.com/2022/221110-error-boundary/
