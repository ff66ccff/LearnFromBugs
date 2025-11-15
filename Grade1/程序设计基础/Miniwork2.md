# 题目
把 1/7 化成循环小数，求小数点后面第199位上的数字是几？

 

要求：

输入3个正整数，分子 分母 位数

输出：小数形式，非循环部分 + 循环节（用括号括起来）位数数字[用括号括起来]

示例：
输入：1 3 10 → 输出：0.([3])
输入：1 6 1 → 输出：0.[1](6)
输入：1 4 5 → 输出：0.25
输入：1 7 199 → 输出：0.([1]42857)


# 解析
手搓一个高精度除法，用字符串string实现。
关键是找到循环节，这里简单看作一个数字出现两次就形成了循环节。
找到循环节后就不需要继续计算了，之后要找到第n位数字位于哪里。
然后输出的时候找一下对应数字。


# 代码
```
#include <bits/stdc++.h>
using namespace std;

int main() {
	int a, b, n;
	cin >> a >> b >> n;
	string c;
	string integer_part = to_string(a / b); //整数部分
	int remain = a % b;
	if (remain == 0) {
		cout << integer_part << ".0" << endl;
		cout << "第" << n << "位数字是: 0" << endl;
		return 0;
	}
	string decimal_part;
	vector<int> remain_pos(b, -1);
	vector<int> digits;
	int pos = 0;
	int cycle_start = -1;
	while (remain != 0) {
		if (remain_pos[remain] != -1) {
			cycle_start = remain_pos[remain];
			break;
		}
		remain_pos[remain] = pos;
		remain *= 10;
		int digit = remain / b;
		digits.push_back(digit);
		remain %= b;
		pos++;
	}//找出循环节开始
	string non_cycle;
	string cycle;
	if (cycle_start == -1) {
		for (auto &i : digits) {
			non_cycle += (i + '0');
		}
	} else {
		for (int i = 0; i < cycle_start; i++) {
			non_cycle += (digits[i] + '0');
		}
		for (int i = cycle_start; i < digits.size(); i++) {
			cycle += (digits[i] + '0');
		}
	}
	// 计算第n位数字的值
	int digit_at_n;
	if (n <= non_cycle.length()) {
		digit_at_n = non_cycle[n - 1] - '0';
	} else if (!cycle.empty()) {
		int offset = n - non_cycle.length();
		digit_at_n = cycle[(offset - 1) % cycle.length()] - '0';
	} else {
		digit_at_n = 0;
	}

	cout << integer_part << ".";
	if (cycle_start == -1) {
		for (int i = 0; i < non_cycle.length(); i++) {
			if (i == n - 1) {
				cout << "[";
			}
			cout << non_cycle[i];
			if (i == n - 1) {
				cout << "]";
			}
		}
	} else {
		for (int i = 0; i < non_cycle.length(); i++) {
			if (i == n - 1) {
				cout << "[";
			}
			cout << non_cycle[i];
			if (i == n - 1) {
				cout << "]";
			}
		}
		if (n > non_cycle.length()) {
			int cycle_pos = (n - non_cycle.length() - 1) % cycle.length();
			cout << "(";
			for (int i = 0; i < cycle.length(); i++) {
				if (i == cycle_pos) {
					cout << "[";
				}
				cout << cycle[i];
				if (i == cycle_pos) {
					cout << "]";
				}
			}
			cout << ")";
		} else {
			cout << "(" << cycle << ")";
		}
	}
}
```