## 高精度加法
#### 高精度加法：高精度 + 高精度
##### 进位```t = t / 10;```
##### 结果位数```C[i] = t % 10;```

```cpp
//高精度加法模板
#include<iostream>
#include<vector>
using namespace std;

vector<int> add(vector<int> &A, vector<int> &B){
    int t = 0;
    vector<int> C;
    for(int i = 0; i < A.size() || i < B.size(); i++){
        if(i < A.size()) t = t + A[i];
        if(i < B.size()) t = t + B[i];
        C.push_back(t % 10);
        t = t / 10;                                              //*****//
    }
    if(t) C.push_back(1);                                        //*****//
    return C;
}

int main(){
    string a,b;
    vector<int> A,B;

    cin >> a >> b;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    for(int i = b.size() - 1; i >= 0; i --) B.push_back(b[i] - '0');

    auto C = add(A, B);

    for(int i = C.size() - 1; i >= 0; i --) printf("%d",C[i]);

    return 0;
}
```
## 高精度减法
#### 高精度减法：高精度A - 高精度B(A >= B)
##### 借位```if(t < 0) t = 1; else t = 0;```
##### 结果位数```C[i] = (t + 10) % 10;```
```cpp
//高精度减法模板
#include<iostream>
#include<vector>
using namespace std;

//比较A>=B                                                      //*****//
bool cmp(vector<int> &A, vector<int> &B){                 
    if(A.size() != B.size()) return A.size() > B.size();
    for(int i = A.size() - 1; i >= 0; i --) 
        if(A[i] != B[i])
            return A[i] > B[i];
    return true;
}

vector<int> sub(vector<int> A, vector<int> B){
    vector<int> C;
    int t = 0;
    for(int i = 0; i < A.size() || i < B.size(); i ++){
        if(i < A.size()) t = A[i] - t;
        if(i < B.size()) t = t - B[i];
        C.push_back((t + 10) % 10);
        if(t < 0) t = 1;                                        //*****//
        else t = 0;
    }
    while(C.size() > 1 && C.back() == 0) C.pop_back();          //*****//
    return C;
}
int main(){
    string a,b;
    vector<int> A,B,C;
    cin >> a >> b;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    for(int i = b.size() - 1; i >= 0; i --) B.push_back(b[i] - '0');
    
    if(cmp(A, B))
        C = sub(A, B);
    else{
        C = sub(B, A);
        cout<<"-";
    }

    for(int i = C.size() - 1; i >= 0; i --) printf("%d",C[i]);

    return 0;
}
```
## 高精度乘法
#### 高精度乘法：高精度 * 整数
##### 进位```t = (A[i] * b + t) / 10;```
##### 结果位数```C[i] = (A[i] * b + t) % 10;```
```cpp
//高精度乘法模板：高精度 * 整数
#include<iostream>
#include<vector>
using namespace std;

vector<int> mul(vector<int> &A, int b){
    vector<int> C;
    int t = 0;
    for(int i = 0; i < A.size(); i ++){
        t = A[i] * b + t;
        C.push_back(t % 10);
        t = t / 10;
    }
    if(t) C.push_back(t);
    while(C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

int main(){
    string a;
    int b;
    vector<int> A;

    cin >> a >> b;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');

    auto C = mul(A, b);

    for(int i = C.size() - 1; i >= 0; i --) printf("%d",C[i]);

    return 0;
}
```
#### 高精度乘法：高精度 * 高精度
#### 进位：```t = C[i + j] / 10;```
#### 结果位数：```C[i + j] = C[i + j] % 10;```
```cpp
////高精度乘法模板：高精度 * 高精度
#include<iostream>
#include<vector>
using namespace std;
const int N = 1e5 + 10;

vector<int>mul(vector<int> &A, vector<int> &B){
    vector<int> C(N, 0);
    int t = 0;
    for(int i = 0; i < A.size(); i ++){
        t = 0;                                           //每前移一层进位都要更新
        for(int j = 0; j < B.size(); j ++){
            C[i + j] = C[i + j] + A[i] * B[j] + t;       //注意：三个数相加，A[i]*B[i],t(上一位的进位),C[i + j](前一次第i + j位上的数值)
            t = C[i + j] / 10;
            C[i + j] = C[i + j] % 10;
        }
        C[i + B.size()] = t;                             ////每前移一层都要为最高位添加进位
    }
    while(C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

int main(){
    string a,b;
    vector<int> A,B;
    cin>>a>>b;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    for(int i = b.size() - 1; i >= 0; i --) B.push_back(b[i] - '0');

    auto C = mul(A, B);

    for(int i = C.size() - 1; i >= 0; i --) printf("%d",C[i]);

    return 0;
}
```
## 高精度除法
#### 高精度除法：高精度 / 整数
##### 余数```r = (r * 10 + A[i]) % b```
##### 结果位数```C[i] = (r * 10 + A[i]) / b```
```cpp
高精度除法模板
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

vector<int> div(vector<int> &A, int b, int &r){           //*****//
    vector<int> C;
    r = 0;
    for(int i = A.size() - 1; i >= 0; i --){
        r = r * 10 + A[i];
        C.push_back(r / b);
        r = r % b;
    }
    reverse(C.begin(), C.end());                         //*****//
    
    while(C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

int main(){
    string a;
    int b;
    vector<int> A;

    cin >> a >> b;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');

    int r;
    auto C = div(A, b, r);

    for(int i = C.size() - 1; i >= 0; i --) printf("%d",C[i]);
    cout<< endl << r;
    return 0;

}
```
