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

如果是二维vector:  
```C++
vector<vector<int>> g;

g.size();//是第一维长度
g[0].size();  //是第二维长度
```

### 图
图和树的区别就是图最下边的节点可以重新链接回root. 如果任意两个节点都有连接, 那就是完全图.  
用邻接矩阵表示图中节点的关系. 如果图比较稀疏, 可以用邻接表(链表)来表达. 用数组表示所有节点, 再用链表表示所有和这个节点相连的边.


一般使用递归的深度优先算法(DFS)都可以用栈来解决.

### 最小生成树
和图类似, 但是每两个节点之间的值不再是1, 而是有各自的长度. 求最小生成树, 要求遍历所有点所用的总长度最小, 即所有点联通(从任意一点, 可以去其他的任意一点)所用的总长度最小. 求最小生成树有很多算法, kruskal算法是其中一种.
kruskal算法: 从最小的边开始加, 如果有个边可以被相邻的两个边所走过的点表示, 就不需要这条边了.
一般适合来找边比较少(远小于顶点个数V^2).  
如果边的数量很多, 接近V^2, 那么使用另外一种算法: Prim算法


实现的话要用到一个`并查集`的数据结构(Disjoint set), 是一种树形的数据结构.其用于处理一些不相交集合（Disjoint Sets）的合并及查询问题.  这个数据结构应该提供的方法: 查找(find), 合并(union). 
Find：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。
Union：将两个子集合并成同一个集合。

### auto关键字
编程语言中的“动态类型”,在运行时来进行类型检查，而C++中类型检查是在编译阶段。动态类型语言能做到在运行时决定类型，主要归功于一技术，这技术是类型推导。事实上，类型推导也可以用于静态类型语言中.C++11中类型推导的实现之一就是重定义auto关键字，另一个实现是decltype。
```C++

// 1. 自动帮助推导类型  
    auto a = 10;  
    auto c = 'A';  
    auto s("hello");  

// 3. 使用模板技术时，如果某个变量的类型依赖于模板参数，  
// 不使用auto将很难确定变量的类型（使用auto后，将由编译器自动进行确定）。  
template <class T, class U>  
void Multiply(T t, U u)  
{  
    auto v = t * u;  
}  

//例子 
using namespace std;  
int main()  
{  
  auto name=‘world\n’  
  cout<<"hello   "<<name<<endl;  
  
}  
```
这里使用auto关键字来要求编译器对变量name的类型进行了自动推导。这里编译器根据它的初始化表达式的类型，推导出name的类型为char*




### list
 C++ STL中的list是双向循环链表，每一个元素都知道前面一个元素和后面一个元素。  
 push_back()方法也是在最后添加一个元素.


### C++参数传递

值传递：形参是实参的拷贝，改变形参的值并不会影响外部实参的值。从被调用函数的角度来说，值传递是单向的（实参->形参），参数的值只能传入,不能传出。当函数内部需要修改参数，并且不希望这个改变影响调用者时，采用值传递。  

指针传递：形参为指向实参地址的指针，当对形参的指向操作时，就相当于对实参本身进行的操作.e.g.
`void change3(int *n)`

引用传递：形参相当于是实参的“别名”，对形参的操作其实就是对实参的操作，在引用传递过程中，被调函数的形式参数虽然也作为局部变量在栈中开辟了内存空间，但是这时存放的是由主调函数放进来的实参变量的地址。被调函数对形参的任何操作都被处理成间接寻址，即通过栈中存放的地址访问主调函数中的实参变量。正因为如此，被调函数对形参做的任何操作都影响了主调函数中的实参变量。e.g.`void change2(int & n)`

引用传递和指针传递有什么区别吗？
引用的规则： 
（1）引用被创建的同时必须被初始化（指针则可以在任何时候被初始化）。   
（2）不能有NULL引用，引用必须与合法的存储单元关联（指针则可以是NULL）。   
（3）一旦引用被初始化，就不能改变引用的关系（指针则可以随时改变所指的对象）。  

从概念上讲。指针从本质上讲就是存放变量地址的一个变量，在逻辑上是独立的，它可以被改变，包括其所指向的地址的改变和其指向的地址中所存放的数据的改变。
而引用是一个别名，它在逻辑上不是独立的，它的存在具有依附性，所以引用必须在一开始就被初始化，而且其引用的对象在其整个生命周期中是不能被改变的（自始至终只能依附于同一个变量）。

总结: C++中尽量用引用传递代替指针传递, 更安全一些.


### C++ struct和class
由于struct 和 class 的可替换性，什么时候用struct 和什么时候用class的选择就相当主观了。通常大家的直觉是一致的: struct 应该应用于POD(Plain old data)类型的对象. 用一个词来描述，他们更像是记录, 一个简单的集合，里面有几个字段， 例如 struct Color, struct Rect, struct Point 等都是我们常见的结构。

