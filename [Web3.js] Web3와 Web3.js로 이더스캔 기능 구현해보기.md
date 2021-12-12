# [Web3.js] Web3와 Web3.js로 이더스캔 기능 구현해보기

![](https://urclass-images.s3.ap-northeast-2.amazonaws.com/beb/section4/unit6/001.png)

### Web1과 Web2

Web3를 살펴보기 전에 Web1과 Web2를 살펴보자. Web1의 문제점을 해결하기 위해 Web2가 생겨났고, Web2의 문제점을 해결하기 위해 Web3가 생겨났으니까.

Web1은 클라이언트가 정보를 조회만 할 수 있는 이코노미를 의미한다. 서버가 정보를 생성해서 제공하면 클라이언트는 오직 조회만 가능하여 일방적으로 정보를 얻는 형태이다. 하지만 web1에서 클라이언트는 정보를 생성할 수 없기 때문에 Web1에서는 인터넷의 정보가 풍성하지 못하다.

Web1의 문제점은 서버측에서만 정보 생성이 가능한 구조로 인해 정보가 풍성하지 못하다는 것이었다. Web2에서는 클라이언트도 정보를 생성할 수 있게 되어 인터넷 상의 정보가 폭발적으로 성장한다. 그 기반에는 AJAX의 등장이 있다. 이를 통해 클라이언트가 동적으로 정보를 생성할 수 있게 되었다. 그 결과 페이스북, 구글, 트위터와 같은 플랫폼이 사용자의 정보 생성에 힘입어 거대해졌다. 하지만 web2에서 사용자들은 자신이 생성한 정보에 대한 권리(검열로부터의 자유로움 등)를 온전히 가지지 못하고, 플랫폼에서 정보를 생성하기 위해 자신의 개인정보를 제공하여야 한다. 이렇게 제공된 개인정보는 거대 플랫폼 기업들이 수익을 위해 사용하기도 하고 본의 아니게 해킹되기도 하는 문제가 발생한다.



### Web3

Web2의 문제점은 정보의 생성이 거대 플랫폼 기업 위에서 이루어지다보니 정보의 생성자가 생성한 정보의 권리(검열로부터의 자유로움 등)를 온전히 가지지 못하고 정보를 생성하기 위해 자신의 개인정보를 제공해야 한다는 것이었다. 그래서 Web2는 플랫폼이라는 중앙집중적인 주체가 있고, 그 주체에 의존하는 구조였다. Web3에서는 중앙집중적인 플랫폼을 벗어나서 모든 참여자가 플랫폼이 된다. 모든 참여자는 자신이 생성한 정보를 검열없이 공유하고 암호화 기술을 사용해 개인정보를 제공하지 않고도 신원을 식별할 수 있다. Web3를 구현하기 위해 탈중앙화 되어있는 블록체인 기술이 사용된다.



### Web3.js

**자바스크립트를 이용한 이더리움 네트워크 API**.  Web3.js는 자바스크립트에서 HTTP, IPC, WebSocket 등을 사용해 이더리움 네트워크의 노드들과 상호작용하기 위한 API이다.



### 이더스캔 기능 구현

![](https://imgur.com/EgbAESS.jpg)

지갑 주소 하나와 블록 넘버(시작, 끝 블록)를 입력받아 해당 블록 범위 내에서 지갑 주소와 관련이 있는 트랜잭션을 출력하는 스크립트를 작성해보자.

```javascript
const Web3 = require("web3");
const fs = require("fs");
const readline = require("readline");
const INFURA_PROJECT_ID = fs.readFileSync("./infura_project_id.txt", {
  encoding: "utf8",
  flag: "r",
});

const rpcURL = `https://ropsten.infura.io/v3/${INFURA_PROJECT_ID}`;
const txs = [];
const web3 = new Web3(rpcURL);

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

const getTransaction = async (account, startBlock, endBlock) => {
  for (var i = parseInt(startBlock); i <= parseInt(endBlock); i++) {
    // console.log("block number: ", i);
    const block = await web3.eth.getBlock(i);
    // console.log('transactions count: ', block.transactions.length)

    for (let tx of block.transactions) {
      const txResult = await web3.eth.getTransaction(tx);

      if (account === txResult.to || account === txResult.from) {
        // console.log(">>> ", tx);
        txs.push(tx);
      }
    }
  }
  console.log(txs);
};

rl.question("탐색할 주소를 입력해주세요: ", (account) => {
  rl.question("탐색할 범위(시작 블록, 끝 블록)를 입력해주세요: ", (block) => {
    const [startBlock, endBlock] = block.split(", ");
    // console.log(`${account}`);
    // console.log(`${startBlock}, ${endBlock}`);

    getTransaction(account, startBlock, endBlock);

    rl.close();
  });
});
```

