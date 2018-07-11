# Minimum Spanning Tree (MST)

> MST란 그래프에서 모든 정점을 연결하되, Cycle이 존재하지 않도록 모든 점정을 간선으로 연결하는 것을 의미한다.
>
> 이때, 간선의 가중치의 합은 최소로 연결되어야 하며 MST는 그래프에서 하나 이상일 수 있다.



### 1. Prim 알고리즘

> Prim 알고리즘은 하나의 시작점(Source)를 잡고 이와 연결된 정점들의 가중치를 작은 간선부터 연결해 가며 MST를 만드는 알고리즘.
>
> 중요한 것은 가장 작은 간선을 연결하되, Cycle이 생긴다면 가중치가 가장 작더라도 무시하여야 한다.
>
> - 시간 복잡도 : O(ElogV)
>
>   Priority queue로 모든 정점을 확인 : O(logV)
>
>   그 정점들에 대해 간선을 확인 : O(E)
>
>   
>
>   * *prim 알고리즘 소스 코드*
>
>   ```
>   while (!q.empty()) {
>   	int here = q.top().second;
>   	int here_cost = q.top().first;
>   	q.pop();
>   
>   	if (visit[here]) continue;
>   	
>   	ans += here_cost;
>   	visit[here] = 1;
>   	for (int i = 0; i < MST[here].size(); ++i) {
>   		int there = MST[here][i].second;
>   		int there_cost = MST[here][i].first;
>   		if (!visit[there])
>   			q.push({ there_cost, there });
>   	}
>   }
>   ```



### 2. Kruskal's 알고리즘

> Kruskal's 알고리즘은 모든 간선에 대해 가장 가중치가 작은 간선부터 연결해 주며, MST를 만드는 알고리즘.
>
> 중요한 것은 가장 작은 간선을 연결하되, 연결 도중 Cycle이 생긴다면 가중치가 작더라도 무시하여야 한다.
>
> 사이클을 확인하는 방법으로는 Union-Find(Disjoint-set)이 있다.
>
> * Union-Find(Disjoint-set) : Cycle 판별을 위해 두 정점이 같은 root를 갖는지 확인하는 것
>
> * 시간 복잡도 : O(ElogE)
>
>   Initialize : O(1)
>
>   Sort : O(ElogE)  -> 간선 E개 정렬, priority_queue
>
>   
>
>   * *Find set 소스 코드*
>
>   ```
>   int find_set(int i) {
>   	if (parent[i] == i)
>   		return i;
>   	else
>   		return parent[i] = find_set(parent[i]);
>   }
>   ```
>
>   * *Union set 소스 코드*
>
>   ```
>   bool union_set(int a, int b) {
>   	a = find_set(a);
>   	b = find_set(b);
>   
>   	if (a == b) 
>   		return false;
>   
>   	parent[a] = b;
>   	return true;
>   }
>   ```



### Prim vs Kruskal's

> Prim 알고리즘과 Kruskal's 알고리즘을 비교하자면,  정점을 기준으로 MST를 만드는지, 간선을 기준으로 MST를 만드는지에 달려 있다.
>
> 따라서  간선의 개수가 작은 경우엔 Kruskal's 알고리즘을, 간선의 개수가 많은 경우엔 Prim 알고리즘이 적합하다고 볼 수 있다.