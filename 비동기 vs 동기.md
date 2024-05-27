## Blocking, 블로킹 

+ 요청한 작업이 모두 완료될 때까지 기다렸다가 완료될 때 응답과 결과를 반환받습니다.

+ 요청한 작업 결과를 기다립니다.

<img width="800" alt="1" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/e442dd76-3df6-434c-b042-62ccc1ec5647">

1. Application 내부 thread가 kernel에 I/O요청

2. Read blocking으로 system call 호출

3. kernel에서 I/O 읽기

4. kernel에서 작업 하는 동안 thread는 blocking

5. kernel에서 읽기 작업이 끝나면 kernel space에서 user space로 데이터 전송

6. thread는 blocking이 끝나고 요청을 완료함 

<br>

## Non-Blocking, 논-블로킹 

+ 작업 요청 이후 결과는 나중에 필요할 때 전달받습니다.

+ 요청한 작업 결과를 기다리지 않고 즉시 요청합니다. 

+ 중간중간 필요하면 상태 확인은 해볼 수 있습니다. (polling)

<img width="800" alt="2" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/7ce60f0d-f13b-406e-97d0-259b5c6875d5">

1. Application 내부 thread가 kernel에 I/O요청

2. Read non-blocking으로 system call 호출

3. kernel에서 즉시 -1(리눅스 요청 값)와 EAGAIN or EWOULDBLOCK 에러 코드 함께 요청을 전달 

4. kernel에서 작업 하는 동안 thread는 작업을 진행함 

5. kernel에서 읽기 작업 종료

6. thread가 Read non-blocking으로 system call 호출

7. kernel의 kernel space에서 user space로 데이터 전송

8. thread는 작업을 함


### 특징 

논-블로킹할 때는 빨간 동그라미처럼 시간의 딜레이가 발생하기 때문에 블로킹방식보다 느릴 수 있습니다. 

위 그림에서도 작업이 끝났는지 계속 확인해야하기 때문에 CPU를 낭비할 수 있습니다.  

많은 방법이 있고 그 중에서 I/O Multiplexing을 사용해서 모니터링하고 요청을 전달합니다. 

<br>

## 비동기

: 여러 작업들을 동시에 실행하는 프로그래밍 방법론 

## 동기

: 요청이 들어오면 순차적으로 작업을 수행하고, 해당 작업이 수행중이면 다음 작업은 대기하게 됩니다. 

멀티스래딩

: 비동기 프로그래밍의 한 종류

싱글 스레드도 non-block으로 비동기로 동작하게 되면 여러 작업을 동시에 처리할 수 있습니다.

