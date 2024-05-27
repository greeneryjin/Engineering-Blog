## Blocking, 블로킹 

+ 요청한 작업이 모두 완료될 때까지 기다렸다가 완료될 때 응답과 결과를 반환받습니다.

+ 요청한 작업 결과를 기다립니다.

<img width="800" alt="1" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/e442dd76-3df6-434c-b042-62ccc1ec5647">

1. 

2. 

3. 

4.

<br>

## Non-Blocking, 논-블로킹 

+ 작업 요청 이후 결과는 나중에 필요할 때 전달받습니다.

+ 요청한 작업 결과를 기다리지 않습니다.

+ 중간중간 필요하면 상태 확인은 해볼 수 있습니다. (polling)

<img width="800" alt="2" src="https://github.com/greeneryjin/Engineering-Blog/assets/87289562/811a456e-c064-4b93-ac57-72108ed7edce">

1.

2. 

3. 

4.

<br>

## 비동기

: 여러 작업들을 동시에 실행하는 프로그래밍 방법론 

## 동기

: 요청이 들어오면 순차적으로 작업을 수행하고, 해당 작업이 수행중이면 다음 작업은 대기하게 됩니다. 

멀티스래딩

: 비동기 프로그래밍의 한 종류

싱글 스레드도 non-block으로 비동기로 동작하게 되면 여러 작업을 동시에 처리할 수 있습니다.


msa에서 
