# Replica management

> 분산 시스템에서 replication이 어디서, 언제, 누구에 의해 일어나는가.
>
> * Content placement
>   * Content 배치를 위한 최적의 서버 찾기
> * Update Propagation
>   * Update된 content를 관련 서버에 전파



### Finding the best server location

> 10년 전에는 개별적 서버를 알맞은 위치에 배치해야하는 문제가 존재했지만, 인터넷을 통한 많은 대규모 데이터 센터의 출현으로 문제가 크게 바뀌었다.



### Content replication and placement

> Content를 복제하고 배치하는 것으로 유형을 나눈다.
>
> * Static replicas
>
>   * Permanent replicas : 분산 데이터 저장소를 구성하는 복제본의 초기 세트 
>
>     - 데이터 저장소 소유자(Owner)가 생성
>     - 데이터 저장소는 단일 위치 또는 여러 지리적으로 분산 된 서버 (Mirror site라고 함)의 서버 클러스터에서 복제 할 수 있음.
>     - 수가 굉장히 적음
>     - 서버를 여러개 만들 때, 처음에 시작할 때 복사하고 시작하는 것
>     - Ex) Web site mirroring, distributed database
>
>     
>
> * Dynamic replicas
>
>   * Server-initiated replicas
>
>     * 시스템의 성능을 향상시키기 위해 생성 된 복제본
>
>     - 데이터 저장소 소유자의 요청에 따라 작성되며, 종종 다른 사용자가 관리하는 서버에 배치됨
>     - 많은 양의 클라이언트가 있는 곳에 배치됨.
>     - Dynamic replication 알고리즘은 contents를 저장할 위치를 결정하는 데 사용됨.
>     - Ex) Content Distribution Network (CDN), Reverse web proxy
>
>     
>
>   * Client-initiated replicas
>
>     * Client의 요청에 의한 복제
>     * Client의 데이터 접근의 효율을 증가시키기 위한 복사본(Client cache)
>     * Ex) Web browser cachess, Web proxy caches(forward proxy)



### Dynamic Replication : Server-Initiated Replicas

> Permanent replica와 달리 Server-Initiated replica는 성능 향상을 위해 존재하며 데이터 저장소의 소유자가 만든 데이터 저장소의 복사본이다. 
>
> 서버와 지리적으로 가까운 곳에서 오는 요청은 쉽게 처리할 수 있지만 예기치 않은 먼곳에서 오는 요청이 폭발적으로 많아진다면, 그 지역에 replica를 설치하는 것이 좋다.
>
> 
>
> * Objectives
>
>   * 서버의 부하를 줄인다. (Load balancing)
>   * Client의 주변의 서버에 파일을 복제할 수 있다.
>
> * Server-Initiated Replica algorithm
>
>   * 각 복제 서버에서 수행된다.
>
>   * 요청한 Client에 가장 가까운 서버(S)를 고려하여 파일(F)에 대한 접근 횟수를 추적한다. cnt<sub>Q</sub> (S, F) 
>
>      <sub>Q : 현재 해당 파일을 가진 서버</sub>
>
>      <sub>S : 요청 Client와 가장 가까운 서버</sub>
>
>      <sub>F : 요청 파일</sub>
>
>   * S에서 F에 대한 cnt<sub>Q</sub> (S, F)가 threshold(del(S, F))보다 낮다면 S에서 F 삭제
>
>   * S에서 F에 대한 cnt<sub>Q</sub> (S, F)가 threshold(rep(S, F))보다 높으면 S에 F를 복제



### Dynamic Replication : Client-Initiated Replicas

> * 일반적으로 Client cache라고 알려져있다.
> * 본질적으로 캐시는 클라이언트가 방금 요청한 데이터 사본을 임시로 저장하는 데 사용되는 로컬 저장 장치이다 - 데이터 접근 시간 향상시키기 위한 용도이다.
> * 대부분의 작업이 read operation이라면, 클라이언트가 요청된 데이터를 주변 캐시에 저장되도록 하여 성능을 향상시킬 수 있다. 이러한 캐시는 클라이언트 컴퓨터 또는 클라이언트와 동일한 로컬 영역 네트워크에 있는 별도의 컴퓨터에 있을 수 있다. 따라서 이후에 동일한 데이터를 읽어야할 때 로컬 캐시에서 데이터를 가져올 수 있다. 이 기간 동안 데이터의 수정이 일어나지 않는다면 문제가 생기지 않는다.
> * 원칙적으로 캐시 관리는 전적으로 고객에게 맡긴다.



