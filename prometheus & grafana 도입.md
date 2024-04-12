저희가 개발한 웹사이트 출시하기 전, 서버나 스프링에서 에러가 발생했는지 실시간으로 확인하기 위해 모니터링이 필요하다고 판단되었습니다. 
그래서 이번 기회에 prometheus, Grafana를 공부하고 적용하기로 결정했습니다. 

### prometheus

: 메트릭을 저장하는 db

먼저 개발 사이트에 바로 적용하기에는 무리가 있기 때문에 AWS EC2 리눅스에 스프링, 도커를 설치하여 테스트를 진행해보았습니다.

Prometheus Server에 대한 yml파일을 작성

```
global:
  scrape_interval: 5s
scrape_configs:
	- job_name: 'spring-boot-test'
  metrics_path: '/actuator/prometheus'
  static_configs: ['사용할 ip주소 넣으세요']
```

위 파일을 작성하면 현재 서버에서 실행되는 프로그램이랑 매핑되기 때문에 연동이 가능해집니다. 

`global:` prometheus의 전역 구성을 제어합니다. 

`scrape_interval:` prometheus가 대상을 모니터링하는 빈도를 제어합니다. 5초마다 대상을 스크랩합니다.  

`scrape_configs`가 실제 수집 대상을 지정하는 설정입니다. 여기서는 하나의 ip만 넣었으나 여러 대상을 설정할 수 있습니다. 

`targets`가 배열로 지정하므로 서버를 모두 지정할 수 있습니다.

<br>

도커 컨테이너 실행하기 

```
sudo docker run -p 9090:9090 -v /home/ubuntu/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml --name prometheus -d prom/prometheus --config.file=/etc/prometheus/prometheus.yml
```

![1](https://github.com/greeneryjin/Engineering-Blog/assets/87289562/f75d2a72-c01d-4d08-b5da-0f3a1ccc8a29)


<Br>


localhost:9090//actuator/prometheus

![1](https://github.com/greeneryjin/Engineering-Blog/assets/87289562/ec395798-3cd1-421b-a679-2ee67e47c24e)


그라파나 연동

**Grafana 설치 및 컨테이너 실행** 

```
docker run -d --name=grafana -p 3000:3000 grafana/grafana
```

![1](https://github.com/greeneryjin/Engineering-Blog/assets/87289562/a2bb06f3-4df2-49dd-98f3-5c8e326731e8)

이런 화면이 나타납니다. 그리고 Configuration에서 prometheus를 설치하고 HTTP에 IP주소를 넣으면 됩니다. 


