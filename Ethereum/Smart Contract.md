# Smart Contract

> 이더리움의 핵심 기술 중 하나로 미국의 컴퓨터 과학자인 'Nick Szabo'가 제안한 개념이다.
>
> 계약 조건을 이행하기 위해 실제 사람이 계약서대로 수행을 해야하던 기존의 서면 계약과 달리 디지털로 계약을 하게 되면 조건에 따라 계약이 자동으로 실행할 수 있다는 이론이다.



### Ganache

> 보통은 Geth, Parity 등을 이용하여 블록을 동기화하고 실제 이더리움 메인/ 테스트 네트워크에 접속해 진행을 하지만 블록을 동기화하는데에 2~3일 정도 걸리고 블록을 채굴하기까지 기다려야 되는 등 개발 중에 불편한 점이 있다. 
>
> 개발 중에는 Ganache와 같은 가상 혹은 프라이빗 네트워크 상에서 스마트 컨트랙트를 구동한다.
>
> Ethereumjs 기반 testRPC
>
> 개발을 위한 블록 생성
>
> Transaction/ gas 및 Ether mining
>
> Ganache-cli 지원



### Remix

> Remix는 이더리움의 스마트 컨트랙트를 작성할 수 있는 웹 기반 IDE이다. Remix는 별도의 설치가 필요없어 편하게 사용할 수 있다. 
>
> Ganache와 연동하여 Remix로 배포되는 계약을 Ganache 가상 네트워크 상에서 확인해 볼것이다. 



### Metamask

> 이더리움 지갑 플러그인으로 오프라인 지갑이 없는 사람들은 구글 크롬에 설치하여 프라이빗키 없이 쉽게 이더리움 지갑을 엑세스할 수 있다. 
>
> 거래 Transaction을 처리
>
> 거래 내역을 저장



### Truffle

> 솔리디티 소스 코드를 직접 컴파일하고 블록체인에 올리는 작업을 매우 쉽게 해주며, 스마트 컨트랙트를 쉽게 테스트할 수 있는 방법을 제공해주는 프레임워크이다.
