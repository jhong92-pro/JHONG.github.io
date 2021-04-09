# Segment Tree

배열의 특정구간의 합(sum(A[N:M]))을 구할 때 쓰입니다.

### 시간복잡도
구간의 합 : O(logN)
수정 : O(logN)


예시)
A = [1,9,6,3,5,7,3]
![프레젠테이션2](https://user-images.githubusercontent.com/68533862/114138740-075b0500-9949-11eb-9bdc-a06ead14e944.jpg)


### 규칙
1. leaf Node 는 배열(A)의 원소입니다.
2. 트리의 각 노드는 자식 노드의 합입니다.
3. Full Binary Tree 입니다.(각 노드의 자식의 수는 0또는 2입니다)

### 구현
1. Tree는 배열로 구현합니다.
2. root node 는 Tree[1] 입니다 (Tree[0]을 root node로 하면 3번에서 자식노드가 Tree[0], Tree[1]이 되기 때문에 1을 root로 해야합니다)
3. Tree[N]의 자식 노드는 각각 Tree[2N], Tree[2N+1] 입니다.

![tree](https://user-images.githubusercontent.com/68533862/114124368-454b2f80-992f-11eb-87dd-715986f285f8.JPG)


### 총 노드의 수
1. N=1 일때 1개
2. N이 1 커질때 추가적으로 필요한 노드의 수는 2개씩 늘어남
3. 따라서 총 노드의 수는 2*N-1
4. 하지만 Tree를 배열로 만들었을 때 비어있는 인덱스가 존재하기 때문에 Perfect Binary Tree가 아닐때는 공간을 더 할당해야 한다.
5. Perfect Binary Tree : Full Binary Tree에서 각 루트에서 리프까지 높이가 같은 노드, Segment Tree 가 Perfect Binary Tree가 될 조건은 N = 2<sup>H</sup>


```python
import math

A = [1, 9, 6, 3, 5, 7, 3]
l = (int)(math.log(len(A), 2))
Tree = [0] * 2**(l+2)

# 참고 : 위 코드는 Perfect Binary Tree 일때 Tree의 배열의 크기가 2배 더 크게 할당됩니다.

def makeTree(start, end, node):
    # start 인덱스 부터 end 인덱스 까지의 합을 Tree[node] 에 저장
    print(node)
    if start == end:    # leaf node
        Tree[node] = A[start]
        return Tree[node]

    mid = (start+end)//2
    Tree[node] = makeTree(start, mid, 2*node) + makeTree(mid+1, end, 2*node+1)
    # left child : Tree[2*node], right child : Tree[2*node+1]
    return Tree[node]


root = 1
Tree[root] = makeTree(0, len(A)-1, root)
# 또는 makeTree(0, len(A)-1, root)

print(Tree)

```

### 합찾기
트리를 순환하며 합을 찾습니다


