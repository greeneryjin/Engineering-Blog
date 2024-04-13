
### CORS

: 브라우저에게 교차출처를 공유할 수 있는 권한을 부여하도록 브라우저에게 알려주는 것입니다. 

출처: 프로토콜, 호스트, 포트를 포함하고 하나라도 틀릴 경우, 브라우저에게 접근을 제한합니다. 

#### CORS의 요청 종류 2가지

1. **Simple Request** 
    
     = Simple Request는 예비 요청(Preflight)을 과정 없이 바로 서버에 본 요청을 보낸 후, 서버가 응답 헤더에 Access-Control-Allow-Origin과 같은 값을 전송하면 브라우저가 서로 비교 후 CORS 정책 위반 여부를 검사하는 방식입니다. 
    
    간단한 방식이지만 제약 사항이 존재합니다. 
    
    + 제약 사항
    
    1. GET, POST, HEAD 중 한 가지 Method를 사용해야 합니다.
    2. 헤더는 Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width Width만 가능하고 커스텀 헤더는 허용되지 않습니다. 
    3. Content-type 은 application/x-www-form-urlencoded, multipart/form-data, text/plain 만 가능합니다.

1. **Preflight Request**
    
    = 브라우저는 요청을 한번에 보내지 않고, 예비요청과 본요청으로 나누어 서버에 전달하는데 브라우저가 예비요청을 보내는 것을 Preflight 라고 하며 이 예비요청의 메소드에는 OPTIONS 가 사용됩니다. 
    
    예비요청의 역할은 본 요청을 보내기 전에 브라우저 스스로 안전한 요청인지 확인하는 것으로 요청 사양이 Simple Request 에 해당하지 않을 경우 브라우저가 Preflight Request 을 실행합니다.


   예비 요청(브라우저 → 서버)
   ![1](https://github.com/greeneryjin/Engineering-Blog/assets/87289562/48cd293c-a43f-4e33-b429-fe56f4da27cd)

   브라우저가 보낸 요청을 보면 Origin에 대한 정보 뿐만 아니라 예비 요청 이후에 전송할 본 요청에 대한 다른 정보들도 함께 포함합니다. 이 예비 요청에서 브라우저는 Access-Control-Request-Headers 를 사용하여 자신이 본 요청에서 Content-Type 헤더를 사용할 것을 알려주거나, Access-Control-Request-Method를 사용하여 GET 메소드를 사용할 것을 서버에게 미리 알려줍니다


  예비 요청 응답(서버 → 브라우저)
  ![1](https://github.com/greeneryjin/Engineering-Blog/assets/87289562/95e9705f-3fff-4ff7-9e6a-6f635d91b8dd)
  서버가 보내준 응답 헤더에 포함된 Access-Control-Allow-Origin: https://security.io 의 의미는 해당 URL 외의 다른 출처로 요청할 경우에는 CORS 정책을 위반했다고 판단하고 오류 메시지를 내고 응답하라는 의미입니다. 


 ##### 요청 과정
 ![1](https://github.com/greeneryjin/Engineering-Blog/assets/87289562/e8830100-2d89-492a-b24b-04f18f1a7e71)

서버에서 설정하는 것이 아닌 브라우저에서 구현된 스펙으로 브라우저에서 클라이언트 요청 헤더와 서버의 응답 헤더를 비교해서 처리합니다.

 브라우저는 요청을 한 번에 보내지 않고 예비 요청과 본 요청을 나눠 서버에 전달합니다. 예비 요청을 보내는 것을 Preflight Request라고 하며 여기서 헤더 정보를 비교하고 일치하지 않을 경우에는 CORS가 발생하고 일치할 경우 200 OK를 응답합니다. 

**CORS 설정** 

- **Access-Control-Allow-Origin**

: 헤더에 작성된 출처만 브라우저가 리소스를 접근할 수 있도록 허용합니다. 

- **Access-Control-Allow-Methods**

: preflight request 에 대한 응답으로 실제 요청 중에 사용할 수 있는 메서드를 나타냅니다. 

- **Access-Control-Allow-Headers**

: preflight request 에 대한 응답으로 실제 요청 중에 사용할 수 있는 헤더 필드 이름을 나타냅니다.

- **Access-Control-Allow-Credentials**

: 실제 요청에 쿠기나 인증 등의 사용자 자격 증명이 포함될 수 있음을 나타냅니다. Client의 credentials:include 일 경우 true 필수입니다. 

- **Access-Control-Max-Age**

: preflight 요청 결과를 캐시 할 수 있는 시간을 나타내는 것으로 해당 시간동안은 preflight요청을 다시 하지 않게 됩니다.


   
