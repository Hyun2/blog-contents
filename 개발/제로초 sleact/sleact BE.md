# NestJS

### 설치

> npm i -g @nestjs/cli



### 프로젝트 생성

> nest new [app-name]



### 설정 커스터마이징

`tsconfig.json`

```json
"esModuleInterop": true,
```



### 핫 모듈 리로딩 설정

참고) [Hot reload | NestJS - A progressive Node.js framework](https://docs.nestjs.com/recipes/hot-reload)



### dotenv 등 설정

참고) [Configuration | NestJS - A progressive Node.js framework](https://docs.nestjs.com/techniques/configuration#getting-started)



### 모듈, 서비스, 컨트롤러 생성

> nest g mo users

> nest g s users

> nest g co users

- g: generate
- mo: module
- s: service
  - Service는 Provider 중 하나로, 보통은 데이터를 저장하거나, 조회하는 코드로 Controller가 호출하는 실제 요청 처리 로직이다.
  - Provider는 디펜던시로서 주입될 수 있는 것으로 services, repositories, factories, helpers 등이 있다.
- co: controller
  - Controller는 클라이트측에서 들어오는 요청을 핸들링하고 응답을 반환하는 역할을 한다.



>  nest g resource [리소스 이름]

이렇게 하면 모듈, 컨트롤러, 서비스, CRUD 용 라우터까지 한 번에 만들어 준다?



### DTO(Data Transfer Object)

Controller에서 처리하는 데이터의 구조를 정의한 객체



### API 문서(Swagger) 만들기

참고) https://docs.nestjs.com/openapi/introduction



### TypeORM 설정

#### typeorm-model-generator

- 기존 DB에서 모델 불러오기

  - > npm i -D typeorm-model-generator

  - > npx typeorm-model-generator -h [localhost] -d [sleact] -u root -x [password] -e [mysql]



#### TypeORM 연결

> npm i @nestjs/typeorm typeorm mysql2