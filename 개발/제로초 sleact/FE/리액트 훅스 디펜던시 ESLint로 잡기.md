### 리액트 훅스 디펜던시 ESLint로 잡기

> npm i -D eslint-config-react-app eslint-plugin-flowtype eslint-plugin-import  eslint-plugin-jsx-a11y eslint-plugin-react



`.eslintrc`

```json
{
    "extends": ["plugin:prettier/recommended", "eslint:recommended", "react-app"],
}
```

- extends에 "react-app" 추가