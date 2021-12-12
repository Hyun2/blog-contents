# Typeorm DB 정보 환경변수로 저장하고 사용하는 방법

참고) https://github.com/typeorm/typeorm/blob/master/docs/using-ormconfig.md



### 프로젝트 root 디렉터리에 .env 파일 생성



### Typeorm에서 정해 놓은 변수명에 값 설정

`.env`

```json
TYPEORM_CONNECTION = mysql
TYPEORM_HOST = localhost
TYPEORM_USERNAME = root
TYPEORM_PASSWORD = admin
TYPEORM_DATABASE = test
TYPEORM_PORT = 3000
TYPEORM_SYNCHRONIZE = true
TYPEORM_LOGGING = true
TYPEORM_ENTITIES = entity/*.js,modules/**/entity/*.js
```



### 환경변수로 Typeorm DB 연결

```typescript
// read connection options from ormconfig file (or ENV variables)
const connectionOptions = await getConnectionOptions();

// do something with connectionOptions,
// for example append a custom naming strategy or a custom logger
Object.assign(connectionOptions, { namingStrategy: new MyNamingStrategy() });

// create a connection using modified connection options
const connection = await createConnection(connectionOptions);
```

