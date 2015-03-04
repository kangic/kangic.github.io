---
layout: post
title:  "codeforces #294"
date:   2015-02-27 01:32:10
categories: codeforces
---

* 2015.2.28 PM 10:00, Div 2

### 후기
첫 번째 문제는 굉장히 손쉽고 빠르게 잘 풀었다. 점수 얼마 안 깎이고 400점 후반대를 기록했고, 두 번째 문제로 넘어갔는데 여기서부터 나의 삽질은 시작되었다.
사실 문제 자체가 어렵다라기보다는 두뇌회전이 느린데다가 쓸데없이 양질의 방법을 찾아보려 한게 화근이 되었다. 대회 끝나고 보니 대부분 평이한 방법으로들 잘 풀었는데, 나는 괜한 욕심 부리다가 결국 아무것도 이루지 못한 꼴이 되어버렸다...ㅠㅠ
해결이 우선이다. 최적의 해를 찾는건 그 다음 스텝으로 진행해도 늦지 않다.
오늘의 삽질을 교훈삼아 같은 실수를 반복하지 말아야겠다.

rating은 1251로 떨어졌다.

### 문제

#### A

체스판에 white와 black의 말들이 주어질 때, 가중치를 구해서 대소 구분하여 더 큰 쪽 혹은 "Draw"문자열 출력

쉬운 문제였다. 입력과 동시에 white와 black의 가중치를 미리 합산하여 비교하면 끝.

```c++
std::string white_s = "QRBNP";
std::string black_s = "qrbnp";
int weights[5] = {9, 5, 3, 3, 1};

int main(void) {
	int white = 0, black = 0;

	for (int i=0; i<8; ++i) {
		for (int j=0; j<8; ++j) {
			char c;
			std::cin >> c;

			if (c != '.') {
				int idx = white_s.find_first_of(c);
				if (idx != std::string::npos) { // white
					white += weights[idx];
				}
				// black 
				else {
					idx = black_s.find_first_of(c);
					black += weights[idx];
				}
			}
		}
	}

	if (white > black)
		std::cout << "White";
	else if (black > white)
		std::cout << "Black";
	else
		std::cout << "Draw";

	return 0;	
}
```

#### B

n 개의 컴파일 에러 번호 리스트가 주어진다.
하나의 에러를 줄이고 나면 없어진 에러 번호를 뺀 나머지 리스트가 출력된다.
총 2개의 에러를 줄인 후 최종적으로 주어진 리스트를 봤을 때 없앤 에러 번호를 출력하는 문제.

이건 처음에 만만히 보고 시작했다가 local 테스트에서 두번 째 예제를 통과 못해서 보니...에러 번호는 중첩이 될 수 있었다.(그렇지...컴파일 에러 번호니까 중첩될 수 있지..어쩐지;;)

다른 방법 생각하다가 잘 생각 안나서 세 번째 문제로 먼저 넘어가봤다..

나중에 다른 사람들이 풀어놓은 코드들을 보고 있었는데 대부분의 사람들은 리스트나 큐 등을 이용해서 푼 반면, 한 명이 재치있게 풀어서 그 사람 소스를 보고 난 뒤 풀이해봤다.
어차피 에러 줄이기는 2단계로 정해진 문제니까 아래 코드가 충분히 가능한 케이스!!

```c++
#include <iostream>
#include <algorithm>

int main(void) {
	int table[100001] = {0, };
	int table2[100001] = {0, };
	int table3[100001] = {0, };

	int n;
	std::cin >> n;

	int sum1 = 0, sum2 = 0, sum3 = 0;

	// first
	int input;
	for (int i=0; i<n; ++i) {
		std::cin >> input;
		sum1 += input;
	}

	// second
	for (int i=0; i<n-1; ++i) {
		std::cin >> input;
		sum2 += input;
	}

	// third
	for (int i=0; i<n-2; ++i) {
		std::cin >> input;
		sum3 += input;
	}

	std::cout << sum1 - sum2 << std::endl;
	std::cout << sum2 - sum3 << std::endl;

	return 0;
}
```

#### C

프로그래밍 콘테스트에 나간 experienced participants와 newbies의 숫자가 주어졌을 때 최대로 이룰 수 있는 팀의 수 구하기.

팀은 3명으로 구성되며, (XP, XP, NB) or (XP, NB, NB)로 구성될 수 있다.

코드 작성하고 예제들 확인 후 서브밋 했는데 시스템 테스트 실패!
원인은 time limiti exceeded! 재귀 호출로 (XP, XP, NB) 로 시작하는 것과 (XP, NB, NB)로 시작하는 케이스에 대해 구한 뒤 max 값을 얻도록 작성했는데 시스템 테스트에서 큰 값이 들어가니..ㅠㅠ

콘테스트 종료 후 재귀호출 없애고 그냥 편하게 아래처럼 while 돌리면서 조건만 잘 체크하니 무사 통과;;;

앞으로는 그냥 brute force로 풀 수 있는지 먼저 확인해보고 나서 접근해야겠다.
아쉽다. 쉬운 문제였는데..

```c++
#include <iostream>
#include <algorithm>

int main(void) {
	int x, n;

	std::cin >> x >> n;
	
	int cnt = 0;
	while (x > 0 && n > 0) {
		if (x > n) {
			if (x >= 2 && n >= 1) {
				x -= 2;
				n -= 1;
				++cnt;
			}
			else {
				break;
			}
		}
		else {
			if (n >= 2 && x >= 1) {
				n -= 2;
				x -= 1;
				++cnt;
			}
			else {
				break;
			}
		}
	}

	std::cout << cnt;
			
	return 0;
}
```
