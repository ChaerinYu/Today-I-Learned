
## Stack과 Queue

| |Stack|Queue|
|:---:|-----|-----|
|정의|후입선출 (Last-In First-Out)<br/>나중에 들어간 원소가 먼저 나온다.|선입선출 (First-In First-Out)<br/>먼저 들어간 원소가 먼저 나온다.|
|Java Collection|Java Collection에서 클래스<br/>java.util.Stack|Java Collection에서 인터페이스<br/>java.util.Queue|
|예시|- 웹 브라우저 (뒤로 가기, 앞으로 가기 ..)<br>- 먹고 토하기🤮<br/>- undo (실행 취소)<br/>- 함수 호출|- 놀이공원/은행 등 대기시간<br/>- 먹고 배출..하기🚽🧻<br>- 프로세스<br>- 캐시(Cache) <br>- 에스컬레이터 <br>- BFS<br/>- 키보드 입력|

<br/>

* Stack
``` java
public class Stack {
	private Node top;
	
	// 삽입
	public void push(String data) {
		Node newNode = new Node(data, top); // 내가 첫 번째가 되기 위해서는 원래 첫 번째인 애가 내 뒤에 와야 된다.
		top = newNode;
	}
	// stack null 체크
	public boolean isEmpty() {
		return top == null;
	}
	// stack 마지막 원소 빼기
	public String pop() {
		if(isEmpty()) {
			System.out.println("스택이 비어있어 pop이 불가합니다.");
			return null;
		}
		Node popNode = top; // 마지막 원소
		top = popNode.link;
		popNode.link = null; // 마지막 원소 후발주자
		return popNode.data;
	}
	// stack 마지막 원소 조회
	public String peek() {
		if(isEmpty()) {
			System.out.println("스택이 비어있어 peek이 불가합니다.");
			return null;
		}
		return top.data;
	}
	
	@Override
	public String toString() {
		StringBuilder sb = new StringBuilder();
		sb.append("S ( ");
		for (Node currNode = top; currNode != null; currNode = currNode.link) {
			sb.append(currNode.data).append(", ");
		}
		// 문자열 마지막에 ", " 제거
		if(!isEmpty())
			sb.setLength(sb.length()-2);
		sb.append(" ) ");
		return sb.toString();
	}
}


```

* Java Collection 참고

![list_set_map](images/list_set_map.png)

<br/>

### PriorityQueue
우선 순위를 가진 항목들을 저장하는 큐  
선입선출(FIFO)가 아니라 우선순위가 높은 순서대로 먼저 나가게 된다.  
java.util.PriorityQueue  
Heap 자료구조 (최대 힙(가장 큰 값 먼저 나옴), 최소 힙(가장 작은 값 먼저 나옴))

### Heap
완전 이진 트리 자료구조, 키 값이 가장 큰 노드나 가장 작은 노드 찾기 유용(항상 루트노드에 존재)  
힙의 키를 우선순위로 활용하여 우선순위 큐를 구현할 수 있다.  


