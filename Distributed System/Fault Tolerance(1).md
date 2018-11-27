# Fault Tolerance

> 어떠한 부품이 고장나더라도 시스템이 정상적으로 작동할 수 있도록 하는 것.
>
> * In non-distributed system
>
>   하나의 failure가 전체 시스템에 영향을 미치며 이로 인해 전체 시스템이 무너질 수 있다.
>
> * In distributed system
>
>   부분적인 failure, 즉 어떠한 요소의 fail이 일어나더라도 다른 부분은 계속해서 정상 작동한다.
>
>   
>
> 분산 시스템에서의 fault tolerance는 failure를 수용하고 자동적으로 복구하며, 이 복구가 전체적 시스템의 작동에 영향을 끼치지 않는 것이다. 



### Basic Concepts

> * **Dependability**
>
>   Dependable system은 fault tolerant와 매우 연관되어 있다.
>
>   - Dependable할 수록 fault tolerant하다.
>
>     
>
>   - **Dependability**의 속성
>
>     * *Availablility* 
>
>       * 전체 시간 중에서 실제로 가동된 시간으로, %로 나타낸다.
>
>       * Ex) 어떤 시스템이 10일 중 9일 동안만 작동하고 하루 고장난다면 이 시스템의 가용성은 90%이다.
>
>     * *Reliability*
>
>       * 시스템이 죽지 않고 정상 작동하던 시간으로, 시간으로 나타낸다.
>       * 시스템이 장애없이 연속적으로 실행될 수 있는 시간 간격을 의미
>       * **Mean Time To Failure(MTTF)** - 구성 요소가 실패할때까지의 평균 시간
>       * Ex) 어떤 시스템이 9일간 살아있다가 하루 고장난다면 신뢰성은 9일이다.
>
>     * *Safety*
>
>       * failure가 발생했을 때 심각한 장애를 일으키지 않는 것.
>
>     * *Maintainability*
>
>       * failure가 얼마나 쉽고 빠르게 복구되느냐를 나타냄.
>       * 이것이 좋으면 가용시간이 늘어나므로 **availability가 좋아진다.**
>
>     
>
>     *Mean Time To Failure(MTTF)* - 실패가 발생할때까지의 평균 시간(가용된 시간)
>
>     *Mean Time Repair(MTTR)* - 복구에 필요한 평균 시간
>
>     *Mean Time Between Failures(MTBF)* - MTTF + MTTR
>
>     *Availability(%) : MTTF / MTBF x 100*
>
>     
>
> * **Terminology**
>
>   * *Failure* : 잘못된 응답을 주거나 응답을 하지 않는 것.
>
>   * *Error* : 실패를 야기할 수 있는 시스템 상태 
>
>     * Ex) 오류가 있는 패킷이 receiver에게 가는 상황과 같음.
>
>   * *Fault* : 에러의 원인
>
>     * Ex) 망가진 프로그램(failure) - 프로그래밍 버그(programming error)에 의해 발생 - 개발자의 잘못된 코딩(fault) 
>
>   * **Fault tolerance**란 이러한 fault가 있더라도 **정상적으로 서비스하는 것**을 의미한다.
>
>     
>
> * **Type of Fault**
>
>   * *Transient Fault* : 한번 났다가 다음부터는 x - 1회성 fault
>
>   * *Intermittent Fault* : 가끔씩 일어남. 하지만 주기를 알 수 없기에 모니터링이 어렵다.
>
>   * *Permanent Fault* : 한번 일어나면 교체나 수리를 해야만 기능이 정상적으로 수행된다.
>
>     
>
> * **Failure Models**
>
>   * *Crash failures* : 서버가 정지하는 것, 재부팅을 통해 해결 가능함.(블루스크린)
>   * *Omission failures* : 서버가 들어오는 요청에 대한 응답의 fail.
>     * Receive omission : 서버가 요청을 받지 못한 것, 서버는 요청을 아예 모름
>     * Send omssion : 서버가 요청을 받고 수행했지만 응답을 하지 못한것.
>   * *Timing failures* : 서버는 응답을 제대로하지만 너무 느려서 fail이라고 느끼는 것.
>   * *Response failures* : 서버의 잘못된 응답.
>     * Value failure : 제대로된 요청에 잘못된 결과가 온 것, 검색하려는데 결과가 키워드와 다른것.
>     * State transition failure : 서버가 예상치 못한 요청을 받았고, 이것을 수행하려다 잘못된 상태로 빠져드는것.
>   * *Arbitrary(Byzantine) failures* :  가장 위험한 failure로 잘못된 응답이 비주기적으로 오는것.



### Main approach of Fault Tolerance

