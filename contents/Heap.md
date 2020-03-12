## 힙(Heap) 자료구조 요약정리

힙은 완전 이진트리의 자료 구조이다. 완전히 정렬된 상태가 아닌 반 정렬 상태이며 완전 이진트리와는 다르게 중복값이 허용된다. 트리는 복잡도가 O(logN) 이므로 삽입,삭제가 빠르다. 

### 힙 구현하기
- 보통 우선순위 큐로 힙을 구현한다.(빼열, 리스트보다 효율적)
- 최대힙, 최소힙 두 가지로 나뉘어진다.
    - 최대힙: 부모노드의 값이 가장 크다
    - 최소힙: 부모노드의 값이 가장 작다
- 최소값이나 최대값을 빨리 찾아야 할 때 유용하다.
- 배열로 구현할때 인덱스 0은 사용하지 않는다. 

![img load fail](../imgs/heapData.png)
- 왼쪽자식은 부모 인덱스x2, 오른쪽 자식은 부모인덱스x2+1 
- 자식노드(N)의 부모노드는 N/2로 표현한다.

### 최소힙 소스 코드
- 배열리스트로 구현한 최소힙

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
	public static class MinHeap{
		private List<Integer> heap;
		
		public MinHeap() {
			this.heap = new ArrayList<>();
			heap.add(0); //인덱스1부터 사용하기 위해
		}
		//요소 삽입 
		public void insert(int val) {
			heap.add(val); //일단넣고
			int p = heap.size() -1; //마지막 요소의 인덱스. 즉 바로 위에 담은 요소의 인덱스. 즉  p=신입
			
			while(p>1 && (heap.get(p/2)>heap.get(p))){ //반복하는 조건 : p보다 p의 부모가 값 더 크다, p는 1보다 크다
				int tmp = heap.get(p/2);
				heap.set(p/2,heap.get(p));
				heap.set(p,tmp);
				p=p/2; //신입이 부모위치로 올라간다!
			}
		}
		//노드 삭제. (루트 노드부터)
		public int delete() {
			if(heap.size()<=1) return 0;
			int deleteValue = heap.get(1);
			
			heap.set(1,heap.get(heap.size()-1));
			heap.remove(heap.size()-1);
			
			int pos=1;
			while(pos*2 < heap.size()) { 
				int minPos = pos*2; //자식 위치
				int minValue = heap.get(pos*2); //자식 값
				if((minPos+1)<heap.size() && (heap.get(minPos+1)<minValue)) {
					minPos = minPos +1;
					minValue = heap.get(minPos+1);
				}
				if(heap.get(pos) < minValue)
					break;
				int tmp = heap.get(pos);
				heap.set(pos, minValue);
				heap.set(minPos, tmp);
				pos = minPos;
			}
			return deleteValue;	
		}
	}
	public static void main(String[] args) {
		MinHeap heap = new MinHeap();
		int[] data = {10,6,8,4,5,3,4,2,1};
		for(int i=0 ; i<data.length; i++)
			heap.insert(data[i]);
		for(int i=1 ; i<heap.heap.size(); i++ )
			System.out.print(heap.heap.get(i)+" ");
	}
}
```

- 출력 결과 구조
![img load fail](./imgs/heap결과.png)


### 탐욕법을 활용한 문제
- [더맵게](https://github.com/TheCopiens/algorithm-study/blob/master/source/ohhako/200312_heap.md)


---
아래의 사이트를 참고해 작성된 글입니다.
- https://hannom.tistory.com/36
- 