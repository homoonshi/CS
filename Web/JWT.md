# JWT (Json Web Token)
- JSON 객체를 사용해 정보 전달
- 필요한 모든 정보를 한 객체에 담아 전달하기에 JWT 하나로 인증 가능
- Header(헤더), Payload(내용), Signature(서명)으로 구성
- 쿠키나 세션을 이용한 인증보다 안전하고 효율적
- STATELESS를 추구함
### 1) Header
```Json
{
    "alg" : "HS256",
    "typ" : "JWT:
}
```
- Algorithm : Signature를 해싱하기 위한 알고리즘 정보
- Type : 토큰 타입 (없어도 됨)
### 2) Payload (Data)
```Json
{
    "sub" : "user10001",
    "iat" : 1569302116,
    "role" : "admin",
    "user_id" : "user10001"
}
```
- 클라이언트 <-> 서버가 주고 받기 위해 필요한 정보들을 포함함
- 정보 = 클레임 이라고 부르며 키와 값의 쌍을 이룸
- iss : issuer (토큰 발급자)
- sub : subject (토큰 제목)
- aud : audience (토큰 대상)
- exp : expiration time (토큰 만료 시간)
- iat : issued at (토큰 발급 시간)
- jti : JWT id (JWT 고유 식별자)
- nbf : Not Before
- 선 정의된 클레임과 충돌나지 않으면 사용자는 자신의 사용자 정의 클레임을 가질 수 있음
- 정보들은 암호화 되지 않기 때문에 public으로 표시됨
- 정보는 변경되거나 수정될 수 없음, 변경 되는 순간 토큰 유효 X
### 3) Signature (Verify)
```Json
{
    HMAC-SHA256(
     base64urlEncoding(header) + '.' +
     base64urlEncoding(payload),
     secret_salt
}
```
- 암호화 된 헤더와 페이로드 해시 값 포함
- Secret을 사용해 해시됨
- Secret = 유저가 지정한 비밀 코드
- Header와 Payload를 변경하면 Signature 와 불일치가 발생하기 때문에 토큰이 유효하지 않음을 판단할 수 있음
### 장점
- Header, Payload를 갖고 Signature를 생성하므로 데이터 위변조 방지 가능
- 토큰 자체가 인증된 정보이기에 세션 저장소와 같은 별도의 저장소가 필수가 아님
- Signature를 공통 키 개인 키 암호화를 통해 막아뒀기 때문에 데이터 보안성 향상
- 다른 서비스에 이용할 수 있는 공통 스펙으로 사용 가능 (OAuth)
- 모바일 어플리케이션 환경에서도 동작
### 단점
- JWT 토큰 길이가 길어 인증 요청이 많아질수록 네트워크 부하 증가
- Payload 자체는 암호화 되어있지 않기에 유저 중요 정보 담을 수 없음
- 토큰 탈취시 대처 힘듬 -> 한번 발급되면 유효기간 만료까지 사용 가능
### 사용하는 이유?
- STATELESS는 결국 Refresh Token을 저장하는 방법에 따라 유지될 수 없음
- 토큰 탈취의 문제로 인해 Refresh Token이 도입됨
- 모바일 앱의 등장 -> 모바일 앱에선 세션 방식을 사용하기 힘듬 -> JWT 인증/인가
- 장시간 로그인을 할 때 세션을 만들면 서버 부하가 엄청남 -> JWT