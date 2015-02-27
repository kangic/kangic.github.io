---
layout: post
title:  "c++에서 new 예외 처리"
date:   2015-02-17 00:12:00
categories: topcoder
---

사실 요즘의 PC 환경에서 일반적인 프로그램을 작성할 때 메모리 할당에 대한 예외 처리를 안해도
엄청나게 큰 문제를 일으키지 않을 수도 있다.(엄청 큰 메모리를 할당하지 않는 이상..)

그러나 임베디드 환경 등의 특수한 경우 자칫 메모리가 풀로 차거나 시스템에서 더이상 할당할 메모리가 없음에도
특별한 예외 처리를 하지 않는다면...그 뒤의 파장은..
<br>
## new를 잘못 처리하고 있는 예

* 아무 처리 안함..
* new로 객체 생성 후 NULL check를 통해 예외 처리

```cpp
char* test = new test[10000000L];
if (NULL == test)	// 여기서의 체크는 아무 의미 없다..
```

그러면 어떻게 처리를 해야 할지는 아래를 참고..ㅎㅎ

<br/>
## new에 대한 예외 처리 방법 3가지
* set_new_handler를 통해 할당 에러 처리자를 등록한다.

```cpp
void outOfMem() {
	std::cerr << "Unable to satisfy request for memory\n";
	std::abort();
}

int main() {
	std::set_new_handler(outOfMem);

	int *pBigDataArray = new int[10000000L];
}
```

* try..catch를 통해 예외를 발생시킨다.

```cpp
try {
	char* test = new test[10000000L];
} catch (std::bad_alloc& exc) {
	// exception 처리
}
```

* std::nothrow를 통해 new 할당 실패시 NULL을 리턴하도록 한다.

```cpp
char* test = new (std::nothrow) test[10000000L];
if (NULL == test) {	// 위에서와는 다르게 여기서는 NULL 체크가 유효!!
	// 예외 처리
	...
}
```

<br/>
보다 더 자세한 내용을 원하시면 Effective C++ chapter 49를 참조하시면 됩니다..^^;;
