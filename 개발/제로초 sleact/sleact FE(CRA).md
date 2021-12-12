### # CRA로 Typescript 프로젝트 생성

> npx create-react-app sleact --template typescript



### # eslint, prettier 세팅

>  npm i -D eslint prettier eslint-config-prettier eslint-plugin-prettier

`.eslintrc`

```json
{
  "extends": ["plugin:prettier/recommended", "eslint:recommended"],
  "env": {
    "browser": true,
    "node": true,
    "es6": true
  },
  "parserOptions": {
    "ecmaVersion": 2017
  },
  "rules": {
    "prettier/prettier": [
      "error",
      {
        "endOfLine": "auto"
      }
    ]
  }
}
```



`.prettierrc`

```json
{
  "printWidth": 120,
  "tabWidth": 2,
  "singleQuote": true,
  "trailingComma": "all",
  "semi": true
}
```



### # CRA + Typescript 절대 경로 설정

> npm i @craco/craco
>
> npm i -D craco-alias



#### Craco(Create React App Configuration Override)란?

CRA 프로젝트의 설정을 쉽게 할 수 있도록 도와주는 도구. eject 없이 webpack, babel, postcss 등의 설정을 할 수 있다.



`craco.config.js`

```js
const CracoAlias = require('craco-alias')

module.exports = {
  plugins: [
    {
      plugin: CracoAlias,
      options: {
        source: 'tsconfig',
        baseUrl: './src',
        tsConfigPath: './tsconfig.extend.json',
      },
    },
  ],
}
```



`tsconfig.extend.json`

```json
{
  "compilerOptions": {
    "baseUrl": "./src",
    "paths": {
      "@hooks/*": ["./hooks/*"],
      "@components/*": ["./components/*"],
      "@layouts/*": ["./layouts/*"],
      "@pages/*": ["./pages/*"],
      "@utils/*": ["./utils/*"],
      "@typings/*": ["./typings/*"]
    }
  }
}
```



`tsconfig.json`

```json
{
  "extends": "./tsconfig.extend.json",
  "compilerOptions": {
    //...
  }
}
```



`package.json`

```json
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject"
  },
```



아래 코드 가능

```tsx
import App from '@layouts/App';
```



### # 라우터 세팅

> npm i react-router react-router-dom

> npm i @types/react-router @types/react-router-dom



참고) 리덕스 대체로 `SWR + zustand ` 사용 가능



`index.tsx`

```tsx
<BrowserRouter>
  <App />
</BrowserRouter>
```

- BrowserRouter로 App 컴포넌트 감싸주기



`App.tsx`

```tsx
<Switch>
  <Redirect exact path="/" to="/login" />
    <Route path="/login" component={LogIn} />
    <Route path="/signup" component={SignUp} />
</Switch>
```

- 페이지 간 이동을 위해서 Switch 컴포넌트에 Redirect(루트), Route(개별 페이지) 넣어주기



---



###  # 코드 스플리팅 세팅

> npm i @loadable/component

> npm i --save-dev @types/loadable__component



`App.tsx`

```tsx
const LogIn = loadable(() => import('@pages/LogIn'));
const SignUp = loadable(() => import('@pages/SignUp'));
```

- 컴퍼넌트 불러오는 부분을 수정하면 해당 컴포넌트가 필요한 시점에 로드 된다.



---



### # emotion(styled-components 대체) 세팅

- styled-compnents 와 거의 비슷한데, 설정이 더 간단하다(SSR도 더 쉽다).

> npm i @emotion/react @emotion/styled



---



### # SWR 세팅

> npm i swr

- 일반적으로 get 요청으로 받은 데이터를 저장
- 리덕스 대신 사용 가능



`pages/login/index.tsx`

```tsx
import useSWR from 'swr';

const {data, error, revalidate, mutate} = useSWR('/api/users', fetcher);
```

- 두 번째 파라미터로 첫 번째 인자로 받은 api 주소(swr 키 값)를 어떻게 처리할 건지에 대한 함수를 받음
- **revalidate**는 해당 useSWR를 원하는 시점에 다시 호출하는 함수
  - 예를 들면, 로그인 / 로그아웃 POST 요청 직후 사용자 정보(data)를 업데이트하기 위해서 다시 get 요청을 보내기 위해서 useSWR 호출
  - `revalidate()`
- **mutate**는 revalidate처럼 useSWR을 다시 호출하지 않고(두 번째 인자로 false 전달), useSWR의 data를 변경하는 함수
  - 예를 들면, 로그인 / 로그아웃 POST 요청 직후 로그인의 경우에는 POST 요청의 응답으로 사용자 정보를 받기 때문에 다시 useSWR을 이용해서 get 요청을 보낼 것이 아니라 사용자 정보를 mutate를 이용해서 바로 업데이트 한다.
  - `mutate(response.data, false)`



`utils/fetcher.ts`

```typescript
import axios from 'axios';

const fetcher = (url: string) => axios.get(url, { withCredentials: true }).then((res) => res.data);

export default fetcher;
```

- axios 요청에서 **withCredentials를 true** 설정으로 보내는 이유
  - 백엔드와 프론트엔드의 도메인이 다른 경우 쿠키를 공유하기 위해서
  - 다시 말하면, 백엔드와 프론트엔드 도메인이 다를 때 백엔드에서 프론트엔드로 쿠키를 생성해주고, 프론트엔드에서는 백엔드로 쿠키를 보낼 수 있도록하기 위해서

