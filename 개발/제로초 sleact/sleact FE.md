# (CRA 없이) React with Typescript 세팅

> npm i react react-dom typescript @types/react @types/react-dom

> npm i -D eslint prettier eslint-plugin-prettier eslint-config-prettier



.prettierrc 파일 생성

```json
{
    "printWidth": 120,
    "tabWidth": 2,
    "singleQuote": true,
    "trailingComma": "all",
    "semi": true
}
```



.eslintrc 파일 생성

```json
{
	"extends": ["plugin:prettier/recommended"]
}
```



tsconfig.json 파일 생성

```json
{
  "compilerOptions": {
    "esModuleInterop": true, // import * as React from 'react'; => import React from 'react';
    "sourceMap": true,
    "lib": ["ES2020", "DOM"],
    "jsx": "react",
    "module": "esnext",
    "moduleResolution": "Node",
    "target": "es5",
    "strict": true,
    "resolveJsonModule": true,
    "baseUrl": ".",
    // TODO: 복/붙 후 수정
    "paths": {
      "@hooks/*": ["hooks/*"],
      "@components/*": ["components/*"],
      "@layouts/*": ["layouts/*"],
      "@pages/*": ["pages/*"],
      "@utils/*": ["utils/*"],
      "@typings/*": ["typings/*"]
    }
  }
}

```

- path: 상대경로로 복잡하게 import 하는 게 아니라 "절대경로" 처럼 import 하기 위해서



typescript 자체적으로 javascript로 변환하는 방법과 중간에 babel을 넣어서 babel이 javascript로 변환하는 방법이 있는데 보통 중간에 babel을 넣어서 많이 사용한다. babel은 css, img 같은 파일들도 js로 변환해 주기 때문에.



### babel, webpack 설정

webpack.config.ts 파일 생성

```typescript
import path from 'path';
import ReactRefreshWebpackPlugin from '@pmmmwh/react-refresh-webpack-plugin';
import webpack from 'webpack';
import ForkTsCheckerWebpackPlugin from 'fork-ts-checker-webpack-plugin';
import { BundleAnalyzerPlugin } from 'webpack-bundle-analyzer';

const isDevelopment = process.env.NODE_ENV !== 'production';

const config: webpack.Configuration = {
  // TODO: 복/붙 후 수정
  name: 'sleact',
  mode: isDevelopment ? 'development' : 'production',
  devtool: !isDevelopment ? 'hidden-source-map' : 'eval',
  resolve: {
    extensions: ['.js', '.jsx', '.ts', '.tsx', '.json'],
    // TODO: 복/붙 후 수정
    alias: {
      '@hooks': path.resolve(__dirname, 'hooks'),
      '@components': path.resolve(__dirname, 'components'),
      '@layouts': path.resolve(__dirname, 'layouts'),
      '@pages': path.resolve(__dirname, 'pages'),
      '@utils': path.resolve(__dirname, 'utils'),
      '@typings': path.resolve(__dirname, 'typings'),
    },
  },
  entry: {
    // TODO: 복/붙 후 수정
    app: './client',
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        loader: 'babel-loader',
        options: {
          presets: [
            [
              '@babel/preset-env',
              {
                targets: { browsers: ['last 2 chrome versions'] },
                debug: isDevelopment,
              },
            ],
            '@babel/preset-react',
            '@babel/preset-typescript',
          ],
          env: {
            development: {
              plugins: [['@emotion', { sourceMap: true }], require.resolve('react-refresh/babel')],
            },
            production: {
              plugins: ['@emotion'],
            },
          },
        },
        exclude: path.join(__dirname, 'node_modules'),
      },
      {
        test: /\.css?$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
  plugins: [
    new ForkTsCheckerWebpackPlugin({
      async: false,
      // eslint: {
      //   files: "./src/**/*",
      // },
    }),
    new webpack.EnvironmentPlugin({ NODE_ENV: isDevelopment ? 'development' : 'production' }),
  ],
  output: {
    path: path.join(__dirname, 'dist'),
    filename: '[name].js',
    publicPath: '/dist/',
  },
  devServer: {
    historyApiFallback: true, // react router
    port: 3090,
    publicPath: '/dist/',
    proxy: {
      '/api/': {
        target: 'http://localhost:3095',
        changeOrigin: true,
      },
    },
  },
};

if (isDevelopment && config.plugins) {
  config.plugins.push(new webpack.HotModuleReplacementPlugin());
  config.plugins.push(new ReactRefreshWebpackPlugin());
  config.plugins.push(new BundleAnalyzerPlugin({ analyzerMode: 'server', openAnalyzer: true }));
}
if (!isDevelopment && config.plugins) {
  config.plugins.push(new webpack.LoaderOptionsPlugin({ minimize: true }));
  config.plugins.push(new BundleAnalyzerPlugin({ analyzerMode: 'static' }));
}

export default config;
```



설정 파일 만든 후 필요 패키지 설치

> npm i -D webpack @babel/core babel-loader @babel/preset-env @babel/preset-react @babel/preset-typescript

> npm i -D @types/webpack @types/node

> npm i -D style-loader css-loader 

style-loader, css-loader // -D 떼야 하는지 확인



index.html 파일 생성

```html
<html>
  <head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>슬리액</title>
    <style>
        html, body {
            margin: 0;
            padding: 0;
            overflow: initial !important;
        }
        body {
            font-size: 15px;
            line-height: 1.46668;
            font-weight: 400;
            font-variant-ligatures: common-ligatures;
            -moz-osx-font-smoothing: grayscale;
            -webkit-font-smoothing: antialiased;
        }
        * {
            box-sizing: border-box;
        }
    </style>
    <link rel="stylesheet" href="https://a.slack-edge.com/bv1-9/client-boot-styles.dc0a11f.css?cacheKey=gantry-1613184053" crossorigin="anonymous" />
    <link rel="shortcut icon" href="https://a.slack-edge.com/cebaa/img/ico/favicon.ico" />
    <link href="https://a.slack-edge.com/bv1-9/slack-icons-v2-16ca3a7.woff2" rel="preload" as="font" crossorigin="anonymous" />
  </head>
  <body>
    <!-- 복/붙 후 필요하면 id 수정 -->
    <div id="app">

    </div>
    <script src="./dist/app.js"></script>
  </body>
</html>
```



```tsx
// client.tsx

import React from 'react';
import { render } from 'react-dom';

render(<div>Comming Soon</div>, document.querySelector('#app')); // index.html body 아래 div의 id와 맞춰줄 것
```



webpack 실행

> npx webpack



#### 혹시 에러가 난다면 아래와 같은 방법으로 실행

tsconfig-for-webpack-config.json 파일 만들기

```json
// tsconfig-for-webpack-config.json 
{
  "compilerOptions": {
    "module": "commonjs",
    "moduleResolution": "Node",
    "target": "es5",
    "esModuleInterop": true
  }
}
```

> TS_NODE_PROJECT="\tsconfig-for-webpack-config.json\" webpack

위 명령어가 너무 기니까 package.json 스크립트에 등록해서 npm run build 로 실행하게끔 만들자

```json
// package.json
"scripts": {
    "build": "cross-env TS_NODE_PROJECT=\"tsconfig-for-webpack-config.json\" webpack",
}
```

- 윈도우에서 동작하게끔 cross-env 설치

> npm i cross-env

> npm i ts-node

