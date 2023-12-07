## 슬랙 연동

슬랙 alert 요청을 보내기 위한 프록시 서버를 별도로 배포해놓음.

프록시를 쓰면 슬랙 url이 직접 노출되는 것이 아니기 때문에 보안상으로도 더 괜찮을 것 같다.

## 에러 바운더리

모든 에러들을 다 에러 바운더리로 처리하면 좋지 않을 것 같다.  
기간계 특성 상 매우 많은 input에 값을 입력하는 경우가 많은데, 한번 에러 났다고 그 값들이 다 날아가면 굉장히 부정적인 경험을 줄 것 같다.

따라서 페이지마다 따로 예외처리된 에러들 말고, 예상치 못하게 발생해 글로벌로 던져진 에러들만 슬랙에 로깅을 통해 트래킹하면 될 것 같다.

## Todo

- 글로벌에서 잡힌 에러들은 슬랙에 로깅하기
- 페이지 별 에러처리하기
- 커스텀 에러 클래스 만들어 재사용하기