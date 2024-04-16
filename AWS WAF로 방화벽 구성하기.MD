AWS WAF (Web Application Firewall) 은 AWS 환경에서 발생하는 Layer 7 에 해당하는 보안 위협 (DDoS 공격 또는 웹 애플리케이션 공격) 에 대응하기 위한 보안 서비스입니다.

구성도
<img width="431" alt="1" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/b6eb8e31-ab57-4d61-aecd-a0024d20e5f3">

두 개의 가용영역에 웹 서비스를 구축하고, ALB (Application LoadBalancer) 로 연결합니다. 그리고 애플리케이션 로드밸런서에 AWS WAF (Web Application Firewall) 서비스를 배포하여 웹 서비스에 대한 공격을 탐지합니다.
