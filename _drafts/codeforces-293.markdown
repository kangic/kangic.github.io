---
layout: post
title:  "codeforces #293 회고"
date:   2015-02-27 00:32:10
categories: codeforces
---

* 2015.2.25 AM 1:30, Div 2

### contest 후기
첫 번째 문제를 예상외로(?) 빨리 풀어서 기분 좋게 시작하는가 싶었는데, Code Hacking 당했다는 메시지가 떴다. 당황. Status를 보니 룸 전체가 Hacked로 도배..<br/>
문제를 푼 한 사람이 많은 이들이 간과한 입력셋을 가지고 무차별 Hacking하여 성공..ㅎㅎ<br/>
혹시나 했던 마음에서 역시나로 바뀌는 마인드를 다시 부여잡고 총 3문제를 풀어보자고 도전한 이번 라운드 역시 실패로 돌아갔다...ㅠㅠ 샘플 예제들을 통과한 뒤 시스템에서 돌아가는 예제들에서 계속 실패는 하는데 문제를 다시 읽어봐도 어디서 이해를 잘못한건지 잘 찾지 못하고, 문제에 대한 이해를 잘 못하니 풀이가 안되는건 당연지사..결론은 영어가 문제다!<br/>

푹 자고 아침에 일어나 확인해보니 역시나 rating은 1339로 떨어졌다.

### 회고
이대로 넘어갈 수는 없으니 도전했던 문제들에 대해 천천히 한 번 풀어보면서 부족한 점을 짚고 넘어가려고 회고를 해보았다. 오픈했던 3문제만이라도 제대로 확인해보자!

#### A. Vitaly and Strings

* 문제 정의
  * 입력
    * s, t : 문자열
  * s와 t 문자열간(둘 다 소문자로 구성) 존재할 수 있는 아무 문자열 하나 출력
  * s가 t보다 작고, 둘의 길이가 같다는 조건이 보장됨

* 틀린 이유

```cpp
#include <iostream>

int main(void) {
	std::string s, c;

	std::cin >> s;
	std::cin >> c;

	std::string found = "";
	for(int i=0; i<s.length(); ++i) {
		if(c[i] - s[i] > 1)
			found += s[i] + 1;
		else if(c[i] == s[i])
			found += c[i];
		// (1) else에 관한 처리가 없다!!
	}

	if (found.length() == s.length()) // (2) 위에서 else를 처리하더라도 여기서 found와 c가 같은걸 거르지 않아 문제 발생!
		std::cout << found;
	else
		std::cout << "No such string";

	return 0;
}
```

  * 우리 room에서 대부분 hacked 된 test case가 {"aab", "aca"} 문자열 구성이었다.
  * 차후 저 (1), (2)에 대한 조건을 추가했지만 시스템 테스트는 통과하지 못했다!
  * 시스템 통과를 못한 문자열은 {"pcncl", "pcndf"} 였는데
    * 실제로 pcnda, pcndb등의 문자열이 올 수 있는데 저 위 소스로는 No such string이 나온다..ㅠㅠ

* 통과한 소스

```cpp
#include <iostream>

int main(void) {
	std::string s, c;

	std::cin >> s;
	std::cin >> c;

	for (int i=s.length() - 1; i>=0; --i) {
		if (s[i] == 'z')
			s[i] = 'a';
		else {
			++s[i];
			break;
		}
	}

	if (s < c)
		std::cout << s;
	else
		std::cout << "No such string";
	
	return 0;
}
```

  * 원 문자열을 통해 변경이 일어나기 때문에 found라는 문자열로 복사도 안일어남.
  * 변경 문자열이 'z'->'a'가 아닌 케이스라면 찾아낸 것이기 때문에 break하여 쓸데없이 for문을 돌지 않음.

#### B. Tanya and Postcard

* 문제 정의
  * 입력
    * s, t : 문자열
  * s 문자열 내에 있는 한 문자를 t 문자열에서 찾기(대소문자 구분)
  * 있으면 "YAY", 값은 맞는데 대소문자가 다르다면 "WHOOPS"
  * "YAY" / "WHOOPS" 외친 횟수 출력

* 틀린 이유
  * 문제를 대충 읽고 지나감
    * 결국은 부족한 영어 실력
    * 문제를 읽고 바로 풀었을 때 전체 s문자열 길이에서 같은 값을 찾은 YAY 횟수를 빼서 WHOOPS 값을 구했다.
  * 실제 문제에서의 정의는 s에서 'a'와 t에서 'A'일 때가 WHOOPS

