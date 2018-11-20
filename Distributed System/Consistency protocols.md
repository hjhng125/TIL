# Consistency protocols

> * Data-centric consistency
>   * Primary-backup protocols
>   * Local-write protocols
>   * Quorum-based protocols
> * Client-centric consistency



### Primary-backup protocol

> * *모든 write operation은 primary server에 의해 처리되어야 한다.* 
>
>   1. write operation에 대한 request가 오면
>
>   2. primary server로 보내진다. 
>
>   3. primary server가 요청을 처리하면 backup server에 값이 수정됨을 알린다. 
>
>   4. Backup server는 이 메세지를 받고 값을 수정한 후에 primary server로 수정을 완료했다는 ack을 보낸다. 
>
>   5. 모든 local copy가 update를 수행하면 primary server는 맨 처음 write operation 요청을 보낸 process에 모든 backup server가 업데이트가 완료되었음을 알리는 ack을 보낸다. 
>
>   6. 이 과정 후에 backup server는 client에 write operation이 성공적으로 완료됬음을 알린다.
>
>      
>
> * Drawbacks 
>
>   * Client는 위의 과정이 완료될 때까지 **block** 상태에 빠진다.
>
> * Alternative
>
>   * Nonblocking approach 
>     * Primary server가 update를 수행하자마자 ack을 반환하여 backup server가 update를 수행하여 client에 알린다.
>     * Drawbacks 
>       - 이 방식은 모든 backup server가 consistency를 유지하지 않을 수 있다.
>       - Blocking 방식은 sequential consistency를 유지하는게 쉽게끔 구현되어 있지만 Nonblocking은 이를 보장할 수 없다.



### Primary-Based protocol

> * Update 요청을 한 *Client의 가장 가까운 server가 Primary server*가 된다.
>   1. write operation에 대한 request가 오면
>   2. 요청에 대한 item을 primary server에서 클라이언트와 가까운 새로운 primary server로 이동시킨다.
>   3. update를 수행한다.
>   4. update가 수행되면 ack을 client에 보낸다.
>   5. backup server에 업데이트를 하라고 알린다.
>   6. backup server에서 업데이트에 대한 ack을 새로운 primary server로 보낸다.



### Quorum-Based protocols

> Decentralized와 비슷한 protocol로 **파일은 N개의 서버에 복제되고 Multiple replica servers에 file read와 write의 허가가 필요하다.**
>
> **N : 복제 서버의 수**
>
> **N(R) : read quorum 읽기 작업 수행을 위한 허가의 수**
>
> **N(W) : write quorum 쓰기 작업 수행을 위한 허가의 수**
>
> * 다음의 조건을 만족해야 한다.
>   * N(R) + N(W) > N 
>     * 이것을 만족하지 않으면 read를 할 때 최신값을 못 받을 수도 있다.
>     * write operation으로 update가 됬는데 N(R)과 N(W)가 하나도 겹치지 않는다면 위처럼 read할 때 최신값을 못받는다.
>   * N(W) > N > 2
>     * 이를 만족하지 않다면 write-write 충돌이 발생할 수 있다. (voting problem)
>   * ROWA(Read-One, Write-All) 
>     * N(R)이 1이고, N(W)가 all 이다.  - 모두 업데이트를 해야해서 비용이 많이든다.