> **Using redundancy** (중복을 이용한다.)
>
> * *Information redundancy*
>   * FEC(Forward Error Correction) : 이전 프레임에 대한 압축된 프레임을 보내는 것 - 중복 이용, checksum을 이용하기도 한다.
> * *Time redundancy* : 재전송 하는것, 일시적이나 일정한 주기로 발생하는 오류에 유용함.
> * *Physical redundancy* : 같은 일을 하는 여러 프로세스를 만드는 것.
>   * Ex) Triple modular redundancy : 같은 역할을 하는 것을 3개씩 두는 것.



### Process Resilience

> 복제 프로세스를 **그룹화**하여 process failure에 대응함.
>
> * **Group Organization**
>
>   * *Flat groups* 
>
>     * 그룹의 구성원이 모두 동일한 지위를 갖는다. 
>
>     * 이들은 동일한 지위로 동일하게 행동하기 때문에 누구하나가 망가지거나 없어진다고해도 문제가 되지 않는다. 
>
>     * 하지만 의사결정 과정에서 어떤 의사를 결정하기 위해 voting과정이 필요한대 이 과정에서 오버헤드가 발생한다.
>
>       
>
>   * *Hierarchical groups* 
>
>     * 그룹내에 coordinator가 존재하여 다른 프로세스를 관리한다.
>     * 이것은 효율적이지만 coordinator가 망가지면 그룹이 동작을 못한다.(not fault tolerance)
>     * coordinator가 망가질 경우 election을 통해 새로운 coordinator를 선정한다.
>
>   
>
> * **Process Replication**
>
>   * *Primary-based protocol*
>
>     * 계층적 구조로 이루어짐 - primary가 모든 write operation 관리
>     * primary가 망가질 경우 election algorithm을 통해 새로운 primary 선정
>
>   * *Quorum-based protocol*
>
>     * 동등한 지위의 프로세스 모음으로 구성됨.(Distributed coordinator)
>
>     * 단일 장애 지점이 없다.
>
>       
>
> * **Group and Failure Masking**
>
>   *응답이 없는 failure는 처리가 간단하다. 하지만 잘못된 응답이거나 잘못된 것인지 아닌지 알 수 없는 응답에 대한 처리는 힘들다.*
>
>   * **K-fault tolerant**: K개의 머신이 fault를 일으켜도 잘 작동하기 위한 시스템
>
>   * **K-fault tolerant를 만족하기 위해서 얼마나 많은 머신이 필요한가?**
>
>     * *Crash failure semantics* : k개가 고장나도 하나만 응답하면 되기 때문에 **k+1**개의 머신이 필요하다.
>     * *Arbitrary failure semantics* : 응답이 잘못된 것인지 아닌지 알 수가 없으므로 **3K+1**개의 머신이 필요하다.(k만큼 고장난 경우 2k+1개의 나머지 머신이 voting을 통해 올바른 값을 산출한다)
>       * 3k+1개의 머신이 필요한 이유 Ex) Byzantine General's Problem
>
>   * **Consensus under arbitrary failure semantics**
>
>     * *System model*
>
>       * 1개의 primary process와 n-1개의 backup processes
>       * primary process는 backup processes에게 메세지를 보낸다. 
>       * backup processes는 서로 메세지를 주고 받으며 합의를 한다.
>
>     * **Byzantine Agreement**
>       * BA1 : 모든 결함이 없는 backup processes는 같은 값을 갖는다.
>
>       * BA2 : primary에 결함이 없다면 모든 결함이 없는 backup들은 primary가 보낸 값과 정확히 같다.
>
>         
>
>   * Arbitrary failure에서 k개의 결함이 있다면 2k+1개의 결함이 없는 프로세스로 인해 결함이 감춰지기 때문에 총합 3k+1개의 프로세스가 필요하다.
>
>   
>
> * **Failure Detection**
>
>   * *Detecting (process) failures*
>
>     * 살아있는지 메세지를 보내어 물어본 후 답을 기다림.
>     * 네트워크 상황에 따라 응답이 안오거나 늦을 수 있다. - 따라서 기간을 정하는 것이 중요하다. (RTT)
>
>   * *Detect failures through timeout mechanisms*
>
>     * 적절한 timeout을 정하는 것은 굉장히 어렵다.
>
>     * 프로세스 failure를 네트워크 failure와 구별할 수 없다.
>
>     * 따라서 주기적으로 이웃 프로세스가 살아있는지 아닌지 계속 물어보며 최신화한다.
>
>       
>
> * Precision-Recall
>
>   * 패턴 인식, 정보 분류, 이진 분류에서 사용됨.
>   * True positive(TP) : I said yes, and it's true
>   * True negative(TN) : I said no, and it's true
>   * False positive(FP) : I said yes, but it's wrong
>   * False negative(FN) :  I said no, but it's wrong
>     * Precision = TP / TP + FP
>     * Recall = TP / TP + FN



