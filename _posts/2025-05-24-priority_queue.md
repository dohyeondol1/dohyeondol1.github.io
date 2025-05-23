---
layout: post
title: "우선순위 큐와 그 응용"
subtitle: "Priority queue"
date: 2025-05-24 18:00:00
author: "dohyeondol1"
header-img: "img/in_post/bus.png"
catalog: true
tags:
  - c++
  - algorithm
---


# 우선순위 큐

**우선순위 큐(Priority Queue)**는 각 원소에 우선순위를 부여해, 삽입 순서와 무관하게 우선순위가 높은 원소부터 꺼낼 수 있는 자료구조입니다.
예를 들어, 숫자가 작을수록 우선순위가 높다고 하면, 우선순위 큐에서 데이터를 꺼낼 때 항상 가장 작은 숫자가 먼저 나옵니다.  

이는 **"가장 먼저 처리해야 할 일을 효율적으로 고르는"** 데 탁월한 도구입니다.  
만약 여러 작업 중 마감 시간이 가장 가까운 작업을 먼저 처리해야 한다면 단순한 큐로는 해결이 어렵겠죠.  
이럴 때 바로 우선순위 큐가 필요합니다.

## 우선순위 큐의 구현
단순히 큐에서 우선순위만 추가해주면 되지만, 직접 구현해보면 간단하게 바꿀 수 있는 부분은 아닙니다.

```cpp
#include <iostream>
using namespace std;

const int MAX_SIZE = 1000;
int heap[MAX_SIZE];
int heapSize = 0;

void push(int x) {
    int i = heapSize++;
    heap[i] = x;

    // heapify up
    while(i > 0) {
        int parent = (i - 1) / 2;
        if(heap[i] < heap[parent]) {
            swap(heap[i], heap[parent]);
            i = parent;
        } 
        else break;
    }
}

void pop() {
    if(heapSize == 0) return;

    heap[0] = heap[--heapSize];

    // heapify down
    int i = 0;
    while(true) {
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        int smallest = i;

        if(left < heapSize && heap[left] < heap[smallest]) smallest = left;
        if(right < heapSize && heap[right] < heap[smallest]) smallest = right;

        if(smallest == i) break;

        swap(heap[i], heap[smallest]);
        i = smallest;
    }
}

int top() {
    if(heapSize == 0) throw runtime_error("Heap is empty"); // 힙이 비었을 경우
    return heap[0];
}

bool empty() {
    return heapSize == 0;
}
```


## 우선순위 큐 container adaptor, priority_queue

직접 구현하는 건 학습에는 좋지만 실전에서는 번거롭기 마련입니다.
  
혹시나 하면 역시나 있는 STL에서 우선순위 큐를 손쉽게 사용하라고 **priority_queue** 라는 container adapter를 만들어두었습니다.
이는 queue header를 include하여 사용할 수 있습니다.

priority_queue의 선언 구조는 다음과 같습니다.

```cpp
#include <queue>
priority_queue<type, container, compare> pq;
```

각각 우선순위 큐에 넣을 요소의 자료형, 요소들을 저장할 클래스 컨테이너, 비교 연산자 함수입니다.
우리가 자주 사용하는 vector와 비슷하며, 다음과 같이 사용합니다.

```cpp
priority_queue<int> pq1;                            // max heap(default)
priority_queue<int, vector<int>, greater<int>> pq2; // min heap
```

우선순위 큐는 일반적으로 **heap**을 사용하여 구현합니다.

### heap
**힙(Heap)**은 완전 이진 트리의 일종으로, 항상 부모 노드의 값이 자식 노드보다 작거나 같은 성질(최소 힙) 또는 **크거나 같은 성질(최대 힙)**을 만족하는 구조입니다.

우선순위 큐를 힙을 기반으로 구현하면 다음과 같은 장점이 있습니다:

- 삽입(push)과 삭제(pop) 연산 모두 O(log n) 시간 복잡도

- 가장 우선순위가 높은 값(최소 힙에서는 최소값)을 **O(1)**에 가져올 수 있음

STL의 priority_queue는 기본적으로 최대 힙으로 구현되어 있으며, 앞서 보여드린 `greater<>`를 통해 최소 힙으로 바꿔 쓸 수 있습니다.

```cpp
    priority_queue<int> pq1;                            // 최대 힙
    priority_queue<int, vector<int>, greater<int>> pq2; // 최소 힙
```

--- 

### priority_queue
앞서 살펴보았듯 priority_queue의 기본적인 선언은 vector의 사용과 다름이 없으나,  
우선순위 큐를 단순히 위 두 가지 방식(max heap, min heap)으로만 사용하기에는 다양한 문제 요구사항을 충족시키기 어렵기 때문에, 사용자 정의 비교 연산자를 통해 보다 유연하게 우선순위를 설정할 수 있도록 합니다.

보편적으로 compare의 약자인 'cmp'라는 네임으로 비교연산을 선언합니다.

```cpp
// 작은 수 부터 반환하는 비교 연산
struct cmp {
    bool operator()(int a, int b) {
        return a > b;
    }
};

```

STL priority_queue는 내부적으로 binary heap 구조를 사용합니다. 이진 힙은 완전 이진 트리를 배열 형태로 구현하며, 삽입 시에는 heapify-up, 삭제 시에는 heapify-down 연산을 통해 힙 속성을 유지합니다. 이런 구조 덕분에 push와 pop 연산은 각각 O(log n)의 시간 복잡도를 가집니다.

