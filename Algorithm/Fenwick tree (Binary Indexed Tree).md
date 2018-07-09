# Fenwick tree (Binary Indexed Tree)

> Segment tree의 메모리 절약을 위해 만들어진 자료구조



### 특징 

###### 1. Segment tree의 트리를 변형하여 나타나는 몇 가지 패턴을 이용하는 자료구조.

###### 2. Segment tree의 right child를 지운 형태이다.

###### 3. Tree[i] : arr[i]까지의 구간 합이다.

----------------------------



### Fewick tree update code

```
void update(vector<ll> &tree, int i, ll diff) 
{
    while (i < tree.size())
    {
        tree[i] += diff;
        i += (i & -i);
    }
}
```

> (i & -i) 는 1이 등장하는 최하위 비트의 위치를 알아내기 위한 코드.
>
> 최하위 비트 위치에 1을 더해줌으로써 다음 인덱스를 구할 수 있다.

### Fenwick tree sum code

```
ll sum(vector<ll> &tree, int i)
{
    ll ans = 0; //long long type
    while (i > 0)
    {
        ans += tree[i];
        i -= (i & -i);  
    }
    return ans;
}
```

> 최하위 비트에 1을 빼줌으로써 이전 인덱스를 구할 수 있다.

----------------------



### *1's complement*

> 2진수의 음수를 나타내기 위한 방법으로 1 -> 0, 0 -> 1 의 방식.
>
> 0이 2개가 생기는 문제점이 발생.

### *2's complement*

> 문제점을 가진 1's complement를 보완하기 위해 만들어진 방식으로 1's complement에 1을 더해주어 0이 2개가 생기는 것을 방지하였다.

----------------

### Fenwick tree의 공간복잡도

> O(N)

### Finally

> Segment tree는 구간의 누적 뿐 아니라 특정 구간의 최대, 최소를 구할 수 있지만 
>
> Fenwick tree는 이 부분에 있어서 제한적이다.
>
> Fenwick tree는 Segment tree를 비트로 변화시키고 불필요한 노드를 제거함으로 메모리를 절약시켰다.



