
우리FISA에서 AWS로 인프라 환경을 구축하던 중 3 tier architecture로 구축을 하는 것이 좋다는 피드백을 
받았습니다. 왜 3 tier architecture의 이점이 궁금해서 정리하려고 합니다.

#### 3 tier architecture

![1](https://github.com/greeneryjin/Engineering-Blog/assets/87289562/308dffc6-2990-48f4-875b-c5fe18218182)
출처: aws

장점

- 각 계층이 별도의 환경에서 실행되기 때문에 각 계층을 동시에 개발할 수 있고 개발하면서 영향을 미치지 않습니다.

- 어떤 계층에서 스케일 아웃을 하게 될 경우, 한 계층만 작업하면 되기 때문에 자원을 아낄 수 있습니다.

- 한 계층씩 관리하기 때문에 운영 측면에서도 유리합니다.

단점

- 구현이 어렵습니다.

  
