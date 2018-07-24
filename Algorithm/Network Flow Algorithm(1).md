# Network Flow Algorithm(1)

> 네트워크 유량 알고리즘은 그래프의 간선의 cost 대신 용량(capacity)이라는 개념이 접합된 알고리즘이다. 
>
> 두  정점 (u, v)를 잇는 간선이 주어졌을 때, 간선의 용량 이하만큼의 유량을 흘려보낸다. 또한 그래프에 서로 다른 정점인 Source와 Sink 정점이 정해지고, Source에서 Sink로 유량을 최대한 많이 보내는 것을 목표로 한다. 
>
> 최초에 유량을 발생시켜 다른 정점으로 보내는 것은 Source 뿐이며 Sink를 제외한 이외의 정점들은 받은 만큼의 유량만 보낼 수 있다.



### 용어 정리

* Source : 시작 정점
* Sink : 도착 정점
* c(u, v) : 정점 u에서 v로 보낼 수 있는 용량
* f(u, v) : 정점 u에서 v로 흐르는 유량
* r(u, v) : 정점 u에서 v로 보낼 수 있는 잔여 용량  
  * r(u, v) = c(u, v) - f(u, v)

### 유량 그래프의 성질

1. 특정 경로를 따라 유량을 보낼 때는 그 경로에 포함된 간선 중 가장 용량이 작은 간선에 의해 결정된다. (Bottleneck)
2. 유량은 용량을 넘을 수 없다. (f(u, v) <= c(u, v))
3. f(u, v) = -f(u, v) 
4. Source와 Sink를 제외한 모든 정점의 들어오는 유량과 나가는 유량의 합은 같다.



> 네트워크 유량 알고리즘을 풀기 위한 방식으로 포드 풀커슨(Ford-Fulkerson) 알고리즘과 에드몬드 카프(Edmonds-Karp) 알고리즘이 있다. 두 알고리즘을 비교하여 다룰 것이다.

#### 1. 포드 풀커슨 알고리즘

> 포드 풀커슨 알고리즘은 최초의 네트워크 유량 알고리즘으로 개념과 구현이 간단한 알고리즘이다.
>
> 네트워크의 모든 간선의 유량을 0으로 시작하여, Source에서 Sink로 보낼 수 있는 경로를 찾아 유량을 보내는 것을 반복하는 방식이다. 이 방식에서 경로를 찾는 방식은 DFS이다.



#### 2. 애드몬드 카프 알고리즘

> 포드 풀커슨 알고리즘과 같이 네트워크 유량 문제를 풀기위한 알고리즘으로 포드 풀커슨 알고리즘의 방식인 DFS의 해가 없는 경로에 깊게 빠질 수 있는 문제를 해결할 수 있는 알고리즘이다.
>
> 이 알고리즘은 경로를 찾을 때 BFS 방식을 사용하는데 BFS는 증가 경로를 VE번 이상 찾지 않는다는 것이 증명되었으므로 네트워크 유량 문제는 BFS를 사용한 애드몬드 카프알고리즘을 사용하는 것이 좋다.
>
> * 시간복잡도 : O(VE<sup>2</sup>)
>   - 증가 경로를 찾는 과정 O(E) 
>   - Sink로 도달하는 증가경로를 찾는 과정 O(VE)
>
> ```
> #include<cstdio>
> #include<vector>
> #include<queue>
> #include<algorithm>
> 
> #define MAX 2002
> #define S 0
> #define T n + m + 1
> using namespace std;
> 
> int c[MAX][MAX];
> int n, m;
> int num;
> vector<vector<int> > adj;
> queue<int> q;
> int main()
> {
> 	scanf("%d %d", &n, &m);
> 	adj.resize(T + 1);
> 	for (int i = 1; i <= n; ++i) {
> 		scanf("%d", &num);
> 		for (int j = 1; j <= num; ++j) {
> 			int a;
> 			scanf("%d", &a);
> 			adj[i].push_back(n + a);
> 			adj[n + a].push_back(i);
> 			c[i][n + a] = 1;
> 		}
> 	}
> 	for (int i = 1; i <= n; ++i) {
> 		c[S][i] = 2;
> 		adj[S].push_back(i);
> 		adj[i].push_back(S);
> 	}
> 	for (int i = n + 1; i <= n + m; ++i) {
> 		c[i][T] = 1;
> 		adj[i].push_back(T);
> 		adj[T].push_back(i);
> 	}
> 	int ans = 0;
> 	while (1) {
> 		int prev[MAX];
> 
> 		fill(prev, prev + T + 1, -1);
> 		q.push(S);
> 		while (!q.empty()) {
> 			int here = q.front();
> 			q.pop();
> 			for (int next : adj[here]) {
> 				//여기서 prev는 visit 과 같은 역할을 한다.
> 				if (c[here][next] > 0 && prev[next] == -1) {
> 					q.push(next);
> 					prev[next] = here;
> 					if (next == T)
> 						break;
> 				}
> 			}
> 		}
> 		if (prev[T] == -1)
> 			break;
> 		ans++;
> 		for (int i = T; i != S; i = prev[i]) {
> 			c[prev[i]][i]--;
> 			c[i][prev[i]]++;
> 		}
> 	}
> 	printf("%d\n",ans);
> }
> ```
>
> * BOJ 11376(열혈강호2) 문제 