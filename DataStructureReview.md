[toc]

# Data Structure Review

----

### 	1-Introduction:

##### 		1.1-Data\DataElement\DataItem\DataObject

​		数据(Data)：客观事物的符号表示。

​		数据元素(Data Element)：数据的基本单位，在计算机中通常作为一个整体进行考虑和处理。

​		数据项(Data Item)：组成数据元素的、有独立含义的、不可分割的最小单位。

​		数据对象(Data Object)：性质相同的数据元素的集合，是数据的一个子集。

##### 					1.2：数据结构：

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

##### 		2.1：线性表的定义和特点：

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
	while(p&& p->data!=e) 
        p= p->next;
   return p;
}
~~~

​					4.插入：

~~~c
Status ListInsert(LinkList &L,int i,ElemType e){  //在带头结点的单链表中第i个位置插入值为e的新节点
    p =L;j=0;
    while(p&&(j<i-1)){
        p=p->next;
        ++j;
    }
    if (!p||j>i-1) return ERROR;  //i>n+1 或者 i<1
    s= new ListNode;
    s->data =e;
    s->next = p->next;
    p->next=s;
    return OK;
}
~~~

​					5.删除：

~~~c
Status ListDelete(LinkList &L,int i){  //在带头结点的单链表中，删除第i个元素
    p=L;j=0;
    while((p->next)&&(j<i-1)){
        p=p->next;
        ++j;
    }
    if (!(p->next)||(j>i-1)) return ERROR;
    q = p->next;
    p-next=q->next;
    delete q;
    return OK;
}
~~~

​					6.创建单链表：

​								6.1前查法创建单链表：

~~~c
void CreateListHead(LinkList &L,int n){ //逆位序输入n个元素的值，建立带头结点的单链表L
    L= new ListNode;
    L->next =NULL;
    for (i=0;i<n;i++){
        p=new ListNode;
        cin>>p->data;
        p->next=L->next;
        L->next = p;
    }
}
~~~

​								6.2尾插法创建单链表：

~~~c
void CreateListTail(LinkList &L,int n){  //正位序输入n个元素的值，建立带头结点的单链表L
    L=new ListNode;
    L->next=NULL;
    tail=L;
    for (i=0;i<n;i++){
        p=new ListNode;
        cin>>p->data;
        p->next = NULL;
        tail->next = p;
        tail = p;
    }
}
~~~

##### 		2.4-循环链表： 看书，感觉不考，书上没有再多说明。

##### 		2.5-双向链表：

​		C语言中描述：

~~~c
//------------双向链表的存储结构---------
typedef struct DuLNode{
    ElemType data;
    struct DulNode *prior;  //指向直接前驱
    struct DulNode *next;  //指向直接后继
}DuLNode,*DuLinkList;
~~~

​		2.5.1:双向链表的插入：

~~~c
Status DuListInsert(DuLinkList &L,int i,ElemType e){  //在带头结点的双向链表中第i个位置之前插入元素
    if(!(p=DuLGetElem(L,i))) return ERROR;  //在L中确定第i个元素的位置指针p  p=NULL，第i个数据元素不存在
    s =new DuLNode;
    s->data =e;
    s->prior = p ->prior;
    p->prior->next = s;
    s->next = p;
    p->prior =s;
    return OK;
}
~~~

​		2.5.2:双向链表的删除：

~~~c
Status DuLListDelete(DuLLinkList &L,int i){ //删除带头结点的双向链表的第i个数据元素
    if (!(p=DuLGetElem(L,i))) return ERROR;  //在L中确定第i个数据元素的位置指针p p=NULL 第i个数据元素不存在
    p->prior->next = p->next;
    p->next-prior = p->prior;
    delete p;
    return OK;
}
~~~

----

### 	3-Stack、Queue:

#### 		Stack(定义)：栈是一种后进先出(Last In First Out)的线性表。

#### 		Queue(定义)：队列是一种先进先出(First In Frist Out)的线性表。

##### 		3.1：栈的表示和操作：

​		顺序栈的实现：

~~~c
//----------顺序栈的存储结构--------
#define MAXSiZE 100   //顺序栈存储空间的初始化分配量
typedef struct{
    SElemType *base;  //栈底指针
    SElemType *top;  //栈顶指针
    int stacksize;  //栈可用的最大容量
}SqStack;
~~~

