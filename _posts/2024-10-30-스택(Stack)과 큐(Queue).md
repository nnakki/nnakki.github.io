---
title: "스택(Stack)과 큐(Queue)"
excerpt: "알고리즘 개념정리 - 스택/큐"

categories:
  - Algorithm
tags: # 포스트 태그
  - [Algorithm, Stack, Queue] 

permalink: /algorithm/스택(Stack)과-큐(Queue)/

date: 2024-10-30 # 작성 날짜
---
# 스택

- 데이터를 한 열로 저장한다 / **LIFO (Last In First Out) : 후입선출 구조를 가진다.**
- 스택에 데이터를 넣는 작업은 ‘**push**’라고 하고, 데이터를 꺼내는 작업은 ‘**pop**’이라고 한다.
- 리스트나 배열처럼 데이터를 일렬로 저장하지만, 데이터의 추가와 삭제가 한쪽 끝에서만 이뤄지고 가장 위에 놓인 데이터에만 접근할 수 있기 때문에 중간에 있는 데이터게 접근하려면 해당 데이터가 스택의 맨 위에 놓일 때까지 데이터를 꺼내야 한다.
- 항상 **‘최신 데이터’**에 접근해야 할 때 유용하게 사용 될 수 있다.
- `ex`  - `(AB (C (DE) F) (G ((H) IJ) K))`  문자열의 괄호 대응 확인 ( 괄호가 제대로 짝을 이루고 있는지 확인)

    1.  왼쪽부터 문자를 읽으면서 왼쪽 괄호가 나타날 때마다 해당 위치를 스택에 푸시한다. 
    2. 오른쪽 괄호가 나타났을 때는 데이터를 팝한다. 그럼 오른쪽 괄호에 해당하는 짝의 위치를 알 수 있다. 
        ```java
        import java.util.*;
        
        public class BracketMatching {
            public static void main(String[] args) {
                String str = "(AB (C (DE) F) (G ((H) IJ) K))";
        
                // 여는 괄호의 인덱스를 저장할 스택
                Stack<Integer> stack = new Stack<>();
                Map<Integer, Integer> bracketPairs = new HashMap<>();
        
                // 문자열을 탐색하며 괄호를 찾음
                for (int i = 0; i < str.length(); i++) {
                    char ch = str.charAt(i);
        
                    if (ch == '(') {
                        // 여는 괄호면 스택에 인덱스를 저장
                        stack.push(i);
                    } else if (ch == ')') {
                        // 닫는 괄호가 나오면 스택에서 여는 괄호 인덱스를 꺼냄
                        if (!stack.isEmpty()) {
                            int openIndex = stack.pop();
                            // 여는 괄호 인덱스와 닫는 괄호 인덱스를 맵에 저장
                            
                            bracketPairs.put(openIndex, i);
                        }
                    }
                }
        
                // 각 괄호 쌍 출력
                for (Map.Entry<Integer, Integer> entry : bracketPairs.entrySet()) {
                    System.out.println("여는 괄호 위치: " + entry.getKey() + ", 닫는 괄호 위치: " + entry.getValue());
                    System.out.println("해당 괄호 내부 문자: " + str.substring(entry.getKey() + 1, entry.getValue()));
                }
            }
        }
        
        ```
        
- <span style="color:red">깊이 우선 탐색</span>의 경우 후보 중 항상 최신의 것을 선택해야 하기 때문에 <span style="color:red">스택</span>을 사용한다.

# 큐
- 데이터를 한 열로 저장한다 / **FIFO (First In First Out) : 선입선출 구조를 가진다.**
- 큐에 데이터를 추가하는 작업을 ‘**enqueqe(인큐)**’라고 하고, 데이터를 꺼내는 작업을 ‘**dequeqe(디큐)**’라고 한다.
- 데이터의 추가와 삭제가 각각 반대쪽에서 이뤄진다. 스택과 마찬가지로 중간에 있는 데이터에 접근할 수 없고 필요한 데이터가 나올 때까지 디큐할 필요가 있다.
- **오래된 데이터**를 차례로 처리하는 경우에 유용하게 사용할 수 있다. (범용적)
- <span style="color:red">너비 우선 탐색</span>의 경우 후보 중 항상 가장 오래된 것을 선택해야 하기 때문에 <span style="color:red">큐</span>를 사용한다.