## 👤 작성자
한지성

---

## 🧩 문제 정보
<!-- [문제 제목](문제 링크) 형식으로 작성하세요 -->
[AC](https://www.acmicpc.net/problem/5430)

---

## 💭 아이디어

- 입력으로 주어진 문자열을 처리하여 배열을 조작하는 문제
- deque를 사용하여 배열의 앞과 뒤에서 효율적으로 요소를 추가 및 제거
- 문자열을 순회하면서 'R'과 'D' 명령을 처리
- 'R'은 뒤집기로 r 변수로 현재 뒤집힌 상태 체크  → 'D' 명령이 오면 제거되는 방향을 결정
- 배열이 비어있을 때 'D' 명령이 오면 'error' 출력하고 종료

---

## 🧑‍💻 코드
<!-- 작성한 코드를 백틱으로 감싸 넣어주세요 --> 
```python
from collections import deque

T = int(input())

for _ in range(T):
    p = input()
    n = int(input())
    
    # 입력받은 배열 양방향 큐에 담기
    x = deque(input()[1:-1].split(',')) 
    
    # deque[''] 의 길이를 0이 아닌 1로 취급
    if n == 0:  
        x = []
    
    # 뒤집힌 상태 체크
    r = 0

    for i in p:
        if i == 'R':
            r += 1
        elif i == 'D':
            if len(x) == 0:
                print('error')
                break
            else:
                if r % 2 == 1:
                    x.pop()
                else:
                    x.popleft()
                        
    else:
        # 최종 뒤집힌 상태 반영
        if r % 2 == 1:
            x.reverse()
           
        print('[' + ','.join(x) + ']')
```

---

## ⏰ 복잡도
- 시간 복잡도: O(T * n)
  - T: 테스트 케이스의 수
  - n: 각 테스트 케이스에서 배열의 길이
- 공간 복잡도: O(n)

---

## 📝 회고
- 문자열을 처리하는 과정에서 입력 형식에 맞춰서 양방향 큐를 사용해야 했음
- 입력받은 배열을 양방향 큐로 변환할 때, 문자열의 양 끝에 있는 대괄호를 제거하는 것을 잊음
- input()으로 입력받은 문자열에서 첫 번째와 마지막 문자를 제거하고 쉼표로 분리하여 deque에 담음
  → deque(input()[1:-1].split(','))

