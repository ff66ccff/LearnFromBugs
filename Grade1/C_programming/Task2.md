# Task2：判断成绩是否合格

## 描述

对于一个整数分数 `num`，判断该分数的状态。

## 输入

输入一个整数分数 `num`。

## 输出

- 如果 `num < 0` 或者 `num > 100`，输出 **分数错误**
- 如果 `0 ≤ num < 60`，输出 **不合格**
- 如果 `60 ≤ num < 80`，输出 **合格**
- 如果 `80 ≤ num ≤ 100`，输出 **优秀**

## 样例

### 输入
```
-1
2
61
88
101
```

### 输出
```
分数错误
不合格
合格
优秀
分数错误
```

## 解答

这个OJ很坑啊，要求我们使用dev cpp，这个垃圾IDE默认编码是GBK，但是OJ却是UTF-8，这样会导致WA。而且必须要判断是否读到数了，很恶心啊。

```c
#include <stdio.h>

int main(){
    int num;
    while((scanf("%d",&num)==1)){
        if(num<0 || num>100){
            printf("分数错误\n");
        }else if(num<60){
            printf("不合格\n");
        }else if(num<80){
            printf("合格\n");
        }else{
            printf("优秀\n");
        }
    }
    return 0;
}
```

---

# 一次函数与坐标轴围成的面积

## 描述

输入两个整数 $A$、$B$（确保 $A$ 和 $B$ 均不为零），对应一次函数：
$$
y = Ax + B
$$
求该函数图像与坐标轴围成的三角形面积，结果保留 **两位小数**。

## 输入格式

一行，包含两个整数 $A$ 和 $B$，以空格分隔。

## 输出格式

一行，输出所围成的面积 $S$，保留两位小数。

## 样例

### 输入
```
1 1
2 3
-1 2
3 -4
-2 -3
1 -5
```

### 输出
```
0.50
2.25
2.00
2.67
2.25
12.50
```

## 解答

C太逆天了，abs还分double 和float，做完这道题我就转CPP了。

```c
#include <stdio.h>
#include <math.h>

int main(){
    float a, b; // y = ax + b, 与坐标轴交于 (0, b) 和 (-b/a, 0)
    while(scanf("%f %f", &a, &b) == 2){
        printf("%.2f\n", fabs(-1 * b * b / (a * 2)));
    }
    return 0;
}
```

---

# 任务3：爬楼梯

## 描述

小明每次只能爬一阶楼梯或者两阶楼梯，对于一个 n 阶楼梯，小明共有多少种方案可以爬完。

## 输入

一个整数 n（0 < n <= 45）

## 输出

共有多少种方案

## 样例

当 n = 3 时：有 3 种方案
```
1 1 1
1 2  
2 1
```

### 输入
```
1
3
```

### 输出
```
1
3
```

## 解答

没啥好说的，就是递归求解，虽然n不大，但我喜欢记忆化，洛谷导致的。  
不过第二期任务就上递归，有点逆天了这个学习曲线。

```cpp
#include <iostream>
using namespace std;

int dp[46] = {0};

int f(int n) {
    if (n == 1) return 1;
    if (n == 2) return 2;
    if (dp[n] != 0) {
        return dp[n];
    }
    dp[n] = f(n - 1) + f(n - 2);
    return dp[n];
}

int main() {
    int n;
    dp[1] = 1;
    dp[2] = 2;
    while (cin >> n) {
        int ret = f(n);
        cout << ret << endl;
    }
    return 0;
}
```

---

# 任务4：水仙花数

## 描述

水仙花是中国十大名花之一，在中国已有一千多年栽培历史，具有极高的观赏性。而在数学上，我们也发现，具有这样的美的"水仙花数"，它是指一个三位数，它的各位数字的立方和等于其本身，例如：
153 = $1^3 + 5^3 + 3^3$。  
请编写程序计算输出指定范围内的水仙花数。

## 输入

第1行是一个整数 $T$，表示随后有 $T$ 组数据。每组数据占一行，包含两个整数 $m, n$（$100 \leq m \leq n \leq 999$）。

## 输出

对每组数据，要求输出 $[m, n]$ 范围内的所有水仙花数，空格隔开；如果不存在则输出 `-1`。

## 样例

### 输入
```
2
100 120
100 999
```

### 输出
```
-1
153 370 371 407
```

## 解答

用cpp做题就是惬意啊！不过redpanda为啥编码改成UTF-8还是无法正常显示，我怀疑并没有转化成UTF-8编码。
这个题就是要判断是不是水仙花数，写在main里面太乱了，所以把判断单独封装了一下。
因为如果范围内没有符合条件的数要输出-1，所以还要想办法检测这个范围内到底有没有数。

```cpp
#include <iostream>

using namespace std;

bool check(int n) {
	int temp = n;
	int ret = 0;
	while (temp > 0) {
		ret += (temp % 10) * (temp % 10) * (temp % 10);
		temp /= 10;
	}
	if (ret == n) {
		return true;
	} else {
		return false;
	}
}

int main() {
	int T;
	cin >> T;
	int n, m;
	for (int i = 0; i < T; i++) {
		cin >> n >> m;
		int marker = 0; 
		for (int j = n; j <= m; j++) {
			if (check(j)) {
				cout << j << " ";
				marker++;
			}
		}
		if (marker == 0) {
			cout << -1;
		}
		cout << endl;
	}
	return 0;
}
```
