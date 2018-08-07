---
layout: post
title: "Dapp 개발기 - 환경설정"
subtitle: "React.js, web3.js, Node.js, Truffle, Ganache"
categories: devlog
tags: blockchain
---



## 우선 아무생각없이 DApp 개발을 하겠다고 나대지 말자.

DApp 개발을 해보려고 이것저것 공부하다보니, 어느덧 기본은 되었다 싶었다.

근데, 정말 진짜 Solidity 개발환경이나 프레임워크에 관심 1따위도 없는 비탈릭은 좀 맞아야한다. 아무리 블록체인 기술과 스마트 컨트렉트가 알파보다 더한 초기 기술임에는 분명한데, 무슨 세달도 안되서 되던게 안되고, 막히는거 천지에 오히려 9달 가까이 수정하지 못한 기능들을 애타게 기다리는 개발자들이 스택오버플로우와 이더리움 스택에 쌓여가는걸 보면서. . . 결국, 예정대로 개발 테스트 환경 구축부터 역시 지옥을 맛봤다.

개발환경 구축이 반이지 뭐

시작은 Geth였으며, 나아가 리믹스, 트러플, 그리고 React로 DApp환경 구축까지 별에 뻘짓을 다한것 같다. 개발환경 하나 변경시켜볼때마다 "하하하, 그건 나도 잘 모르는 버근데 고치고있어" 같은 해당 프레임 워크 개발자의 말을 보았다. 여차저차해서 결국 지금 당장 돌아가는 개발환경을 찾긴했는데, 아직 코드 분석이 덜 되었다.



OS 환경은

- 우분투

였고, 필요한 개발 환경은

- Truffle
- Node.js
- React.js
- Ganache
- Metamask

이정도 인듯 하다. 하도 뻘짓한게 많아서 저거로 안되면 알아서 깔아주십사. . .(나도 모르는 환경도 많이 깔았다)



흠 설치를 해보자

```
$ sudo apt-get update && sudo apt-get upgrade //상태 업뎃좀 하고
$ sudo apt-get -y install curl git vim build-essential  //curl 사용할거니까 없으면 좀 받고
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - //Node.js 다운
$ sudo apt-get install -y nodejs  //Node.js 설치
$ sudo npm install -g npm //최신 Node.js 설치
$ sudo npm install -g express
$ sudo npm install -g truffle //Node.js 이용하여 truffle 설치
$ sudo npm install -g ethereumjs-testrpc //이더리움js testRPC 설치
```



테스트 RPC 잘 설치됬나?

```
$ testrpc
EthereumJS TestRPC v2.1.0
Available Accounts
==================
```

굳



리액트를 좀 깔아보자

```
$ sudo npm install -g babel webpack webpack-dev-server //글로벌 패키지 먼저 설치하자
$ sudo npm install --save react react-dom //Dependency 설치
$ sudo npm install --save-dev babel-core babel-loader babel-preset-react babel-preset-es2015 webpack webpack-dev-server  //Plugin 설치
```

이정도면 되겠지



가나슈도 깔아보자

```
$ mkdir ~/ganache
$ git clone https://github.com/trufflesuite/ganache.git //git 없는건 아니겠지. . .? 없으면 깔자
$ cd ~/ganache && sudo npm install
$ sudo npm start
```

이쁜 가나슈 GUI가 나온다면 됬음. 아참 CLI도 깔아야지 쉬움쉬움

```
$ sudo npm install -g ganache-cli //Ganache-cli 설치
$ ganache-cli
Ganache CLI v6.0.3 (ganache-core: 2.0.2)

Available Accounts
==================
```

끝. 이지이지. 잘됨ㅇㅇ



메타마스크는. . .구글 확장프로그램가서 메타마스크 치면 나와요 설치하세욥. . .



환경설정은 끝났고. . . 이제 프로젝트 설정하는 방법을 알려드립니다. 후. .  이게 제일 고생이지

1. 프로젝트 생성

   1. 아무대나 원하는 작업 폴더에 들어간다.
   2. `$ npx create-react-dapp 프로젝트명` 친다.
   3. 기본 프로젝트 알아서 생성해준다. 기다린다.
   4. `$ cd 프로젝트명 && npm run ganache` 친다.
   5. 서버가 구동된다. 기다린다. ganache-cli 동작한다.

2. 코드 작업

   1. 원하는 에디터 연다. (참고로 전 Visual Code 씁니다)

   2. 해당 프로젝트 폴더 연다.

      1. 컨트렉트나 dapp관련은 dapp에
      2. React관련은 src에 있다.

   3. 컨트렉트를 만들어보자

      1. contracts에 새 스마트컨트렉트파일을 추가한다. 아 물론 솔리디티 파일이므로 `컨트렉트명.sol` 이다.

      2. 컨트렉트를 짠다.

      3. 다 짰으면 migration을 생성해줘야하는데, 생성하는 방법엔 두가지가 있다.

         1. truffle 이용
            1. 터미널의 해당 프로젝트 위치에서
            2. `$ truffle create migration 컨트렉트명`
            3. 하면, 에디터의 프로젝트-dapp-migrations에 `숫자아아_컨트랙트명.js` 같은게 생김
            4. 개발자끼리의 약속으로 `숫자_deploy_컨트렉트명.js`로 바꿔주자.(안바꿔도 상관없단소리)
         2. 에디터 상에서 생성
            1. 프로젝트-dapp-migrations 폴더에 우클릭하여 새 파일 추가
            2. `숫자_deploy_컨트렉트명.js` 생성

      4. 생성된 migration에 코드 넣기

         1. 이건 솔직히 기본으로 준 `2_Voting.js`를 보고 따라하는게 쉽다.

         2. 

            ```
            //기본 형식
            const deployInfo = require('../helpers/deployInfo'); //배포를 쉽게하기 위한 친구
            const 컨트렉트명 = artifiacts.require('컨트렉트명'); //작성한 컨트렉트 불러오기
            // 변수라 굳이 컨트렉트명일 필요는 없는데, 이렇게 그냥 쓰자.
            module.exports = async deployer => {
                await deployer.deploy(컨트렉트명);
                return deployInfo(deployer, 컨트렉트명);
            };
            ```

         

      5. 배포하기

         1. 터미널의 해당 프로젝트 위치에서
         2. `$ npm run migrate` 하면 ganache-cli의 Private Blockchain에 배포를 하게된다.
         3. 배포가 완료되면,  프로젝트-dapp-build-contracts에 컴파일된 solidity파일의 json이 있는 것을 확인 가능하며, 프로젝트-public- contract-info에도 json파일이 생성된 것을 확인할 수 있다.

      6. 끝

위의 과정으로 스마트 컨트렉트와 React.js, Node.js, Truffle이 연동되는 것을 확인 할 수 있다. 스마트 컨트렉트의 컴파일, 배포를 마치면, 나머지는 사실 React의 영역이라고 봐도 무방하다.



이번엔 여기까지 : )