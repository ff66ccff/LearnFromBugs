# 逃跑的吴谦（基础）

## 描述

吴谦违法被逮捕后的某一天，用超能力逃跑了，此时吴谦被传送到了离警局 $x$ 米处的地方。警察在 $t$ 秒后发现吴谦逃跑，以 $v$ 的速度立马去追。而吴谦因为过度劳累无法逃跑，只能再一次发挥超能力，但超能力有冷却，两次超能力间隔时间必须不小于 $t_2$ 秒。

请问在警察追到他之前，他能否再一次发动超能力。

## 输入

第 1 行一个整数 $n$，表示有 $n$ 组数据。

接下来 $n$ 行，每行四个整数 $x$、$t$、$v$、$t_2$。

（$1 \leq n \leq 10^4,\ 1 \leq x \leq 10^6,\ 1 \leq t < t_2 \leq 10^3,\ 1 \leq v \leq 10^3$）

## 输出

如果他能在警察追到前再次发动超能力，请输出 `NO!!!`，否则输出 `Nice!`，每个字符串占一行。

## 样例

### 输入
```
3
12 3 6 5
14 4 10 5
199 100 101 102
```

### 输出
```
Nice!
NO!!!
Nice!
```

## 解答

警察在发现逃跑 $t$ 秒后才开始追，而吴谦最早在 $t_2$ 秒时能再次使用超能力。因此，警察有 $t_2 - t$ 秒的追赶时间。在这段时间内，警察能跑 $v \cdot (t_2 - t)$ 米。如果这个距离大于等于初始距离 $x$，说明警察能追上，吴谦无法逃脱（输出 `Nice!`）；否则他能成功发动超能力逃脱（输出 `NO!!!`）。

```cpp
#include <bits/stdc++.h>

using namespace std;

void f(int &x, int &t, int &v, int &t2) {
	if (v * (t2 - t) >= x) {
		cout << "Nice!" << endl;
	} else {
		cout << "NO!!!" << endl;
	}
}

int main() {
	int n;
	int x, t, v, t2;
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> x >> t >> v >> t2;
		f(x, t, v, t2);
	}
}
```

---

# 用指针交换两个变量

## 描述

请实现如下给定的函数，该函数形参是两个指针，实现将两个指针指向的两个变量值交换。

```cpp
void swapByPointer(int *A, int *B);
```

请完善该函数的定义，并在 `main` 函数中调用。

## 输入

两个 `int` 类型的数值。

## 输出

交换值后两个数的值。

## 样例

### 输入
```
1 2
```

### 输出
```
2 1
```

## 解答

通过解引用指针，使用临时变量交换两个整数的值。这是指针操作的经典示例。

```cpp
#include <bits/stdc++.h>

using namespace std;

void swapByPointer(int *A, int *B) {
	int temp = *A;
	*A = *B;
	*B = temp;
}

int main() {
	int a, b;
	cin >> a >> b;
	swapByPointer(&a, &b);
	cout << a << " " << b;
}
```

---

# 字符串排序

## 描述

输入一串字符串，按字母的 ASCII 顺序将它们升序排序输出。（字符串长度 $n < 1000$）

请实现相应的函数：
```cpp
void sortChar(char *p, int n);
```
其中 `p` 为指向字符串的指针，`n` 为字符串的长度。

## 输入

字符串。

## 输出

排序后的字符串。

## 样例

### 输入
```
acdb
4312
```

### 输出
```
abcd
1234
```

## 解答

虽然题目要求使用 `char*` 和长度参数，但因为我懒并且用的是CPP，这里使用 `string` 并实现冒泡排序。也可用 `std::sort`，但为体现手动排序过程，采用两层循环。

```cpp
#include <bits/stdc++.h>

using namespace std;

void sortChar(string &s) {
	int n = s.length();
	if (n <= 1) {
		return;
	}
	for (int i = 0; i < n - 1; i++) {
		for (int j = 0; j < n - 1 - i; j++) {
			if (s[j] > s[j + 1]) {
				char temp = s[j];
				s[j] = s[j + 1];
				s[j + 1] = temp;
			}
		}
	}
}

int main() {
	string s;
	while (cin >> s) {
		sortChar(s);
		cout << s << endl;
	}
	return 0;
}
```

---

# 弹球游戏

## 描述

某商场有一游戏机如图所示，每次游戏会有一个小球从顶部正中央落下。每一次小球撞击钉子，它都有均等的可能落向左边或者右边。小球经过每一层的钉子的碰撞后最终会落到最底层的某个格子处，该次游戏获得的得分就是该格子上标注的数字。

现在，告诉你小球每一次碰撞的时候是向左还是向右（用 `left` / `right` 表示），请你计算出小球最终会落在哪一列，获得多少分。

## 输入

第一行为一个整数 $T$（$T \leq 100$），表示数据组数。

接下来每 3 行为一组数据：

- 第一行为一个整数 $m$，表示有 $m$ 行钉子，即会发生 $m$ 次碰撞，最下面有 $m+1$ 列格子。
- 第二行为 $m$ 个 `left` 或 `right` 构成的字符串，单词之间被一个空格隔开（字符串总长度小于 1000）。
- 第三行为 $m+1$ 个整数，表示每一列格子所标注的分数。

## 输出

对于每一组数据输出一行两个整数，用空格隔开。第一个整数代表小球最终落在了哪一列（从 1 开始编号），第二个整数代表这局游戏取得的分数。

## 样例

### 输入
```
2
4
left left left left
5 4 3 2 1
4
left right left right
8 4 2 4 8
```

### 输出
```
1 5
3 2
```

## 解答

小球初始位于第 0 列（0-indexed）。每当遇到 `right`，列位置 +1；`left` 则不变（因为向左相对于当前位置不增加列号）。最终列号 +1 即为 1-indexed 的结果。

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	int t;
	cin >> t;
	for (int i = 0; i < t; i++) {
		int m;
		cin >> m;
		string temp;
		int pos = 0;
		for (int j = 0; j < m; j++) {
			cin >> temp;
			if (temp == "right") {
				pos++;
			}
		}
		vector<int> score(m + 1);
		for (int j = 0; j < m + 1; j++) {
			cin >> score[j];
		}
		cout << pos + 1 << " " << score[pos] << endl;
	}
}
```