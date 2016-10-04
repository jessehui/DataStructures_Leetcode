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

###二叉树
tree: 一种新型的链表数据结构. 对于一个节点来说它只有0,1,2个子节点, 就是二叉树. 没有子节点的节点是叶子leaf节点










