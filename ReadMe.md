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
应用: 函数的调用. 比如:
```C
void fun_A();
void fun_B();
void fun_C();

int main()
{
  fun_A();
}

void fun_A()
{
 fun_B();
}

void fun_C()
{
  //...
}
```
程序在运行的时候, 先装载主程序`main`函数, 然后在`main`中调用`fun_A`, 就把该函数也装载到栈中, 然后装载`fun_B`, `fun_C`. 所以在`fun_C`中是不能调用fun_A中的资源的. 然后`fun_C`执行完后, pop掉, `fun_B`继续执行. 就是利用了栈来恢复现场.

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
在c++Template中很多地方都用到了typename与class这两个关键字，而且好像可以替换，是不是这两个关键字完全一样呢?  
答：class用于定义类，在模板引入c++后，最初定义模板的方法为：template<class T>，这里class关键字表明T是一个类型，后来为了避免class在这两个地方的使用可能给人带来混淆，所以引入了typename这个关键字，它的作用同class一样表明后面的符号为一个类型，这样在定义模板的时候就可以使用下面的方式了：       template<typename T>. 在模板定义语法中关键字class与typename的作用完全一样。

一个类模板（也称为类属类或类生成类）允许用户为类定义一种模式，使得类中的某些数据成员、默写成员函数的参数、某些成员函数的返回值，能够取任意类型（包括系统预定义的和用户自定义的）。  
如果一个类中数据成员的数据类型不能确定，或者是某个成员函数的参数或返回值的类型不能确定，就必须将此类声明为模板，它的存在不是代表一个具体的、实际的类，而是代表着一类类。






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

### static关键字
static全局变量与普通的全局变量有什么区别 ?  
静态全局变量则限制了其作用域， 即只在定义该变量的源文件内有效， 在同一源程序的其它源文件中不能使用它。由于静态全局变量的作用域局限于一个源文件内，只能为该源文件内的函数公用，因此可以避免在其它源文件中引起错误。 __static全局变量只初使化一次，防止在其他文件单元中被引用.__

static局部变量和普通局部变量有什么区别 ？  
把局部变量改变为静态变量后是改变了它的存储方式即改变了它的生存期。把全局变量改变为静态变量后是改变了它的作用域，限制了它的使用范围。 __static局部变量只被初始化一次，下一次依据上一次结果.__

static函数与普通函数有什么区别？  
static函数与普通函数作用域不同,仅在本文件。只在当前源文件中使用的函数应该说明为内部函数(static修饰的函数)，内部函数应该在当前源文件中说明和定义。对于可在当前源文件以外使用的函数，应该在一个头文件中说明，要使用这些函数的源文件要包含这个头文件. static函数在内存中只有一份，普通函数在每个被调用中维持一份拷贝

### const关键字
1. 可以定义const常量 const int Max = 100; 可以用来代替宏定义. 宏定义不能定义类型, 使用const便于编译器检查.
2. 可以保护被修饰的东西. 

```C
void f(const int i) { i=10;//error! }
      //如果在函数体内修改了i，编译器就会报错
```

3. 节省空间, 提高效率. const定义常量从汇编的角度来看，只是给出了对应的内存地址，而不是象#define一样给出的是立即数，所以，const定义的常量在程序运行过程中只有一份拷贝，而#define定义的常量在内存中有若干个拷贝. 编译器通常不为普通const常量分配存储空间，而是将它们保存在符号表中，这使得它成为一个编译期间的常量，没有了存储与读内存的操作，使得它的效率也很高


### 使用数组存储二叉树
完全二叉树: 若设二叉树的深度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第 h 层所有的结点都连续集中在最左边，这就是完全二叉树。  
除叶子节点外，每一层上的所有结点都有两个子结点（最后一层上的无子结点的结点为叶子结点）  
平衡二叉树: 当且仅当两个子树的高度差不超过1时，这个树是平衡二叉树。（同时是排序二叉树）  

只有完全二叉树才能放在数组中. 每层从左到右, 从浅到深放置. 这个时候如果要取子节点, 注意: 对于从0开始作为index的数组, 第i个元素的子节点分别为`2i+1`, `2i+2`(因为每一层的节点数都是上一层的2倍)

堆(heap): 堆(heap)又被为优先队列(priority queue). 尽管名为优先队列，但堆并不是队列。在堆中，我们不是按照元素进入队列的先后顺序取出元素的，而是按照元素的优先级取出元素。
堆的一个经典的实现是完全二叉树(complete binary tree)。这样实现的堆成为二叉堆(binary heap)。
为了实现堆的操作，我们额外增加一个要求: 任意节点的优先级不小于它的子节点或者反过来都不大于.

要插入新元素: 先放在数组最后, 然后看它符合的关系再调换位置.  
要删除节点: 比如删除根节点, 就把最后的元素放在删除了的节点上, 然后按符合的关系调换位置直到满足要求. (拿取根节点可以看做是堆排序)


