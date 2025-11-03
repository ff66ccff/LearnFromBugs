# 回文数判断

## 描述

输入一个正整数（32位 int 可以存放下），请判断它是否为回文数。

所谓回文数：类似 `1234321`、`123321` 等这样，其十进制表示下，顺序和逆序相同的整数为回文数。

## 输入格式

第一行为正整数 `n`，表示数据组数。  
随后 `n` 行，每行包含一个正整数 `m_i`。

## 输出格式

对每个 `m_i`，如果是回文数，输出 `Yes`，否则输出 `No`，并换行。

## 样例

### 输入
```
3
1231
12321
12345
```

### 输出
```
No
Yes
No
```

## 解答

将整数转为字符串，再反转字符串进行比较，时间复杂度小于直接进行数值运算。

```cpp
#include <bits/stdc++.h>
using namespace std;

long long m;
string s;

bool check(){
    s = to_string(m);
    string temp = s;
    reverse(temp.begin(), temp.end());
    return temp == s;
}

int main() {
    int n;
    cin >> n;
    for(int i = 0; i < n; i++){
        cin >> m;
        if(check()){
            cout << "Yes" << endl;
        } else {
            cout << "No" << endl;
        }
    }
    return 0;
}
```

---

# 大数加法

## 描述

实现一个函数，用于计算两个非负整数的和。

## 输入格式

一行，包含两个非负整数（以空格分隔）。

## 输出格式

输出这两个整数的和。

**提示**：两数之和可能非常大（`0 <= 两数之和 <= 2^80`），无法用普通整型存储。

## 样例

### 输入
```
123456 789012
```

### 输出
```
912468
```

## 解答

这是一道高精度题，普通的long long 表示不了这么大范围的数，所以需要人工模拟。
这里使用string处理，char-'0'会变为数字。
手动模拟人类加法。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    string a, b;
    cin >> a >> b;
    reverse(a.begin(), a.end());
    reverse(b.begin(), b.end());
    string result = "";
    int carry = 0;
    int i = 0;
    while (i < max(a.length(), b.length()) || carry != 0) {
        int digita = 0;
        int digitb = 0;
        if (i < a.length()) {
            digita = a[i] - '0';
        }
        if (i < b.length()) {
            digitb = b[i] - '0';
        }
        int current = digita + digitb + carry;
        result += (current % 10) + '0';
        carry = current / 10;
        i++;
    }
    reverse(result.begin(), result.end());
    cout << result << endl;
    return 0;
}
```

---

# 买卖股票的最佳时机

## 描述

给定一个数组 `prices`，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天买入**，并在 **未来的某一天卖出**。设计一个算法计算你能获得的最大利润。

**限制**：最多只能进行一次买入和一次卖出操作。

## 输入格式

第一行输入一个整数 `n`，表示价格数组长度。  
第二行输入 `n` 个整数，表示 `prices` 数组的值。

## 输出格式

输出你能获得的最大利润。如果无法获利，输出 `0`。

## 样例

### 输入
```
6
7 1 5 3 6 4
```

### 输出
```
5
```

## 解答

遍历过程中维护到目前为止的最低价格，并计算当天卖出能获得的最大利润，动态规划。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    int now;
    int min_price = INT_MAX;
    int max_profit = 0;
    for (int i = 0; i < n; i++) {
        cin >> now;
        if (now < min_price) {
            min_price = now;
        }
        if (now - min_price > max_profit) {
            max_profit = now - min_price;
        }
    }
    cout << max_profit << endl;
    return 0;
}
```

---

# 盛最多水的容器

## 描述

给定一个长度为 `n` 的整数数组 `height`。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])`。

找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

输出容器可以储存的最大水量。

**说明**：你不能倾斜容器。

## 输入格式

第一行输入一个整数 `n`，表示数组长度。  
第二行输入 `n` 个整数，表示 `height` 数组的值。

## 输出格式

输出容器可以储存的最大水量。

## 样例

### 输入
```
9
1 8 6 2 5 4 8 3 7
```

### 输出
```
49
```

## 解答

贪心算法。
默认取两端可以得到初始的面积，此时我们由两侧向中间逼近。由于一次移动两侧会导致遗漏，所以一次只移动一侧，由于使用贪心算法追求局部最优解，所以将较短的一侧移动。

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> h;
int n;
int s = 0;

void process(int a, int b) {
    if (a >= b) {
        return;
    }
    int left = h[a];
    int right = h[b];
    int length = b - a;
    s = max(s, length * min(left, right));
    if (left < right) {
        process(a + 1, b);
    } else {
        process(a, b - 1);
    }
}

int main() {
    cin >> n;
    h.resize(n);
    for (int i = 0; i < n; i++) {
        cin >> h[i];
    }
    process(0, n - 1);
    cout << s << endl;
    return 0;
}
```