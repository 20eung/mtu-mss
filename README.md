# MTU, MSS 이야기
### MTU(Maximum Transmission Unit)
- 네트워크에서 한 번에 보낼 수 있는 데이터 크기
- 일반적인 이더넷에서 수용할 수 있는 크기는 1,500바이트
- MTU는 2계층의 데이터값

### MSS(Maximum Segment Size)
- 일반적인 IP 표준 헤더 20바이트, TCP 표준 헤더 20바이트(추가되는 옵션 헤더 제외)
- 일반적인 이더넷인 경우, MSS 값은 1,460바이트(1500-20-20)
- MSS는 4계층에서 가질 수 있는 최대 데이터 값

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FBUphv%2FbtrcUkz494L%2FMVTxBa5o5QP82MCP7YmUhK%2Fimg.png "image")