### C++友元
只有类的成员函数才能访问类的私有成员，程式中的其他函数是无法访问私有成员的。非成员函数能够访问类中的公有成员，但是假如将数据成员都定义 为公有的，这又破坏了隐藏的特性。另外，应该看到在某些情况下，特别是在对某些成员函数多次调用时，由于参数传递，类型检查和安全性检查等都需要时间开 销，而影响程式的运行效率。

　　为了解决上述问题，提出一种使用友元的方案。友元是一种定义在类外部的普通函数，但他需要在类体内进行说 明，为了和该类的成员函数加以区别，在说明时前面加以关键字friend。友元不是成员函数，但是他能够访问类中的私有成员。友元的作用在于提高程式的运 行效率，但是，他破坏了类的封装性和隐藏性，使得非成员函数能够访问类的私有成员。

　　友元能够是个函数，该函数被称为友元函数(friend function)；友元也能够是个类，该类被称为友元类。

```C++
class Point
　　{
　　public:
　　　　Point(double xx, double yy) { x=xx; y=yy; }
　　　　void Getxy();
　　　　friend double Distance(Point &a, Point &b);
　　private:
　　　　double x, y;
　　};

　　void Point::Getxy()
　　{
　　cout<<"("<<<","<<<")"<< FONT>
　　}

　　double Distance(Point &a, Point &b)
　　{
　　double dx = a.x - b.x;
　　double dy = a.y - b.y;
　　return sqrt(dx*dx+dy*dy);
　　}

　　void main()
　　{
　　Point p1(3.0, 4.0), p2(6.0, 8.0);
　　p1.Getxy();
　　p2.Getxy();
　　double d = Distance(p1, p2);
　　cout<<"Distance is"<<< FONT>
　　}```

在该程式中的Point类中说明了一个友元函数Distance()，他在说明时前边加friend关键字，标识他不是成员函数，而是友元函数。 他的定义方法和普通函数定义相同，而不同于成员函数的定义，因为他无需指出所属的类。但是，他能够引用类中的私有成员，函数体中 a.x，b.x，a.y，b.y都是类的私有成员，他们是通过对象引用的。在调用友元函数时，也是同普通函数的调用相同，不要像成员函数那样调用。本例 中，p1.Getxy()和p2.Getxy()这是成员函数的调用，要用对象来表示。而Distance(p1, p2)是友元函数的调用，他直接调 用，无需对象表示，他的参数是对象。(该程式的功能是已知两点坐标，求出两点的距离。)



### C++ class中的 public, private, protected
类的一个特征就是封装，public和private作用就是实现这一目的。所以：
用户代码（类外）可以访问public成员而不能访问private成员；private成员只能由类成员（类内）和友元访问。

类的另一个特征就是继承，protected的作用就是实现这一目的。所以：
protected成员可以被派生类对象访问，不能被用户代码（类外）访问。

有public, protected, private三种继承方式，它们相应地改变了基类成员的访问属性。  
1.public继承：基类public成员，protected成员，private成员的访问属性在派生类中分别变成：public, protected, private
2.protected继承：基类public成员，protected成员，private成员的访问属性在派生类中分别变成：protected, protected, private
3.private继承：基类public成员，protected成员，private成员的访问属性在派生类中分别变成：private, private, private
但无论哪种继承方式，上面两点都没有改变：
1.private成员只能被本类成员（类内）和友元访问，不能被派生类访问；
2.protected成员可以被派生类访问。


