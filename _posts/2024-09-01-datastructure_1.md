---
title: "[자료구조 C++]  배열(Array)"
excerpt: 배열(Array)
categories: 
  - datastructure
tags:
  - [argorithm, datastructure, Array]
date: 2024-09-01
toc: true
toc_sticky: true
lastmode: 2024-09-01 10:00:00
sitemap:
  changefreq: weekly
  priority : 0.5
---

### 1. 배열의 특징
    - 인덱스를 사용하여 원하는 원소에 바로 접근 가능하다.
    - 캐시 지역성: 배열의 각 원소는 서로 인접해 있기 떄문에 하나의 원소에 접근할 떄 그 근방에 있는 원소도 함께 캐시로 가져오게 된다.
    - 스택 메모리 영역에 할당한다. 

(1) std::array의 특징
- C언어 스타일 배열처럼 사용할 수 있는 [] 연산자 오버로딩을 제공한다.
- 대입 연산자를 지원(깊은 복사)
- 배열 크기를 정확하게 알 수 있다. => array::size();
- 반복자 지원(Iterator) 

(2) std::array의 단점
- std::array 배열은 크기를 명시적으로 지정한다.
- 스택 메모리를 사용한다. 
- 하지만 실제 사용시 고정 크기 배열이 아닌 가변 크기 배열을 주로 선호하며 vector 컨테이너를 주로 이용한다.

</br>

[C언어 스타일 배열]
```cpp
#include <iostream>

using namespace std;

int main()
{
    int scores[5] = {50, 60, 70, 80, 90};

    int sz = sizeof(scores) / sizeof(int);
    int s = 0;

    for (int i = 0; i < sz; i++){
        s += scores[i];
    }

    float m = (float) s / sz;

    cout << "Mean Score: " << m << endl;
}
```



[std::array를 이용한 배열]

```cpp
#include <array>
#include <iostream>

using namespace std;

int main()
{
    array<int, 5> scores = {50, 60, 70, 80, 90};

    int s = 0;

    for (int i = 0; i < scores.size(); i++){
        s += scores[i];
    }

    float m = (float) s / scores.size();
    cout << "Mean score: " << m << endl;
}
```



### 2. 동적 배열 메모리 해제
c++ 같은 경우에는 동적으로 생성된 메모리를 자동으로 해제시켜주지 않아 delete를 이용한 메모리 해제가 필수적이다.

```cpp
#include <iostream>

using namespace std;

int main(){
    int* ptr = new int[3] {};

    ptr[0] = 10;
    ptr[1] = 20;

    for (int i = 0; i < 3; i++){
        cout << ptr[i] << endl;
    }

    delete [] ptr; // 동적 배열을 생성했으면 ptr은 메모리 해제
    ptr = nullptr; // 그리고 nullptr을 통해 정리한다.
}
```



### 3. 템플릿을 사용하여 동적 배열 일반화 
- 템플릿을 이용하여 코드 재사용성 증가.
- 하지만 아직 크기를 사전에 정의해야함.

```cpp
#include <iostream>

using namespace std;

template <typename T>
class DynamicArray{
private:
    unsigned int sz;
    T* arr;

public:
    explicit DynamicArray(int n) : sz(n) { //생성자로 sz필드 초기화
        arr = new T[sz] {};
    }

    ~DynamicArray() { delete [] arr; }

    unsigned int size() { return sz; }

    T& operator[] (const int i) { return arr[i]; } //[] 연산자 추가
    const T& operator[] (const int i) const { return arr[i]; }

};

int main(){
    DynamicArray<int> arr(3);

    arr[0] = 10;
    arr[1] = 20;

    for (int i = 0; i < arr.size(); i++){
        cout << arr[i] << endl;
    }

    cout << arr.size() << endl;
}
```

### 4. vector를 이용한 동적 배열 사용
vector를 이용해야 런타임 중에 배열의 크기 조정 가능하다.

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

class Person{
public:
    string name;
    int age;
};

int main(){
    vector<Person> p1;

    p1.push_back({"kim", 10});
    p1.push_back({"aim", 20});
    p1.push_back({"sim", 30});
    p1.push_back({"dim", 40});

    sort(p1.begin(), p1.end(), [](const Person& a1, const Person& a2){
        return a1.name < a2.name;
    });

    for (const auto& p: p1){
        cout << p.name << ":" << p.age << endl;
    }
}
```
### 5. vector 함수 정리

(1) vector의 iterator
- v.begin(): 백터 시작 주소 반환
- v.end(): 백터 끝 부분 + 1 주소 반환
- v.rbegin(): 백터 끝 지점을 시작점으로 반환
- v.rend(): 백터의 시작점 + 1을 끝 지점으로 반환

(2) vector 요소 접근
- v.at(i): 백터 i번째 요소 접근
- v.[i](operator []): 백터 i번째 요소 접근
- v.front(): 백터 첫번쨰 요소 접근
- v.back():백터 마지막 요소 접근

(3) vector 요소 삽입
- v.push_back(): 백터 끝지점에 요소 추가
- v.pop_back(): 백터 끝지점 요소 제거
- v.insert(): 원하는 지점에 요소 삽입
- v.emplace(): 원하는 지점에 요소 삽입 (복사생성자X)
- v.emplace_back(): 백터 마지막 지점에 요소 삽입 (복사생성자X)
- v.erase(): 사용자 지정값 삭제
- v.clear(): 백터 모든 요소 삭제
- v.resize(): 백터 사이즈 조정
- v.swap(): 백터간의 스왑 연산

(4) vector 용량
- v.empty(): 백터가 비었으면 1, 값이 있으면 0 반환
- v.size(): 백터 크기 반환
- v.capacity(): 백터 공간 반환(전체 사이즈)
- v.max_size(): 백터가 만들어질 수 있는 최대 크기 반환
- v.reserve():백터 크기 조정
- v.shrink_to_fit():capacity 크기를 실제 백터 크기로 조정