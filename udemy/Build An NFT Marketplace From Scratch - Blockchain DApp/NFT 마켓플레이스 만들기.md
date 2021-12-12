# NFT 마켓플레이스 만들기



### 개발환경 셋업

> npm i ethers hardhat @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers web3modal @openzeppelin/contracts ipfs-http-client axios 

- ehters: 이더리움 블록체인과 인터렉팅을 위한 라이브러리
- hardhat: 개발환경을 제공, 컨트랙트를 컴파일하고 디플로이하기 위한 라이브러리(truffle과 같은 역할, 이 프로젝트에서는 truffle 대신 hardhat 사용)
- chai: 테스팅을 위한 라이브러리
- @openzeppelin/contracts: NFT 토큰 발행을 위한 라이브러리?
- ipfs-http-client: 탈중앙화된 파일 호스팅을 위한 라이브러리

> npm i add -D tailwindcss@latest postcss@latest autoprefixer@latest

> npx tailwindcss init -p

- tailwind.config.js 생성
- postcss.config.js 생성

> npx hardhat

- basic sample project 선택
- nextjs 프로젝트를 프로젝트 root로 지정
- .gitignore 추가 y



### Hardhat 설정

- **Private 키 설정**

  - private 키는 .env 파일에 조차 넣는 것을 추천하지 않음

  - p-key.txt 파일 생성해서 private 키 저장


- **Infura 설정**
  - nfura.io 에 접속해서 프로젝트를 생성하고 네트워크의 엔드포인트를 얻는다.
  - 블록 동기화 없이 블록체인 네트워크 노드에 접근하기 위해 필요



```js
// hardhat.config.js

require("@nomiclabs/hardhat-waffle");
const fs = require("fs");
const keyData = fs.readFileSync("./p-key.txt", {
  encoding: "utf8",
  flag: "r",
});

module.exports = {
  defaultNetwork: "hardhat",
  networks: {
    hardhat: {
      chainId: 1337, // config standard
    },
    mumbai: {
      url: `https://polygon-mumbai.infura.io/v3/${process.env.INFURA_PROJECT_ID}`,
      accounts: [keyData],
    },
    mainnet: {
      url: `https://mainnet.infura.io/v3/${process.env.INFURA_PROJECT_ID}`,
      accounts: [keyData],
    },
  },
  solidity: {
    verson: "0.8.4",
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
};
```



### 스마트 컨트랙트 작성

- contract/NFT.sol 생성
- contract/KBMarket.sol 생성



#### NFT 민팅 스마트 컨트랙트

- Counters는 safemath를 위해 사용 - 스마트 컨트랙트 코드를 보다 안전하게 만듦
- 