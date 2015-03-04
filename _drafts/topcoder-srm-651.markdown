---
layout: post
title:  "topcoder srm 651"
date:   2015-03-04 17:00:10
categories: topcoder 
---

* 2015.03.01 AM 02:00, Div 2

### 후기

문제 열심히 잘 풀고 submit 하려는데 안되서 한참 고생하고 있다보니 나 말고도 여럿 그런 현상이 있는터라 이번 라운드는 rating에 반영되지 않는다는 broadcasting이 왔다.ㅎㅎ

나는 문제를 하나 푼터라 좀 아쉬웠다. rating을 올릴 수 있는 찬스를 놓쳐서 ㅠㅠ

### 문제

#### 250

Map이 주어지고 장애물 및 로봇의 위치를 보고 로봇이 명령줄 string에 따라 움직일 때 최종적으로 사는지 죽는지 출력하는 문제.
Map을 벗어나게 되면 로봇은 죽는다.

Map의 구성은 `'S' : 로봇의 위치, '#' : 장애물, '.' : 이동 가능 지역` 으로 구성되어 있다.

```c++
class RobotOnMoonEasy
{
public:
	string isSafeCommand(vector <string> board, string S)
	{
		int r_x = 0, r_y = 0;
		int b_x = board[0].size();
		int b_y = board.size();

		for (int i=0; i<b_y; ++i) {
			std::string text = board[i];
			for (int j=0; j<b_x; ++j) {
				if (text[j] == 'S') {
					r_x = j;
					r_y = i;
					break;
				}
			}
		}

		for (int i=0; i<S.size(); ++i) {
			if (S[i] == 'U') {
				if (r_y == 0)
					return "Dead";
				std::string up_line = board[r_y-1];
				if (up_line[r_x] != '#')
					--r_y;
			}
			else if (S[i] == 'D') {
				if (r_y == b_y - 1)
					return "Dead";
				std::string down_line = board[r_y+1];
				if (down_line[r_x] != '#')
					++r_y;	
			}
			else if (S[i] == 'L') {
				if (r_x == 0)
					return "Dead";
				std::string curr_line = board[r_y];
				if (curr_line[r_x-1] != '#')
					--r_x;
			}
			else if (S[i] == 'R') {
				if (r_x == b_x - 1)
 					return "Dead";
				std::string curr_line = board[r_y];
				if (curr_line[r_x+1] != '#')
					++r_x;
			}
		}

		return "Alive";
	}
};
```

#### 500

선물 리스트가 주어지는데, 이걸 엄마, 아빠에게 동일한 갯수와 값(?)으로 나눌 수 있는지 여부를 출력하는 문제.
예를 들어 {1,2,3,4,5,6,7,8,9,10} 이렇게 주어지면 Impossible.
갯수는 짝수로 5개 나눌 수 있지만, 총 합이 55니까 동일한 값으로 나눌 수 있는 방법은 없다.

이건 시도했는데 시간 안에 못 풀었다..ㅠㅠ
아래 코드는 다 끝나고 나서 나중에 풀어본 소스..

```
int memo[51][51][3030];
vector<int> v;

class FoxAndSouvenirTheNext
{
public:
	string ableToSplit(vector <int> value)
	{
		if (value.size() % 2 != 0)
			return "Impossible";

		int sum = 0;
		for (int i=0; i<value.size(); ++i)
			sum += value[i];

		if (sum % 2 == 0) {
			memset(memo, -1, sizeof(memo));
			v = value;
			return solve(0, value.size() / 2, sum / 2) ? "Possible" : "Impossible";
		}

		return "Impossible";
	}

private:
	int solve(int idx, int n, int t) {
		int &ref = memo[idx][n][t];
		if (ref != -1)
			return ref;

		ref = 0;

		if (n == 0 && t == 0)
			return ref = 1;

		if (n == 0 || t == 0)
			return ref = 0;

		if (idx == v.size())
			return ref = 0;

		for (int i=idx; i<v.size(); ++i) {
			ref |= solve(i, n - 1, t - v[i]);
		}

		return ref;
	}


};
```

#### 1000

이건 그냥 open만 해보고 문제는 제대로 보지도 못하고 끝났다..에디토리얼 참고하자! ㅎㅎ

