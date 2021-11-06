# 자료구조 총 정리
모든 자료구조의 data수는 n개라는 가정하에 정리
# Linked List, Double Linked List
-	data 정보와 다음 Node or 이전 Node를 가리키는 포인터를 지니고 있는 객체
-	Memory 낭비가 있다. (다음 or 이전 Node를 가리키는 Pointer를 위한 Memory)
장점: Element Insertion, Deletion에 있어 O(1) time에 수행이 가능하다. -> 빠름
단점: Element Access에 있어 O(n) time에 수행한다.
     배열은 Index로의 접근 O(1) time에 수행한다.

# Stack, Queue
Stack: 후입선출 – 가장 늦게 들어온 data가 가장 먼저 나온다. -> DFS에 활용 가능하다.
Queue: 선입선출 – 가장 먼저 들어온 data가 가장 먼저 나온다. -> BFS에 활용 가능하다.

# Sort
정렬의 기준은 오름차순으로 생각하겠습니다.
# Selection Sort
0번째 index의 값을 1,2,.... n-1번째 값까지 비교해 나가면서 가장 작은 값을 찾는다.
배열의 끝까지 도달했을 때 그 찾은 가장 작은 값을 0번째 index와 swap 해준다. 반복
n-1번 비교 * n-2번 비교 * .... * 1번 비교 = O(n!) = O(n^2) time에 수행된다
공간 복잡도의 경우 n칸의 배열에서만 진행되기 때문에 O(n) Space이다.

# Insertion Sort
1번째 index에서 시작한다. 1번째 index의 값을 0번째 index의 값과 비교하여 1번째 index 값이 작다면 0번째 index와 swap 해준다.
그리고 2번째 index는 1번째 index와 비교 작으면 swap
2번째 index에는 원래 1번째 index였던 것이 저장되어 있고 1번째 index에는 원래 2번째 index였던 것이 저장되어 있던 상태로 바뀐다. (swap 진행)
그리고 나서 바뀐 1번째 index와 0번째 index를 비교하여 작으면 swap을 진행해준다.
swap은 O(1) time이지만 이 또한 (n-1)!의 비교를 수행해야하기 때문에 O(n^2) time에 수행된다
공간 복잡도의 경우 n칸의 배열에서만 진행되기 때문에 O(n) space이다.

# Bubble Sort
0번째 index와 1번째 index 비교하여 swap 진행 / 1번째 index와 2번째 index 비교해서 swap
n-2번째 index와 n-1번째 index와 비교하여 swap 진행 => n-1번째 index에는 최대값이 저장
그럼 다시 0번째 index와 1번째 index 비교하여 swap 진행, 쭉 이어서 진행하다
n-3번째 index와 n-2번째 index 비교하여 swap 진행 -> n-2번쨰 index에는 2등이 저장
n-1 * n-2 * ... * 1 = O(n^2)의 time에 수행된다.
당연히 공간 복잡도도 O(n) space이다.

# Merge Sort
1. 현재 배열을 반으로 쪼갠다. 기준점은 (0 + n-1) / 2 로 기준점을 잡는다.
2. 이를 배열의 크기가 쪼갤 수 없을 때까지(보통 1) 쪼갠다.
3. 2칸짜리 배열을 만드는데 쪼개진 1번째 배열과 2번째 배열 (지금은 1칸짜리)을 비교한다.
2칸짜리 정렬된 배열이 완성되었다. 나머지 1칸짜리 배열에 대해서도 인접한 배열과 비교한다.
n / 2 개의 배열이 완성되었다. (2칸짜리) [n이 홀수인 경우는 n-1번째 배열과 다음에 합친다.]
2칸짜리 정렬된 배열끼리 비교하는데 a, b 배열이 있다고 생각해보자
a의 0번째 index값과 b의 0번째 index값을 비교해서 4칸짜리 배열의 0번째 index에 넣는다.
a의 0번째 index값이 들어갔다면 a의 1번째 index와 b의 0번째 index값을 비교한다. 그리고
b의 0번째 index값이 들어갔다면 a의 1번째 index와 b의 1번째 index를 비교한다.
이와 같은 방식으로 n칸짜리 배열이 될 때까지 반복을 해주면 된다.
n/2, n/4, n/8 ... 1이 될 때까지 분할하기 때문에 Logn번의 분할과정이 생긴다. 
분할된 n개의 배열들이 서로 비교하기 때문에 O(Logn * n) time에 수행된다.

# Quick Sort
Pivot Point라는 기준점이 되는 값을 하나 설정한다. -> Random하게 잡으면 된다.
그래서 정해진 Pivot Point를 기준으로 작은 값은 왼쪽, 큰 값은 오른쪽으로 옮긴다.
세분화해서 보자면 Pivot Point를 기준으로 리스트를 둘로 분할한다고 생각하자.
K번째 index인 Pivot Point 왼쪽의 배열의 0번째 index를 left라 하고
오른쪽 배열의 마지막(n-1)번째 index를 right로 한다.
그리고 Right부터 시작해서 Right가 Pivot보다 작으면 Left와 swap 해준다.
그리고 Right를 n-2번쨰 index로 update해주고 Left를 1번째 index로 update 해준다.
이런 식으로 한무 반복을 하여 left와 right가 pivot이 되면 pivot 기준으로 왼쪽 배열에서 pivot을 정해줘서 똑같은 식으로 하고 오른쪽 배열에서도 pivot을 정해줘서 똑같은 방식으로 동작시키면 결국에 정렬이 완성이 된다.
시간 복잡도는 O(nlogn) time에 수행되고 공간 복잡도는 O(n) space이다.