* 통과한 소스

```cpp
#include <iostream>
#include <string>

int main(void) {
	std::string s, t;

	std::cin >> s;
	std::cin >> t;

	int asc_cnt[128] = {0, };
	for (int i=0; i<t.size(); ++i) {
		++asc_cnt[t[i]];
	}
	
	int yay = 0;
	int whoops = 0;
	int found[200005] = {0, };

	for (int i=0; i<s.size(); ++i) {
		if (asc_cnt[s[i]]) {
			--asc_cnt[s[i]];
			++yay;
			found[i] = 1;
		}
	}

	for (int i=0; i<s.size(); ++i) {
		int conv_asc;
		if (s[i] >= 'a' && s[i] <= 'z')
			conv_asc = s[i] - 32;
		else if (s[i] >= 'A' && s[i] <= 'Z')
			conv_asc = s[i] + 32;

		if (asc_cnt[conv_asc] && !found[i]) {
			--asc_cnt[conv_asc];
			++whoops;
		}
	}

	std::cout << yay << " " << whoops;

	return 0;
}
```

#### C. Anya and Smartphone

* 문제 정의
  * 입력
    * 1st line => n, m, k : 어플리케이션의 갯수, 실행할 어플리케이션의 갯수, 한 화면당 아이콘의 갯수
    * 2nd line => 스마트폰에 배치되어 있는 어플리케이션 id값 목록(n개)
    * 3rd line => 실행할 어플리케이션 목록(m개)
  * 조건
    * n번 째 화면의 아이콘을 클릭하여 어플리케이션을 실행시키면 n-1번의 스크롤과 1번의 터치가 있어야 함.  
	  * 맨 앞의 어플리케이션(1번에 배치된)을 제외하고 나머지가 실행될 시 자기 앞의 어플리케이션 위치와 서로 맞바꾼다.(배치되어 있는 화면이 바뀔 수도 있다.)
  * 문제
    * 어플리케이션 목록에 있는 어플리케이션들을 순차적으로 실행했을시 드는 gesture는 몇 번인가?

* 틀린 이유
  * 문제를 대충 이해하고 넘어간 나의 패착
    * 한 화면당 아이콘 갯수가 주어지는 거였는데 스크린 갯수가 주어지는 것으로 오인
  * 그래서 한 화면당 아이콘 갯수를 구해버리고 문제를 품..
    * 애석하게도 문제에 나온 예제 1, 2의 케이스가 맞아 떨어져 한참을 헤매고 A, B 두 문제를 계속 오가며 풀다 결국 시간 부족으로 못 품..

* 통과한 소스

```cpp
#include <iostream>

#define MAX_NUM 100000

int main(void) {
	int n, m, k;
	int menu[MAX_NUM + 2] = {0, };
	int place[MAX_NUM + 2] = {0, };
	
	std::cin >> n >> m >> k;

	for (int i=1; i<=n; ++i) {
		std::cin >> menu[i];
		place[menu[i]] = i;
	}

	unsigned long long total = 0;
	for (int i=0; i<m; ++i) {
		int val;
		std::cin >> val;

		total += ((place[val]-1) / k) + 1;
		std::cout << "total : " << total << std::endl;

		if (place[val] > 1) {
			// place 와 menu 모두 변경해줘야한다!
			int tmp_place = place[val];
			place[val] = place[menu[tmp_place-1]];
			menu[tmp_place] = menu[tmp_place-1];
			place[menu[tmp_place-1]] = tmp_place;
			menu[tmp_place-1] = val;

			for (int j=1; j<=n; ++j)
				std::cout << menu[j] << " ";
			std::cout << std::endl;
		}
	}

	std::cout << total;

	return 0;
}	
```

### 총평
역시나 졸전이었다. 가장 큰 문제점은 역시나 문제의 이해. 문제를 제대로 정의하기만 해도 반은 푼거라는 말을 어느 책에서 보았는데, 그게 맞는 말인 것 같다. 시간을 들여 문제를 천천히 읽고 제대로 이해한 뒤 풀이에 들어가니 시간이 조금 더 걸릴 뿐 문제 자체는 해결할 수 있었다.(비록 유려한 풀이 방법인지는 모르겠지만 ^^;;)<br/>
문제를 조금 늦게 풀더라도 제대로 된 문제 풀이를 하는게 더 중요한 것 같다. 다음 번에는 점수에 연연하지 말고(물론 완벽히 그럴 수는 없겠지만..) 한 문제라도 제대로 푸는 과정을 거쳐야겠다.
