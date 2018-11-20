# Client-centric consistency models

> Consistency models는 시스템에서 일관성을 유지하기 위한 규약이다.
>
> * Data-centric consistency models는 데이터 저장소 관점에서 시스템 전반의 일관성 제공을 목표로 한다. 이는 앞서 기술할 Client-centic consistency models 보다 tight한 정책으로 내부 절차가 더욱 복잡하다.



### Client-centric consistency models

* 동시 업데이트가 없거나 발생하더라도 쉽게 해결할 수 있다고 가정한다.

* 현재 데이터 저장소에서 작동중인 <strong>개별 클라이언트 프로세스(단일 클라이언트)</strong>에 대한 일관된 관점을 유지하기 위한 모델  - *여러 클라이언트의 동시 접근은 보장하지 않는다.*

* 현재 데이터 저장소에서 작동중인 *개별 클라이언트 프로세스(단일 클라이언트)*에 대한 일관된 관점을 유지하기 위한 모델  - *여러 클라이언트의 동시 접근은 보장하지 않는다.*


 #### Eventual consistency

> 충분한 기간 동안 업데이트가 이루어지지 않으면 모든 복제본은 점차적으로 일관성을 갖게 된다.
>
> * Eventual consistency가 적합한 경우
>
>   ```
>   1. read 작업이 대부분인 경우.
>   2. 동시적 업데이트가 없는 경우. 
>   ```
>
> * Example
>
>   ```
>   1. Database systems - 대부분의 작업이 read이며, 적은 수의 process만 업데이트를 한다.
>   2. Web cache server, browser cache
>   - webmaster에 의해서만 업데이트되며 구식 웹 페이지(특정 정도까지)는 허용 가능하다.
>   ```
>
> 
>
> * Client가  항상 동일한 복제본에 접근하는 한 eventual consistency는 문제 없다. 하지만 단기간에 여러 복제본에 접근한다면 문제가 발생한다.
>
>   * 모바일 디바이스에서 데이터베이스에 접근하여 하나의 복사본의 업데이트를 실행하였다고 가정하자. 이후에 다른 네트워크에서 다른 디바이스로 데이터베이스에 접근하였다. 이럴 경우 다른 복제본에 연결할 것이다. 이 때 업데이트가 네트워크상에 전파가 다 된 후가 아니라면 업데이트의 이전 자료를 확인할 수 있을 것이다.
>
>     
>
> * Client-centric consistency model은 단일 클라이언트의 다른 복제본 접근과 서로 다른 클라이언트의 update를 지원하지 않기 때문에 고안된 4가지 Model이 있다.



### 1.Monotonic read

> 만약 한 클라이언트가 x라는 데이터를 읽었다고 했을 때, x에 대한 어떤 연속적인 read operation이던 간에 해당 클라이언트는 같은 값이거나 더욱 최신인 x값을 받는다. 
>
> - Monotonic read consistency는 절대 이전의 x 값을 보여주지 않는다.
> - Ex) E-mail server



### 2. Monotonic write

> 많은 상황에서 write operation은 data store의 모든 복사본에 올바른 순서로 전파되어야 한다.
>
> * 데이터 x에 있어서 한 클라이언트에 의한 write operation은 동일한 클라이언트가 x에 대해 연속적인 write operation을 하기 전에 완료된다. 
> * 필요하다면 새로운 write operation은 이전의 write operaion이 완료되기 까지 기다려야한다.
> * Data-centric의 FIFO와 상당히 비슷하다. 



### 3.  Read-Your-Write consistency

> 한 클라이언트에 의한 데이터 x의 write operation는 동일한 클라이언트의 연속적인 read operation에 의해서 항상 보여져야 한다.
>
> * write operation은  read operation이 어디서 일어나는 가에 관계없이 동일한 프로세스에 의한 연속적인 read operation 전에 항상 완료된다.
> - Ex) Web page(Server vs Cache)



### 4. Writes-Follw-Reads consistency

> 동일한 클라이언트가 write operation을 수행할 때, 반드시 자기가 read한 값으로부터 수행되어야 한다.
>
> * 데이터 항목 x에 대한 임의의 연속적인 write operation은 해당 클라이언트가 최근에 읽은 값과 동일하거나 더 최신본으로 수행된다.
