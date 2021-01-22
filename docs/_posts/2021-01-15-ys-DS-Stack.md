---
title: "[자료구조] 스택(Stack)"
subtitle: "자료구조"
layout: post
author: "Yuseon"
header-style: text
tags:
  - 자료구조
---

추상적 자료형(Abstract Data Structure)는 자료의 형태와 연관된 연산들을 수학적으로 정의한 것으로 ```집합(Set)```, ```리스트(List)```, ```스택(Stack)```, ```큐(Queue)```, ```트리(Tree)```, ```그래프(Graph)``` 등이 있다.  

오늘은 **스택(Stack)**에 대해 알아보는 시간을 갖도록 하자.  

## 스택 (Stack)

### 개념
스택(Stack)은 말 그대로 데이터를 **쌓아올린다**는 것을 의미한다.  
스택은 ```LIFO(Last In First Out)```, ```후입선출```, ```선출후입``` 형식이 자료구조를 갖는다.  
따라서, 한쪽 끝에서만 자료를 넣고 뺄 수 있다.  
![Stack](/img/in-post/2021-01-15-ys-DS-Stack.png)

### 연산
#### 삽입 (Push)
스택에서 데이터를 **삽입**하는 것을 ```Push``` 한다고 표현한다.  
새로운 데이터를 스택의 가장 윗 부분에 추가하는 작업을 말한다.  

#### 삭제 (Pop)
스택에서 데이터를 **삭제**하는 것을 ```Pop```한다고 표현한다.  
스택에서 가장 위에 있는 데이터를 제거한다.  

#### 접근 (Peek/Top)
스택은 **가장 위에 있는 데이터만** 접근할 수 있다. 이 작업을 ```Peek``` 또는 ```Top``` 이라고 한다.  
따라서, 탐색을 위해서는 원소를 하나하나 꺼내고 옮기면서 해야야 하므로 매우 비효율적이다.  

중간 삽입/삭제/탐색은 스택의 목적과 맞지 않는다.  

#### 그 외(etc)
스택이 비어있을 때, ```pop```연산을 해 데이터를 삭제하고자 할 때 ```Stack Underflow```가 발생하고,  
스택이 꽉 차있을 때, ```push```연산을 통해 데이터를 추가하려 해 스택이 넘치는 현상을 ```Stack Overflow```라고 한다.  

### 시간 복잡도
**삽입/삭제**는 ```pop```과 ```push```와 같이 한 번의 작업으로 가능하므로 ```O(1)```의 시간 복잡도를 갖는다.

### 사용
스택은 ```BFS(Breath First Search)```와 ```재귀(Recursion)``` 알고리즘에 사용된다.  
데이터를 역추적을 할 때 유용하며, 문서 작업할 때 실행 취소(undo), 브라우저에서 뒤로가기 등에서 사용된다.  

### 구현
스택은 ```배열(Array)```와 ```연결 리스트(Linked List)```를 이용하여 구현할 수 있다.  

#### 배열 (Array)
배열을 통헤 스택을 구현할 때에는 배열의 크기를 선언하고, 가장 마지막 인덱스를 top으로 지정하여 top을 움직이면서 구현하면 된다.

#### 연결 리스트 (Linked List)
연결 리스트를 통해 구현할 때에는 가장 마지막에 추가한 데이터를 포인터로 top으로 지정하여 top을 움직이면서 삭제, 추가를 할 수 있다.  

코드로 구현하는 것은 다음에 알아보도록 하자 ʕ•ᴥ•ʔ  

### 스택 (Stack) 관련 C++ 함수
```cpp
#include <stack>

using namespace std;

int main() {
	stack<int> ist;                 // Initialization
	stack<string> sst;              // stack<[Data Type]> var_name

	ist.push(1);                    // push : add item on the 'top'
	ist.push(25);
	ist.push(30);

	cout << ist.size() << "\n";     // output : 3
	cout << ist.top()  << "\n";     // output : 30
	
	ist.pop();                      // pop : delete the 'top' item
	cout << ist.size() << "\n";     // output : 2
	cout << ist.top()  << "\n";     // output : 25

	/* 
		** WARNING **
		When using top() and pop() function, 
                it's safe to check if it's empty first.
	*/
	while(!ist.empty()) {           // check if stack is empty
		cout << ist.top() << "\n";  // return 'top' item
		ist.pop();
	}

	return 0;
}
```

### 정리
요약하면, 스택(Stack)은 ```LIFO(후입선출)```의 자료구조를 갖는다.  
```Push```, ```Pop```, ```Top``` 등의 연산을 할 수 있으며,  
삽입과 삭제는 용이하지만, 탐색에는 비효율적인 자료구조이다.  
BFS(Breath First Search)와 재귀(Recursion) 알고리즘에 사용된다.  

The End! ⁽⁽◝( ˙ ꒳ ˙ )◜⁾⁾
<br>