<!--
 * @Date: 2020-07-21 15:06:19
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-21 16:41:35
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

搜索与排序
====

#### 搜索

线性搜索
* 在一个数组中找到某个数的位置（或确认是否存在）
* 基本方法：遍历

```c
int search (int key, int a[], int len)
{
    int ret = -1;
    for ( int i = 0; i<len; i++)
    {
        if ( key == a[i]){
            ret = i;
            break;
        }
    }
    return ret;
}

int main()
{
    int a[]={1,3,12,13,14,23,34,45,3,5};
    int r = search(13, a, sizeof(a)/sizeof(a[0]));
    printf("%d\n", r);

    return 0;
}
```

搜索例子

* 将美元硬币面值与名称相对应

方法一：

```c
#include <stdio.h>

int amount[] = {1,5,10,25,50};
char *name[] = {"penny", "nickel", "dime", "quater", "half-dollar"};

int search (int key, int a[], int len)
{
    int ret = -1;
    for ( int i = 0; i<len; i++)
    {
        if ( key == a[i]){
            ret = i;
            break;
        }
    }
    return ret;
}

int main()
{
    int k = 25;
    int r = search(k, amount, sizeof(amount)/sizeof(amount[0]));
    printf("%s\n", name[r]);

    return 0;
}
```

* 分裂的程序结构对cashe是不友好的，见方法二

方法二：

```c
#include <stdio.h>

struct {
    int amount;
    char *name;
} coins[] = {
    {1, "penny"},
    {5, "nickel"},
    {10, "dime"},
    {25, "quarter"},
    {50, "half-dollar"}
};

int main()
{
    int key = 25;
    for ( int i = 0; i < sizeof(coins)/sizeof(coins[0]); i++)
    {
        if (key == coins[i].amount)
        {
            printf("%s\n", coins[i].name);
            break;
        }
        
    }

    return 0;
}
```

二分搜索
* 前提：数组是有序的

```c
#include <stdio.h>

int search (int key, int a[], int len)
{
    int ret = -1;
    int left = 0;
    int right = len - 1;

    while (right > left)
    {
        int mid = ( left + right ) / 2;
        if ( a[mid] == key )
        {
            ret = mid;
            break;
        }else if ( a[mid]>key )
        {
            right = mid - 1;
        }else {
            left = mid + 1;
        }
        if ( a[left] == key)
        {
            ret = left;
            break;
        }
        if ( a[right] == key)
        {
            ret = right;
            break;
        } 
    }
    return ret;
}

int main()
{
    int key = 7;
    int a[] = {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15};
    int r = search(key, a, sizeof(a)/sizeof(a[0]));
    printf("%d\n", r);

    return 0;
}
```


排序
----

选择排序
* 当数组是无序时？
* 先排序

```c
#include <stdio.h>

int max (int a[], int len)
{
    int maxid = 0;
    for ( int i = 1; i < len; i++)
    {
        if ( a[i] > a[maxid])
        {
            maxid = i;
        }   
    }
    return maxid;
}

int main()
{
    int a[] = {2,5,3,7,3,9,12,3,4,87,90};
    int len = sizeof(a)/sizeof(a[0]);

    for ( int i = len-1; i > 0; i--)
    {
        int maxid = max(a, i+1);
        // swap a[maxid] & a[len-1]
        int t = a[maxid];
        a[maxid] = a[i];
        a[i] = t;
    }
    for ( int i = 0; i < len; i++)
    {
        printf("%d ", a[i]);
    }
    
    return 0;
}
```

