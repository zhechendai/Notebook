<!--
 * @Date: 2020-07-17 13:12:22
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-17 14:40:28
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

链表
====

可变数组
----

自己实现一个可变大小的数组
* 可以长大，不够后自己变大
* 需要知道现在有多大
* 能够访问当中的单元

可以实现一个函数库
```c
Array array_create(int init_size);             // 创建一个数组
void array_free(Array *a);                     // 空间回收
int array_size(const Array *a);                // 数组有多大
int* array_at(Array *a, int index);            // 访问数组当中的单元
void array_inflate(Array *a, int more_size);   // 让数组长大
```

函数实现

```c
Array array_create(int init_size)
{
    Array a;
    a.size = init_size;
    a.array = (int*)malloc(sizeof(int)*a.size);
    return a;
}

void array_free(Array *a)
{
    free(a->array);
    a->array = NULL;
    a->size = 0;
}

int array_size(const Array *a)
{
    return a->size;
}

const BLOCK_SIZE = 20;

int* array_at(Array *a, int index)
{
    if ( index >= a->size ){
        array_inflate(a, (index/BLOCK_SIZE+1)*BLOCK_SIZE-a->size);
    }
    return &(a->array[index]);
}

void array_inflate(Array *a, int more_size)
{
    int *p = (int*)malloc(sizeof(int)(a->size +more_size));
    int i;
    for (i=0; i<a->size; i++){      // 可以换成库函数中的memcpy
        p[i] = a->array[i];
    }
    free(a->array);
    a->array = p;
    a->size += more_size; 
}

int main(int argc, char const *argv[])
{
    Array a = array_create(100);
    printf("%d\n",array_size(&a));
    *array_at(&a, 0) = 10;
    printf("%d\n", *array_at(&a, 0));
    int number = 0;
    int cnt = 0;
    while (number != -1){
        scanf("%d", &number);
        if (number != -1)
            *array_at(&a, cnt++) = number;
    }
    array_free(&a);

    return 0;
}
```

链表
----

可变数组的缺陷
* 每一次长大都要去申请一块新的内存空间，可以容纳下新的群不的东西，然后拷贝
  * 拷贝需要花费时间，随着数组长大，拷贝时间更长
  * 存在明明有足够内存，但不能申请空间

解决方案（Linked list）
* 不拷贝
* 将BLOCK链接起来
* 让一个数组，指向下一个数组，不够空间后再指向下一个数组
  * 一个东西分成两部分，一部分数据，一部分指针，指针指向下一个一样的东西，如此反复，一个指针指向第一个数据表示是head
  * 这个东西叫做节点

```c
#ifndef _NODE_H_
#define _NODE_H_

typedef struct _node{
    int value;
    struct _node *next;
} NOde;

#endif

// 示例
#include "node.h"
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char const *argc[])
{
    Node * head = NULL;
    int number;
    do{
        scanf("%d", &number);
        if( number != -1){
            // add to linked-list
            Node *p = (Node*)malloc(sizeof(Node));
            p->value = number;
            p->next = NULL;
            // find the last
            Node *last = head;
            if ( last ){      // 判断last此时是不是head，是否NULL
                while ( last->next ){
                    last = last->next;
                }
                // attach
                last->next = p;
            } else {
                head = p;
            }
        }
    } while (number != -1 );

    return 0;
}

// 将链表的一部分抽出作为函数
// 示例 2
#include "node.h"
#include <stdio.h>
#include <stdlib.h>

typedef struct _list {
    Node* head;
}List;

void add(Node* head, int number);
void print(List *pList);
int main(int argc, char const *argc[])
{
    List list;
    list.hrad = NULL;
    int number;
    do{
        scanf("%d", &number);
        if( number != -1){
            add(&list, number);
        }
    } while (number != -1 );
    printf(&list);
    scanf("%d", &number);
    Node *p;
    int isFound = 0;
    for ( p=list.head; p; p=p->next){
        if( p->value == number){
            printf("找到了\n");
            idFound = 1;
            break;
        }
    }
    if( !isFound){
        printf("没找到\n");
    }
    Node *q;
    for ( q=NULL, p=list.head; p; q=p, p=p->next){
        if( p->value == number){
            if ( q ){
                q->next = q-> next;
            } else {
                list.head = p->next;   // 小技巧：是否存在一个指针出现在->左边,它使用时是否保证了安全，不为NULL
            }
            free(p);
            break;
        }
    }
    // 清除整个列表
    for ( p=head; p; p=q) {
        q = p->next;
        free(p);
    }
    return 0;
}

void add(List* pList, int number)
    // add to linked-list
    Node *p = (Node*)malloc(sizeof(Node));
    p->value = number;
    p->next = NULL;
    // find the last
    Node *last = pList->head;
    if ( last ){      // 判断last此时是不是head，是否NULL
        while ( last->next ){
            last = last->next;
        }
        // attach
        last->next = p;
    } else {
        pList->head = p;
    }

void print(List *pList){
    Node *p;
    for ( p=pList->head; p; p= p->next){
        printf("%d"\t, p->value);
    }
    printf("\n");
}
```

