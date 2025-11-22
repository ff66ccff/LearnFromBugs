# Task6
# 纸币支付问题

## 描述

现有1分、5分、1角、1元、2元、5元、10元、50元、100元面额的人民币各若干。现要求从键盘读入任意一个金额A（以元为单位，如345.78），然后显示支付该金额（A）需要的各种面额人民币的数量。即，显示100元、50元、10元、5元、2元、1元、1角、5分、1分这9种面额的人民币各需多少张，各种不用面值的人民币之间用逗号分隔。

## 输入

第1行是一个整数n，表示随后有n组数据。每组数据占一行。

## 输出

对每组数据输出显示支付该金额（A）需要的各种面额人民币的数量，并换行。

## 样例

### 输入
```
2
221.5
201
```

### 输出
```
2,0,2,0,0,1,5,0,0
2,0,0,0,0,1,0,0,0
```

## 解答

浮点数读入太容易出错了，直接读字符串最稳妥。这题关键是把金额转换成分单位，再取各个面值。注意1角 = 10分，1元 = 100分。

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	int n;
	cin >> n;
	string s;
	for (int i = 0; i < n; i++) {
		cin >> s;
		long long money = 0;
		bool after_dot = false;
		int decimal_digits = 0;
		for (char c : s) {
			if (c == '.') {
				after_dot = true;
				continue;
			}
			if (!after_dot) {
				money = money * 10 + (c - '0');
			} else {
				if (decimal_digits < 2) {
					money = money * 10 + (c - '0');
					decimal_digits++;
				}
			}
		}
		while (decimal_digits < 2) {
			money *= 10;
			decimal_digits++;
		}

		// 现在 money 是总分（如 221.5 → 22150）
		vector<int> v(9);
		v[0] = money / 10000;
		money %= 10000;
		v[1] = money / 5000;
		money %= 5000;
		v[2] = money / 1000;
		money %= 1000;
		v[3] = money / 500;
		money %= 500;
		v[4] = money / 200;
		money %= 200;
		v[5] = money / 100;
		money %= 100;
		v[6] = money / 10;
		money %= 10;
		v[7] = money / 5;
		money %= 5;
		v[8] = money;
		for (int j = 0; j < 9; j++) {
			cout << v[j];
			if (j < 8) {
				cout << ",";
			}
			if (j == 8) {
				cout << endl;
			}
		}
	}
}
```

---

# 9*9乘法表

## 描述

编写程序输出9*9乘法表。

## 输入

无。

## 输出

输出乘法表，每个被乘数占一行，每个等式之间用一个空格隔开，具体格式见样例。

## 样例

### 输出
```
1*1=1 
2*1=2 2*2=4 
3*1=3 3*2=6 3*3=9 
4*1=4 4*2=8 4*3=12 4*4=16 
5*1=5 5*2=10 5*3=15 5*4=20 5*5=25 
6*1=6 6*2=12 6*3=18 6*4=24 6*5=30 6*6=36 
7*1=7 7*2=14 7*3=21 7*4=28 7*5=35 7*6=42 7*7=49 
8*1=8 8*2=16 8*3=24 8*4=32 8*5=40 8*6=48 8*7=56 8*8=64 
9*1=9 9*2=18 9*3=27 9*4=36 9*5=45 9*6=54 9*7=63 9*8=72 9*9=81 
```

## 解答

简单嵌套循环，注意格式控制。每行末尾会多一个空格。（按理来说打表也可以，但显然这么写代码更快）

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	for(int i=1;i<10;i++){
		for(int j=1;j<=i;j++){
			cout<<i<<"*"<<j<<"="<<i*j<<" ";
		}
		cout<<endl;
	}
}
```

---

# 纪念日

## 描述

时间也能成为衡量情感的尺度。

小R要求小J计算出“一段旅程”的时间，现在需要你写出一个程序帮助小J计算两个时间节点之间相差了多少分钟。

## 输入

共2行，分别表示时间的起止时间，数据保证第一行的时间早于第二行的时间，且时间表示均合法，小时按照24小时制表示，且范围均在2024年内。

第1行，共4个整数表示开始时间的月，日，时，分。

第2行，共4个整数表示结束时间的月，日，时，分。

## 输出

输出1个整数，表示两个时间节点之间相差的分钟。

## 样例

### 输入1
```
4 1 21 21
4 1 21 22
```

### 输出1
```
1
```

### 输入2
```
6 30 23 49
7 1 6 30
```

### 输出2
```
401
```

## 解答

2024年是闰年，2月有29天。把每个时间点转换成从年初开始经过的总分钟数，再相减即可。注意数据范围可能很大，用long long更保险。

```cpp
#include <bits/stdc++.h>

using namespace std;
int months[] = {0, 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}; //每个月的天数
long long  process(int month, int day, int hour, int minute) {
	for (int i = 1; i < month; i++) {
		day += months[i];
	}
	return day * 24 * 60 + hour * 60 + minute;
}
int main() {
	int month, day, hour, minute;
	cin >> month >> day >> hour >> minute;
	long long time1 = process(month, day, hour, minute);
	cin >> month >> day >> hour >> minute;
	long long time2 = process(month, day, hour, minute);
	cout << time2 - time1;
}
```