## 👤 작성자Add commentMore actions
이현지

---

## 🧩 문제 정보
<!-- [문제 제목](문제 링크) 형식으로 작성하세요 -->
[AC](https://www.acmicpc.net/problem/5430)

---

## 💭 아이디어

### 문제 조건 요약
- 주어진 명령어에 따라 배열을 조작하는 문제다. 주어진 명령어는 'R' (뒤집기)와 'D' (첫 번째 원소 제거)로 구성된다.
- 배열에 연산을 적용한 후, 최종 결과를 출력해야 한다.
- 배열이 비어 있을 때 'D' 명령을 사용하면 에러가 발생한다.

### 접근 방식
- 처음에는 배열 자체를 뒤집으려는 방식으로 접근했다. 하지만, 'R' 연산을 매번 배열을 뒤집는 방식으로 구현했을 때 불필요한 연산이 많다는 것을 알게 되었다.
- 'R' 명령어가 반복될 경우, 실제로 뒤집히는 순서를 추적하는 방식이 더 효율적임을 깨닫고, `reverse`라는 boolean 값을 사용하여 뒤집힘 상태를 관리하기로 했다.
- 'D' 명령어는 배열의 첫 번째 원소를 제거하거나, `reverse`가 참일 경우 배열의 마지막 원소를 제거하도록 처리했다.

### 사용 자료구조 & 알고리즘
- `Deque`: 배열의 앞뒤에서 원소를 빠르게 삭제할 수 있는 자료구조다. 이를 이용해 'D' 명령을 효율적으로 처리할 수 있다.

---

## 🧑‍💻 코드
<!-- 작성한 코드를 백틱으로 감싸 넣어주세요 --> 
```java
import java.io.*;
import java.util.*;

public class Main {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());

		for (int tc = 0; tc < T; tc++) {
			String p = br.readLine();
			int n = Integer.parseInt(br.readLine());

			String arrStr = br.readLine().trim();
			Deque<Integer> deque = new ArrayDeque<>();
			if (n > 0) {
				arrStr = arrStr.substring(1, arrStr.length() - 1); // Remove '[' and ']'
				String[] arr = arrStr.split(",");
				for (String num : arr) {
					if (!num.isEmpty()) {
						deque.add(Integer.parseInt(num));
					}
				}
			}

			System.out.println(processCommands(deque, p));
		}
	}

	static String processCommands(Deque<Integer> deque, String commands) {
		boolean reverse = false;

		for (char command : commands.toCharArray()) {
			if (command == 'R') {
				reverse = !reverse;
			} else if (command == 'D') {
				if (deque.isEmpty()) {
					return "error";
				}
				if (reverse) {
					deque.removeLast();
				} else {
					deque.removeFirst();
				}
			}
		}

		StringBuilder sb = new StringBuilder("[");
		if (reverse) {
			while (!deque.isEmpty()) {
				sb.append(deque.removeLast());
				if (!deque.isEmpty()) {
					sb.append(",");
				}
			}
		} else {
			while (!deque.isEmpty()) {
				sb.append(deque.removeFirst());
				if (!deque.isEmpty()) {
					sb.append(",");
				}
			}
		}
		sb.append("]");

		return sb.toString();
	}
}
```

---

## ⏰ 복잡도

- **시간 복잡도**: O(N × M)
    - N: 테스트 케이스의 개수 (최대 100)
    - M: 각 테스트 케이스에서 배열의 크기 (최대 100)
    - 각 테스트 케이스마다 명령어와 배열을 한 번씩 처리하며, 명령어는 배열 길이에 비례하지 않으므로 전체 시간 복잡도는 O(N × M)이다.

- **공간 복잡도**: O(N)
    - 입력 배열을 저장하는 데 필요한 공간이 최대 N에 비례하므로, 공간 복잡도는 O(N)이다.

---

## 📝 회고

- 처음에는 배열 자체를 계속 뒤집으려는 방식으로 접근했다. 그러나 매번 배열을 뒤집는 방식은 불필요한 연산이 많아 비효율적이라는 걸 깨달았다.
- 'R' 연산이 반복될 경우, 실제로 배열을 뒤집는 것이 아니라, `reverse`라는 boolean 값을 사용하여 뒤집힘 상태를 추적하는 방식으로 변경했다. 이를 통해 불필요한 연산을 줄일 수 있었고 성능이 크게 개선됐다.
- `Deque`를 사용하여 배열의 앞뒤에서 원소를 빠르게 제거할 수 있었다. 이는 'D' 명령어를 처리할 때 매우 유용했다.