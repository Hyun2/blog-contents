### 1. Typeorm DB 정보 환경변수로 저장하고 사용하는 방법

참고) https://github.com/typeorm/typeorm/blob/master/docs/using-ormconfig.md



#### 1-1. 프로젝트 root 디렉터리에 .env 파일 생성



#### 1-2. Typeorm에서 정해 놓은 변수명에 값 설정

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
TYPEORM_ENTITIES = src/entities/*.ts, modules/**/entities/*.ts
```



### 1-3. 환경변수로 Typeorm DB 연결

```typescript
// read connection options from ormconfig file (or ENV variables)
const connectionOptions = await getConnectionOptions();

// do something with connectionOptions,
// for example append a custom naming strategy or a custom logger
Object.assign(connectionOptions, { namingStrategy: new MyNamingStrategy() });

// create a connection using modified connection options
const connection = await createConnection(connectionOptions);
```



### 2. 테이블(Entity) 생성

#### 2-1. src/utils/[상속 용도 클래스 이름].ts 파일 생성

```typescript
import {
  BaseEntity,
  Column,
  CreateDateColumn,
  DeleteDateColumn,
  Entity,
  PrimaryColumn,
  UpdateDateColumn,
} from "typeorm";

@Entity()
export class Person extends BaseEntity {
  @PrimaryColumn({
    type: "bigint",
  })
  id: number;

  @Column()
  first_name: string;

  @Column()
  last_name: string;

  @Column({
    unique: true,
  })
  email: string;

  @Column({
    unique: true,
    length: 10,
  })
  card_number: string;
}
```

- Person 클래스는 실제 DB의 테이블로 생성 할 엔티티가 아니기 때문에 **`@Entity()` 데코레이터 안에 테이블 이름을 넣지 않는다.**



#### 2-2. src/entities/[테이블 | 엔티티 이름].ts 파일 생성

```typescript
// Client.ts

import { Column, Entity } from "typeorm";
import { Person } from "./utils/Person";

@Entity("client")
export class Client extends Person {
  @Column({
    type: "numeric",
  })
  balance: number;

  @Column({
    default: true,
    name: "active", // 데이터베이스에서는 active 라는 컬럼명을 사용
  })
  is_active: boolean; // typescript에서는 is_active 라는 필드 값을 사용

  // 데이터베이스의 is_active와 typescript에서의 active는 같은 컬럼을 가리키는데 표현 방식만 다름

  @Column({
    type: "simple-json",
    nullable: true,
  })
  additional_info: {
    age: number;
    hair_color: string;
  };

  @Column({
    type: "simple-array",
    default: [],
  })
  faimly_members: string[];
    
  @CreateDateColumn({
      type: "timestamptz"
  })
  created_at: Date;

  @UpdateDateColumn({
      type: "timestamptz"
  })
  updated_at: Date;

  @DeleteDateColumn({
      type: "timestamptz"
  })
  deleted_at: Date;
}
```

- Client 엔티티는 Person 클래스를 상속받는다.
- `Column()` 데코레이터 안에 DB 설정 값을 object로 전달한다. 예) type, name, nullable 등



```typescript
//Banker.ts

import { Column, Entity } from "typeorm";
import { Person } from "./utils/Person";

@Entity("banker")
export class Banker extends Person {
  @Column({
    unique: true,
    length: 10,
  })
  employee_number: string;

  @CreateDateColumn({
      type: "timestamptz"
  })
  created_at: Date;

  @UpdateDateColumn({
      type: "timestamptz"
  })
  updated_at: Date;

  @DeleteDateColumn({
      type: "timestamptz"
  })
  deleted_at: Date;
}
```

- Banker 엔티티는 Person 클래스를 상속받는다.

- created_at, updated_at, deleted_at도 Person 클래스에 넣을 수 있지만, 해당 컬럼들을 DB 상에서 가장 마지막 컬럼들로 위치 시키고 싶기 때문에 그렇게 하지 않았다.



```typescript
// Transaction.ts

import {
  BaseEntity,
  Column,
  CreateDateColumn,
  DeleteDateColumn,
  Entity,
  PrimaryGeneratedColumn,
  UpdateDateColumn,
} from "typeorm";

export enum TransactionTypes {
  DEPOSIT = "deposit",
  WITHDRAW = "withdraw",
}

@Entity("transaction")
export class Transaction extends BaseEntity {
  @PrimaryGeneratedColumn({
    type: "bigint",
  })
  id: number;

  @Column({
    type: "enum",
    enum: TransactionTypes,
  })
  type: string;

  @Column({
    type: "numeric",
  })
  amount: number;

  @CreateDateColumn({
      type: "timestamptz"
  })
  created_at: Date;

  @UpdateDateColumn({
      type: "timestamptz"
  })
  updated_at: Date;

  @DeleteDateColumn({
      type: "timestamptz"
  })
  deleted_at: Date;
}
```

- transaction의 type 컬럼에 들어 갈 값을 deposit과 withdraw로 한정하기 위해 **enum 타입**으로 선언할 수 있다.



### 3. 관계 설정

- Client - Banker: Many to Many Relationship
- Client - Transaction: Many to One Relationship



#### 3-1. Many To One 관계

```typescript
// Transaction.ts

...
  @ManyToOne(() => Client, (client) => client.transactions, {
    onDelete: "CASCADE",
  })
  @JoinColumn({
    name: "client_id",
    referencedColumnName: "id",
  })
  client: Client;
...
```

- `onDelete: "CASCADE"`



```typescript
// Client.ts

...
	@OneToMany(() => Transaction, (transaction) => transaction.client)
	transactions: Transaction[];
...
```

- 위 코드의 결과로 Transaction 테이블에 `client_id` 컬럼 생성

![](https://imgur.com/uXugc5a.jpg)



#### 3-2. Many To Many 관계

```typescript
// Banker.ts

...
	@ManyToMany(() => Client, {
		cascade: true,
	})
	@JoinTable({
    	name: "bankers_clients",
    	joinColumn: {
            name: "banker",
            referencedColumnName: "id"
        }
    	inverseJoinColumn: {
    		name: "client",
    		referencedColumnName: "id"
		}
	})
	clients: Client[];
...
```

- `cascade: true`



```typescript
// Client.ts

...
	@ManyToMany(() => Banker)
	bankers: Banker[];
...
```



### 4. 데이터 생성(Create ) with express

```typescript
// src/routes/create_client.ts

import express from "express";
import { Client } from "../entities/Client";
const router = express.Router();

router.post("/api/client", async (req, res) => {
  const { firstName, lastName, email, cardNumber, balance } = req.body;

  const client = Client.create({
    first_name: firstName,
    last_name: lastName,
    email: email,
    card_number: cardNumber,
    balance,
  });

  await client.save();
  return res.json(client);
});

export { router as createClientRouter };
```

- client를 DB 저장하는 작업을 할 때 async/await 사용



```typescript
// src/index.ts

...
const app = express();
...

const main = async () => {
  try {
	...
    app.use(express.json());
    app.use(createClientRouter);
    app.use(createBankerRouter);

    app.listen(8080, () => {
      console.log("Now running on Port 8080");
    });   
  } catch (error) {
    console.log(error);
    throw new Error("Unable to connect to Postgres");
  }
};
```

- 포스트맨으로 데이터 생성



### 4. 관계 형성

#### 4.1. Many To One

```typescript
// src/routes/create_transaction.ts

import express from "express";
import { Client } from "../entities/Client";
import { Transaction, TransactionTypes } from "../entities/Transaction";

const router = express.Router();

router.post("/api/client/:clientId/transaction", async (req, res) => {
  const { clientId } = req.params;

  const { type, amount } = req.body;

  const client = await Client.findOne(parseInt(clientId));

  if (!client) {
    return res.json({
      msg: "Client not found",
    });
  }

  const transaction = Transaction.create({
    amount,
    type,
    client,
  });

  await transaction.save();

  if (type === TransactionTypes.DEPOSIT) {
    client.balance = client.balance + amount;
  } else if (type === TransactionTypes.WITHDRAW) {
    client.balance = client.balance - amount;
  }
  await client.save();

  return res.json({
    msg: "Transaction added",
  });
});

export { router as createTransactionRouter };

```

### TODO

- client.balance를 numeric 타입으로 선언하였는데, 자릿수 문제로 javascript에서는 string 타입으로 불러오는 문제 발생
  - 30 + 30 = 3030이 되는 문제 발생
  - 검색 키워드) typeorm postgresql numeric string



#### 4.2. Many To Many

```typescript
// src/routes/connect_banker_to_client.ts

import express from "express";
import { Client } from "../entities/Client";
import { Banker } from "../entities/Banker";

const router = express.Router();

router.put("/api/banker/:bankerId/client/:clientId", async (req, res) => {
  const { bankerId, clientId } = req.params;

  const client = await Client.findOne(parseInt(clientId));
  const banker = await Banker.findOne(parseInt(bankerId));

  if (!banker || !client) {
    return res.json({
      msg: "Banker or Client not found",
    });
  }

  banker.clients = [client];

  await banker.save();

  return res.json({
    msg: "Banker connected to client",
  });
});

export { router as connectBankerToClient };
```

#### TODO

- `  banker.clients = [...banker.clients, client];`가 동작하지 않아서 `  banker.clients = [client];`로 억지로 변경.
- banker 한 명이 여러 명의 client를 가질 수 있도록 설계하였는데, 지금은 banker 한 명이 client 한 명만 가질 수 있게 되어있음.



### 5. 데이터 삭제(Delete)

```typescript
// src/routes/delete_client.ts

import express from "express";
import { Client } from "../entities/Client";

const router = express.Router();

router.delete("/api/client/:clientId", async (req, res) => {
  const { clientId } = req.params;

  const response = await Client.delete(parseInt(clientId));

  return res.json(response);
});

export { router as deleteClientRouter };
```



### 6. 데이터 조회(Read)

- **다양한 조건**으로 데이터 조회
  - createQueryBuilder 사용

```typescript
// src/routes/fetch_client.ts

import express from "express";
import { Client } from "../entities/Client";
import { createQueryBuilder } from "typeorm";

const router = express.Router();

router.get("/api/clients", async (req, res) => {
  // const client = await Client.find();
  const clients = await createQueryBuilder("client")
    .select("client.first_name")
    .addSelect("client.last_name")
    .from(Client, "client")
    .leftJoinAndSelect("client.transactions", "transactions")
    // .where("client.id = :clientId", { clientId: 7 })
    .where("client.id >= :minID AND client.id <= :maxID", {
      minID: 4,
      maxID: 444,
    })
    .getMany();

  return res.json(clients);
});

export { router as fetchClientRouter };
```

