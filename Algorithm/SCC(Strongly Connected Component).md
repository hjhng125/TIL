# SCC(Strongly Connected Component)

> SCC란 방향 그래프에서 임의의 두 정점 u, v가 있을 때, u -> v 로 가는 경로가 존재한다면 그 그룹은 SCC이다. 이 때의 경로는 직/ 간접적 경로를 모두 포함한다.



### SCC의 특징

>1.  같은 SCC 내에서 뽑은 임의의 점 u, v에서 u -> v 혹은 v -> u 로 가는 경로는 항상 존재한다.
>
>2.  서로 다른 SCC에서 뽑은 임의의 점 u, v에서의 양방향 경로는 항상 존재하지 않는다. 
>
>   -> 서로 다른 SCC 끼리는 사이클이 존재하지 않는다.
>
>3.  SCC는 Maximal한 성질을 가지고 있어서 SCC가 형성된다면 형성될 수 있는 가장 큰 집합으로 형성된다.



### Source code & Sequence

>```
>int v, e, cnt, sccNum;
>vector<vector<int> > adj, organization;
>vector<int> dfsn, finished, sn;
>stack<int> s;
>int dfs(int cur) {
>	dfsn[cur] = ++cnt;
>	s.push(cur);
>	int result = dfsn[cur];
>	for (auto next : adj[cur]) {
>		if (!dfsn[next]) {
>			result = min(result, dfs(next));
>		}
>		else if (!finished[next]) {
>			result = min(result, dfsn[next]);
>		}
>	}
>	if (result == dfsn[cur]) {
>		vector<int> curScc;
>		while (1) {
>			int t = s.top();
>			s.pop();
>			curScc.push_back(t);
>			finished[t] = 1;
>			sn[t] = sccNum;
>
>			if (t == cur)
>				break;
>		}
>		sort(curScc.begin(), curScc.end());
>		organization.push_back(curScc);
>		sccNum++;
>	}
>	return result;
>}
>```
>
>1. 임의의 시작점으로부터 인접한 방문하지 않은 정점들을 DFS로 확인하며 dfsn이라는 배열로써 방문 순서를 입력한다.
>
>2. 더 이상 방문하지 않은 정점이 없다면 방문한 정점들 중 아직 SCC로 추출되지 않은 이웃을 확인한다.
>
>3. 자신이나 자신의 자손 정점들이 도달 가능한 제일 높은 정점이 자신일 경우 SCC로 추출한다. (같은 SCC안에 있음을 의미함) 
>
>   이 과정에서 스택안에 push했던 정점들을 자기 자신이 나올때까지 pop하며, 추출된 정점들이 같은 SCC에 존재함을 finished라는 배열에 체크해주며 sn이라는 배열로써 몇번째 SCC인지 체크한다.
>
>* curScc 배열에는 현재 확인 중이 SCC의 요소들이 들어있다.
>* organization 배열에는 전체 SCC의 요소들이 들어있다.
>* SccNum은 SCC의 개수를 의미한다.



