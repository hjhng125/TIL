#Segment tree

######주어진 query에 대해 빠르게 응답하기 위한 자료구조

##특징 ######1. 트리구조로 O(n)의 연산을 O(lgN)만에 할 수 있다. ######2. 주어진 배열의 구간을 이진트리로 표현 ######3. 각 노드는 다음과 같은 의미를 가진다. * 리프노드 : 배열의 그 수 * 부모노드 : 자식노드의 합 ######4. Heap 자료구조와 마찬가지로 일차원 배열로 표현 가능하다. * Root 노드 : 1 * 노드 i의 왼쪽 child : 2i * 노드 i의 오른쪽 child : 2i+1 * 완전 이진 트리

#Segment tree의 크기 * 2^(h+1)-1

###segment tree 초기화 code`
void init(arr,  tree , node, start, end) {
    if (start == end)
        return tree[node] = arr[start];
    int mid = (start + end) / 2;
    return tree[node] = init(arr, tree, node * 2, start, mid) + 
    init(arr, tree, node * 2 + 1, mid + 1, end);
}
` ###segment tree 구간 합`
ll sum(vector<ll> &tree, int node, int start, int end, int left, int right)
{
    if (left > end || right < start)
        return 0; 
    if (left <= start && end <= right)
        return tree[node];
 
    int mid = (start + end) / 2;
    return sum(tree, node * 2, start, mid, left, right) + sum(tree, node*2+1, mid+1, end, left, right);
}
` ###segment tree 업데이트`
void update( tree, node, start, end, index,  diff)
{
    if (!(start <= index && index <= end))
        return;
    tree[node] += diff;
    if(start != end)
    {
        int mid = (start + end) / 2;
        update(tree, node * 2, start, mid, index, diff);
        update(tree, node * 2 + 1, mid + 1, end, index, diff);
    }
}
` ###segment tree 시간복잡도 ######update : O(logN) ######sum : O(logN) ######M번 수행 시 : O(MlogN) ###segment tree 공간복잡도

###### O(NlogN)
