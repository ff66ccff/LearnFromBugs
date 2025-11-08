# Miniwork
# 题目
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