### Content Distribution

> 관련 복제본 서버에 대한 (업데이트 된) 내용 전파를 처리한다.
>
> 중요한 문제는 실제로 전파 될 내용과 관련되며 3가지 타입으로 나뉜다.
>
> * 업데이트 알림만 전파한다(무효화 프로토콜)
>   * '너의 것은 최신버전이 아니야' 라고 최신화되지 않은 데이터를 알리기만 한다
>   *  read - write ratio가 비교적 낮은 경우 (read가 상대적 적고 write가 상대적 많은 경우)
>   *  bandwidth를 거의 사용하지 않음.
> * 수정된 데이터를 전송한다 
>   * read - write ratio가 비교적 높은 경우 (read가 상대적 많고 write가 상대적 적은 경우)
>   * 수정된 데이터를 전파하는 대신 변경 사항을 기록하고 로그만 전송하여 대역폭을 절약할 수 있다. 여러개의 수정내용이 하나의 메세지로 묶여 오버 헤드를 줄일 수 있다.
> * 데이터의 정보만 전파한다(실제 데이터  x)
>   * 수정된 데이터를 전송하지 않고 복제본에게 수행해야하는 업데이트 작업만 알려준다.
>   * 해당 업데이트에 필요한 파라미터만 전송한다.
>   * 파라미터의 크기가 작다면 최소한의 bandwidth로 업데이트를 전파할 수 있다.
>   * 작업이 임의의 복잡성을 가질 수 있으므로 복제본을 일관성 있게 유지하는 것이 개선될 수 있지만, 연산이 복잡한 경우 복제본에 더 많은 처리 능력이 필요할 수 있다.
>
> *각 타입은 사용가능한 bandwidth와 read-write ratio에 크게 의존하며 각 상황에 맞게 사용해야 한다.*
>
> 
>
> * Pushing updates
>   * 서버가 요청에 관계없이 수정될 경우 다른 복제본 서버로 업데이트를 보내는 것.
>   * 일반적으로 Permanent replica와 Server-initiated replica에 사용됨.
>   * 강력한 일관성이 필요한 경우(서버 기반 프로토콜)
>   * read-write ratio가 높은 경우 효율적이다.
>   * 요청 시 일관된 데이터를 즉시 사용할 수 있다.
>
>   
>
> * Pulling updates
>   * 클라이언트의 요청에 의해 서버로부터 수정된 데이터를 가져오는 것.
>   * 일반적으로 Client-initiated replica에 사용됨(client cache).
>     * Web cache에 적용되는 일반적 전략은 캐시된 데이터가 최신인지 Web server를 통해 확인해야 한다.
>     * 수정된 경우 수정된 데이터가 먼저 캐시로 전송된 후 클라이언트에게 리턴된다.
>     * 수정되지 않은 경우 캐시된 데이터가 바로 리턴된다.
>   * read-write ratio가 낮은 경우 효율적이다.
>     * 단일 클라이언트 캐시인 경우이거나 캐시기반 데이터의 공유가 거의 없는 경우
>
>     * 캐시 미스의 경우 Response time이 길어질 수 있다. 
>
>       
>
> * Comparing push and pull
>
>   * 서버의 상태 
>     * push-based : 클라이언트의 복제와 캐쉬의 리스트를 추적해야 한다. overhead 발생 가능
>     * pull-based : x
>   * 보내는 메세지
>     * push-based : 모든 클라이언트에게 update 메세지를 보낸다. update가 무효화되는 경우 수정된 데이터를 가져오기 위한 추가적인 통신이 필요하다.
>     * pull-based : 클라이언트가 서버를 polling 해야한다. 필요한 경우 수정된 데이터를 요청해서 가져온다.
>   * 클라이언트의 응답 시간 
>     * push-based : 즉시 업데이트된다. or fetch-update time(update가 무효화되는 겨우)
>     * pull-based : fetch-update time(클라이언트가 요청해서 가져오는 시간)