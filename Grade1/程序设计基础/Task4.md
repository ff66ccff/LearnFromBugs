# Task4
# 斐波那契数列求余

## 描述

斐波那契数列的递推公式为：  
$$
F_n = F_{n-1} + F_{n-2}
$$  
其中 $F_1 = F_2 = 1$。

当 $n$ 比较大时，$F_n$ 也非常大。现在我们想知道，$F_n$ 除以 10007 的余数是多少。

## 输入

输入包含一个整数 $n$（$1 \leq n \leq 10^{20}$）。

## 输出

输出一行，包含一个整数，表示 $F_n$ 除以 10007 的余数。

## 样例

### 输入
```
10
22
```

### 输出
```
55
7704
```

## 解答

由于 $n$ 可达 $10^{20}$，直接递归或普通迭代会超时。但斐波那契数列模 10007 具有**周期性**（皮萨诺周期），经测试（看后面的代码）周期为 20016。因此可先将 $n$ 对周期取模，再计算斐波那契数列对应项模 10007 的值。

注意：若取模后结果为 0，应视为周期末尾（即 $n = 20016$）。

```cpp
#include <bits/stdc++.h>
using namespace std;

const long long mod = 10007;
long long arr[100000] = {0};
const long long magicnumber = 20016; // Pisano period for mod 10007

long long f(long long n) {
    if (n == 1 || n == 2) {
        return 1;
    }
    if (arr[n] != 0) {
        return arr[n] % mod;
    }
    arr[n] = (f(n - 1) + f(n - 2)) % mod;
    return arr[n];
}

int main() {
    string s;
    cin >> s;
    long long n = 0;
    for (char c : s) {
        n = (n * 10 + (c - '0')) % magicnumber;
    }
    if (n == 0) {
        n = magicnumber;
    }
    cout << f(n) << endl;
    return 0;
}
```

```
//以下程序用于计算周期。
#include <bits/stdc++.h>
using namespace std;

const long long  mod = 10007;
long long arr[100000] = {0};
const long long  magicnumber = 20016;

long long f(long long n) {
	if (n == 1) {
		return 1;
	}
	if (n == 2) {
		return 1;
	}
	if (arr[n] != 0) {
		return arr[n] % mod;
	} else {
		arr[n] = f(n - 1) + f(n - 2);
		return arr[n] % mod;
	}
}


int main(){
	long long period=1;
	for(int i=1;i<10000000;i++){
		f(i);
	}
	while(1){
		if(arr[1000]==arr[1000+period]&&arr[100]==arr[100+period]){
			break;
		}else{
			period++;
		}
	}
	cout<<period;
}
```