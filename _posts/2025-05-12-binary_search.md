---
layout: post
title: "이분 탐색과 그 응용"
subtitle: "Binary Search Algorithm"
date: 2025-05-12 00:00:00
author: "dohyeondol1"
header-img: "img/in_post/books.png"
catalog: true
tags:
  - c++
  - algorithm
---


# 이분 탐색 알고리즘

**이분 탐색 알고리즘**은 오름차순으로 정렬된 리스트에서 특정한 값의 위치를 찾는 알고리즘입니다.

처음 중간의 값을 임의의 값으로 선택하여, 그 값과 찾고자 하는 값의 크고 작음을 비교하는 방식을 채택하고 있습니다. 만약 임의의 값이 찾고자 하는 값보다 크다면 임의의 값보다 작은 값을 탐색에서 제외시키고, 작다면 임의의 값보다 큰 값을 탐색에서 제외시키므로 탐색 범위를 절반씩 줄여가며 목표한 값을 찾습니다.

이렇게 반씩 탐색 범위를 줄여가면, 탐색에 소요되는 시간 복잡도는 𝑂(log 𝑛)입니다.  
이는 순차 탐색에 비해 압도적으로 높은 성능을 보입니다.


## 이분 탐색의 구현

이분 탐색은 보통 재귀형과 반복문, 두 가지 방식으로 구현할 수 있습니다.  
예제 코드를 통해 살펴봅시다.

### 반복문

```cpp
int binarySearch(const vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;

    while(left <= right) {
        int mid = (left + right) / 2;

        if(arr[mid] == target) {
            return mid; // target의 인덱스를 반환
        } else if(arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return -1; // target을 찾지 못한 경우
}
```

`target`이 배열의 중간값과 비교하였을 때, 같다면 `target`의 인덱스를 반환하고,  
작다면 `left`를 `mid + 1`로 설정하여 **중간값보다 작은 값들을 탐색 범위에서 제외**시킵니다.  
크다면 `right`를 `mid - 1`로 설정하여 **중간값보다 큰 값들을 탐색 범위에서 제외**시키죠.

`left`와 `right`가 각각 `mid`가 아닌 `mid + 1`, `mid - 1` 로 갱신되는 이유는  
`mid`에 해당하는 중간값이 이미 `target`의 대소 비교에 포함되었기 때문입니다.  
중간 값 또한 `target`보다 크거나 작은 경우에 포함되므로, 기존의 중간값 또한 탐색 범위에서 제외시켜야 합니다.

### 재귀형

```cpp
int binarySearchRecursive(const vector<int>& arr, int target, int left, int right) {
    if(left > right) return -1; // base case: 탐색 실패

    int mid = (left + right) / 2;

    if(arr[mid] == target) {
        return mid; 
    } else if(arr[mid] < target) {
        return binarySearchRecursive(arr, target, mid + 1, right); // 오른쪽 탐색
    } else {
        return binarySearchRecursive(arr, target, left, mid - 1); // 왼쪽 탐색
    }
}
```
Basecase와 중간값 계산 방식은 반복문과 똑같습니다.  
하지만 반복이 많거나 큰 입력이 들어올 경우 재귀형 이분 탐색에서는 **오버플로우의 위험이 존재**하기 때문에 반복문을 사용하는 방식을 일반적으로 사용합니다.

---
실제 문제를 풀며 이분 탐색을 적용해봅시다.

<a href="https://www.acmicpc.net/problem/1920" target="_blank">baekjoon 1920. 수 찾기</a>  
N개의 정수 중에 X가 존재하는지 찾는 문제입니다.

앞서 설명했던 반복문을 사용하는 방식의 이분 탐색을 통해 간단히 해결할 수 있습니다.

<div markdown="1">

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int N, M;
int N_array[1000000];