비교 함수는 `<` 연산자를 오버라이드한 클래스나 람다 표현식을 사용할 수 있습니다.
예를 들어, 작업마다 마감 시간이 다르거나, 우선순위가 여러 조건의 조합일 때에는 다음과 같이 복잡한 비교 조건도 정의할 수 있습니다.
```cpp
struct Job {
    int deadline, priority;
};

struct cmp {
    bool operator()(const Job& a, const Job& b) {
        if(a.deadline == b.deadline) return a.priority < b.priority;
        return a.deadline > b.deadline;
    }
};
priority_queue<Job, vector<Job>, cmp> jobQueue;
```

```cpp
auto cmp = [](int a, int b) {
    return abs(a) > abs(b);
};
priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
```

---

간단한 문제를 풀며 우선순위 큐에 익숙해져 봅시다.

<a href="https://www.acmicpc.net/problem/1927" target="_blank">baekjoon 1927. 최소 힙</a>  
최소 힙을 구현하면 되는 문제입니다.  
우리는 앞서 priority_queue를 사용하여 최소 힙을 사용할 수 있음을 확인했으므로 간단하게 해결할 수 있습니다.

<div markdown="1">

```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

int main() {
    cin.tie(nullptr)->sync_with_stdio(false);
    
    int N;
    cin >> N;
    priority_queue<int, vector<int>, greater<int>> pq;
    for(int i = 0; i < N; i++) {
        long long x;
        cin >> x;
        if(x == 0) {
            if(pq.empty()) cout << 0 << '\n';
            else {
                cout << pq.top() << '\n';
                pq.pop();
            }
        }
        else {
            pq.push(x);
        }
    }
    
    return 0;
}

```

</div>

같은 방식으로  
<a href="https://www.acmicpc.net/problem/11279" target="_blank">baekjoon 11279. 최대 힙</a> 또한 해결할 수 있습니다.

우선순위를 직접 설정한다면  
<a href="https://www.acmicpc.net/problem/11286" target="_blank">baekjoon 11286. 절댓값 힙</a> 까지 해결할 수 있습니다.


다른 알고리즘들을 함께 사용하는 문제들을 풀어봅시다.
<a href="https://www.acmicpc.net/problem/13975" target="_blank">baekjoon 13975. 파일 합치기 3</a>  
  
<a href="https://www.acmicpc.net/problem/1715" target="_blank">baekjoon 1715. 카드 정렬하기</a> 


## 우선순위 큐의 응용

**가장 우선순위가 높은 원소를 빠르게 찾고 꺼내야 할 때** 우선순위를 사용할 수 있으므로, 다양한 알고리즘에서 범용적으로 사용할 수 있습니다.

### 1. 다익스트라 알고리즘 (Dijkstra's Algorithm)
그래프에서 한 정점에서 다른 모든 정점까지의 최단 거리를 구할 때 자주 사용됩니다.
이때 우선순위 큐는 아직 방문하지 않은 노드 중 가장 거리가 짧은 노드를 빠르게 찾는 데 사용됩니다.

`priority_queue<pair<int, int>`, `vector<pair<int, int>>, greater<>>`를 사용하면, pair의 첫 번째 값(거리)을 기준으로 최소값이 가장 높은 우선순위를 갖게됩니다. 이는 매번 가장 짧은 거리의 정점을 먼저 탐색하게 해줘서, 다익스트라 알고리즘의 핵심 흐름인 greedy 방식과 일치한다고 볼 수 있죠.

<div markdown="1">

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
```

</div>


### 2. 작업 스케줄링 (Job Scheduling)
각 작업마다 우선순위가 있는 경우, 우선순위 큐를 통해 항상 가장 중요한 작업부터 수행할 수 있습니다.

예를 들어, 작업 스케줄링 문제에서 작업이 도착하는 순서가 있고 실행 시간이 짧은 작업을 우선 수행해야 한다면, 
실행 시간을 기준으로 우선순위 큐를 구성해 다음과 같이 해결할 수 있습니다.

```cpp
struct Job {
    int requestTime, duration;
};

struct cmp {
    bool operator()(const Job& a, const Job& b) {
        return a.duration > b.duration;
    }
};
priority_queue<Job, vector<Job>, cmp> pq;
```

아래는 대표적인 작업 스케줄링 문제입니다.  
요청 시간이 서로 다른 여러 작업이 주어졌을 때, 작업이 모두 끝나는 데 걸리는 평균 시간을 최소화하는 방식으로 스케줄링을 요구합니다.

<a href="https://school.programmers.co.kr/learn/courses/30/lessons/42627">programmers 42627. 디스크 컨트롤러</a>   

### 3. A* 알고리즘
A* 알고리즘은 추후 포스팅하고 링크를 추가하도록 하겠습니다.



## 마치며

제가 공부한 내용의 깊이가 그렇게 깊지 않아 쉽게 이해할 수 있는 글이라 생각합니다.

우선순위 큐는 자료구조 자체도 어렵지 않고, STL 덕분에 간단하게 사용할 수 있습니다.  
특히 priority_queue는 기본 제공 기능으로도 대부분의 알고리즘 문제를 커버할 수 있을 정도로 충분히 유연하며, 커스텀 비교 함수까지 제공하므로 다양한 우선순위를 설정해줄 수 있습니다.  


## 참고자료
<a href="https://en.cppreference.com/w/cpp/container/priority_queue">priority_queue details1</a>  
<a href="https://cplusplus.com/reference/queue/priority_queue/">priority_queue details2</a>  