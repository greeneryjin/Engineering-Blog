
### JWT

Json Web Token의 약자로 일반적으로 클라이언트와 서버 사이에서 통신할 때 권한을 위해 사용하는 토큰입니다.

표준규약에 따라 생성한 암호화된 토큰으로 복잡하고 읽을 수 없는 string 형태로 저장됩니다. 

장점으로는 확정성이 있으나 토큰을 탈취하게 될 경우, 사용자를 구별하지 못하는 문제가 발생합니다. 


```
HEADER.PAYLAD.SIGNATURE
헤더    내용   서명

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

----

### Session & Cookie

Session과 Cookie를 알기 전 HTTP 프로토콜의 특징을 알고 있어야 합니다. 

#### HTTP 특징

`Connectionless`는 클라이언트의 요청에 맞는 응답을 보낸 후 연결을 끊어버리는 것을 의미합니다. 더 많은 요청을 처리할 수 있습니다. 

`Stateless`: 연결을 끊는 순간 클라이언트와 서버의 통신이 끝나며 상태 정보를 유지하지 않는 특징을 가집니다. 

두 가지 특징으로 인해 쿠키와 세션을 사용합니다.

+ Session

일정 시간 동안 같은 사용자(브라우저)로부터 들어오는 일련의 요구를 하나의 상태로 보고, 그 상태를 유지시키는 기술입니다.

##### 특징

+ 웹 서버에 웹 컨테이너의 상태를 유지하기 위한 정보를 저장합니다.

+ 웹 서버에 저장되는 쿠키(세션 쿠키 / session cookie)입니다.

+ 브라우저를 닫거나, 서버에서 세션을 삭제했을 때만 삭제가 되기 때문에 쿠키보다 비교적 보안적으로 우수합니다.

+ 저장 데이터에 서버 용량 허용 범위 내에서는 제한이 없습니다.

+ 각 클라이언트에 고유 세션 ID(Session ID)를 부여합니다.

##### 동작 방식

<img width="528" alt="1" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/a2b3da01-3796-4cc1-8a5c-1028c3cf2ad4">

<br>

----

+ Cookie

HTTP의 일종으로 사용자가 어떤 웹 사이트를 방문할 경우, 해당 사이트가 사용하고 있는 서버에서 사용자의 컴퓨터에 저장하는 작은 기록 정보 파일입니다. 

HTTP에서 클라이언트의 상태정보를 쿠키 형태로 클라이언트 PC에 저장하였다가 필요 시 정보를 참조하거나 재사용할 수 있습니다.

##### 동작 방식

<img width="448" alt="1" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/ba525e5c-c768-4c23-bd26-3f797d69fefb">

1. 클라이언트가 서버에 로그인 요청합니다.

2. 서버는 클라이언트의 로그인 요청의 유효성을 확인하고(아이디와 비밀번호 검사) 응답헤더에 set-cookie: 를 통해 쿠키를 추가하여 응답합니다.

3. 클라이언트는 이후 서버에 요청할 때 전달받은 쿠키를 자동으로 요청헤더에 추가하여 요청합니다. 헤더에 쿠키값을 자동으로 추가하여 주는데 이는 브라우저에서 처리해줍니다.



