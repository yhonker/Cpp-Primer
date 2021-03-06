## 拷贝控制习题解答
### 13.1.1

#### 13.1
```c++
1.如果一个构造函数的第一个参数是自身类类型的引用，且任何额外参数都有默认值，则此构造函数是拷贝构造函数。
2.当我们使用拷贝初始化时会调用拷贝构造函数。
```
#### 13.2
```c++
1.参数类型应该是引用类型。
```
#### 13.3
```c++

```
#### 13.4
```c++
//标记处调用了拷贝构造函数
Point global;
Point foo_bar(Point arg){/* 1 */
    Point local = arg /* 2 */ , *heap = new Point(global)/* 3 */;
    *heap = local;
    Point pa[4] = {local/* 4 */, *heap/* 5 */};
    return *heap;/* 6 */
}
```
#### 13.5
```c++
class HasPtr{
public:
    HasPtr(const std::string &s = std::string()):\
        ps(new std::string(s), i(0)){ }
    HasPtr(const HasPtr &rhs);//新增拷贝构造函数
private:
    std::string *ps;
    int i;
};

HasPtr::HasPtr(const HasPtr &rhs):ps(new std::string(*rhs.ps)), i(rhs.i){ }

```

### 13.1.2
#### 13.6
```c++
1.拷贝赋值运算符是一个名为 operator= 的函数。
2.当赋值运算发生时就会用到它。
3.合成拷贝赋值运算符可以用来禁止该类型对象的赋值。
4.如果一个类未定义自己的拷贝赋值运算符，编译器会为它生成一个合成拷贝赋值运算符。
```
#### 13.7
```c++
1.发生浅复制
2.
```
#### 13.8
```c++
HasPtr &HasPtr::operator=(const &rhs){
    string *temp = new string(*rhs.ps);//支持自赋值
    delete ps;
    ps = temp;
    i = rhs.i;
    return *this;
}
```
### 13.1.3
#### 13.9
```c++
1.名字由波浪号接类名构成的成员函数。
2.在一个析构函数中，首先执行函数体，然后销毁成员，成员按逆序销毁。
3.无论何时一个对象被销毁,就会自动调用其析构函数。
```
#### 13.10
```c++

```
#### 13.11
```c++
HasPtr::~HasPtr(){
    delete ps;
}
```
#### 13.12
```c++
bool fcn(const Sales_data *trans, Sales_data accum){
    Sales_data item1(*trans), item2(accum);
    return item1.isbn() != item2.isbn();
}//3次析构，accum, item1, item2
```
#### 13.13
```c++
struct X{
    X(){cout << "X()" << endl;}
    X(const &rhs){cout << "X(const &rhs)" <<endl;}
    X &operator=(const &rhs){cout << "operator=(const &rhs)" <<endl;}
    ~X(){cout << "~X()" <<endl;}
};
```
### 13.1.4
#### 13.14
```c++
1.输出三个完全一样的数字
```
#### 13.15
```c++
1.这个数不属于这三个对象，结果会输出三个不同的数。
```
#### 13.16
```c++
1.输出a, b, c中的数。
```
#### 13.17
```c++
class numbered{//13.16
public:
    numbered(){mysn = sn++;}   
    numbered(const numbered &rhs){mysn = sn++;}
    int mysn;
    static int sn;
};
int numbered::sn = 1;
void f(const numbered& rhs){cout << rhs.mysn << endl;}

```
### 13.1.6
#### 13.18, 13.19
```c++
1.需要

class Employee{
public:
    Employee():eid(id++){}
    Employee(const string& s):name(s),eid(id++){}
    Employee(const Employee &rhs):name(rhs.s),eid(id++){}
    string name;
    int eid;
    static int id; 
};
int Employee::id = 0;
```
#### 13.20
```c++

```
#### 13.21
```c++

```
### 13.2
#### 13.22
```c++
HasPtr::HasPtr(const HasPtr &rhs):ps(new string(*rhs.ps)),i(rhs.i){}
HasPtr &HasPtr::operator=(const HasPtr &rhs){
    HasPtr *temp = new string(*rhs.ps);
    delete ps;
    ps = temp;
    i = rhs.i;
    return *this;
}
```
### 13.2.1
#### 13.23
```c++

```
#### 13.24
```c++
1.ps得不到释放，会造成内存泄漏。
2.多个对象的ps指向同一个地址。
```
#### 13.25
```c++

```
#### 13.26
```c++

```
### 13.2.2
#### 13.27
```c++
类指针版本HasPtr
class HasPtr{
public:
    HasPtr(const string &s):ps(new string(s)),i(0),use(new size_t(1)) {}
    HasPtr(const HasPtr &rhs):ps(rhs.ps),i(rhs.i),use(rhs.use) {*use++}
    HasPtr &operator=(const HasPtr &rhs){
       ++*rhs.use;
       if(--*use == 0){//?
           delete ps;
           delete use;
       }
       ps = rhs.ps;
       i = rhs.i;
       use = rhs.use
       return *this;
    }
    ~HasPtr(){
        if(--*use == 0){
            delete ps;
            delete use;
        }
    }
};    
```
#### 13.28
```c++
class TreeNode{
public:
    TreeNode(const string &s, int c):value(s),count(c),\
            left(new TreeNode()),right(new TreeNode()){}
    TreeNode(const TreeNode &rhs):value(rhs.value),count(rhs.count),\
        left(new TreeNode(*rhs.left)),right(new TreeNode(*rhs.right)){}
    TreeNode &operator=(const TreeNode &rhs){
        TreeNode *l = new TreeNode(*rhs.l);
        TreeNode *r = new TreeNode(*rhs.r);
        delete left;
        delete right;
        left = l;
        right = r;
        value = rhs.value;
        count = rhs.count;
        return *this;
    }
    ~TreeNode(){
        delete left;
        delete right;
    }
private:
    string value;
    int count;
    TreeNode *left;
    TreeNode *right;
};

class BinStrTree{
public:
    BinTree(const TreeNode* r):root(new TreeNode()){ }
    BinTree(const BinTree& rhs):root(new TreeNode(new TreeNode(*rhs.root)))
    BinTree &operator=(const BinTree& rhs){
        TreeNode *r = new TreeNode(*rhs.root);
        delete root;
        root = r;
        return *this;
    }
    ~BinTree(){
        delete root;
    }
};
```
### 13.3
#### 13.29
```c++
1.实际上是三个不同swap()函数。
```
#### 13.30
```c++
void swap(HasPtr2 &lhs, HasPtr2& rhs){
    using std::swap;
    swap(lhs.ps, rhs.ps);
    swap(lhs.i, rhs.i);
}    
```
#### 13.31
```c++

```
#### 13.32
```c++
1.不会。类值的版本利用swap交换指针不用进行内存分配，因此得到了性能上的提升。
类指针的版本本来就不用进行内存分配，所以不会得到性能提升。
```
### 13.4
#### 13.33
```c++
1.Message的save和remove会更新传入的Folder对象。
```
#### 13.34
```c++
class Folder;

class Message{
    friend class Folder;
    friend void swap(Message &lhs, Message &rhs);
public:
    explicit Message(const string &s = ""):contents(s) { }
    Message(const Message &rhs);
    Message &operator=(const Message &rhs);
    ~Message();
    void save(Folder &f);
    void remove(Folder &f);

private:
    string contents;
    set<Folder*> folders;
    void add_to_Folders(const Message &m);
    void remove_from_Folder();
    void addFlr(Folder *f){ folders.insert(f);}
    void removeFlr(Folder *f){ folders.erase(f);}
    
};

void Message::save(Folder &f){
    f.insert(&f);
    f.addMsg(this);
}

void Message::remove(Folder &f){
    f.erase(&f);
    f.remMsg(this);
}

void Message::add_to_Folders(const Message &m){
    for(auto it:m.folders){
        it->addMsg(this);
    }
}

Message::Message(const Message &rhs):contents(rhs.contents),\
    folders(rhs.folders){
    add_to_Folders(rhs);
}
void Message::remove_from_Folders(){
    for(auto it:folders){
        it->remMsg(this);
    }
}
Message &Message::operator=(const Message &rhs){
    remove_from_Folders();
    contents = rhs.contents;
    folders = rhs.folders;
    add_to_Folders(rhs);
    return *this;
}

Message::~Message(){
    remove_from_Folders();
}

void swap(Message &lhs, Message &rhs){
    using std::swap;
    lhs.remove_from_Folders();
    rhs.remove_from_Folders();
    swap(lhs.folders, rhs.folders);
    swap(lhs.contents, rhs.contents);
    lhs.add_to_Folders(lhs);
    rhs.add_to_Folders(rhs);
}
```
#### 13.35
```c++
1.Message无法在Folder中得到添加。
```
#### 13.36
```c++
class Folder{
    friend class Message;
    friend void swap(Folder &lhs, Folder &rhs);
    
public:
    Folder() = default;
    Folder(const Folder &rhs):messages(rhs.messages){ add_to_Messages(rhs);}
    Folder &operator=(const Folder &rhs);
    ~Folder(){ remove_from_Messages();}
    
private:
    set<Message*> messages;
    void addMsg(Message *m){ messages.insert(m);}
    void remMsg(Message *m){ messages.erase(m);}//给Message对象调用,不能主动添加Message
    void add_to_Messages(const Folder &f);
    void remove_from_Messages();

}    
    
void Folder::add_to_Messages(const Folder &f){
    for(auto it:f.messages)
    it->addFlr(this);
}

void Folder::remove_from_Messages(){
    for(auto it:f.messages);
    it->remFlr(this);
}

Folder &Folder::operator=(const Folder &rhs){
    remove_from_Messages();
    messages = rhs.messages;
    add_to_Messages(rhs);
    return *this;
}

viod swap(Folder &lhs, Folder &&rhs){
    lhs.remove_from_Messages();
    rhs.remove_from_Messages();
    swap(lhs.messages, rhs.messages);
    lhs.add_to_Message(lhs);
    rhs.add_to_Message(rhs);
}
```
#### 13.37
```c++
-->13.34
```
#### 13.38
```c++
1.Message对象不需要开辟新的堆空间，即不需要动态分配空间。
```
### 13.5
#### 13.39
```c++
void StrVec::reserve(size_t n) {
    if(n<=capacity()) return;
    auto first = alloc.allocate(n);
    auto last = uninitialized_copy(make_move_iterator(begin()), make_move_iterator(end()), first);
    free();
    elements = first;
    first_free = last;
    cap = elements + n;
}

void StrVec::resize(size_t count) {
    if(count > size()){
        if(count > capacity()) reserve(count * 2);
        for(size_t i = size(); i != count; ++i)
            alloc.construct(first_free++, string(" "));
    } else if(count < size()){
        while(first_free != elements + count)
            alloc.destroy(--first_free);
    }
}
```
#### 13.40
```c++
StrVec(initializer_list<string> il);
void range_initialize(const std::string *first, const std::string *last);

StrVec::StrVec(initializer_list<string> il) {
range_initialize(il.begin(), il.end());
}

void StrVec::range_initialize(const std::string *first, const std::string *last)
{
auto newdata = alloc_n_copy(first, last);
elements = newdata.first;
first_free = cap = newdata.second;
}
```
#### 13.41
```c++
1.因为first_free指向第一个空位置,后置递增会使第一个空位置被忽略。
```
#### 13.42
```c++

```
#### 13.43
```c++

```
### 13.6.1
#### 13.45
```c++
1.常规引用被定义为左值引用。
2.绑定到右值的引用被称为右值引用。
```
#### 13.46
```c++
1.右值
2.左值
3.左值
4.右值
```
#### 13.47
```c++

```
#### 13.48
```c++

```