而class 实际上更适合用于抽象主动的对象, 他们通常可以有复杂的继承关系(个人认为太复杂是一种作死的行为，稍后解释)。 或许有更多的方法和逻辑。对于class来讲，内部数据除了理解为记录, 更有一部分是“状态”。

另外一个struct 的好处是:

它可以很方便的序列化和反序列话，比如，直接拿到一个struct 的指针。 sizeof取得大小，直接把对象存储到文件或写入网络。当然基于某些原因。我也不建议这么做。

### git提交例子
shell文件反选例子: 
```shell
git add `ls |grep -v "kruskal.cpp"`
```

### std::sort()
利用标准模板库中的sort函数进行升序排序.
```C++
//a<b 1; a>b 0
bool compare_weight(edge a, edge b)
{
  return a.weight < b.weight
}
  //sorting edges by weight
  //这里用的C++ STL中的排序函数sort(), 最后一个参数是一个自己定义的函数,表示用这个函数来进行排序, 这样可以sort升序或者降序
  //前边的两个参数表示范围
  std::sort(edges.begin(), edges.end(), compare_weight);

```

### 单源最短路径算法
出发点固定 从出发点到任意一点的距离固定. Dijkastra算法.

###C++ operator关键字
```C++ 
class test1
{
public: 
        void operator()(int x)
        {
                cout<<x<<endl;
        }
};

void test2(int x)
{
        cout<<x<<endl;
}

int main()
{
        f(test1());
        f(test2);
}
```
所以operator()就是重载操作符. 

###priority_queue
优先队列是计算机科学中的一类抽象数据类型。优先队列中的每个元素都有各自的优先级，优先级最高的元素最先得到服务；优先级相同的元素按照其在优先队列中的顺序得到服务。优先队列往往用堆来实现。
优先队列是队列的一种，不过它可以按照自定义的一种方式（数据的优先级）来对队列中的数据进行动态的排序每次的push和pop操作，队列都会动态的调整，以达到我们预期的方式来存储。例如：我们常用的操作就是对数据排序，优先队列默认的是数据大的优先级高所以我们无论按照什么顺序push一堆数，最终在队列里总是top出最大的元素。  
`priority_queue<int, vector<int>, greater<int> >pq; `  
第二个参数为容器类型。 第三个参数为比较函数.

###hash函数 和 hash表
哈希查找因使用哈希 (Hash)函数而得名，哈希函数又叫散列函数，它是一种能把关键字映射成记录存贮地址的函数。  
哈希表（Hash table，也叫散列表），是根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。
哈希表hash table(key，value) 的做法其实很简单，就是把Key通过一个固定的算法函数既所谓的哈希函数转换成一个整型数字，然后就将该数字对数组长度进行取余，取余结果就当作数组的下标，将value存储在以该数字为下标的数组空间里。而当使用哈希表进行查询的时候，就是再次使用哈希函数将key转换为对应的数组下标，并定位到该空间获取value，如此一来，就可以充分利用到数组的定位性能进行数据定位.
Hash，一般翻译做“散列”，也有直接音译为“哈希”的，就是把任意长度的输入（又叫做预映射， pre-image），通过散列算法，变换成固定长度的输出，该输出就是散列值。这种转换是一种压缩映射，也就是，散列值的空间通常远小于输入的空间.不同的输入可能会散列成相同的输出，而不可能从散列值来唯一的确定输入值。简单的说就是一种将任意长度的消息压缩到某一固定长度的消息摘要的函数。
HASH主要用于信息安全领域中加密算法，它把一些不同长度的信息转化成杂乱的128位的编码,这些编码叫做HASH值. 也可以说，hash就是找到一种数据内容和数据存放地址之间的映射关系。(MD5校验)
数组的特点是：寻址容易，插入和删除困难；而链表的特点是：寻址困难，插入和删除容易。那么我们能不能综合两者的特性，做出一种寻址容易，插入删除也容易的数据结构？答案是肯定的，这就是哈希表，哈希表有多种不同的实现方法，最常用的一种方法——拉链法，我们可以理解为“链表的数组”

set和map: set是一个集合, map是key和value的有序对(通过key查找时间复杂度为O(1),通过value查找时间复杂度是O(n))


### C++ 模板库unordered_set
C++ 11中出现了两种新的关联容器:unordered_set和unordered_map，其内部实现与set和map大有不同，set和map内部实现是基于RB-Tree(红黑树)，而unordered_set和unordered_map内部实现是基于哈希表(hashtable).
通过前面说的哈希函数，会发现其都位于数组的相同位置，这里，就涉及到“冲突”。准确来说，冲突是不可避免的，而解决冲突的方法常见的有：开发地址法、再散列法、链地址法(也称拉链法)。而unordered_set内部解决冲突采用的是----链地址法，当用冲突发生时把具有同一关键码的数据组成一个链表。


