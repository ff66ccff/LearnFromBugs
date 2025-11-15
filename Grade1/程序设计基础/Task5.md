# Task5
# 1: 排序四个整数

## 描述

将4个整数从小到大排序，然后依次输出。

## 输入

第1行是一个整数n，表示随后有n组数据。每组数据占一行，包含4个整数，用空格隔开。

## 输出

对每组数据输出从小到大排序后的结果，整数之间用1个空格隔开。

## 样例

### 输入
```
3
1 5 3 9
3 0 3 7
2 3 2 3
```

### 输出
```
1 3 5 9
0 3 3 7
2 2 3 3
```

# 解析
这道题就是要考排序，冒泡能AC，快排能AC，归并也能AC（不过四个数字感觉冒泡足够了），或者直接用sort函数。


## 解答

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> v(4);

void fsort() {
	for (int j = 0; j < 3; j++) {
		for (int i = 0; i < 3 - j; i++) {
			if (v[i] > v[i + 1]) {
				swap(v[i], v[i + 1]);
			}
		}
	}
}

int main() {
	int n;
	cin >> n;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < 4; j++) {
			cin >> v[j];
		}
		fsort();
		for (int i = 0; i < 4; i++) {
			cout << v[i];
			if (i < 3) cout << " ";
		}
		cout << endl;
	}
	return 0;
}
```

---

# 2: 打印菱形

## 描述

输入一个正整数，按要求打印菱形。要求，第一行打印1个*，第二行打印3个*，每行等差递增直到输出n个*，之后每行递减输出，直到输出1个*。注意：所有*构成一个菱形的形状,即菱形本身靠中轴线对齐。

## 输入

正整数n，n为奇数。

## 输出

若n不符合要求，输出error；否则输出print然后换行按要求打印菱形。注意：菱形最长的一排*靠左边，无空格。

## 样例

### 输入
```
5
```

### 输出
```
print
  *
 ***
*****
 ***
  *
```


# 解析
就是直接打印，没啥好说的。
## 解答

```cpp
#include <bits/stdc++.h>

using namespace std;

void print_space(int n) {
	if (n == 0) {
		return;
	}
	for (int i = 0; i < n; i++) {
		cout << " ";
	}
}
void print(int n) {
	for (int i = 0; i < n; i++) {
		cout << "*";
	}
}

int main() {
	int n;
	cin >> n;
	if (n % 2 == 0) {
		cout << "error";
		return 0;
	} else {
		cout << "print" << endl;
		for (int i = 1; i <= n; i = i + 2) {
			int space = (n - i) / 2;
			print_space(space);
			print(i);
			print_space(space);
			cout << endl;
		}
		for (int i = n - 2; i >= 1; i = i - 2) {
			int space = (n - i) / 2;
			print_space(space);
			print(i);
			print_space(space);
			cout << endl;
		}
	}
}
```

---

# 3: 猜数字

## 描述

现在，我想让你猜一个数字x（1000<=x<=9999），它满足以下要求：
（1）x % a = 0；
（2）(x+1) % b = 0；
（3）(x+2) % c = 0；
其中1<=a，b，c<=100。
给你a，b，c的值，你能告诉我x是多少吗？

## 输入

正整数n代表要测试的数据组数。然后输入n组测试数据，每组包含三个正整数分别代表a，b，c。

## 输出

符合条件的正整数x。若不存在输出Impossible

## 样例

### 输入
```
2
3 3 3
8 27 94
```

### 输出
```
Impossible
7424
```
# 解析
$x%a=0$
上式说明x是a的倍数，所以直接列出a的倍数再判断，逐一循环枚举会TLE的。

## 解答

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
	int n;
	int a, b, c;
	bool found = false;
	cin >> n;
	for (int j = 0; j < n; j++) {
		found=false;
		cin >> a >> b >> c;
		for (int i = (1000 / a) * a; i <= 9999; i += a) {
			if (i % a == 0 && (i + 1)%b == 0 && (i + 2) % c == 0) {
				found = true;
				cout << i << endl;
			}
		}
		if (found == false) {
			cout << "Impossible" << endl;
		}
	}
}
```

---

# 4: 找出丢失的数

## 描述

现有一堆由1,2,3,...,n组成的共n-1个数，其中丢失了一个数k，请找出这个数。

## 输入

第一行m，表示测试数据的组数。

接下来m行，每行即为一组测试样例。每组样例有n个正整数，其中最后一个整数为0，表示结束，不参与计算。

## 输出

对每组数据，输出1至n中缺失的那个整数。

## 样例

### 输入
```
2
2 3 1 5 0
1 2 6 4 5 7 0
```

### 输出
```
4
3
```
# 解析
用vector存储每个数的状态，若没出现，则v[i]=0


## 解答

```cpp
#include <bits/stdc++.h>  
using namespace std;

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        vector<int> v(2001);  
        int temp;
        do {
            cin >> temp;
            if (temp != 0) {
                v[temp]++;
            }
        } while (temp != 0);
        for (int j = 1; j <= 2000; j++) {
            if (v[j] == 0) {
                cout << j << endl;
                break;
            }
        }
    }
    return 0;
}
```