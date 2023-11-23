## 서버 측 에러

공통된 에러 처리를 할 default callback을 QueryCache를 활용해 작성

```typescript
const queryClient = new QueryClient({
  ...
  queryCache: new QueryCache({
    onError: (error, query) => {
      if (process.env.NODE_ENV === 'development') {
        // 개발자에게 에러를 바로 표시
        window.alert(error);
      } else {
        // 사용자에게는 에러를 직접 표시하지 않음
        window.alert('네트워크 에러');
      }
    }
  })
})
```

💡 query에 특정한 메세지를 담아서 global onError에 전달하고 싶다면, query를 선언할 때에 meta 정보를 같이 전달하면 된다.

```typescript
// someQuery.ts
const query = useQuery(["key"], queryFn, {
  meta: { message: "hello from query" },
});
```

```typescript
// queryClient
queryCache: new QueryCache({
  onError: (error, query) => {
    console.log(query.meta?.message); // hello from query
  },
});
```

### 고민점

- offline일 때의 상태를 고려해야 할까?  
  tanstack query는 offline일 때는 기본적으로 동작하지 않도록 설정되어 있다. [참고 링크](https://github.com/TanStack/query/issues/2179#issuecomment-826054174)  
  queryClient의 defaultOptions > networkMode를 always로 설정해놓아 동작하게 할 수는 있지만, 현재는 굳이 필요없을 것 같아 오프라인인 경우는 고려하지 않음

- 어떤 에러들을 글로벌 콜백으로 처리해야 할까?  
  서버 단의 에러인 경우에만 글로벌 콜백으로 넘기는게 좋겠다.

- 데이터 fetching을 실패했을 때 `다시시도`와 같은 버튼을 만드는게 나을까?  
  괜히 하나가 로딩되는 중에 나머지는 조작할 수 있도록 하면 의도치 않은 에러가 발생할 수도 있다. UX보다는 안정성이 중요시되는 기간계 프로젝트 특성 상 부분 리로드는 지양해야 할 것 같다. 하지만 모바일에선 고려해볼만 할 수 있다.

## 클라이언트 측 에러

react 18 이후에서는, `에러`는 ErrorBoundary로, 로딩은 Suspense로 처리하는 경우가 많다.  
하지만 현재 기간계에서 사용하는 react 17.0.0 버전에서는 해당 기능을 suspense가 지원하지 않는다. 따라서 로딩 처리는 지금과 같이 리액트 쿼리에서 처리해야 할 것 같다.

```typescript
if (!isLoading) {
  return <LoadingComponent />;
}
```

💡 ErrorBoundary에서 발생한 에러는 개발 환경에서는 빨간 화면으로 개발자에게 에러를 알려주지만, 프로덕션 환경에서는 fallback UI를 보여주게 된다.

## Todo

슬랙에서 에러 연동하기
