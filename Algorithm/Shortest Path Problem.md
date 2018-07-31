# Shortest Path Problem

>가장 짧은 경로에서 두 정점을 찾는 문제로, 가중치의 합이 최소가 되도록 하는 경로를 찾는 것.
>
>* Single-source shortest path : 
>
>  단일 노드(u)에서 출발하여 그래프 내의 모든 다른 노드에 도착하는 가장 짧은 경로를 찾는 문제
>
>* Single-destination shortest path :
>
>  모든 노드들로부터 출발하여 그래프 내의 한 단일 노드(v)로 도착하는 가장 짧은 경로를 찾는 문제
>
>  역으로 생각할 경우 single-source shortest path와 동일함.
>
>* Single-pair shortest path : 
>
>  주어진 정점 (u, v)에 대해 최단 경로를 찾는 문제
>
>* All-pair shortest path :
>
>  그래프 내의 모든 노드 쌍들 사이의 최단 경로를 찾는 문제

### 1. Dijkstra algorithm

> 다익스트라 알고리즘은 단일 출발 최단 경로 문제를 푸는데 적합한 알고리즘
>
> 그래프 내에 음의 간선이 존재할 경우 적합하지 않다.
>
> 음의 간선이 없는 그래프에서 단일 출발 최단 경로 알고리즘 중 가장 빠른 알고리즘이다.
>
> * 시간 복잡도 : O(|E|+|V|<sup>2</sup> ) = O(|V|<sup>2</sup>)
>
> * Min heap 구현 : O((|E|+|V|)log|V|)
>
> * 소스코드
>
>   ```
>   while (!pq.empty()) {
>   		int x = pq.top().x;
>   		int y = pq.top().y;
>   		int cnt = pq.top().cnt;
>   		pq.pop();
>   		for (int k = 0; k < 4; ++k) {
>   			int nexty = y + dy[k];
>   			int nextx = x + dx[k];
>   			
>   			if ((nexty >= 1 && nexty <= n) && (nextx >= 1 && nextx <= m)) {
>   				if (dist[nexty][nextx] > dist[y][x] + miro[nexty][nextx]) {
>   					dist[nexty][nextx] = dist[y][x] + miro[nexty][nextx];
>   					pq.push({nextx, nexty, dist[nexty][nextx]});
>   				}
>   			}
>   		}
>   ```
>
>    <sub>BOJ1261문제</sub>
>
>   
>
> * *Dijkstra algorithm의 정당성에 대한 증명*
>
>   * 귀류법에 의한 증명
>
>   3번점을 방문하지 않은 상태고 다음 방문 할 점이 3번점이라고 가정해보자.
>
>   이 말은 곧 방문 하지 않은 점 중 cost배열의 값이 3번이 가장 작다는 말이다.
>
>   그런데 cost배열에 있는 3번점의 값이 최소가 아니라면 어떨까 생각해보자.
>
>   만약 cost배열에 있는 3번점의 값이 최소가 아니라면 3번점을 가기위해선 다른점을 거쳐서 3번점을 가야하고 그 점을 4번점이라고 하면 4번점의 cost의 값이 3번점 보다 작아야된다.
>
>   하지만 아직 방문하지 않은 점중 3번 점의 cost값이 가장 작다고 했지만 4번점의 cost값이 3번점 보다 작다는 것은 말이 안된다.
>
>   따라서 다익스트라는 모든 경우에 최단거리라고 말할수 있다. 물론 앞서 언급했듯이 가중치의 값이 음수인 경우가 있으면 위의 증명은 틀리게 된다. 따라서 다익스트라는 가중치가 양수일 경우만 성립한다.



### 2. Bellman-Ford algorithm

> 다익스트라 알고리즘과 마찬가지로 단일 출발 최단 경로를 푸는데 적합한 알고리즘
>
> 그래프 내의 음의 간선이 존재할 경우에 가장 적합하지만 다익스트라보다 시간 복잡도가 높기 때문에 어떤 상황에서 이용해야 할 지 잘 생각하여야 한다.
>
> 최단 경로는 Cycle을 포함하지 않기 때문에 V-1개의 간선만 사용한다.
>
> 음의 간선으로 인하여 무한적으로 경로를 업데이트하는 경우 반복문을 탈출하여야 한다.
>
> * 시간복잡도 : O(|V||E|), |E|는 최대 |V|<sup>2</sup>
>
> * 공간복잡고 : O(|V|) -> 각 정점마다 최소 거리 저장
>
> * 소스코드
>
>   ```
>   bool flag = true;
>   	int cnt = n;
>   	while (flag && cnt != 0) {
>   		flag = false;
>   		for (int i = 0; i < n; ++i) {
>   			for (int j = 0; j < adj[i].size(); ++j) {
>   				int there = adj[i][j].first;
>   				if (dist[there] < adj[i][j].second + dist[i] + profit[there]) {
>   					dist[there] = adj[i][j].second + dist[i] + profit[there];
>   					flag = true;
>   				}
>   			}
>   		}
>   		cnt--;
>   	}
>   ```



### 3. Floyd-Warshall algorithm

> 플로이드 워셜 알고리즘은 위의 두 알고리즘과 다르게 모든 정점에 대해 모든 다른 정점으로의 최단 경로를 구할 때 적합한 알고리즘이다. 모든 정점이 다른 모든 정점으로 가는 최단 경로를 계산하기 때문에 많은 시간이 소요된다.
>
> 음의 간선 또한 Cycle이 없다면 잘 처리할 수 있다.
>
> * 시간복잡도 : O(|V|<sup>3</sup>) -> 매번 모든 노드들의 조합에 대해 현재까지의 최단경로를 구해야한다. 
>
> * 소스코드
>
>   ```
>   for (int k = 1; k < v; ++k) {
>   		for (int i = 1; i <= v; ++i) {
>   			for (int j = 1; j <= v; ++j) 
>   				adj_table[i][j] = min(adj_table[i][j], adj_table[i][k] + adj_table[k][j]);
>   		}
>   	}
>   ```