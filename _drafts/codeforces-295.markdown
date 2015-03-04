---
layout: post
title:  "codeforces #295"
date:   2015-03-04 16:42:10
categories: codeforces
---

* 2015.03.02 PM 04:00, Div 2

### 후기
첫 문제를 비교적 빠른 시간안에 풀고 기분 좋게 점수 확인한 뒤에 두 번째 문제로 넘어갔는데 턱 막혔다..이건 프로그램이 문제가 아니라 수학적 사고력이 많이 떨어져 있음을 느낄 수 있었던 문제였다. 세 번째 문제로 바로 넘어가볼까 했지만 한 문제씩 차근차근 풀어나가는 방식으로 접근해보고자 한 저번의 다짐을 지키려고 두 번째 문제를 최대한 오래 잡고 있었으나, 결국 풀지 못하고 내 머리를 탓하며 포기!

예제 케이스를 넘기고 서브밋 후 시스템 테스트에서 통과를 못하는데 어떤 테스트 케이스인지를 몰라서 반증을 찾으려고 엄청 헤매기만 했다. 문제에 맞게 알고리즘을 바로 설계해서 한 번에 통과하면 모르겠지만, 반례를 찾아내는 방법에 대한 연습도 좀 해야겠다고 느꼈다.

rating은 1207로 또 하락..

### 문제

contest 도중 시도한 두 문제에 대해서만 확인해보자.(기왕이면 다 해야 하는데..실력이 잘 늘지 않는 이유가 여기있는듯 ㅋ)

#### A

이건 쉬웠다. 대소문자가 섞인 string이 주어지는데 대소문자 구분 없이 alphabet 모두가 한 번씩 출력됐는지 체크하는 문제.

```c++
#include <iostream>
#include <string>

int main(void) {
	int n;
	std::string str;

	std::cin >> n;
	std::cin >> str;

	int cnt[27] = {0, };
	
	for (int i=0; i<str.size(); ++i) {
		if (str[i] >= 'a' && str[i] <= 'z')
			++cnt[str[i] - 'a'];
		if (str[i] >= 'A' && str[i] <= 'Z')
			++cnt[str[i] - 'A'];
	}

	int sum = 0;
	for (int i=0; i<27; ++i) {
		if (cnt[i]) {
			++sum;
		}
	}

	if (sum == 26)
		std::cout << "YES";
	else
		std::cout << "NO";

	return 0;
}
```

#### B

문제는 숫자 A, B가 주어졌을 때 빨간 버튼은 A값을 두 배로, 파란 버튼은 하나 감소하는 장치를 이용해서 B값으로 도달하는 최소값을 구하는 문제이다.

처음에 문제를 보고 크게 어렵지 않을 거라고 생각하고 풀었는데, 결국은 system test case fail에 대한 반례를 못 찾아내고 contest 종료할 때까지 submit 못했다..ㅠㅠ

끝나고 나서 다시 천천히 생각해보니 문제를 풀 수 있었고, 때는 이미 늦었다!
굳어있는 수학적 사고력을 끌어올리도록 노력해야겠다..

```c++
#include <iostream>

int main(void) {
	int n, m;

	std::cin >> n >> m;

	if (n >= m) {
		std::cout << n - m;
		return 0;
	}

	int cnt = 0;
	while (n < m) {
		if (m % 2) {
			++cnt;
			++m;
		}

		m /= 2;
		++cnt;
	}

	std::cout << cnt + n - m;

	return 0;
}
```
