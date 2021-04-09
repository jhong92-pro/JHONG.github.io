### Segment Tree

배열의 특정구간의 합을 구할 때 쓰입니다.

시간복잡도
합 구할때 : O(logN)
수정 : O(logN)


예시)
A = [1,6,3,5,7,8,1]

![segment tree](https://user-images.githubusercontent.com/68533862/114123332-0d42ed00-992d-11eb-851b-1bfbfae920e5.jpg)


규칙
1. leaf Node 는 배열(A)의 원소입니다.
2. 트리의 각 노드는 자식 노드의 합입니다.
3. Full Binary Tree 입니다.(각 노드의 자식의 수는 0또는 2입니다)

구현
1. Tree는 배열로 구현합니다.
2. Tree[N]의 자식 노드는 각각 Tree[2N], Tree[2N+1] 입니다.

![tree](https://user-images.githubusercontent.com/68533862/114124368-454b2f80-992f-11eb-87dd-715986f285f8.JPG)


공간복잡도
1. len(A) == [리프노드의수] 이기 때문에 len(A)가 제곱수라면 Tree는 Perfect Binary Tree입니다(각 리프노드부터 루트까지 depth가 동일)
2. Perfect Binary Tree 일때 필요한 노드의 수는 2*len(A)+1
3. len(A)이 제곱수가 아닌 경우에는 높이가 H = [lgN], 필요한 노드의 수는 2^(H+1)-1 입니다

