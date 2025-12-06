# 任务1：计算立方和

## 描述

输入 `3` 个十进制整数，计算它们的立方和并输出。

## 输入

输入 `3` 个十进制整数，范围是 `-100 ∼ 100`，中间以空格分隔。

## 输出

输出它们的立方和。

## 样例

### 输入
```
1 2 3
```

### 输出
```
36
```
## 解析
直接写就完了。

## 解答

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int i, j, k;
	cin >> i >> j >> k;
	cout << i * i * i + j * j * j + k * k * k << endl;
}
```

---

# 任务2：统计奇数和

## 描述

输入一个数组，计算给定下标区间 `[L, R]` 内的奇数和 `S`。数组的下标 `L`, `R` 从 `0` 开始计。

例如：数组为 `11, 33, 21, 15, 26, 78, 98, 23, 37, 22`， `L=3`， `R=7`，则 `S = 15 + 23 = 38`。

## 输入

输入 `1` 个正整数 `N`，表示数组的大小，范围是 `1 ∼ 100`。

然后输入 `N` 个整数，空格隔开，表示数组的内容，范围是 `0 ∼ 100000`。

最后输入 `2` 个整数 `L` 和 `R`，表示计算区间，满足 `0 ≤ L, R < N`。

## 输出

输出奇数和 `S`。

## 样例

### 输入
```
10
11 33 21 15 26 78 98 23 37 22
3 7
```

### 输出
```
38
```
## 解析
用前缀和，边读数边维护求和数列，最后两端点相架就得到了中间部分的和。
## 解答

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
	int n;
	cin >> n;
	vector<long long> v(n);
	vector<long long> sum(n + 1, 0);
	for (int i = 0; i < n; i++) {
		cin >> v[i];
	}
	for (int i = 0; i < n; i++) {
		if (v[i] % 2 == 1) {
			sum[i + 1] = sum[i] + v[i];
		} else {
			sum[i + 1] = sum[i];
		}
	}
	int L, R;
	cin >> L >> R;
	cout << sum[R + 1] - sum[L] << endl;
	return 0;
}
```

---

# 任务3：字符串操作员

## 描述

小白是一个字符操作员，对于给定的一行字符串，他根据输入的指令串对字符串进行操作，指令包括：

- **L**：把字符串膨胀`2`倍，例如 `abc -> aabbcc`
- **R**：把字符串进行翻转，例如 `abc -> cba`
- **Dn**，其中 `n` 为一个整数：删除中心左侧的 `n` 个字母，如果字符数不够，则删除左侧全部字符。
  - 例如：
    - `D1`：`abcde -> acde`
    - `D2`：`abcde -> cde`
    - `D1`：`abcd -> acd`
    - `D3`：`abcd -> cd`

请编写程序帮助小白完成他的工作，并计算执行完成指令串后字符串第 `k` (从 `1` 开始计)个位置上的字母是什么？

## 输入

第一行输入字符串 `S`，长度不超过 `32`。其中可能包含空格。

随后若干行，每行为一组指令。前半部分为指令串，长度不超过 `16`。随后为整数 `k`，中间以空格分隔。

指令 `End` 表示结束输入。

可以保证，执行指令过程中的字符串长度不会超过 `1000`。

## 输出

对每一组指令，按照要求从字符串 `S` 开始，依次执行指令后，得到一个字符串，输出该字符串第 `k` 个位置上的字符。

如果没有满足条件的字符则输出 `NULL`。

每组指令都在原始字符串上进行操作，每组指令的输出占一行。

## 样例

### 输入
```
abcd
RD2L 3
D3R 20
End
```

### 输出
```
a
NULL
```

### 解释
对于第1条指令，`abcd -> dcba -> ba -> bbaa`，所以第3个字符是 `a`；
对于第2条指令，`abcd -> cd -> dc`，没有第20个字符，所以结果是 `NULL`。

## 解析
手写三个函数就完事。


## 解答

```cpp
#include <bits/stdc++.h>
using namespace std;

string s;

void L() {
	int length = s.length();
	string temp = "";
	for (int i = 0; i < length; i++) {
		temp += s[i];
		temp += s[i];
	}
	s = temp;
}
void R() {
	reverse(s.begin(), s.end());
}

void D(int n) {
	int length = s.length();
	int center_left;
	if (length % 2 == 1) {
		center_left = length / 2;
	} else {
		center_left = length / 2 - 1;
	}
	int delete_count = min(n, center_left + 1);
	string temp;
	for (int i = 0; i < center_left + 1 - delete_count; i++) {
		temp += s[i];
	}
	for (int i = center_left + 1; i < length; i++) {
		temp += s[i];
	}
	s = temp;
}

int main() {
	getline(cin, s);
	string original = s;
	string command;
	int n;
	while (1) {
		cin >> command;
		if (command == "End") {
			return 0;
		} else {
			cin >> n;
		}
		s = original;
		for (int i = 0; i < command.length(); i++) {
			if (command[i] == 'L') {
				L();
			} else if (command[i] == 'R') {
				R();
			} else if (command[i] == 'D') {
				int num = 0;
				i++;
				while (i < command.length() && isdigit(command[i])) {
					num = num * 10 + (command[i] - '0');
					i++;
				}
				i--;
				D(num);
			}
		}
		if (n < 1 || n > s.length()) {
			cout << "NULL" << endl;
		} else {
			cout << s[n - 1] << endl;
		}
	}
}
```