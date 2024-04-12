
## profiles - 운영/개발 분리

개발을 본격적으로 진행하니 운영과 개발을 진행 중인 코드를 분리할 필요가 있다는 것을 알게 되었습니다. 

→ 개발 중인 코드에서 사용할 DB와 운영 중인 환경에서 사용하는 DB을 분리해서 작업해야 개발이 가능합니다. 

properties 파일을 두 가지 파일로 나눠 만들었습니다. 

하나는 개발용 application-dev.properties, 운영용 application-prod.properties입니다. 

dev는 개발 서버에서 설정 값들이 저장되어 있고 나머지는 운영에서 사용할 설정 값들이 저장되어 있습니다.

그리고 spring.profiles.active = 내부에 작성해줘야 합니다.

그리고 테스트를 하기 위해서는 테스트에도 똑같이 설정해줘야 합니다. 

설정을 하지 않고 작업을 들어갔더니 기존의 properties 연결하기 때문에 테스트 에러가 발생했었고 해결하기 위해 한참을 헤맸습니다. 

properties를 만들어주고 Edit configuration에서 Active profiles를 dev로 설정해주면 됩니다. 

