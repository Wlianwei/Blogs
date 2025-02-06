## 快速排序
#### 快速排序的原理和代码实现
>思想：快速排序是一种分治思想，做法是找到主元x（主元：q[(r+l)/2]）然后将待排序列分为两部分，左边小于等于x,右边大于等于x，重复上述过程即可完成排序。
>###### 由大到小处理

>步骤：快排属S于分治算法，分治算法都有三步：
1.分成子问题
2.递归处理子问题
3.子问题合并

```cpp
void quick_sort(int q[], int l, int r)
{
    //递归的终止情况
    if(l >= r) return;

    //第一步：分成子问题
    int  x = q[l + r >> 1], i = l - 1, j = r + 1;        //取数组中间值为主元，i和j都往外扩一
    while(i < j)  //i < j
    {
        do i++; while(q[i] < x);       //q[i] < x,不能等于
        do j--; while(q[j] > x);       //q[i] < x,不能等于
        if(i < j) swap(q[i], q[j]); //i < j
    }

    //第二步：递归处理子问题
    quick_sort(q, l, j), quick_sort(q, j + 1, r);        //以j为划分，分为q[l...j]和q[j + 1,r]两部分

    //第三步：子问题合并.快排这一步不需要操作，但归并排序的核心在这一步骤
}
```

#### 快速排序的边界问题
[ACWing快速排序边界问题题解](https://www.acwing.com/solution/content/16777/)

## 归并排序
#### 归并排序的原理和代码实现
>思想：归并排序同样是一种分治思想，做法是将区间划分为n个小区间，然后再依次按序合并小区间。
>###### 由小到大处理

>步骤：归并排序属于分治算法，分治算法都有三步：
1.分成子问题
2.递归处理子问题
3.子问题合并

```cpp
#include<iostream>
using namespace std;
const int N=1e5+10;

int q[N];
int tmp[N];
int n;

void merge_sort(int q[],int l,int r){
    //递归的终止情况
    if(l >= r) return;
    
    //第一步：分成子问题    //第二步：递归处理子问题
    int mid = l + r >> 1,i = l,j = mid + 1;     //****//
    merge_sort(q, l, j - 1),merge_sort(q, j, r);

    //第三步：子问题合并.归并排序的核心
    int k = 0;      //****//
    while(i <= mid && j <= r){
        if(q[i] <= q[j]) tmp[k ++] = q[i ++];
        else  tmp[k ++] = q[j ++];
    }
    while(i <= mid) tmp[k ++] = q[i ++];
    while(j <= r) tmp[k ++] = q[j ++];
    for(int i = l, j = 0; i <= r; i ++, j ++) q[i] = tmp[j];      //****//
}
int main(){
    scanf("%d",&n);
    for(int i = 0; i < n; i ++) scanf("%d",&q[i]);
    merge_sort(q,0,n-1);
    for(int i = 0; i < n; i ++) printf("%d ",q[i]);
    return 0;
}
```
#### 归并排序的边界问题
[ACWing归并排序边界问题题解](https://www.acwing.com/solution/content/16778/)

## 二分查找
#### 二分查找根据查找分界点所在区间的不同分为两个模板。
>使用方法：每次使用这这两个模板的时候，先想是找这个区间的左端点还是还是右端点，然后选择用模板，最后再去写判断条件。

>应用(无非下面这四种情况):
1：找大于等于数的第一个位置 （满足某个条件的第一个数）
2：找小于等于数的最后一个数 （满足某个条件的最后一个数）
3.查找最大值 （满足该边界的右边界）
4.查找最小值 (满足该边界的左边界)
```cpp
模板一:查找左边界
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int SR(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}

```

```cpp
模板二：查找右边界
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int SR(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;      
        if (check(mid)) l = mid;       
        else r = mid - 1;
    }
    return l;
}
```

#### 二分查找的边界问题
[ACWing二分查找边界问题题解](https://www.acwing.com/solution/content/107848/)
