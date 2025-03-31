# HTTP
### 특징
- 클라이언트-서버 구조
- 무상태성 (Stateless)
    - 요청 간 상태 저장 X
    - 로그인 상태 유지를 위해 쿠키, 세션, 토큰 사용
- 캐시 가능 (Cacheable)
    - 브라우저나 중간 서버가 데이터를 저장하고 재사용 가능
- 계층적 구조
    - 프록시, 게이트웨이, 캐시 서버 등을 거침
- 표현 기반 
    - 리소스 자체가 아닌 리소스 표현을 주고 받음
    - HTML, JSON, XML 등 다양한 포맷으로 리소스 표현 가능
### 멱등성
- 같은 요청을 여러번 보내도 결과가 같음
### 안전성
- 서버의 데이터를 변경하지 않는 요청은 안전한 요청
### Header (헤더)
- Date
- Connection
- Cache-Control
- Pragma
- Trailer
#### 헤더 내 엔티티/개체헤더 항목
- Content-Type
- Content-Language
- Content-Encoding
- Content-Length
- Content-Location
- Content-Disposition
- Content-Security-Policy
- Location
- Last-Modified
#### 헤더 내 요청 헤더 항목
- Host
- User-Agent
- From
- Cookie
- Referer
- If-Modified-Since
- Authorization
- Origin
#### 헤더 내 응답 헤더 항목
- Server
- Accept-Range
- Set-Cookie
- Expires
- Age
- ETag
- Proxy-Authenticate
- Allow
- Access-Control-Allow-Origin