### const 
对于非内部数据类型的参数而言，象voidFunc(A a) 这样声明的函数注定效率比较底。因为函数体内将产生A类型的临时对象用于复制参数a，而临时对象的构造、复制、析构过程都将消耗时间。
为了提高效率，可以将函数声明改为voidFunc(A &a)，因为“引用传递”仅借用一下参数的别名而已，不需要产生临时对象。但是函数voidFunc(A &a) 存在一个缺点：  
“引用传递”有可能改变参数a，这是我们不期望的。解决这个问题很容易，加const修饰即可，因此函数最终成为voidFunc(const A &a)。

The addition of an ‘&’ to the parameter name in C++ (which was a very confusing choice of symbol because an ‘&’ in front of variables elsewhere in C generates pointers!) causes the actual variable itself, rather than a copy, to be used as the parameter in the subroutine and therefore can be written to thereby passing data back out the subroutine. Therefore  
```C++
void Subroutine3(int &Parameter1) 
{ Parameter1=96;}
```  
would set the variable it was called with to 96. This method of passing a variable as itself rather than a copy is called a ‘reference’ in C++.

>const 成员函数

任何不会修改数据成员的函数都应该声明为const类型。如果在编写const成员函数时，不慎修改了数据成员，或者调用了其它非const成员函数，编译器将指出错误，这无疑会提高程序的健壮性。以下程序中，类stack的成员函数GetCount仅用于计数，从逻辑上讲GetCount应当为const函数。编译器将指出GetCount函数中的错误。
```C++ 
class Stack
{
public:
  void Push(int elem);
  int Pop(void);
  intGetCount(void) const; // const 成员函数
private:
  intm_num;
  int m_data[100];
};

int Stack::GetCount(void)const
{
  ++ m_num; // 编译错误，企图修改数据成员m_num
  Pop();// 编译错误，企图调用非const函数
  returnm_num;
}
```

### 红黑树



### 寻路算法
1. Lee's algorithm:   
1) Initialization

 - Select start point, mark with 0
 - i := 0

2) Wave expansion

 - REPEAT
     - Mark all unlabeled neighbors of points marked with i with i+1
     - i := i+1
   UNTIL ((target reached) or (no points can be marked))

3) Backtrace

   - go to the target point
   REPEAT
     - go to next node that has a lower mark than the current node
     - add this node to path
   UNTIL (start point reached)

4) Clearance

 - Block the path for future wirings
 - Delete all marks


2. A* algorithm
video tutorial: [https://www.youtube.com/watch?v=KNXfSOx4eEE]  
tutorial: [http://www.cnblogs.com/yanlingyin/archive/2012/01/15/2322640.html]

算法演示及与Lee's Algorithm 的比较: [https://www.youtube.com/watch?v=QWLIWr9V7dQ]

### 关于计算机算法中的Heuristic方法
Heuristic 启发式方法（试探法）是一种帮你寻求答案的技术，但它给出的答案是具有偶然性的（subject to chance），因为启发式方法仅仅告诉你该如何去找，而没有告诉你要找什么。它并不告诉你该如何直接从A 点到达B 点，它甚至可能连A点和B点在哪里都不知道。实际上，启发式方法是穿着小丑儿外套的算法：它的结果不太好预测，也更有趣，但不会给你什么30 天无效退款的保证。   

驾驶汽车到达某人的家，写成算法是这样的：沿167 号高速公路往南行至Puyallup；从South Hill Mall 出口出来后往山上开4.5 英里；在一个杂物店旁边的红绿灯路口右转，接着在第一个路口左转；从左边褐色大房子的车道进去，就是North Cedar 路714 号。

用启发式方法来描述则可能是这样：找出上一次我们寄给你的信，照着信上面的寄出地址开车到这个镇；到了之后你问一下我们的房子在哪里。这里每个人都认识我们——肯定有人会很愿意帮助你的；如果你找不到人，那就找个公共电话亭给我们打电话，我们会出来接你。

算法和启发式方法之间的差别很微妙，两个术语的意思也有一些重叠。就本书的目的而言，它们之间的差别就在于其距离最终解决办法的间接程度：算法直接给你解决问题的指导，而启发式方法则告诉你该如何发现这些指导信息，或者至少到哪里去寻找它们。

启发式算法的优点在于它比盲目型的搜索法要高效，一个经过仔细设计的启发函数，往往在很快的时间内就可得到一个搜索问题的最优解，对于NP问题，亦可在多项式时间内得到一个较优解。


### 强联通子图
从任一点出发可以到达任意一点. 无环.  
可以用来控制模块文件之间的依赖关系. 


