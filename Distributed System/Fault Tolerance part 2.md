# Fault Tolerance part 2

> * **Reliable Group Communication**
>
>   Multicast - 모든 노드가 통신할 필요없이 어플리케이션을 공유하는 노드끼리만 통신함.
>
>   * *Providing reliability to Multicast* 
>
>     * In presence of message failure - Reliable Multicast 메세지를 잃어버리지 않게 한다.
>
>     * In presence of process failure - Atomic Multicast 
>
>       * Atomic : 원자, 즉 쪼갤 수 없는, 전체가 하나인 것처럼
>
>         
>
>   * *Reliable Multicast* 
>
>     * 프로세스 그룹에 속해있는 모든 멤버에게 메세지가 전달된다고 보장한다.
>
>     * *Receiver feedback*
>
>       * 그룹에서 관련된 모두에게 ack을 받아야만 모두 받았음을 알 수 있다.
>
>     * *Basic Reliable-Multicasting*
>
>       * receiver가 NACK을 보냈거나, ACK이 timeout이 된 경우 재전송이 일어난다.
>
>     * *Reliable Multicasting Scalability*
>
>       * 그룹 내의 멤버가 많을 경우 sender는 모두에게 feedback message를 받다가 ACK implosion이 발생할 수 있다.
>       * 따라서 많은 수의 ACK을 수용하지 말고 NACK만 수용하는 것이다. - 확장성 증가
>       * Problem 
>         * sender는 데이터 패킷 목록을 저장할 수 있는 버퍼를 둬야 한다. - 재전송을 위해서
>         * NACK만 수용하는 것도 implosion이 일어나지 않는다고 단언할 수 없다.
>
>     * *Scalable Reliable Multicasting*
>
>       * random delay를 이용한다. 
>
>         데이터를 못받은 receiver들은 NACK을 sender에게 보내주어야 한다. 하지만 재전송이 항상 전체 그룹에 멀티태스킹 된다면, 재전송에 대한 feedback이 하나여도 충분하다. 따라서 receiver들은 임의의 지연을 갖는 피드백 메세지를 스케쥴링한다. 이렇게 되면 제일 처음 하나의 NACK이 sender에게 도착한다. 그러면 하나의 NACK만으로 모두가 재전송을 받게되어 implosion이 방지된다.
>
>       * problem : interrupt successful receivers 
>
>         메세지를 성공적으로 받은 receiver가 쓸모없는 메세지를 수신하고 처리해야한다.
>
>     * *Hierarchical Feedback Control*
>
>       * 그룹을 서브그룹으로 나눈다 - 서브그룹을 하나의 노드라고 생각한다.
>
>       * 각 서브그룹에는 coordinator가 정해지고 root로 지정된다.
>
>       * 그룹내의 프로세스가 다른 그룹의 프로세스로 메세지를 보내려한다면 이를 그룹 내에 멀티캐스킹을 하여 coordinator가 메세지를 받게 된다. coordinator가 메세지를 받으면 인접한 coordinator로 메세지를 멀티캐스킹을 한다. 이렇게 타겟 프로세스가 있는 그룹의 coordinator가 메세지를 받으면 coordinator는 메세지를 멀티캐스팅하여 해당 프로세스가 메세지를 받게되는 것이다.
>
>         * ACK-based 
>
>           sender coordinator는 receiver coordinator가 확실하게 받음을 알리는 ACK이 올때까지 메세지를 버퍼에 저장한다 - 재전송을 위해서
>
>         * NACK-based
>
>           ACK-based와 원리는 같으나 receiver coordinator는 메세지를 못받은 경우에 NACK을 보낸다.
>
>       * 가장 확장성이 좋다. Better Scalability
>
>     * *Receiving vs Delivering*
>
>       Receiving : 머신의 interface까지 패킷이 온것
>
>       Delivering : 그 후에 머신 내의 어플리케이션이 패킷을 받은것.
>
>     * *Reliable multicast*는 단지 멀티캐스팅에서 메세지의 손실을 없애기 위한 것이다. 그렇다면 process faliure가 있는 상황에서 생각해보자. 
>
>       
>
>   * *Atomic Multicast*
>
>     * *Virtually synchronous*
>
>       메세지가 모든 정상적인 프로세스로 전달이 되거나 전부 전달이 안되는 것을 보장하는것.
>
>       * Group view = group list 그룹내의 프로세스들은 동일한 관점을 갖는다.
>
>       * View change 
>
>         프로세스가 떠나거나 새로 추가되는 것.
>
>         멀티캐스트하는 동안에는 view change가 일어나지 않는다. 그렇지 않으면 충돌이 발생함.
>
>     * *Message ordering*
>
>       프로세스 간 메세지를 주고 받을 때, 메세지의 순서를 정하는 것.
>
>       * Unordered multicast - 순서가 없다.
>       * FIFO-ordered multicast - **같은 프로세스에서의 메세지의 순서**가 보장되야 함. 다른 프로세서에서 받은 메세지의 순서는 상관이 없음.
>       * Causally ordered multicast - **인과 관계가 있는 순서**는 지켜져야 한다. causal하면 FIFO함.
>       * Totally-ordered multicast - 받은 순서에 상관 없이 **모든 프로세스들이 같은 순서**를 갖는다. sequentail consistency와 유사함 - **atomic multicasting**
>
>       
>
> * **Distributed Commit**
>
>   * Commit : 확정짓는것.
>   * 모든 프로세스는 메세지를 받았을 때 같은 결정을 해야한다.(모두 commit 하거나 abort 하거나)
>   * *One-phase commit protocol* : coordinator가 모든 노드가 메세지를 받으면 commit 명령을 내리는데 어떤 한 노드가 갑자기 그 리소스를 사용할 수 없게 되어 commit을 못하게 되면 일관성이 깨진다 - **Two-phase commit protocol** 등장
>   * *Two-phase commit protocol* : fail인 노드가 없고 모두 commit할 준비가 되었다면 모두 **COMMIT**, 하나라도 안된다면 모두 **ABORT**.
>     * One coordinator
>     * N workers(replicas)
>     * init : 데이터를 받고 커밋할 지 버릴 지 결정하기 전 단계(초기 단계)
>     * vote-request : coordinator가 워커들이 커밋할 수 있는지 물어보는 것. 모든 노드가 대답할때까지 wait.
>     * vote-commit : 나는 commit이 가능하다. 이 메세지를 coordinator에게 보내면 ready 상태가 됨.
>     * vote-abort : 나는 commit이 불가능하다. ready로 가지않고 바로 abort.
>     * 모두 commit이 가능하면 coordinator는 GLOBAL-COMMIT을 보내고 하나라도 아니라면 GLOBAL-ABORT를 보낸다. 
>     * *워커들은 commit전에 coodinator에게 피드백을 보낸다.*
>     * worker의 failure를 처리하는 방식
>       * coordinator가 wait 상태에서 워커들의 답을 기다리는데 time out이 난다면 워커가 crash라고 여기고 GLOBAL-ABORT를 보낸다.
>     * Coordinator의 failure를 처리하는 방식
>       * 워커가 init에서 vote-request를 기다리다가 time out이 되면 그냥 데이터를 abort한다.
>       * 워커가 vote-commit을 보내고 ready상태에 있다가 coordinator가 crash되면  coordinator가 복구될때까지 기다려야하는데 이것은 coordinator가 바로 복구가 안된다면 매우 비효율적이다. 이 상황에서는 이웃 노드에 상태를 물어보고 결정한다.
>         * 다른 노드가 commit이라면 coordinator가 global-commit을 보냈었는데 나에게 보내기 전에 crash됬구나라고 생각하고 commit한다.
>         * 다른 노드가 abort했다면 abort한다.(하나라도 abort면 global-abort)
>         * 다른 노드가 init상태라면 coordinator는 wait상태였던 것이므로 아직 글로벌 메세지를 보내지 않았다 - abort한다.
>         * 다른 노드가 ready라면 다른 노드를 확인해야한다 - coordinator가 뭐였는지 확정x
>           * 만약 모든 노드가 ready라면 vote-commit을 모두 한 경우이지만 어떤 한 노드가 abort하고 crash났다면 이 또한 확정할 수 없는 것이다.
>           * 따라서 Three-phase commit protocol 등장
>   * *Three-phase commit protocol*
>     * precommit : commit전 준비단계
>     * Worker의 failure 처리하는 방식
>       * coordinator가 첫번째 wait일때, vote 메세지가 안오면 global-abort를 보낸다. 하지만 두번째 wait인 precommit 상태일때는 이미 vote-commit을 모든 노드가 한 상태이기 때문에 모든 노드로부터 메세지가 안왔더라도 global-commit을 보낸다.
>     * Coordinator의 failure 처리하는 방식
>       * 워커들은 init상태에서 vote-request를 기다린다.
>         * 이때의 time out때는 abort
>       * 워커는 ready상태에서는 prepare-commit을 기다린다.
>         * 다른 노드가 init상태라면 precommit인 노드가 없는것이므로 abort
>         * 다른 노드가 abort라면 abort
>         * 모든 노드가 ready라면 commit한 노드가 없는 것이므로 abort
>         * 다른 노드가 precommit이라면 init이나 ready가 없고 모두 vote-commit을 한 것이므로 commit 
>       * 워커는 precommit상태에서는 global-commit을 기다린다.
>         * precommit 상태에 있다는 것은 모든 노드들이 ready에 있다가 coordinator로부터 prepare-commit을 받은 상태이므로 단 하나의 노드라도 init이나 abort가 있을 수 없다.(최소 ready) - commit한다.

