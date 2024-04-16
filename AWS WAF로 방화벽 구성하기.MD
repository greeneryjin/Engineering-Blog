AWS WAF (Web Application Firewall) 은 AWS 환경에서 발생하는 Layer 7 에 해당하는 보안 위협 (DDoS 공격 또는 웹 애플리케이션 공격) 에 대응하기 위한 보안 서비스입니다.

구성도
<br>

<img width="431" alt="1" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/b6eb8e31-ab57-4d61-aecd-a0024d20e5f3">

두 개의 가용영역에 웹 서비스를 구축하고, ALB (Application LoadBalancer) 로 연결합니다. 그리고 애플리케이션 로드밸런서에 AWS WAF (Web Application Firewall) 서비스를 배포하여 웹 서비스에 대한 공격을 탐지합니다.

### SSRF 공격 방지

SSRF는 Server-Side Request Forgery의 약자로 Server Side에서 이루어지는 요청을 변조해 해커가 의도한 서버로 요청을 가게 되거나 요청 자체를 변경할 수 있는 공격을 말합니다.

URL 요청 
https://www.woorionebanks.site/?tes=http://localhost:8080/

<img width="641" alt="1" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/f7e0a994-d21a-4d48-bdb9-c0e54f4fab03">

WAF가 요청을 막는 것을 확인할 수 있습니다. 

### 해외IP 차단하기

특정 국가에서 지속적으로 접속을 하려는 시도가 있었기 때문에 ip를 차단하려고 합니다.

1. rule 추가

2. 빌더 선택

3. 이름 작성

4. Inspect

5. Originates from a country in 선택

6. 차단할 국가 선택

<br>

ip가 차단되는 것을 확인할 수 있습니다.

<img width="632" alt="1" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/4ffcfd7d-3180-4db8-8c40-c5e98f6c39d3">


