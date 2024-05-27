## DBCP (DB Connection Pool)

application에서 DB까지 연결을 재사용하는 것

----

#### Backend application에서 DB까지 여러 request 요청처리 순서

1. OPEN Connection 연결(TCP 기반이기 때문에 3-way handshake으로 요청 연결)

2. 쿼리 요청(application -> DB)

3. 쿼리 응답(application <- DB)

4. CLOSE Connection 연결 해제 (TCP 기반이기 때문에 4-way handshake으로 요청 해제)

위 4가지 순서가 발생하기 때문에 매번 연결을 하게 될 경우 성능상 좋지 않습니다. 

이러한 방법을 해결하기 위해 DBCP를 사용합니다. 

#### DBCP 도입

1. connection pool에 스레드들은 TCP 기반으로 3-way handshake 방식을 사용해 미리 연결이 모두 된 상태입니다.

2. connection pool에서 스레드를 가지고 쿼리 요청(application -> DB)

3. 쿼리 응답(application <- DB)

4. connection pool에서 스레드 반납


#### DBCP 설정

1) MySQL

   + max_connections: DB에서 client와 맺을 수 있는 최대 connection 수(신규로  WAS를 늘려도 max_connections를 넘기면 연결이 되지 않음)
  
   + wait_timeout: connection이 inactive 할 때, 다시 요청이 오기까지 얼마의 시간을 기다린 뒤에 close할 것인지 결정(connection이 반환되지 않을 경우, 네트워크가 갑자기 끊기는 경우에 반환을 위해 설정)
  

2) applicaton

   + minimumldle: pool에서 유지하는 최소한의 idle connection 수를 의미, idle connection 수가 minimumldle 작고, 전체 connection 수도 maximumPoolSize 보다 작으면 connection 추가로 생성
  
     +  minimumldle의 수가 2면 DB와 연결된 스레드 개수도 2, 이때 요청이 오면 스레드가 사용되고 idle connection는 1개가 되고 idle connection는 1로 됐기 때문에 하나를 추가로 자동으로 생성하게 됩니다. 하지만 maximumPoolSize의 개수를 넘지 않습니다.
    
     +  요청이 더 이상 오지 않으면 스레드를 자동으로 minimumldle의 수로 맞춰 줍니다.
    
     +  applicaton에서는 minimumldle == maximumPoolSize 같도록 설정을 권장합니다. 왜냐하면 추가로 자동으로 생성할 때마다 성능이 떨어지기 때문입니다. 
  
   + maximumPoolSize: pool이 가질 수 있는 최대 connection 수, idle connection을과 active connection을 합쳐서 최대 수(우선순위가 가장 높음)
  
   + maxLifetime: pool에서 connection의 최대 수명, maxLifetime을 넘기면 idle일 경우, pool에서 바로 제거되고 active인 경우 pool로 반환 후 제거
  
     + DB의 connection time limit보다 몇 초 짧게 설정하는 것을 권장합니다. 요청이 들어와서 쿼리를 전달하는 동안 DB와 연결 시간이 다 되서 연결이 끊어지는 경우가 있기 때문입니다.
    
   + connectionTimeout: pool에서 connection을 받기 위한 대기 시간
  

#### 적절한 connection 수를 찾는 방법

+ 모니터링 환경 구축

+ 부하테스트 진행(nGrinder, jmeter)

+ request per second와 avg response time 확인

+ request per second가 병목현상이 발생하면 active thread를 확인하고 MAX일 경우 스레드 풀의 스레드를 늘려야 합니다. 

+ request per second가 병목현상이 발생했으나 active thread의 수가 max이 아닐 경우에는 DBCP의 active thread를 확인하고 maximumPoolSize 수를 늘려야 합니다.

+ maximumPoolSize의 개수가 DB에서 설정한 max_connections 수와 같아질 경우에는 max_connections의 수를 늘려야 합니다. 

+ 요청을 확인하면서 maximumPoolSize, max_connections 적절하게 늘려주고 DB는 샤딩, 캐시 서버 등을 도입하는 방법으로 부하를 낮춰야 합니다.
