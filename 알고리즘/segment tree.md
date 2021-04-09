# Segment Tree

배열의 특정구간의 합(예 : sum(A[N:M]))을 구할 때 쓰입니다. 배열의 변경이 많이 일어날 때 유용합니다.
  
### 시간복잡도
구간의 합 : O(logN)
수정 : O(logN)
</br>
예시)
A = [1,9,6,3,5,7,3]
![기본트리](https://user-images.githubusercontent.com/68533862/114152032-ba7f2a80-9958-11eb-8ea7-34d51b21906b.jpg)  
</br>

## 규칙
1. leaf Node 는 배열(A)의 원소입니다.
2. 트리의 각 노드는 자식 노드의 합입니다.
3. Full Binary Tree 입니다.(각 노드의 자식의 수는 0또는 2입니다)
</br>

## 구현
1. Tree는 배열로 구현합니다.
2. root node 는 Tree[1] 입니다 (Tree[0]을 root node로 하면 3번에서 자식노드가 Tree[0], Tree[1]이 되기 때문에 노드가 겹칩니다)
3. Tree[N]의 자식 노드는 각각 Tree[2N], Tree[2N+1] 입니다.
</br>

### 총 노드의 수
1. N=1 일때 1개
2. N이 1 커질때 추가적으로 필요한 노드의 수는 2개씩 늘어남
3. 따라서 총 노드의 수는 2*N-1
4. 하지만 Tree를 배열로 만들었을 때 비어있는 인덱스가 존재하기 때문에 Perfect Binary Tree가 아닐때는 공간을 더 할당해야 한다.
5. Perfect Binary Tree : Full Binary Tree에서 각 리프에서 높이까지 depth가 모두 같은 노드, Segment Tree 가 Perfect Binary Tree가 될 조건은 N = 2<sup>H</sup>
</br>

```python
import math

A = [1, 9, 6, 3, 5, 7, 3]
l = (int)(math.log(len(A), 2))
Tree = [0] * 2**(l+2)

# 참고 : 위 코드는 Perfect Binary Tree 일때 Tree의 배열의 크기가 2배 더 크게 할당됩니다.

def makeTree(start, end, node):
    # start 인덱스 부터 end 인덱스 까지의 합을 Tree[node] 에 저장
    if start == end:    # leaf node
        Tree[node] = A[start]
        return Tree[node]

    mid = (start+end)//2
    Tree[node] = makeTree(start, mid, 2*node) + makeTree(mid+1, end, 2*node+1)
    # left child : Tree[2*node], right child : Tree[2*node+1]
    return Tree[node]


Tree[root] = makeTree(0, len(A)-1, 1)  # maketree(A배열의 시작 인덱스, A배열의 끝 인덱스, rootnode)
# 또는 makeTree(0, len(A)-1, 1)

print(Tree)

```
</br>

## 구간합 구하기

전체 트리를 순환하며 합을 찾습니다
예제 : sum(A[0:4])


![구간합트리](https://user-images.githubusercontent.com/68533862/114152075-cc60cd80-9958-11eb-99cc-bb5b74716c4d.jpg)

```python
def find(start, end, left, right, node):
    if left > end or start > right:  # 범위 밖
        return 0

    if left <= start and end <= right:
        return Tree[node]
        # 예제에서 [start, end] = [0,3] 그리고 [start, end] = [4,4] 일때 return 됨

    mid = (start + end) // 2
    return find(start, mid, left, right, 2*node) + find(mid+1, end, left, right, 2*node + 1)


# sum(A[0:4]) 구하기
print(find(0, len(A)-1, 0, 4, 1))
```
</br>

## 수정
트리를 순환하며 수정될 원소를 자손으로 가지고 있는 노드를 모두 변경해줍니다

![트리수정](https://user-images.githubusercontent.com/68533862/114152169-ea2e3280-9958-11eb-995f-932905927ce2.jpg)


```python
def update(start, end, index, diff, node):
    if start > index or index > end:
        return

    Tree[node] += diff

    if start == end:
        return

    mid = (start + end) // 2
    update(start, mid, index, diff, 2*node)
    update(mid+1, end, index, diff, 2*node + 1)


# A = [1, 9, 6, 3, 5, 7, 3] => [1, 9, 6, 3, 5, 5, 3]
# A[5] 수정, diff = -2
update(0, len(A)-1, 5, -2, 1)
print(Tree)

```
</br>
</br>

# 추가자료
### prefix sum
배열의 값이 수정되지 않는다면 Prefix Sum 으로 구간합을 구하는 것이 더 좋은 방법입니다.

구간합 : O(1)  
삽입 : O(1)  
수정 : O(N)  

예시)
A = [1,9,6,3,5,7,3]
prefix_sum_A = [0,1,10,16,19,24,31,34]



```python
A = [1, 9, 6, 3, 5, 7, 3]


def find_prefix_sum_A(arr):
    prefix_sum_arr = [0] * (len(arr)+1)
    for i in range(1, len(arr)+1):
        prefix_sum_arr[i] = prefix_sum_arr[i-1] + arr[i-1]
    return prefix_sum_arr


prefix_sum_A = find_prefix_sum_A(A)

# sum(A[n:m]) = prefix_sum_A[m+1] - prefix_sum_A[n]

# sum(A[0:4]) 구하기
print(prefix_sum_A[5] - prefix_sum_A[0])


def prefix_sum_insert(prefix_sum_arr, inserted):
    prefix_sum_arr.append(prefix_sum_arr[-1] + inserted)


# A = [1, 9, 6, 3, 5, 7, 3] => A = [1, 9, 6, 3, 5, 7, 3, 12]
A.append(12)
prefix_sum_insert(prefix_sum_A, 12)
print(prefix_sum_A)


def prefix_sum_update(prefix_sum_arr, index, diff):
    for i in range(index+1, len(prefix_sum_arr)):
        prefix_sum_arr[i] += diff


# A = [1, 9, 6, 3, 5, 7, 3, 12] => A = [1, 909, 6, 3, 5, 7, 3, 12]
diff = 900
A[1] += diff
prefix_sum_update(prefix_sum_A, 1, diff)
print(prefix_sum_A)
```
