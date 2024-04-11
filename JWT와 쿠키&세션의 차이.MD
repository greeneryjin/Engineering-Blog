
### JWT

Json Web Token의 약자로 일반적으로 클라이언트와 서버 사이에서 통신할 때 권한을 위해 사용하는 토큰입니다.

표준규약에 따라 생성한 암호화된 토큰으로 복잡하고 읽을 수 없는 string 형태로 저장됩니다. 

장점으로는 확정성이 있으나 토큰을 탈취하게 될 경우, 사용자를 구별하지 못하는 문제가 발생합니다. 


```
HEADER.PAYLAD.SIGNATURE
헤더    내용   서명

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