```C++
#include<iostream>
#include<assert.h>
using namespace std;

class A{
public:
  int a;
  A(){
    a1 = 1;
    a2 = 2;
    a3 = 3;
    a = 4;
  }
  void fun(){
    cout << a << endl;    //正确
    cout << a1 << endl;   //正确
    cout << a2 << endl;   //正确
    cout << a3 << endl;   //正确
  }
public:
  int a1;
protected:
  int a2;
private:
  int a3;
};

//public 继承
class B : public A{
public:
  int a;
  B(int i){
    A();
    a = i;
  }
  void fun(){
    cout << a << endl;       //正确，public成员
    cout << a1 << endl;       //正确，基类的public成员，在派生类中仍是public成员。
    cout << a2 << endl;       //正确，基类的protected成员，在派生类中仍是protected可以被派生类访问。
    cout << a3 << endl;       //错误，基类的private成员不能被派生类访问。
  }
};
int main(){
  B b(10);
  cout << b.a << endl;
  cout << b.a1 << endl;   //正确
  cout << b.a2 << endl;   //错误，类外不能访问protected成员
  cout << b.a3 << endl;   //错误，类外不能访问private成员
  system("pause");
  return 0;
}

//protected 继承
class B : protected A{
public:
  int a;
  B(int i){
    A();
    a = i;
  }
  void fun(){
    cout << a << endl;       //正确，public成员。
    cout << a1 << endl;       //正确，基类的public成员，在派生类中变成了protected，可以被派生类访问。
    cout << a2 << endl;       //正确，基类的protected成员，在派生类中还是protected，可以被派生类访问。
    cout << a3 << endl;       //错误，基类的private成员不能被派生类访问。
  }
};
int main(){
  B b(10);
  cout << b.a << endl;       //正确。public成员
  cout << b.a1 << endl;      //错误，protected成员不能在类外访问。
  cout << b.a2 << endl;      //错误，protected成员不能在类外访问。
  cout << b.a3 << endl;      //错误，private成员不能在类外访问。
  system("pause");
  return 0;
}




//private 继承
class B : private A{
public:
  int a;
  B(int i){
    A();
    a = i;
  }
  void fun(){
    cout << a << endl;       //正确，public成员。
    cout << a1 << endl;       //正确，基类public成员,在派生类中变成了private,可以被派生类访问。
    cout << a2 << endl;       //正确，基类的protected成员，在派生类中变成了private,可以被派生类访问。
    cout << a3 << endl;       //错误，基类的private成员不能被派生类访问。
  }
};
int main(){
  B b(10);
  cout << b.a << endl;       //正确。public成员
  cout << b.a1 << endl;      //错误，private成员不能在类外访问。
  cout << b.a2 << endl;      //错误, private成员不能在类外访问。
  cout << b.a3 << endl;      //错误，private成员不能在类外访问。
  system("pause");
  return 0;
}

```


### C++的一些名词
面向对象的三大特征：
1.封装(Encapsulation)：保证对象自身数据的完整性、安全性
2.继承(inheritance)：建立类之间的关系，实现代码复用、方便系统的扩展
3.多态(polymorphism)：相同的方法调用可实现不同的实现方式。多态是指两个或多个属于不同类的对象，对于同一个消息（方法调用）作出不同响应的方式。多态性可以简单地概括为“一个接口，多种方法”，程序在运行时才决定调用的函数，它是面向对象编程领域的核心概念。多态(polymorphism)，字面意思多种形状。
C++支持两种多态性：编译时多态性，运行时多态性。

a. 编译时多态性：通过重载函数实现

b. 运行时多态性：通过虚函数实现。

### 虚函数
同一类中是不能定义两个名字相同、参数个数和类型都相同的函数的，否则就是“重复定义”。但是在类的继承层次结构中，在不同的层次中可以出现名字相同、参数个数和类型都相同而功能不同的函数。  
人们提出这样的设想，能否用同一个调用形式，既能调用派生类又能调用基类的同名函数。在程序中不是通过不同的对象名去调用不同派生层次中的同名函数，而是通过__指针__调用它们。  
例如，用同一个语句“pt->display( );”可以调用不同派生层次中的display函数，只需在调用前给指针变量 pt 赋以不同的值(使之指向不同的类对象)即可。  
C++中的虚函数就是用来解决这个问题的。虚函数的作用是允许在派生类中重新定义与基类同名的函数，并且可以通过基类指针或引用来访问基类和派生类中的同名函数。


虚函数为了重载和多态的需要，在基类中是有定义的，即便定义是空. 所以子类中可以重写也可以不写基类中的函数！

纯虚函数在基类中是没有定义的，必须在子类中加以实现. 引入原因：为了方便使用多态特性，我们常常需要在基类中定义虚函数。  
Example: 
```C++
class Cman

{

public:

virtual void Eat(){……};

void Move();

private:

};

class CChild : public CMan

{

public:

virtual void Eat(){……};

private:

};

CMan m_man;

CChild m_child;


//这才是使用的精髓，如果不定义基类的指针去使用，没有太大的意义

CMan *p ;

p = &m_man ;

p->Eat(); //始终调用CMan的Eat成员函数，不会调用 CChild 的

p = &m_child;

p->Eat(); //如果子类实现(覆盖)了该方法，则始终调用CChild的Eat函数

//不会调用CMan 的 Eat 方法；如果子类没有实现该函数，则调用CMan的Eat函数

p->Move(); //子类中没有该成员函数，所以调用的是基类中的
```

基类的析构函数都为虚函数. 


### vector
```C++
vector<int> vec;
vec.push_back(1);  
```
push_back在vector结尾插入一个新的元素. vector是用数组实现的，每次执行push_back操作，相当于底层的数组实现要重新分配大小（即先free掉原存储，后重新malloc）；这种实现体现到vector实现就是每当push_back一个元素,都要重新分配一个大一个元素的存储，然后将原来的元素拷贝到新的存储，之后在拷贝push_back的元素，最后要析构原有的vector并释放原有的内存。

vec.back();      // 传回最后一个数据，不检查这个数据是否存在。
vec.end();       // 指向迭代器中末端元素的下一个，指向一个不存在元素。
vec.pop_back();  //删除最后一个数据, 并减少容器尺寸












