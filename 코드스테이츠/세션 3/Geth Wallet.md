# Geth Wallet

소요시간 : 6 min

## Geth Wallet

여러분은 메타마스크와 마이이더월렛을 통하여 웹을 이용한 계정 생성방법을 알아보았습니다. 이번에는 CLI(명령줄 인터페이스)를 통해 이더리움 계정을 생성하고, 테스트넷에서 테스트 이더를 받는 방법까지 실습하겠습니다.

### MacOS Geth 설치(Homebrew)

> Homebrew가 설치되어 있다는 전제하에 실습이 진행됩니다. Homebrew가 설치되어 있지 않다면, 이 [페이지](https://brew.sh/)에서 설치 후 진행해주세요.

**1. 다음 명령을 실행하여 탭을 추가합니다.**

```
$ brew tap ethereum/ethereum
```

**2. 탭 추가가 완료되면, Geth를 설치합니다.**

```
$ brew install ethereum
```

### Ubuntu Geth 설치(PPA)

Ubuntu에서 PPA(Personal Package Archives)를 사용하여 Geth를 설치합니다.

**1. 런치패드 저장소 활성화를 위해 다음을 실행하세요.**

```
$ sudo add-apt-repository -y ppa:ethereum/ethereum
```

**2. Geth를 설치합니다.**

```
$ sudo apt-get update
$ sudo apt-get install ethereum
```



## Geth 계정 만들기

**1. Geth 명령어를 사용하여 새로운 계정을 만듭니다.**

```
$ geth account new
```

**2. 위의 명령어를 실행하면, `비밀번호`를 입력하라고 합니다. `재확인 비밀번호`도 입력해줍니다.**

```
Your new account is locked with a password. Please give a password. Do not forget this password.
Password: 
Repeat password: 
```

**3. 알맞게 입력했다면, 새로운 키를 생성했다는 말과 함께 `address`와 `secret key file 경로`를 알려줍니다.**

> !! 중요 !! `address`와 `secret key file 경로`, `비밀번호`는 잊지 않도록 안전한 곳에 백업합니다. 특히, `secret key file`의 경우 절대로 잃어버리시면 안됩니다.

```
Your new key was generated

Public address of the key:   [계정 주소]
Path of the secret key file:    [secret key file 경로]

- You can share your public address with anyone. Others need it to interact with you.
- You must NEVER share the secret key with anyone! The key controls access to your funds!
- You must BACKUP your key file! Without the key, it's impossible to access account funds!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
```

**4. 현재 자신이 가지고 있는 계정 리스트를 보려면 다음의 명령어를 실행합니다.**

```
$ geth account list
Account #0: {a1ce1ae4e215858f7c79da67b2996331af3ee7b1} keystore:///Users/insulee/Library/Ethereum/keystore/UTC--2021-11-11T15-29-11.509514000Z--a1ce1ae4e215858f7c79da67b2996331af3ee7b1
```



## Geth에서 롭스텐 테스트넷 이더 받고, 확인하기

**1. Geth JavaScript 콘솔을 사용하여 롭스텐 테스트넷에 접속합니다.**

> !! 생각해보기 !! `geth console --ropsten` 과 `geth console 2> /dev/null --ropsten`의 차이점은 무엇이 있나요?

```
> geth console 2> /dev/null --ropsten
```

- 네트워크 연결이 잘 되었는지 확인하기 위해 다음의 명령어를 실행합니다.

```
> net.listening
true
```



**2. 롭스텐 테스트넷 용 계정을 생성합니다.**

> 계정 생성 전에 다음의 명령어를 실행하고, 생성 전과 후가 어떤 차이가 있는지 확인해보세요!
>
> ```
> > eth.accounts;
> ```

명령어를 실행하고, `비밀번호`와 `재확인 비밀번호`를 입력하세요.

```
> personal.newAccount();
Passphrase:
Repeat passphrase:
```



**3. [롭스텐 테스트 이더 받는 페이지](https://urclass.codestates.com/faucet.ropsten.be)에서 테스트 이더를 받은 후 실제로 들어왔는지 확인합니다.**

```
# eth.accounts[ ] 에 있는 숫자는 `eth.accounts;` 실행 후 몇 번째 주소인지 입력하면 됩니다. ex, 첫 번째 주소는 "0"
> eth.getBalance(eth.accounts[0]);
```



<div style="padding: 15px; border: 1px solid transparent; border-color: transparent; margin-bottom: 20px; border-radius: 4px; color: #a94442; background-color: #f2dede; border-color: #ebccd1;">
    밸런스가 계속 0으로 보이는 문제 발생<br>
</div>



## 이더스캔으로 내 계정 확인하기

이더스캔(Etherscan)은 이더리움 블록체인에서 일어나고 있는 모든 활동과 정보를 쉽게 검색할 수 있는 사이트입니다. 이더리움의 블록 생성 내역, 트랜잭션 조회, 지갑 정보 조회, 이더리움 기반의 토큰 검색 등 블록체인에서 일어나고 있는 모든 활동과 정보를 쉽게 검색할 수 있습니다. 지금까지 생성해본 메타마스크, 마이이더월렛, GethWallet은 결국 같은 형태의 지갑이기 때문에 각각의 주소를 검색하면 찾을수 있습니다.

### 사용방법

- [이더스캔](https://etherscan.io/)에 접속하여, 검색창에 지갑 주소를 `복사` & `붙여넣기`합니다.

- 지갑이 보유하고 있는 계정 정보를 확인해보세요.

  > 테스트넷에서 발생한 트랜잭션 확인은 시간이 걸립니다.