Heap Sort
우선 Heap에 대해서 알아야할 필요가 있다. 그리고 BT에 대해 알아야할 필요가 있다. 그리고 Tree에 대해 알아야할 필요가 있다. 한무로 알아야 되는 것 같지만 한번 알아보자

# Tree
-	임의의 Node에서 다른 Node로 가는 경로는 유일하다.
-	Cycle이 존재하지 않는다.
-	모든 Node는 연결됨 == 연결 안된 Node는 또 하나의 Tree이다.
# Binary Tree
-	자식 노드가 최대 2개로 구성된 Tree이다.

일단 이 정도만 알아 두고 Heap에 대해 정말로 알아보자.

# Heap
BCT(Binary Complete Tree)의 일종으로 우선순위 큐를 위하여 만들어진 자료구조
여러 개의 값들 중 최댓값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조
1. Max Heap(최대 힙)
- 부모 노드의 Key 값이 자식 노드의 Key 값보다 크거나 같은 BCT
2. Min Heap(최소 힙)
- 부모 노드의 Key 값이 자식 노드의 Key 값보다 작거나 같은 BCT

이것이 중요한 것이 아니라 Heap에 대해서 Deletion, Insertion을 하는 과정이 중요하다.
1. Insertion
-> 새로운 Node를 Heap의 Leaf Node 자리인 맨 마지막 Node에 이어서 삽입한다.
-> 새로운 Node를 부모 Node들과 비교를 통해 Swap 한다.
== 부모 Node랑만 계속해서 비교해 나가면 됨

2. Deletion
-> Max Heap 기준으로 최대 값을 가진 Node를 삭제해야 하므로 Root Node를 삭제한다.
-> 삭제된 곳에 현재 Heap의 Leaf Node 중 맨 마지막 Node를 가져온다.
-> 이제 Heap을 재구성해야 되는데 자식 Node들이랑 비교를 통해서 재구성한다.
-> 자식 노드 2개 중 큰 값과 Swap을 해주는데 자식 Node가 자기보다 작아질 때까지 수행
-> 재구성 완료

# Heap Sort: 그럼 이제 진짜로 Heap Sort에 대해 알아보자
내림차순 정렬 -> Max Heap
오름차순 정렬 -> Min Heap
기준으로 생각하고 Max Heap에서 Tree의 Node 개수만큼 Deletion 연산 수행하여 순서대로 배열에 저장하게 되면 그 배열이 내림차순 정렬이 된 상태이다.
똑같이 Min Heap에서 Tree의 node 개수만큼 Deletion 연산 수행하여 순서대로 배열에 저장하게 되면 그 배열은 오름차순 정렬이 된 상태이다.
== n개의 Node를 가진 Tree를 n번 Deletion 해야 하기 때문에
	deletion후 Root Node에서 Leaf Node까지 내려가는데 O(Logn) time에 수행되기 때문
O(nLogn) time에 수행이 된다. 공간 복잡도는 O(n) space이다.

# BST(Binary Search Tree) 
-	이진탐색과 연결리스트를 결합한 자료구조의 일종
-	이진탐색의 효율적인 탐색 능력 + 빈번한 자료 삭제, 입력이 가능하게끔
-	각 Node의 왼쪽 서브 트리에는 해당 노드의 값보다 작은 값을 지닌 노드들만
-	각 Node의 오른쪽 서브 트리에는 해당 노드의 값보다 큰 값을 지닌 노드들만
-	중복된 Node가 있으면 X, 왼쪽, 오른쪽 서브 트리 또한 BST이다.

BST의 Insertion
반드시 LeafNode에서 이루어진다.
Root Node에서 LeafNode까지 찾아가는데 O(logn) | 삽입하는데 O(1) time => O(logn) time
순회 시에는 왼쪽 서브 트리 -> Node -> 오른쪽 서브 트리 순으로 순회를 돌면 정렬된 순서

BST의 Deletion
1. 삭제하려는 Node의 자식이 없을 경우 == Leaf Node
-> 그냥 삭제하면 되므로 O(1) time에 수행 가능하다.
2. 삭제하려는 Node의 자식이 1개인 경우
-> 삭제하려는 Node의 자식과 삭제하려는 Node의 부모를 그냥 연결시켜주면 됨
-> 삭제하려는 Node까지 가는데 O(logn) time에 수행, 삭제 연결 O(1) time에 수행 = O(logn)
3. 삭제하려는 Node의 자식이 2개인 경우
-> 삭제하려는 Node의 레벨이 d일때 삭제 대상의 오른쪽 서브트리 찾기 O(d) time
-> 삭제 대상의 오른쪽 서브트리 높이에 해당하는 비교 연산 O(h-d) time
-> 삭제하기 복사하기 O(1) time
=> O(h-d+d)time => O(h) time

4. 가장 최악의 경우가 있는데 오른쪽 노드로 1열로 쭉 늘어진 BST의 경우 n개를 다 검색해야 하기 때문에 O(logn)이 아닌 O(n) time에 수행이 된다.
쭉 늘어진 경우에도 BST의 조건에는 위반하지 않기 때문에 벌어지는 일이다.
그래서 우리는 AVL TREE를 사용하여 Insertion, Deletion 연산 시 균형을 맞추도록 한다.

AVL TREE는 나오면 틀리도록 하겠습니다. -> 시간이 없어용ㅇㅇ

# 순회는 3가지 방식이 있는데
1. Preorder(전위 순회)
- Node -> 왼쪽 서브 트리 -> 오른쪽 서브 트리 (DFS)
2. Postorder(후위 순회)
- 왼쪽 서브 트리 -> 오른쪽 서브 트리 -> Node
3. Inorder(중위 순회)
- 왼쪽 서브 트리 -> Node -> 오른쪽 서브 트리

