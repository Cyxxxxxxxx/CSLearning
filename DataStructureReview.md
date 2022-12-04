[toc]

# Data Structure Review

----

### 	1-Introduction:

##### 		1.1-Data\DataElement\DataItem\DataObject

​		数据(Data)：客观事物的符号表示。

​		数据元素(Data Element)：数据的基本单位，在计算机中通常作为一个整体进行考虑和处理。

​		数据项(Data Item)：组成数据元素的、有独立含义的、不可分割的最小单位。

​		数据对象(Data Object)：性质相同的数据元素的集合，是数据的一个子集。

##### 					1.2数据结构：

​		数据结构(Data Structure)是相互之前存在一种或多种特定关系的数据元素的集合。

###### 				数据结构包含逻辑结构和存储结构两个层次：

​				1.2.1.逻辑结构(数据元素、关系)：数据逻辑结构是从逻辑关系上描述数据，它与数据的存储无关，是独立于计算机的。

​						1.2.1.1：集合结构：数据元素之间除了“属于同一集合”的关系之外，别无其他关系。

​						1.2.1.2：线性结构：数据元素之前存在一对一的关系。

​						1.2.1.3：树结构：数据元素之前存在一对多的关系。

​						1.2.1.4：图结构或网状结构：数据元素之前存在多对多的关系。

###### 																	PS：集合结构、树结构和图结构都属于非线性结构。

​				1.2.2.存储结构：数据对象在计算机中的存储表示称为数据的存储结构，也称为物理结构。

​						1.2.2.1：顺序存储结构：借助元素在存储中的相对位置来表示数据元素之间的逻辑关系。要求所有元素一次存放在一片连续的存储空间中。

​						1.2.2.2：链式存储结构：无需占用一整块存储空间，需添加指针字段，存放后续元素的存储地址。

##### 					1.3：算法与算法分析：

​				1.3.1：算法的定义及特性：

​							算法的定义：为了解决某类问题而指定的一个有限长的操作序列。

​							算法的特性：1-有穷性  2-确定性  3-可行性  4-输入  5-输出

​				1.3.2：算法的优劣从以下几点评价：1-正确性  2-可读性  3-健壮性  4-高效性(时间、空间)

##### 		1.4：算法的时间复杂度：算法的渐进时间复杂度(简称：时间复杂度--Time Complexity)

##### 		1.5：算法的空间复杂度：算法的渐进空间复杂度(--Space Complexity)

----

### 	2-LinearTable:

##### 		2.1线性表的定义和特点：

​			定义：有n(n>=0)个数据特性相同的元素构成的有序序列称为线性表。PS:n=0时，称为空表。

​			特点：除第一个外，每个数据元素都只有一个前驱；

​						除最后一个外，每个数据元素都只有一个后继。

##### 		2.2：线性表的顺序存储表示和实现：

​			定义：线性表的顺序表示指的是用一组地址连续的存储单元一次存储线性表的数据元素，这种表示也称作线性表的顺粗存储结构或顺序映像。PS：逻辑上相邻的数据元素，其在物理位置上也是相邻的。

​			表示：C语言中可用动态分配的一维数组表示线性表：

~~~c
//-------顺序表的存储结构------------

#define MAXSIZE 100                   //顺序表可能到达的最大长度

typedef struct{
    ElemType *elem;
    int lengtyh;
}SqlList;                                                                                                                                                                                      
~~~

​			顺序表中的基本操作：

​				1.初始化：

~~~c
//初始化
Status InitList(SqlList &L){  //构造一个空的顺序表L
    L.elem = new ElemType[MAXSIZE];  //为顺序表分配一个大小为MAXSIZE的数组空间
    if (!L.elem) exit(OVERFLOW);  //存储分配失败退出
    L.lenght=0;
    return OK;
}
~~~

​				2.取值：

~~~c
Status GetElem(SqlList L,int i,ElemType &e){
    if (i<1||i>L.lenght) return ERROR;  //判断i指是否合理
    e = L.elem[i-1];
    return OK;
}
~~~

​				3.查找：

~~~c
int LocateElem(SqlList L,ElemType e){  //在顺序表中查找值为e的数据元素，返回其序号
    for (i=0;i<L.lenght;i++)
        if (L.elem[i]==e) return i+1;
    return 0;
}
~~~

​				4.插入：

~~~c
Status ListInsert(SqlList &L,int i,ElemType e){  //在顺序表中第i个位置插入新的数据元素e  i值得合法范围：1<=i<=L.lenght+1
    if (i<1||i>L.lenght+1) return ERROR;
    if (L.lenght == MAXSIZE) return ERROR;
    for (j=L.lenght;j>=i-1;j--)
        L.elem[j+1] == L.elem[j];  //插入位置及以后的数据元素后移一个位置
    L.elem[i-1] =e;
    ++L.lenght;
    return OK;
}
~~~

​				5.删除：

~~~c
Status ListDel(SqlList &L,int i){  //在顺序表中删除第i个元素，i值得合法范围：1<=i<=L.lenght
    if (i<1||i>L.lenght) return ERROR;
    for (j=i;j<=L.lenght;j++)
        L.elem[j-1]=L.elem[j];
   	--L.lenght;
    return OK;
}
~~~

​				PS:缺点--在做插入或删除操作的时候，需要移动大量得数据元素。必然导致存储空间的浪费。   下面，链式存储来解决。

##### 		2.3：线性表的链式存储表示和实现：

​				PS：线性链表，也称为单链表。

​				表示：C语言中用"结构指针"描述：

~~~c
//-----------单链表的存储结构----------
typedef struct ListNode{
    ElemType data;   //节点的数据域
    struct ListNode *next;   //节点的指针域--指向下一个节点
}LNode,*LinkList;   //LinkList为指向结构体ListNode的指针类型
~~~

​				单链表的基本操作：

​					1.初始化：

~~~c
Status InitList(LinkList &L){  //构造一个空的单链表L
    L = new ListNode;  //生成新结点作为头节点，用头指针L指向头节点
    L->next = NULL;
    return OK;
}
~~~

​					2.取值：

~~~c
Status GetElem(LinkList L,int i,ElemType &e){//在带头结点的单链表中根据序号i获取元素的值，用e返回L中第i个数据元素的值
	p = L->next;j=1;
    while(p&&j<i){
        p = p->next;
        ++j;
    }
    if (!p||j>i) return ERROR;
    e= p->data;
    return OK;
}
~~~

​					3.查找：

~~~c
ListNode * LocateElem(LinkList L,ElemType e){  //在带头节点的单链表中查找值为e的数据元素
    p = l->next  //初始化，p指向首元节点
    if (!(p->next)||(j>i-1)) return ERROR;
}
~~~





----

### 	3-Stack、Queue:

----

### 	4-String、Array、GeneralizedTable:

----

### 	5-Tree、BinaryTree:

---

### 	6-Graphs:

---

### 	7-LookUp(Seek):

---

### 	8-Sequence:

