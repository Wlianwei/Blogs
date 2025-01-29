## 快速排序
#### 快速排序的思想的代码实现
>思想：快速排序是一种分治思想，做法是找到主元x（主元：q[(r+l)/2]）然后将待排序列分为两部分，左边小于等于x,右边大于等于x，重复上述过程即可完成排序。

>步骤：快排属于分治算法，分治算法都有三步：
1.分成子问题
2.递归处理子问题
3.子问题合并

```
void quick_sort(int q[], int l, int r)
{
    //递归的终止情况
    if(l >= r) return;

    //第一步：分成子问题
    int i = l - 1, j = r + 1, x = q[l + r >> 1]; //取数组中间值为主元
    while(i < j)  //i < j
    {
        do i++; while(q[i] < x); //q[i] < x,不能等于
        do j--; while(q[j] > x); //q[i] < x,不能等于
        if(i < j) swap(q[i], q[j]); //i < j
    }

    //第二步：递归处理子问题
    quick_sort(q, l, j), quick_sort(q, j + 1, r); //以j为划分，分为q[l...j]和q[j + 1,r]两部分

    //第三步：子问题合并.快排这一步不需要操作，但归并排序的核心在这一步骤
}
```


#### 快速排序的边界问题
[ACWing快速排序边界问题题解](https://www.acwing.com/solution/content/16777/)

## 二分查找(Binary search)