void binary_search(int x) {
    int left = 0, mid = 0, right = N - 1;

    while(left <= right) {
        mid = (left + right) / 2;

        if(N_array[mid] == x) {
            cout << 1 << '\n';
            return;
        } else if(N_array[mid] > x) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    cout << 0 << "\n";

    return;
}

int main() {
    cin.tie(nullptr)->sync_with_stdio(false);

    cin >> N;
    for(int i = 0; i < N; i++)
        cin >> N_array[i];

    sort(N_array, N_array + N);
    
    cin >> M; int x;
    for(int i = 0; i < M; i++) {
        cin >> x;
        binary_search(x);
    }

    return 0;
}
```

</div>

## 이분 탐색 함수, binary_search()
고맙게도 C++는 STL에서 이분 탐색을 구현해놓은 함수를 만들어두었습니다.  
algorithm header의 `std::binary_search()`가 바로 그것인데, 사용하기도 쉽습니다.

```cpp
bool result = binary_search(array, array + n, target);
```

코드만 봐도 어떻게 사용하는지 감이 올거라 생각합니다.  
첫번째 인자로 **배열의 시작 주소**를,  
두번째 인자로 **배열의 끝 주소**를,  
세번째 인자로 **찾을 값**을 받습니다.  
참고로 배열의 시작 주소와 끝 주소라고 해서 꼭 배열의 전체일 필요는 없습니다.

반환하는 값은 bool type으로,  
**찾는 값이 존재한다면 1을, 존재하지 않는다면 0을 반환합니다.**

이 함수를 통해 앞서 풀었던 <a href="https://www.acmicpc.net/problem/1920" target="_blank">baekjoon 1920. 수 찾기</a>를 보다 간결하게 해결할 수 있습니다.  

<div markdown="1">

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    cin.tie(nullptr)->sync_with_stdio(false);
    
    int N, M;
    cin >> N;
    
    vector<int> N_array(N);
    for(int i = 0; i < N; i++)
        cin >> N_array[i];

    sort(N_array.begin(), N_array.end());
    
    cin >> M; int x;
    for(int i = 0; i < M; i++) {
        cin >> x;
        cout << binary_search(N_array.begin(), N_array.end(), x) << '\n';
    }

    return 0;
}
```

</div>

하지만 이는 찾는 값의 존재 여부를 확인하기 위한 용도로만 사용됩니다.  
우리는 또 다른 함수를 통해 쉽게 찾고자 하는 값의 개수를 구할 수 있습니다.

### upper_bound, lower_bound
upper bound와 lower bound는 각각 상한, 하한이라는 뜻을 가집니다.


```cpp
upper = upper_bound(array, array + n, target);
lower = lower_bound(array, array + n, target);
```

받는 값은 `binary_search()`와 동일합니다.  
첫번째 인자로 **배열의 시작 주소**를,  
두번째 인자로 **배열의 끝 주소**를,  
세번째 인자로 **찾을 값**을 받습니다.

`upper_bound()`는 **target이 담긴 마지막 요소의 다음 주소**를 반환합니다.
`lower_bound()`는 **target이 담긴 첫 번째 요소의 주소**를 반환합니다.
target이 담긴 요소를 찾지 못할 경우 두번째 인자(배열의 마지막 주소)를 반환합니다.

이분 탐색은 이미 오름차순으로 정렬된 리스트에서 사용하는 알고리즘이었기 때문에,  
`upper_bound()`-`lower_bound()` = **target의 개수**  
위가 성립합니다.

해당 함수에 대해 더 자세한 정보는 글 하단의 참고자료를 이용해주세요.


## 이분 탐색의 응용

이분 탐색을 단순히 구현하는 것에서 그친다면 특정 값을 찾는 문제로 해결할 수 있지만,
이분 탐색을 응용하는 과정으로 넘어가면 주어진 조건 중에서 특정 값(최댓값, 최솟값)을 찾는 문제로 확장할 수 있습니다.

정답이 특정 조건을 만족하는 최솟값 또는 최댓값인 경우, 조건을 만족하는지의 여부를 기준으로 이분 탐색을 적용할 수 있습니다.

문제를 풀며 응용을 이해해봅시다.  


<a href="https://www.acmicpc.net/problem/17179" target="_blank">baekjoon 17179. 케이크 자르기</a>

이분 탐색을 활용해 문제를 풀 때, 중요하게 생각해야 할 것은 탐색 범위를 잡을 기준입니다.  
이 문제에서는 케이크를 자를 수 있는 최소 간격의 최댓값을 구해야 하므로, 이 간격을 이분 탐색의 대상 값으로 설정합니다.  
  
간격을 mid라고 했을 때, mid 이상의 간격으로 케이크를 최소 K조각 낼 수 있는지를 판단하는 조건 함수를 정의하고,
이를 통해 조건을 만족하는 가장 큰 간격(mid)의 최댓값을 구하는 형태로 문제를 풉니다.  

이 접근을 이해하고 문제를 해결할 수 있다면 다음 문제들도 비슷한 방식으로 해결할 수 있습니다.

<a href="https://www.acmicpc.net/problem/2110" target="_blank">baekjoon 2110. 공유기 설치</a>

공유기 간의 최소 거리를 mid로 두고,  
그 거리 이상으로 C개의 공유기를 설치할 수 있는지를 판단합니다.

<a href="https://www.acmicpc.net/problem/1654" target="_blank">baekjoon 1654. 랜선 자르기</a>

자를 길이(mid)를 기준으로,  
해당 길이로 잘랐을 때 N개 이상을 만들 수 있는지 확인합니다.

---

이처럼, 다음과 같은 조건이 있다면 이분 탐색 문제임을 의심해볼 수 있습니다.

- 조건을 만족하는 값 중에서 최대/최소를 구하는 경우  

- 문제를 결정 문제 형태(길이가 X일 때 가능한가? → Yes/No)로 변환 가능한 경우  

- 탐색해야 하는 범위가 너무 커서 완전 탐색으로는 제한 시간 내 해결할 수 경우  

- 배열이 정렬되어 있거나 정렬하더라도 문제의 의미가 바뀌지 않는 경우  


이분 탐색은 단순히 정렬된 배열에서 값을 찾는 데 그치지 않고,  
조건을 만족하는 값을 효율적으로 찾는 다양한 문제에 적용되는 유용한 알고리즘입니다.

그리고 이분 탐색은 연습하면 할수록 감이 잡히는 알고리즘이기도 합니다.  
처음엔 어렵게 느껴지더라도, 대표적인 문제들을 반복해서 풀면서  
**“탐색 대상이 값이 될 수도 있다”**는 관점을 익히는 것이 중요합니다.


## 참고자료
<a href="https://cplusplus.com/reference/algorithm/binary_search/" target="_blank">binary_search() details</a>  
<a href="https://cplusplus.com/reference/algorithm/upper_bound/" target="_blank">upper_bound details</a>  
<a href="https://cplusplus.com/reference/algorithm/lower_bound/" target="_blank">lower_bound details</a>  
<a href="https://en.wikipedia.org/wiki/Binary_search " target="_blank">binary search with wikipedia</a>  