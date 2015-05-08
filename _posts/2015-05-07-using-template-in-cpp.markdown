---
layout: post
title:  "c++ template 사용하기"
date:   2015-05-07 21:22:54
categories: cpp
---

C++을 사용해 프로그램을 만드는 것을 업으로 삼은지가 6년이 지났음에도 template 프로그래밍에 대한 얄팍한 이론적 지식만 있을 뿐 이를 이용해 제대로 된 프로그램을 단 한 번도 작성해본 적이 없었음에 스스로 놀라 이번 기회에 기본 개념과 함수와 클래스에서의 활용법에 대해 간략하게나마 정리해 놓고자 한다.

## Template이란?
C++에서의 Template은 Generic programming[^1]을 하기 위해 사용되는 도구이다.

흔히들 많이 비유하는 또 다른 설명으로는 "붕어빵을 만들기 위한 붕어빵 틀이다."가 있다.(나도 학교에서 그렇게 배웠다. ㅡㅡ;)

## 문법
* `template <typename T>` or `template <class T>`
  * C++에서의 class는 또 다른 의미가 있기 때문에, 개인적으로는 typename을 사용하는 것이 더 나은 것 같다.

## 제약사항
* template class를 작성할 때, 선언과 정의가 한 파일에 있어야 한다.
  * 개인적으로 사용한 방법은 *.h와의 구분을 위해서 *.hpp 확장자를 만들어 구현부를 넣어놓고 header file에서 include 시켰다.

## Template의 적용 예
C++을 이용해 프로그래밍을 해봤다면 STL은 한 번쯤은 들어봤을 것이다. Standard Template Library인데 C++ 프로그래머에게는 가장 중요한 라이브러리이다. 이 표준 라이브러리 안의 list, vector, map 등등이 template을 사용하여 구현되어 있다.(그러니까 이름에 Template이 들어가있겠지..ㅎㅎ)

Template을 사용한 클래스 혹은 함수는 객체 타입에 대해 따로 고려하지 않고 프로그램을 만들 수 있다. 예를 들어 두 개의 int형을 더해주는 함수를 만들었다고 치자. 추가적으로 double 타입에 대한 입력에 대해서도 처리하고자 한다고 하면 코드를 Ctrl + C, Ctrl + V해서 int를 double 타입으로 바꿔 작성해야 할 것이다. 이 때 template을 이용한 class를 작성하면 하나의 class code만으로 위의 상황에 적용할 수 있다.

설명은 이쯤하고, 함수에 적용된 템플릿의 간단한 예제를 한 번 살펴보자.

~~~ cpp
#include <cstdio>

class calc {
 public:
  template<typename T>
  T sum(T a, T b) {
    return a + b;
  }
};  // calc class

int main(void) {
  calc c;
  printf("%d\n", c.sum(3, 4));
  printf("%.2f\n", c.sum(3.3, 4.4));

  return 0;
}
~~~

위 코드를 컴파일하여 실행해보면, int와 double 타입의 덧셈에 대한 출력이 정상적으로 출력됨을 알 수 있다. 소스가 컴파일 될 때 코드에서 사용된 자료형인 int와 double 타입에 대한 sum 함수를 모두 만들어 놓은 것이다.[^2] 이 때문에 템플릿을 사용한 소스 코드의 컴파일이 간혹 굉장한 시간이 걸릴 때도 있다.

다음으로 클래스에 템플릿을 적용한 예제를 살펴보자. 아래는 array를 통해 만든 list 클래스인데, 템플릿 클래스에 대한 내용을 공부하고자 만든 예제이니(정말?) 허접해도 이해를 바란다...ㅠㅠ

클래스를 선언부에 template <typename T>를 통해 template class임을 선언하고, 멤버 함수 및 변수들에 T를 통해 자료형을 선언하거나 사용한 것을 확인하자. 또한 array\_list.hpp로 정의 부분을 따로 작성해 놓았으며, 헤더 파일 하단 부에 array\_list.hpp를 include 한 부분을 유의하자.

~~~ cpp
// array_list.h
#ifndef _ARRAY_LIST_H_
#define _ARRAY_LIST_H_