### Reliable Client - Server Communication

> 앞서 보았던 프로세스 문제들은 클라이언트 - 서버 통신 간에도 발생할 수 있다. 
>
> *  **Reliable Communication**
>
>   * *Error detection *
>
>     * checksum을 통해서 에러 체크
>     * frame의 수를 확인하여 패킷 손실 확인
>
>   * *Error correction*
>
>     * 손실된 패킷을 자동을 수정할 수 있도록 중복 비트를 추가
>
>     * 손실된 패킷에 대한 재전송 요청을 보낸다.
>
>       
>
> * **Reliable RPC**
>
>   외부 함수를 내 pc에서 실행하는 것처럼 보여야 하는데 서버가 망가질 경우 이게 불가능하다. 이럴때 투명성이 깨지게 되는데 어떻게 처리해야 할까.
>
>   * *Client cannot locate server - 클라이언트가 서버를 찾을 수 없다.*
>
>     * 모든 서버가 다운되거나 클라이언트가 모르게 서버가 업데이트 되었다.
>
>     * 클라이언트에게 다시 보고하여 클라이언트가 failure를 처리한다.
>
>       
>
>   * *Client request is lost - 클라이언트 요청이 손실됬다.*
>
>     * OS나 클라이언트 stub은 요청을 보내고 시간을 잰다.**(timeout)**
>
>     * 응답이 오기전에 시간이 만료되면 **재전송**한다.
>
>     * 이런 요청을 계속 보내는데 손실이 계속 났다 - 클라이언트는 포기
>
>       
>
>   * *Server crashes - 서버가 망가졌다.*
>
>     * 서버가 요청을 수행하였지만 응답을 하기전 망가진 경우
>
>     * 서버가 수행을 하기전 망가진 경우
>
>     * 어떤 행동을 취할 것인가
>
>       * *At-least-once-semantics* : 서버가 요청에 대한 작업을 적어도 한번은 수행할 것이라는 방법론
>       * *At-most-once-semantics* : 서버가 요청에 대한 작업을 최대 한번 수행하거나 안할 수도 있다는 방법론
>
>     * Example - printing server
>
>       * Assumption
>         * 서버가 망가진후 복구가 된 상황
>         * 서버는 복구가 되었음을 클라이언트들에게 알린다.
>         * 이 경우 클라이언트는 프린팅 작업이 실제로 수행됬는지 아닌지 알 수가 없다.
>       * Server strategy
>         * 서버의 완료 메세지는 보통 **모든 작업이 끝난 후** 보내지는게 일반적이다. 하지만 작업이 굉장히 오래 걸리는 경우 클라이언트가 오랜 시간 블락킹될 수 있다. 이럴때는 **서버가 앞으로 작업을 수행할 것이라고 미리 메세지를 보내는게** 효율적일 수 있다. 따라서 각 상황에 따라 두 방법을 잘 사용해야 한다.
>       * Client strategy - 서버가 고장 후 복구 메세지를 받았을 때의 전략
>         * *Always issue a request*
>         * *Never issue a request*
>         * *Issue a request only when not Acked*
>         * *Issue a request only when Acked*
>
>       
>
>   * *Server response is lost - 서버의 응답이 손실됬다.*
>
>     * *Lost replies*
>
>       * 이 문제는 사실 다루기 어려우며 요청에 대한 timeout을 두는 방법밖에 없다.
>
>       * 서버가 망가진 경우이거나 요청이 손실된 경우
>
>       * 서버가 작업을 수행했는지 여부를 알 수 없다.
>
>       * Solution - **idempotent**(멱등성 - 여러번 연산해도 결과가 달라지지 않는 것.)
>
>         * 요청을 보낼때 **sequence number**를 넣어 보낸다.
>
>         * 메세지 헤더에 **약간의 비트**를 넣어 보낸다.
>
>           * 재전송과 초기 요청을 구별하기 위해
>
>             
>
>   * *Client crashes - 클라이언트가 망가졌다.*
>
>     * 이것은 방법이 없다 - 받을 사람이 없기 때문
>     * *orphan computation* 발생 - CPU cycle를 낭비, 귀중한 자원을 묶고 있음.
>     * Solution
>       * 클라이언트는 복구 이후에 복구가 되었음을 알리는 메세지를 보낸다. 서버는 이 메세지를 받으면 orphan을 죽인다.
>       * 일정 시간 T를 정한다.  T가 만료되면 orphan을 죽임 
>       * orphan을 제거할 때 생길 수 있는 문제
>         * orphan이 파일이나 DB에 잠금을 확보한 경우 orphan이 갑자기 죽으면 저 자원들은 영원히 잠길 수 있다.