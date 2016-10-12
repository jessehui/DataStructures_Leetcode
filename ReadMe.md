# Data Structures and Algorithm Notes
> Basic

### 数组和链表
数组优劣: 易于查找引用元素, 但是不容易插入删除元素.   
链表: 易于插入删除元素. 查找比较麻烦, 用循环一个一个next找.
```C++
struct node{
    int payload;
    node* next;
};
```


`node *head = nullptr;` nullptr表示空指针.  
表示 head 是一个指向 node 类型的指针. 初始化时把它定义成空的.


```C++
int* p1 = 0;  //ok
int* p2 = nullptr;  //ok

int n1 = 0;             // ok  
int n2 = nullptr;       // error 
```
reference: [http://blog.csdn.net/huang_xw/article/details/8764346]

### new 和 malloc
`long *pNumber = (long*)malloc(sizeof(long) * 1000000); //c `  
`long* pNumber = new long[1000000]; //c++`  


### 数组倒序排列
要让数组倒序只要交换最左边和最右边的元素, 然后交换次左边和次右边的元素. 然后持续, 直到遍历了所有元素(偶数个元素), 或者剩下唯一一个元素(奇数个元素), 这样整个数组就被倒序排列了. 

并不是所有指令都和数据量的增长有关系.   O(n)增长是线性的. 下表时间复杂度从上到下增加.

|O(1) | 常数阶 |
|-------|------|
|O(log n)| 对数 |
|--------|------|
|O((log n)^C) | 多对数 |
|-------|------|
|O(n)| 线性 |
|--------|------|
|O(n log * n) | 迭代对数 |
|-------|------|
|O(n log n)| 线性对数 |
|--------|------|
|O(n^2) | 平方 |
|-------|------|
|O(n^c)| 多项式 |
|--------|------|
|O(c^n)| 指数 |
|--------|------|
|O(n!) | 阶乘 |
|-------|------|


一实数的迭代对数是指需对实数连续进行几次对数运算后，其结果才会小于等于1。  
关于递归算法的时间复杂度. 例如斐波那契数列的时间复杂度为O(2^n).

利用递归算法reverse链表:
```C++
node* reverse_recursive(node *head)
{
    if(head == nullptr || head->next == nullptr)
    return head;
    node* second = head->next;
    node* new_head = reverse_recursive(second);
    second->next = head;
    head->next = nullptr;
    return new_head;

}
```
时间复杂度计算: 
T(0) = O(1);  
T(1) = O(1);  
T(n) = T(n-1) + O(1);  
     = T(n-2) + O(1) + O(1);  
     = T(n-3) + O(1) + O(1);  
     = n*O(1);  
     = O(n);    


### C++补充知识
this指针:  
`node(int payload) {this->payload = payload;};`  
1. this指针是一个隐含于每一个成员函数中的特殊指针。它指向正在被该成员函数操作的那个对象。
2. 当对一个对象调用成员函数时，编译程序先将对象的地址赋给this指针，然后调用成员函数，每次成员函数存取数据成员时，由隐含使用this指针。
3. 当一个成员函数被调用时，自动向它传递一个隐含的参数，该参数是一个指向这个成员函数所在的对象的指针。 
4. 在C++中，this指针被隐含地声明为: X *const this,这意味着不能给this 指针赋值；
   在X类的const成员函数中，this指针的类型为：const X* const, 这说明this指针所指向的这种对象是不可修改的（即不能对这种对象的数据成员进行赋值操作）; 
5. 由于this并不是一个常规变量，所以，不能取得this的地址。


C++类的继承和派生:  
1. 类的继承，是新的类从已有类那里得到已有的特性。或从已有类产生新类的过程就是类的派生。原有的类称为基类或父类，产生的新类称为派生类或子类。  
2. 派生类的声明：  
class 派生类名：继承方式 基类名1， 继承方式 基类名2，...，继承方式 基类名n
{
    派生类成员声明；
};  
3. 一个派生类可以同时有多个基类，这种情况称为多重继承，派生类只有一个基类，称为单继承。
4. 继承方式规定了如何访问基类继承的成员。继承方式有public, private, protected。如果不显示给出继承方式，默认为private继承。继承方式指定了派生类成员以及类外对象对于从基类继承来的成员的访问权限




关于C++的类中 private, public, protected:  
1.类的一个特征就是封装，public和private作用就是实现这一目的。所以：
用户代码（类外）可以访问public成员而不能访问private成员；private成员只能由类成员（类内）和友元访问。
2.类的另一个特征就是继承，protected的作用就是实现这一目的。所以：
protected成员可以被派生类对象访问，不能被用户代码（类外）访问。private成员只能被本类成员（类内）和友元访问，不能被派生类访问；

关于C++类中的构造函数:
```C++
class Counter
{

public:
         // 类Counter的构造函数
         // 特点：以类名作为函数名，无返回类型
         Counter()
         {
                m_value = 0;
         }
         
private:
      
         // 数据成员
         int m_value;
}
```
该类对象被创建时，编译系统对象分配内存空间，并自动调用该构造函数->由构造函数完成成员的初始化工作  

eg:    Counter c1;  
        编译系统为对象c1的每个数据成员(m_value)分配内存空间，并调用构造函数Counter( )自动地初始化对象c1的m_value值设置为0

故：

        构造函数的作用：初始化对象的数据成员。


> 无参数构造函数

如果创建一个类你没有写任何构造函数,则系统会自动生成默认的无参构造函数，函数为空，什么都不做.只要你写了一个下面的某一种构造函数，系统就不会再自动生成这样一个默认的构造函数，如果希望有一个这样的无参构造函数，则需要自己显示地写出来.  
这种构造函数既可以用在类中, 也可以用在结构体中.



C++ 用new创建对象和直接定义的区别
系统分配的内存空间位置不同. new可以完全自己掌握什么时候归还空间, 而直接定义的变量由系统控制其生存时间. 


###排序
归并排序: divide and conquer 分治法的典型应用.  
快速排序: 选定一个pivot轴(一般把第一个数作为轴心), 然后所有其他数如果大于轴心,放在轴心右边, 小于轴心放在左边. 

###折半搜索法(Binary Search)
适用于已经排好序的数列. 每次比较减少一半的搜索量.   
递归算法(recursive)会增加内存的使用. 

###二叉树
tree: 一种新型的链表数据结构. 对于一个节点来说它只有0,1,2个子节点, 就是二叉树. 没有子节点的节点是叶子leaf节点.
Binary search只能搜索排好序的, 对于没排好序的就要使用二叉搜索树Binary search tree(BST). 对于二叉搜索树的每个节点来说, 他的左边的子节点的数都要比他本身小.

DFS: Deep first search 深度优先算法(沿一个方向一直向下走 走到头). 对于DFS的树的遍历算法有三种遍历方法: 前序 中序 后序.  
前序: 到达一个节点, 打印value, 再遍历左子树, 再遍历右子树.   
中序: 先遍历左子树, 然后打印value,然后遍历右子树. 可以作为排序算法.

BFS breadth firs search: 逐层遍历

### 栈和队列
栈(stack): 新建只能从最上层, 删也只能从最上层删. 后进先出LIFO. 类似一个容器. 自己构造一个栈的话需要这些构造函数:   
`init(), push(), pop(), top(), isempty(). `  
因为栈太重要了, C++标准库有标准的实现方式. 但是我们也可以自己构造自己需要的栈结构.
应用: 函数的调用.

队列(queue): FIFO先进先出原则.






### malloc 和 new
　　第一、malloc 函数返回的是 void * 类型，如果你写成：p = malloc (sizeof(int)); 则程序无法通过编译，报错：“不能将 void* 赋值给 int * 类型变量”。所以必须通过 (int *) 来将强制转换。  
   第二、函数的实参为 sizeof(int) ，用于指明一个整型数据需要的大小。如果你写成：

　　`int* p = (int *) malloc (1);`

　　代码也能通过编译，但事实上只分配了1个字节大小的内存空间，当你往里头存入一个整数，就会有3个字节无家可归，而直接“住进邻居家”！造成的结果是后面的内存中原有数据内容全部被清空。

　　malloc 也可以达到 new [] 的效果，申请出一段连续的内存，方法无非是指定你所需要内存大小。

　　比如想分配100个int类型的空间：

　`　int* p = (int *) malloc ( sizeof(int) * 100 ); //分配可以放得下100个整数的内存空间。`

　　另外有一点不能直接看出的区别是，malloc只管分配内存，并不能对所得的内存进行初始化，所以得到的一片新内存中，其值将是随机的。

　　除了分配及最后释放的方法不一样以外，通过malloc或new得到指针，在其它操作上保持一致.

new 返回指定类型的指针，并且可以自动计算所需要大小。比如：

      int *p;

　　p = new int; //返回类型为int* 类型(整数型指针)，分配大小为 sizeof(int);

　　或：

　　int* parr;

　　parr = new int [100]; //返回类型为 int* 类型(整数型指针)，分配大小为 sizeof(int) * 100;

　

　   而 malloc 则必须由我们计算要字节数，并且在返回后强行转换为实际类型的指针。

   　int* p;

　　p = (int *) malloc (sizeof(int));

当无法知道内存具体位置的时候，想要绑定真正的内存空间，就需要用到动态的分配内存，即malloc函数。


### template 和 typename
typename什么地方使用: 用在模板定义里，标明其后的模板参数是类型参数. e.g.:  
```C++
template<typename  T, typename Y>
T foo(const T& t, const Y& y){//....};

templace<typename T>
class CTest
{
private:
 T t;
public:
 //...
}
```

typename第二个应用: 模板中标明“内嵌依赖类型名”.

### class构造函数冒号问题
```C++
class TEST
{
    public:

    int b;

    int c;

    int a;

   TEST(int x, int y):a(x),b(y),c(0){}
   //相当于: 
   TEST(int x, int y)
   {
    a = x; b = y; c =0;
   }
};
```
冒号的效果就是用括号内的值，来初始化成员变量值。与函数内部赋值相比，初始化列表的方式更高效。

### 字符串搜索
KMP算法(复杂, 后面学)










