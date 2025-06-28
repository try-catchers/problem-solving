## 👤 작성자
김지수

---

## 🧩 문제 정보
<!-- [문제 제목](문제 링크) 형식으로 작성하세요 -->
[AC](https://www.acmicpc.net/problem/5430)

---

## 💭 아이디어
- 양방향 삽입 및 삭제가 가능한 Deque 자료구조 사용
- 명령어 R(뒤집기)와 D(삭제)에 따라 배열을 처리
- R 연산 시 실제로 배열을 뒤집지 않고 `isReversed` 플래그 토글 처리
- D 연산 시 `isReversed` 값에 따라 앞에서 삭제(pollFirst) 또는 뒤에서 삭제(pollLast)
- 최종 출력 시 `isReversed` 값에 따라 앞 또는 뒤에서 순차적으로 꺼내 문자열 구성

---

## 🧑‍💻 코드
<!-- 작성한 코드를 백틱으로 감싸 넣어주세요 --> 
```java
import java.util.*;
import java.util.stream.*;
import java.io.*;

class Main {
    private static final char REVERSE = 'R'; // 뒤집기 연산
    private static final char DELETE = 'D'; // 첫번째 숫자 삭제 연산
    private static Deque<Integer> deque;

    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        int T = Integer.parseInt(reader.readLine()); // 테스트 케이스 개수

        for (int i = 0; i < T; i++) {
            char[] operations = reader.readLine().toCharArray();
            int N = Integer.parseInt(reader.readLine()); // 배열의 길이

            deque = parseArrayString(N, reader.readLine());
            boolean isReversed = false;
            boolean isError = false;

            for (char operation : operations) {
                if (operation == REVERSE) isReversed = (!isReversed); // 실제로 뒤집지 않고 토글 처리
                else if (operation == DELETE) {
                    if (deque.isEmpty()) {
                        isError = true;
                        System.out.println("error");
                        break;
                    } else if (isReversed) { // isReversed가 true면 뒤집힌 상태이므로 뒤에서 삭제
                        deque.pollLast();
                    } else { // isReversed가 false면 정방향 상태이므로 앞에서 삭제
                        deque.pollFirst();
                    }
                }
            }

            if (isError) continue;

            System.out.println(formatDeque(isReversed));
        }
    }

    // [1,2,3] 형태의 문자열을 파싱해 Deque<Integer> 타입으로 반환
    private static Deque<Integer> parseArrayString(int arrayLength, String arrayString) {
        if (arrayLength == 0) return new ArrayDeque<>();

        return Arrays.stream(arrayString.substring(1, arrayString.length() - 1).split(","))
            .map(String::trim)
            .filter(s -> !s.isEmpty())
            .map(Integer::parseInt)
            .collect(Collectors.toCollection(ArrayDeque::new));
    }

    // Deque<Integer> 타입 객체를 [1,2,3] 형태의 문자열로 변환
    private static String formatDeque(boolean isReversed) {
        StringBuilder sb = new StringBuilder();

        sb.append("[");

        while (!deque.isEmpty()) {
            sb.append(isReversed ? deque.pollLast() : deque.pollFirst());

            if (!deque.isEmpty()) sb.append(",");
        }

        sb.append("]");

        return sb.toString();
    }
}
```

---

## ⏰ 복잡도
- 시간 복잡도: O(T × (n + m))
    - n: 배열 길이, m: 명령어 길이, T: 테스트 케이스 개수
- 공간 복잡도: O(n)
    - 각 테스트 케이스마다 입력 배열을 저장하는 Deque와 출력용 StringBuilder에 최대 n개의 요소 저장

---

## 📝 회고
- 문자열 파싱이 파이썬에 비해 번거로웠다.
- 스트림에 익숙해질 필요가 있다고 느꼈다.
- 실제 자료구조를 뒤집지 않고 처리하는 방식이 효율적이었다.