#define MAX_SIZE	100

template <typename T>
class array_list {
 public:
  array_list(void);
  virtual ~array_list(void);

  void add_to_front(const T& data);
  void add_to_back(const T& data);

  void delete_data(int pos);

  bool is_empty();
  bool is_full();

  int	size();
  const T& get_data(int pos);

  void clear();
  void display();

 private:
  T list[MAX_SIZE];
  int length;
};

#endif // _ARRAY_LIST_H_

#include "array_list.hpp"


// array_list.hpp
#include <iostream>

template <typename T>
array_list<T>::array_list(void) {
  length = 0;
}

template <typename T>
array_list<T>::~array_list(void) {

}

template <typename T>
void array_list<T>::add_to_front(const T& data) {
  for (int i=length; i>=0; --i) {
    list[i+1] = list[i];
  }

  list[0] = data;
  ++length;
}

template <typename T>
void array_list<T>::add_to_back(const T& data) {
  list[length] = data;
  ++length;
}

template <typename T>
void array_list<T>::delete_data(int pos) {
  list[pos] = 0;

  if (pos != length) {
    for (int i=pos+1; i<length; ++i) {
      list[i-1] = list[i];
    }
    list[length] = 0;
  }

  --length;
}

template <typename T>
bool array_list<T>::is_empty() {
  if (length != 0)
    return false;

  return true;
}

template <typename T>
bool array_list<T>::is_full() {
  if (length != MAX_SIZE)
    return false;

  return true;
}

template <typename T>
int	array_list<T>::size() {
  return length;
}

template <typename T>
const T& array_list<T>::get_data(int pos) {
  return list[pos];
}

template <typename T>
void array_list<T>::clear() {
  for (int i=0; i<MAX_SIZE; ++i) {
    list[i] = 0;
  }

  length = 0;
}

template <typename T>
void array_list<T>::display() {
  for (int i=0; i<length; ++i) {
    std::cout << list[i] << std::endl;
  }
}

// test_array_list.cc
#include "../array_list.h"

#include <iostream>

int main(void) {
  array_list<int> list;

  list.clear();

  list.add_to_back(100);
  list.add_to_back(30);
  list.add_to_front(40);

  std::cout << "length = " << list.size() << std::endl;
	
  list.add_to_front(70);
  list.add_to_back(80);

  std::cout << "length = " << list.size() << std::endl;

  list.display();

  int del_num = list.get_data(3);
  list.delete_data(3);
  std::cout << "delete = " << del_num << std::endl;

  std::cout << "length = " << list.size() << std::endl;

  list.display();

  return 0;
}
~~~

위 테스트 소스에서 볼 수 있듯이 클래스를 선언할 당시 사용할 타입을 정의한 후 사용하면 해당 타입을 통해서 클래스를 이용할 수 있다. 위의 예제에서는 int 타입에 대해서만 사용하였지만, 추가적으로 여러 타입들을 사용할 경우 이전에 언급한 바와 같이 컴파일 당시 여러 타입들에 대해 인스턴스를 만들기 위해 시간 및 크기가 커질 수 있음에 유의해야 한다.


## 마치며
템플릿에 관해 기본 개념과 함수 및 클래스에서의 사용 예를 살펴보았다. 전문가를 위한 C++ 2권을 보면 템플릿을 객체 지향 프로그래밍과 결합하면 강력한 효과를 발휘한다고 나와있다. STL만 놓고 보더라도 분명히 그런 것 같다.

템플릿에 관한 내용은 책 한 권으로 따로 출판이 되어 나올 정도로 내용도 많고 복잡하고 어렵다. 오늘 살펴본 내용은 극히 일부분, 초입 단계에 지나지 않는다. 템플릿에 대해 더 열심히 공부해서 더 많은 내용을 공유할 수 있도록 노력해야겠다!

[^1]: 데이터 타입에 의존하지 않고, 하나의 값이 여러 다른 데이터 타입들을 가질 수 있도록 하여 재사용성을 높일 수 있는 프로그래밍 방식
[^2]: object 파일의 내용을 보면 알 수 있다. objdump -t -C <filename>
