### Stack
![image](https://user-images.githubusercontent.com/118147296/219331826-5e21a38e-212b-4c40-81e5-959e578b2255.png)

- 스택의 사전적인 정의는 '쌓아올림', '무더기'라는 뜻이며, 의미 그대로 자료를 계속 쌓아올라가는 방식으로 데이터를 임시 저장하는 자료구조를 의미한다.
- 스택은 데이터의 입출입이 단 하나의 방향에서만 이루어진다.
- 가장 먼저 들어온 데이터가 가장 늦게 사용되고, 가장 나중에 들어온 데이터가 가장 먼저 사용되는 후입선출(Last In First Out) 방식의 자료구조이다.
- 스택에 자료를 쌓아올리는 작업을 푸시(push), 스택에서 자료를 꺼내는 작업을(pop)이라고 한다.
- 스택의 상단을 탑(top), 하단(bottom)이라고 한다.

> Stack의 활용 예제
- 웹 브라우저 방문 기록 (뒤로가기)
- 역순 문자열 만들기
- 실행 취소 (undo)
- 후위 표기법 계산
- 수식의 괄호 검사

```
import java.util.Stack;

public class StackQueue {
	
	public static void main(String[] args) {		
		Stack<String> st = new Stack<String>();
		st.push("첫번째");
		st.push("두번째");
		st.push("세번째");
		System.out.println("----- Stack -----");
		while(!st.isEmpty()){
			System.out.println(st.pop());
		}
	}
}
```
```
실행결과
----- Stack -----
세번째
두번째
첫번째
```


### Queue
![image](https://user-images.githubusercontent.com/118147296/219336187-56cd54dc-8a9d-4370-84b2-0441184e4724.png)

- Queue의 사전적 의미는 '대기 줄'인데, 자료가 일정 순서로 처리되는 자료구조 방식이다.
- 임시 데이터를 저장하는 자료 구조라는 점에서 스택과 동일하나, 가장 먼저 들어온 데이터가 먼저 꺼내지는 선입선출(FIFO) 방식의 자료구조이다.
- 자료가 들어오는 부분을 rear, 자료가 삭제되어 나가는 부분을 front라고 한다.
- 먼저 들어온 원소를 front 원소, 마지막에 들어온 원소를 rear 원소라고 한다.
- 큐에 데이터를 추가하는 것을 인큐(enqueue), 데이터를 꺼내는 작업을 디큐(dequeue)라고 한다.

> Queue의 활용 예제
- 우선 순위가 같은 작업 예약
- 은행 업무
- 콜센터 고객 대기 시간
- 프로세스 관리
- 너비 우선 탐색
- 캐시 구현
```
import java.util.LinkedList;
import java.util.Queue;

public class StackQueue {
	public static void main(String[] args) {	
		Queue<String> qu = new LinkedList<>();
		qu.offer("첫번째");
		qu.offer("두번째");
		qu.offer("세번째");
		System.out.println("----- Queue -----");
		while(!qu.isEmpty()){
			System.out.println(qu.poll());
		}
	}
}
```
```
// 실행결과
----- Queue -----
첫번째
두번째
세번째
```


출처. https://tragramming.tistory.com/106
출처. https://developerbee.tistory.com/148
출처. https://veneas.tistory.com/entry/Java-LIFO-%EC%8A%A4%ED%83%9D-FIFO-%ED%81%90-Stack-Queue
