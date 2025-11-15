# Miniwork
# 题目1
一个6位数6ABABA是6倍数，这样的六位数共有多少个？请列出它们。

# 解析
构造满足6ABABA格式的数，验证这个数模6是否为0。

# 代码
```
#include <bits/stdc++.h>
using namespace std;

int main() {
	int result = 0;
	int amount = 0;
	for (int a = 0; a <= 9; a++) {
		for (int b = 0; b <= 9; b++) {
			result = 6 * 100000 + a * 10101 + b * 1010;
			if (result % 6 == 0) {
				amount++;
				cout << result << endl;
			}
		}
	}
	cout << "一共有" << amount << "个" << endl;
}

```

# 题目2
一个两位数是质数，个位上的数字与十位上的数字交换位置后，仍然是一个质数，这样的质数有几个？各是哪些数呢？

# 解析
循环判断，先判断这个数是否为质数，再判断这个数变换后是否为质数。  

# 代码
```
#include <bits/stdc++.h>
using namespace std;

vector<int> ret;

bool check(int n) {
	if (n == 0 || n == 1) {
		return false;
	}
	if (n == 2) {
		return true;
	} else if (n % 2 == 0) {
		return false;
	} else {
		for (int i = 3; i * i <= n; i++) {
			if (n % i == 0) {
				return false;
			}
		}
		return true;
	}
}

int fswap(int a) {
	int temp = 0;
	while (a > 0) {
		temp = (temp * 10) + (a % 10);
		a /= 10;
	}
	return temp;
}

int main() {
	for (int i = 10; i <= 99; i++) {
		if (check(i) && check(fswap(i))) {
			ret.push_back(i);
		}
	}
	cout << ret.size() << "个" << endl;
	for (auto &i : ret) {
		cout << i << endl;
	}
}

```