​				1.初始化：

~~~c
Status InitStack(SqStack &S){
    S.base = new SElemType[MAXSIZE];
    if (!S.base) exit(OVERFLOW);
    S.top=S.base;
    S.stacksize = MAXSIZE;
    return OK;
}
~~~

​				2.入栈：

~~~c
Status Push(SqStack &S,SElemType e){
    if (S.top=S.base) return ERROR;
    *S.top++ = e;
    return OK; 
}
~~~

​				3.出栈：

~~~c
Status Pop(SqStack &S,SElemType &e){
    if (S.top == S.base) return ERROR;
    e = *--S.top;
    return OK;
}
~~~

​				4.取栈顶元素：

~~~c
SElemType GetTop(SqStack S){
    if (S.top != S.base)
        return *(S.top-1);
}
~~~



​		链栈的实现：

~~~c
//----------链栈的存储结构------------
typedef struct StackNode{
    ElemType data;
    struct StackNode *next;
}StackNode,*LinkStack;
~~~

​					1.初始化：

~~~c
Status InitStack(LinkStack &S){
	S=NULL;
	return OK;
}
~~~

​					2.入栈：

~~~c
Status Push(LinkStack &S,SElemType e){
    p = new StackNode;
    p->data = e;
    p->next = S;
    S = p;
    return OK;
}
~~~

​					3.出栈：

~~~c
Status Pop (LinkStack &S,SElemType &e){
    if (S ==Null) return ERROR;
    e = S->data;
    p=S;
    S=S->next;
    delete p;
    return OK;
}
~~~

​					4.取栈顶元素：

~~~c
SElemType GetTop(LinkStack S){
    if (S!=NULL)
        return S-data;
}
~~~



##### 		3.2：队列的表示和操作：

​		循环队列(顺序存储)：

~~~c
//-----------队列的顺序存储结构-------
typedef struct{
    QElemType *base;
    int front;
    int rear;
}SqQueue;
~~~

​				1.初始化：

~~~~c
Status InitQueue(SqQueue &Q){
    Q.base = new QElemType[MAXQSIZE];
    if (!Q.base) exit(OVERFLOW);
    Q.fornt = Q.rear;
    return OK;
}
~~~~

​				2.求队列长度：

~~~c
int QueueLength(SqQueue Q){
    return (Q.rear - Q.front +MAXSIZE)%MAXSIZE;
}
~~~

​				3.入队：

~~~~c
Status EnQueue(SqQueue &Q,QElemType e){
    if ((Q.rear+1)%MAXSIZE==Q.front) return ERROR;
    Q.base[Q.reat]=e;
    Q.rear = (Q.rear +1)%MAXSIZE;
    return OK;
}
~~~~

​				4.出队：

~~~c
Status DeQueue(SqQueue &Q,QElemType &e){
    if(Q.front == Q.rear) return ERROR;
    e = Q.base[Q.front];
    Q.front = (Q.front+1)%MAXQSIZE;
    return OK;
}
~~~

​				5.取对头元素：

~~~c
SElemType GetHead(SqQueue Q){
    if (Q.rear != Q.front) return Q.base[Q.front];
}
~~~



​		链队的实现：

~~~c++
//-----------队列的链式存储结构----------
typedef struct QNode{
    QElemType data;
    struct QNode *next;
}QNode, *QueuePtr;
~~~

​					1.初始化：

 ~~~C
 Status InitQueue(LinkStack &Q){
     Q.front = Q.rear = new QNode;
     Q.front->next = NULL;
     return OK;
 }
 ~~~

​					2.入队：

~~~c
Status EnQueue(LinkQueue &Q,QElemType e){
    p= new QNode;
    p->data = e;
    p->next = NUll; Q.rear->next = p;
    Q.rear  = p;
    return OK;
}
~~~

​					3.出队：

~~~c
Status DeQueue(LinkQueue &Q,QElemType &e){
    if(Q.front == Q.rear) return ERROR;
    p=Q.front->next;
    e = p->data;
    Q.front->next = p->next;
    if(Q.rear ==p) Q.rear = Q.front;
    delete p;
    return OK;
}
~~~

​					4.取链队元素：

~~~c++
SElelmType GetHead(LinkQueue Q){
    if (Q.front == Q.rear) 
        return Q.front->next->data;
}
~~~

​           

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

​	
