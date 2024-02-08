<a name="GKrsu"></a>
# STL初始
<a name="GGipQ"></a>
## STL基本概念

- STL（Standard Template Library）标准模板库
- STL从广义上分，容器（container）、算法（algorithm）、迭代器（iterator）
- 容器和算法之间通过迭代器进行无缝连接
- STL几乎所有代码都采用了模板类或者模板函数

函数参数范围都是左闭右开区间
<a name="lXTcd"></a>
## STL六大组件

1. 容器：各种数据结构，如vector、list、deque、set、map等，用来存放数据
2. 算法：各种常用算法，如sort、find、copy、for_each等
3. 迭代器：扮演了容器与算法之间的胶合剂
4. 仿函数：行为类似函数，可作为算法的某种策略
5. 适配器：一种用来修饰容器或者仿函数或迭代器接口的东西
6. 空间适配器：负责空间的配置与管理
<a name="aQ4sv"></a>
## STL中的容器、算法、迭代器
**容器**：置物之所也<br />就是将运用最广泛的一些数据结构实现出来<br />常用的数据结构：数组，链表，树，栈，队列，集合，映射表等<br />这些容器分为序列式容器和关联式容器<br />序列式容器：强调值的排序，序列式容器中的每个元素均有固定的位置<br />关联式容器：二叉树结构，各元素之间没有严格的物理上的顺序关系

**算法**：问题之解也<br />有限的步骤，解决逻辑或数学上的问题，这一门学科叫算法<br />算法分为：质变算法和非质变算法<br />质变算法：是指运算过程中会更改区间内的元素的内容。如拷贝，替换，删除等<br />非质变算法：是指运算过程中不会更改区间内的元素内容，例如查找，计数遍历，找极值等

**迭代器**：提供一种方法，能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部方式<br />每个容器都有自己专属的迭代器<br />迭代器使用非常类似于指针<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697891704314-8900e429-9f49-4931-bfcc-7171ef60e3fe.png#averageHue=%23f8f7f6&clientId=u02ce8fae-9e97-4&from=paste&height=395&id=u8f84fd1c&originHeight=494&originWidth=1497&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=492869&status=done&style=none&taskId=u22c68ec5-76cc-42ad-b0eb-b11270d4308&title=&width=1197.6)<br />常用的容器中迭代器种类为双向迭代器，和随机访问迭代器
<a name="UWTQY"></a>
## 容器算法迭代器初识
<a name="dZqOB"></a>
### vector存放内置数据类型
容器：vector<br />算法：for_each<br />迭代器：vector<int>::iterator
```cpp
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

void MyPrint(int _val)
{
    cout << _val << endl;
}

void Test01()
{
    vector<int> v;

    v.push_back(10);
    v.push_back(20);

    vector<int>::iterator pb = v.begin();
    vector<int>::iterator pe = v.end();

    //第一种遍历
    while(pb != pe)
    {
        cout << *pb << endl;
        pb++;
    }

    //第二种
    for(vector<int>::iterator it = v.begin(); it != v.end(); it++)
    {
       cout << *it << endl; 
    }

    //第三种,#include<algorithm>
    for_each(v.begin(), v.end(), MyPrint);
}

int main()
{
    Test01();
}


```
<a name="PevT4"></a>
### vector存放自定义数据类型
```cpp
#include<iostream>
#include<vector>
#include<string>
using namespace std;

class Person()
{
public:
    Preson(string _name, int _age)
    {
        mName = _name;
        mAge = _age;
    }
public:
    string mName;
    int mAge;
}

void Test01()
{
    vector<Preson> v;

    Preson p1("a", 1);
    Preson p2("b", 2);

    v.push_back(p1);
    v.push_back(p2);
    
    for(vector<Preson>::iterator it = v.begin(); it != v.end(); it++)
    {
       cout << (*it).mName << (*it).mAge <<endl; 
    }
}
void Test02()
{
    vector<*Preson> v;

    Preson p1("a", 1);
    Preson p2("b", 2);

    v.push_back(p1);
    v.push_back(p2);
    
    for(vector<*Preson>::iterator it = v.begin(); it != v.end(); it++)
    {
       cout << (**it).mName << (**it).mAge <<endl; 
    }
}
int main()
{
    Test01();
    Test02();
}
```
<a name="aEHdC"></a>
### vector容器嵌套容器
```cpp
vector<vector<int>> v;
vector<int> v1;
vector<int> v2;
for(int i = 0; i < 2; i++)
{
    v1.push_back(i + 1);
    v2.push_back(i + 2);
}

for(vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++)
{
    for(vector<int>::iterator vit = (*it).begin(); vit != (*it).end(); vit++)
    {
        cout << *vit << " ";
    }
}
```
<a name="mQA9P"></a>
# STL常用容器
<a name="FKDhG"></a>
## string容器
<a name="EXDlt"></a>
### 构造函数
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697894475547-171d596a-f7c2-4589-9082-7d2d8402097e.png#averageHue=%23f8fbf8&clientId=u02ce8fae-9e97-4&from=paste&height=250&id=ubc8ff814&originHeight=612&originWidth=1151&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=505337&status=done&style=none&taskId=ud3a2e4cc-22cc-4ded-be63-757f581f1f2&title=&width=470.4000244140625)

- string();	创建一个空字符串
- string(const char* s);	使用字符串s初始化
- string(const string& s);	使用string对象初始化另一个string对象
- string(int n, char c);	使用n个字符c初始化
```cpp
#include<string>

string s1;

string s2("aaa");

 char* str = "bbb";
string s3(str);

string s4(10, 'a');
```
<a name="V7Ext"></a>
### 赋值=, assign
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697973845964-8c2f8f69-79b2-4108-b494-1208fdd79bc2.png#averageHue=%23f4f7f4&clientId=u7f7ba3c7-f8cd-4&from=paste&height=398&id=uff0f8257&originHeight=497&originWidth=1250&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=607531&status=done&style=none&taskId=u542ca877-70be-4187-b82f-101ea5fcbbd&title=&width=1000)
```cpp
string str1.assign("aaa");
string str2.assign("bbbbbb", 3);
string str3.assign(str1);
string str4.assign(5, "a"
    );
```
<a name="aRKaq"></a>
### 拼接+, append
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697974103505-b9599a2b-4dce-453d-b531-4ab18a739240.png#averageHue=%23f7faf7&clientId=u7f7ba3c7-f8cd-4&from=paste&height=402&id=u0c2ffb55&originHeight=502&originWidth=1489&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=676881&status=done&style=none&taskId=uf92d6e59-7249-4247-9913-2d1c9d42f82&title=&width=1191.2)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697974353853-be72bd9a-d34a-4dc5-a898-a66e0da9d429.png#averageHue=%23f4f7f4&clientId=u7f7ba3c7-f8cd-4&from=paste&height=210&id=u0df0e525&originHeight=262&originWidth=1092&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=269891&status=done&style=none&taskId=uba56415a-ceb9-4260-9e0c-ed976c0f73c&title=&width=873.6)
<a name="fd1xt"></a>
### 查找和替换find，replace
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697974399903-72275741-9e8f-46aa-a973-e3fbd88f04c5.png#averageHue=%23f6f9f6&clientId=u7f7ba3c7-f8cd-4&from=paste&height=550&id=u5f15f906&originHeight=687&originWidth=1487&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1096895&status=done&style=none&taskId=u33db136e-a309-483f-893b-749d8360eed&title=&width=1189.6)<br />rfind从右往左查，find从左往右查。下标位置都是从左开始<br />替换，会将整个字符串替换进去
```cpp
string s1 = "111111111";
s1.replace(0, 3, "222");

//222111111111
```
<a name="AvhLg"></a>
### 字符串比较compare
字符串比较是按字符的ASCII码进行对比<br />=返回0<br />>返回1<br /><返回-1<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697974921506-c85c4b2f-faba-447a-8c82-ec232ed8471f.png#averageHue=%23f7faf7&clientId=u7f7ba3c7-f8cd-4&from=paste&height=158&id=u5fb18b78&originHeight=197&originWidth=846&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=152055&status=done&style=none&taskId=u5c8fc166-453b-446f-8681-1bffb8ad5eb&title=&width=676.8)<br />有第一个不同的就会结束比较，一般用来判断两个字符串是否相等
<a name="bG1QE"></a>
### 字符存取[],at
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697975174997-b41ad3f7-2c3d-48ff-ad1f-f46095ef450a.png#averageHue=%23f6f9f6&clientId=u7f7ba3c7-f8cd-4&from=paste&height=110&id=u5e3c2b22&originHeight=138&originWidth=774&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=121729&status=done&style=none&taskId=u5b4b6a4e-a6c4-45a4-aa86-d3b1758ff93&title=&width=619.2)
<a name="eG74M"></a>
### 字符串插入和删除insert,erase
起始下标从0开始<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697975301603-fe745bd6-3825-439d-a733-61e093d84623.png#averageHue=%23f7faf7&clientId=u7f7ba3c7-f8cd-4&from=paste&height=255&id=ud7be0b1a&originHeight=319&originWidth=1159&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=333258&status=done&style=none&taskId=u5c0a88b5-1839-4ea7-9629-43cfb095e13&title=&width=927.2)
<a name="ODgSC"></a>
### 子串获取substr
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697975433414-b3936c91-ce80-45ce-92f0-5d68b46fea73.png#averageHue=%23e1dfd8&clientId=u7f7ba3c7-f8cd-4&from=paste&height=106&id=ucff2e7ad&originHeight=133&originWidth=1357&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=121007&status=done&style=none&taskId=u1f32646b-71a0-46db-8620-175c9fe83db&title=&width=1085.6)
<a name="KcFSj"></a>
## vector容器
<a name="cwalh"></a>
### 构造函数
vector数据结构和数组非常相似，也称为**单端数组**<br />数组是静态空间，vector是动态空间<br />**动态扩展：**并不是在原空间之后续接新空间，而是找更大的空间，然后将原数据拷贝新空间，释放原空间<br />跟realloc有点像<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697976232145-00e288ff-6653-49a2-9dbf-dc811730f4b3.png#averageHue=%23f5f8f5&clientId=u7f7ba3c7-f8cd-4&from=paste&height=242&id=u2f861e06&originHeight=470&originWidth=921&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=295755&status=done&style=none&taskId=ua2c6deaa-557e-4562-b812-71092295c6e&title=&width=474.79998779296875)<br />vector容器的迭代器是支持随机访问的迭代器<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697976340547-ee340e7b-7cb2-41a8-81dd-db3ff8de912f.png#averageHue=%23f8fbf8&clientId=u7f7ba3c7-f8cd-4&from=paste&height=265&id=ua8dc2494&originHeight=331&originWidth=1166&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=341173&status=done&style=none&taskId=u4d3a83c1-e814-46ab-b63a-881e96f5af1&title=&width=932.8)
```cpp
vector<int> v1;

for(int i = 0; i < 10; i++)
{
    v1.push_back(i);
}

for(vector<int>::iterator it = v1.begin(); it != v1.end(); it++)
{
    cout << *it << " ";
}

vector<int> v2(v1.begin(), v1.end());
vector<int> v3(10, 100);
vector<int> v4(v1);
```
<a name="HuoK1"></a>
### 赋值操作=, assign
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697976850286-85c1db38-85e2-418d-85e5-fc0f7192b127.png#averageHue=%23f7f9f6&clientId=u7f7ba3c7-f8cd-4&from=paste&height=172&id=ucc76faa0&originHeight=215&originWidth=1019&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=240429&status=done&style=none&taskId=ub0bc72b9-5fd7-47e5-8ed7-162856e44b7&title=&width=815.2)
<a name="d3j3y"></a>
### 容量和大小empty, capacity, size, resize
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697977130845-53bf24ea-7ef1-4cb2-899f-cec64d2c89cd.png#averageHue=%23f8fbf8&clientId=u7f7ba3c7-f8cd-4&from=paste&height=405&id=u4583944c&originHeight=506&originWidth=1372&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=571121&status=done&style=none&taskId=u6a0b8765-7265-440d-b411-4cfba6eb921&title=&width=1097.6)<br />缩小大小，容量不会变化
<a name="GongU"></a>
### vector插入和删除insert, push_back, pop_back, erase, clear
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697977326386-9cab662f-3d05-4884-b8de-93f45569d432.png#averageHue=%23f8fbf8&clientId=u7f7ba3c7-f8cd-4&from=paste&height=416&id=ud1ef629d&originHeight=520&originWidth=1308&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=576183&status=done&style=none&taskId=uedbe3f0f-f931-4946-948e-b0fc9e6aeb2&title=&width=1046.4)
<a name="s74wz"></a>
### 数据存取[], at, front, back
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697977412185-8474205b-2a5a-4a8c-b926-ee0bc907b308.png#averageHue=%23f7faf6&clientId=u7f7ba3c7-f8cd-4&from=paste&height=251&id=u58f98e7b&originHeight=314&originWidth=732&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=246704&status=done&style=none&taskId=u51fe73ba-f830-4184-9bdb-114fa6c8868&title=&width=585.6)
<a name="HZvZK"></a>
### 互换容器swap
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697977505778-e3c8b2ba-2c25-4d69-a5a5-bf66719b469e.png#averageHue=%23f9fcf9&clientId=u7f7ba3c7-f8cd-4&from=paste&height=119&id=ueab7b525&originHeight=149&originWidth=617&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=73384&status=done&style=none&taskId=ua68dd825-bb81-4ae3-8eb4-d1ddb21638a&title=&width=493.6)
```cpp
vecto<int> v1;
vector<int> v2;
v1.swap(v2);

//收缩内存
vector<int>(v1).swap(v1)
//匿名对象，执行完，系统会回收内存
```
<a name="y7yV1"></a>
### 预留空间reserve
减少vector在动态扩展容量时的扩展次数<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697978072508-4835f67c-73b3-4df6-80e3-d594918ef778.png#averageHue=%23f9fcf9&clientId=u7f7ba3c7-f8cd-4&from=paste&height=106&id=u95bae6fb&originHeight=133&originWidth=1204&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=129390&status=done&style=none&taskId=uda2d834b-1734-43de-87b8-750b9e2c350&title=&width=963.2)
```cpp
vector<int> v;
int* p = NULL;
int num = 0;

//计算动态扩展了多少次空间
for(int i = 0; i < 100000; i++)
{
    v.push_back(i);
    if(p != &v[0])
    {
        p = &v[0];
        ++num;
    }
}

v.reserve(100000);
```
<a name="ezvFi"></a>
## deque容器
<a name="Q8b58"></a>
### 构造函数
双端数组，可以对头端进行插入删除操作<br />**deque与vector区别**

- vector对于头部插入删除效率低，数据量越大，效率越低
- deque，对头部的插入删除操作更快
- vector访问元素时的速度更快，与两者的内部实现有关

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697978800404-372af40e-a0ae-41d7-bfcd-84915e25e357.png#averageHue=%23f8fbf8&clientId=u7f7ba3c7-f8cd-4&from=paste&height=244&id=ubeca05cf&originHeight=586&originWidth=957&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=346650&status=done&style=none&taskId=u009562fb-e1c5-48d6-be17-c298f5d3ca6&title=&width=398.4000244140625)<br />**内部工作原理：**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697978933426-ef6d3650-7ca4-462d-9bf0-6a2f9e65728d.png#averageHue=%23f9fdfa&clientId=u7f7ba3c7-f8cd-4&from=paste&height=293&id=u710c4adc&originHeight=702&originWidth=1256&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=580207&status=done&style=none&taskId=u4b3040ee-9c8d-4063-90ba-d055b5e67cc&title=&width=523.4000244140625)<br />deque的迭代器也是支持随机访问的<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697979002174-4ef7e065-1995-48d0-9d0b-d8f6ca63091e.png#averageHue=%23f8fbf8&clientId=u7f7ba3c7-f8cd-4&from=paste&height=265&id=u901194b9&originHeight=331&originWidth=1130&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=308585&status=done&style=none&taskId=u2eabee7b-3d98-495f-b54f-10e46bdb335&title=&width=904)
```cpp
void Print(const deque<int>& d)
{
	for(deque<int>::const_iterator it = d.begin(); it != d.end(); it++)
	{
        cout << *it << " ";
	}
}
```
<a name="Ftjra"></a>
### 赋值=，assign
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697979286112-53d371bb-2dc4-43ae-8af6-d861d003fc18.png#averageHue=%23f8fbf8&clientId=u7f7ba3c7-f8cd-4&from=paste&height=170&id=ud43703e9&originHeight=212&originWidth=1263&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=232044&status=done&style=none&taskId=uaef293ad-3368-4825-8cbe-cc738bd3c1d&title=&width=1010.4)
<a name="rFTpe"></a>
### 大小操作empty，size，resize
跟vector比没有容量的概念<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697979340416-ebe5779e-f45f-4dbd-8280-e40a75c4aa5d.png#averageHue=%23f7f7f7&clientId=u7f7ba3c7-f8cd-4&from=paste&height=355&id=udd2b06d0&originHeight=444&originWidth=1400&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=461226&status=done&style=none&taskId=u8e3b35d7-80ba-4fb6-83eb-49e66375c05&title=&width=1120)
<a name="W9iwK"></a>
### 插入和删除
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697979464610-862ead34-8538-487b-b8b0-4e50b620d088.png#averageHue=%23f8fbf8&clientId=u7f7ba3c7-f8cd-4&from=paste&height=676&id=ue85febec&originHeight=845&originWidth=1217&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=827375&status=done&style=none&taskId=u87d2c539-1b66-488f-a947-0c98252bf16&title=&width=973.6)
<a name="WyXp7"></a>
### 数据存取at，[]，front，back
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697979589202-78b277fa-2c38-46cf-974b-98f62a99028f.png#averageHue=%23f7faf6&clientId=u7f7ba3c7-f8cd-4&from=paste&height=246&id=u9dc07670&originHeight=308&originWidth=718&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=238288&status=done&style=none&taskId=ufb92a35e-fa04-4856-95f4-86ca95c0bdc&title=&width=574.4)
<a name="JHIUn"></a>
### deque排序sort
利用算法实现对deque容器进行排序0<br />需要头文件, algorithm<br />默认从小到大排序，升序<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697979699957-a025fba7-f4ab-464d-b01f-5cd0561464d7.png#averageHue=%23f9fcf9&clientId=u7f7ba3c7-f8cd-4&from=paste&height=113&id=u02306244&originHeight=141&originWidth=1073&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=109476&status=done&style=none&taskId=u617b11eb-f765-41f6-a595-4a1a9451bf1&title=&width=858.4)
<a name="smbxI"></a>
## stack容器
<a name="q6253"></a>
### 基本概念
FILO，先进后出。栈中只有顶端元素才可以被外界使用，因此栈不允许有遍历行为<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697980592110-4a0b1e80-f8d0-4680-b409-21559e514e9d.png#averageHue=%23f8fbf8&clientId=u7f7ba3c7-f8cd-4&from=paste&height=407&id=u77f76579&originHeight=748&originWidth=784&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=324001&status=done&style=none&taskId=u4307e3e5-0bc7-41d1-b760-010bc12f313&title=&width=426.20001220703125)
<a name="Sf1Pa"></a>
### 常用接口
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697980564084-39ec2158-3145-4396-b1ae-02295b704592.png#averageHue=%23f9fcf9&clientId=u7f7ba3c7-f8cd-4&from=paste&height=680&id=ude972476&originHeight=850&originWidth=1268&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=526222&status=done&style=none&taskId=u6d4b0766-7917-4403-9504-04550c3cd7d&title=&width=1014.4)
<a name="BtmrI"></a>
## queue容器
<a name="bFIXp"></a>
### 基本概念
先进先出，FIFO，有两个出口<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697980783351-879dbb14-28a2-4349-a266-58ff22677391.png#averageHue=%23f9fcf9&clientId=u7f7ba3c7-f8cd-4&from=paste&height=258&id=uf1b1d210&originHeight=530&originWidth=1085&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=329285&status=done&style=none&taskId=u32c62813-e177-4d3b-b573-2f340d72278&title=&width=527.4000244140625)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697980828645-a29e8de4-e460-4e21-bcab-cb17dc5225e7.png#averageHue=%23f9fcf9&clientId=u7f7ba3c7-f8cd-4&from=paste&height=222&id=u647042e1&originHeight=277&originWidth=954&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=263225&status=done&style=none&taskId=ucf5446e0-2503-4581-baed-63e19c7ab31&title=&width=763.2)
<a name="jmyYv"></a>
### 常用接口
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1697980896878-d540153e-78a3-405d-ad8d-af1ae0cc6b76.png#averageHue=%23f9fcf9&clientId=u7f7ba3c7-f8cd-4&from=paste&height=719&id=u5aca8b55&originHeight=899&originWidth=1274&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=590622&status=done&style=none&taskId=u388690b0-5542-4d38-9dbc-ea85fad5224&title=&width=1019.2)
<a name="p9DzO"></a>
## list容器
<a name="u2WKt"></a>
### 构造函数
将数据进行链式储存<br />链表是一种物理储存单元上非连续性的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的<br />链表由一系列结点组成<br />结点：数据域和指针域<br />STL中的链表是双向循环链表<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698137057086-dcf3ea7a-52fd-4770-9d7a-220528b4f191.png#averageHue=%23f8fbf8&clientId=u8da1d6aa-2818-4&from=paste&height=317&id=u8e9053b5&originHeight=714&originWidth=1119&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=611333&status=done&style=none&taskId=uf211669f-a6cd-4832-9bfd-18e54b8ca95&title=&width=497.4000244140625)<br />链表list中的迭代器只支持前移和后移，属于双向迭代器<br />优点：

- 采用动态存储分配，不会造成内存浪费和溢出
- 齿形插入和删除操作十分方便，修改指针即可，不需要移动大量元素

缺点：

- 链表灵活，但空间（指针域）和时间（遍历）额外耗费较大

list插入删除操作都不会造成原有list迭代器失效，这在vector是不成立的，vector会寻找新的空间<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698137321966-07960930-417c-4643-9b4a-e540ac000195.png#averageHue=%23f8f8f8&clientId=u8da1d6aa-2818-4&from=paste&height=284&id=ub082160e&originHeight=355&originWidth=1165&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=282410&status=done&style=none&taskId=ue8fc6984-f8b8-4727-a62b-6a820954e8c&title=&width=932)
<a name="UxkLy"></a>
### 赋值和交换assign，=，swap
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698137339723-2db1aea0-3d57-4bb0-8be6-fd701ef3fb14.png#averageHue=%23f8fbf8&clientId=u8da1d6aa-2818-4&from=paste&height=188&id=ua20af43e&originHeight=183&originWidth=587&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=105332&status=done&style=none&taskId=u04b6e315-8704-4394-ba9d-9f77827f195&title=&width=604.6000061035156)
<a name="mp6eF"></a>
### 大小操作size，empty，rsize
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698137367418-9387a436-f5c3-4fbf-9a25-15f983b0ff14.png#averageHue=%23f8fbf8&clientId=u8da1d6aa-2818-4&from=paste&height=362&id=ucf79419b&originHeight=452&originWidth=1338&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=499538&status=done&style=none&taskId=u28e31d06-53d1-4854-8c51-b01f66388e2&title=&width=1070.4)
<a name="nlbrw"></a>
### 插入和删除
可以直接删除某个值<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698137461126-f470d4c1-f7e4-4d58-a564-d58945d60588.png#averageHue=%23f8fbf8&clientId=u8da1d6aa-2818-4&from=paste&height=607&id=u3ad0cb88&originHeight=759&originWidth=1041&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=810259&status=done&style=none&taskId=ub256ec04-23e4-4441-a6d6-3e49df20b09&title=&width=832.8)
<a name="fc4d0"></a>
### 数据存取front，back
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698137631748-44ee6f90-41fd-4a0b-9f33-da6fe9c4c89d.png#averageHue=%23f8fbf8&clientId=u8da1d6aa-2818-4&from=paste&height=173&id=u526ad9c4&originHeight=216&originWidth=569&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=102190&status=done&style=none&taskId=ue4b2355d-79f8-4cc5-bf34-76af19aa383&title=&width=455.2)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698137929457-8632825f-d7fe-4af2-8677-aa83858ce9f5.png#averageHue=%23eef4ef&clientId=u8da1d6aa-2818-4&from=paste&height=190&id=u2a3f1487&originHeight=237&originWidth=661&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=216022&status=done&style=none&taskId=u59e6a492-bc91-4c9e-931b-c0279aa098e&title=&width=528.8)
<a name="be8Jz"></a>
### 反转和排序reverse，sort
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698137964585-9a45875d-97d4-4bd7-b407-47855386051c.png#averageHue=%23f7faf7&clientId=u8da1d6aa-2818-4&from=paste&height=151&id=ub54953f4&originHeight=189&originWidth=458&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=82867&status=done&style=none&taskId=u723b4dda-fc63-4215-821c-805ff2b973e&title=&width=366.4)
```cpp
bool MyCompare(int val1, int val2)
{
    return val1 > val2;
}

List<int> L;

L.reverse();
L.sort();	//默认从小到大
L.sort(MyCompare);	//设定为从大到小排序
```
<a name="zjv9N"></a>
## set / multiset 容器（集合）
<a name="lXSyS"></a>
### 构造和赋值
所有元素插入时**自动被排序**<br />set/multiset属于**关联式容器**，底层结构二叉树实现<br />只用包含set头文件<br />**set和multiset区别：**

- set不允许容器中有重复元素
- multise中允许容器中有重复元素

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698138663034-236eb726-0ee4-4ac4-b2ac-ee5fcfc3b8f6.png#averageHue=%23f9fcf9&clientId=u8da1d6aa-2818-4&from=paste&height=274&id=u9e4ea7e9&originHeight=343&originWidth=824&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=189860&status=done&style=none&taskId=u523990da-62da-4ebb-8176-0964d6630e5&title=&width=659.2)
<a name="TvfJq"></a>
### 大小和交换size，empty，swap
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698138842712-8d9194ef-265d-4b95-a81c-c2f9fdbd6271.png#averageHue=%23f7faf7&clientId=u8da1d6aa-2818-4&from=paste&height=213&id=ud598ff1b&originHeight=266&originWidth=603&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=167396&status=done&style=none&taskId=u1b639364-29b1-4e89-91e5-480f18b60bf&title=&width=482.4)
<a name="LdGJs"></a>
### 插入和删除insert，clear，erase
**插入只有insert！**<br />**可以直接删除某个值**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698138911789-4ed795dd-9f9c-4047-8692-6a1f53d652fe.png#averageHue=%23f8fbf7&clientId=u8da1d6aa-2818-4&from=paste&height=302&id=u89dd16c8&originHeight=377&originWidth=1178&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=400439&status=done&style=none&taskId=u6a404906-a1fa-4efe-aa4d-0a6937edeb5&title=&width=942.4)
<a name="JN9lv"></a>
### 查找和统计find，count
count对于set只会是0或1，对multiset不一定<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698139174409-98852f57-42ae-4c22-b828-1aee919a14c4.png#averageHue=%23f9f9f8&clientId=u8da1d6aa-2818-4&from=paste&height=177&id=ub9eb18b1&originHeight=221&originWidth=1450&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=149437&status=done&style=none&taskId=u89b3cb05-24fa-43d3-a31a-8e88cd39a24&title=&width=1160)
```cpp
set<int> s1;

set<int>::iterator pos = s1.find(30);
```
<a name="nTKx0"></a>
### set和multiset的区别
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698139390470-44b0a8ca-b507-4e64-b1bf-38fa7e7e141c.png#averageHue=%23f7faf7&clientId=u8da1d6aa-2818-4&from=paste&height=207&id=u36c8ea75&originHeight=259&originWidth=809&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=212828&status=done&style=none&taskId=uddc8ecaa-46b4-45d9-9b9c-c5ecd0efbe3&title=&width=647.2)
```cpp
set<int> s;
pair<set<int>::iterator, bool> ret = s.insert(10);
if(set.second)
{
    cout << "Successful insertion";
}

multiset<int> ms;
ms.insert(10);
ms.insert(10);
```
<a name="AiWNL"></a>
### pair对组的创建
成对出现的数据，利用对组可返回两个数据<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698139834911-a64dc50a-26cf-4977-bf44-812aa4972729.png#averageHue=%23f7faf7&clientId=u8da1d6aa-2818-4&from=paste&height=160&id=u75fe0995&originHeight=200&originWidth=826&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=134848&status=done&style=none&taskId=u5a51f508-2258-400e-a8bc-13e95dca732&title=&width=660.8)
```cpp
pair<string, int> p(string("Tom"), 20);
cout << p.first << p.second;

pair<string, int> p2 = make_pair("Jerry", 10);
cout << p2.first << p2.second;
```
<a name="H5FOF"></a>
### 内置类型指定排序规则
利用仿函数，可以改变排序规则
```cpp
class MyCompare()
{
public:
    bool operator()(int v1, int v2)
    {
        return v1 > v2;
    }
};

set<int, MyCompare> s;
s.insert(10);
s.insert(220);
s.insert(50);

for(set<int, MyCompare>::iterator it = s.begin(); it != s.end(); it++)
{
    cout << *it << " ";
}
```
<a name="B9f9D"></a>
### 自定义数据类型指定排序规则
```cpp
class Person
{
public:
	Person(string _name, int _age)
	{
        name = _name;
        age = _age;
    }

	string name;
	int age;
};

class MyCompare()
{
public: 
    bool operator()(const Person& p1, const Person& p2)
    {
        return p1.age > p2.age;
    }
};

set<Person, MyCompare> s;
s.insert("鲁路修", 16);
s.insert("C.C.", 500);

for(set<Person, MyCompare>::iterator it = s.begin(); it != s.end(); it++)
{
    cout << *it.name << *it.age;
}
```
<a name="mjYcT"></a>
## map / multimap容器
<a name="nbGhw"></a>
### 构造和赋值
高性能，高效率<br />**简介：**

- map中所有元素都是pair
- pair中第一个元素为key（键值），起到索引作用，第二个元素为value（实值）
- 所有元素都会根据元素的键值自动排序

**本质：**

- map/multimap属于关联式容器，底层结构是用二叉树实现

**优点：**

- 可以根据key值快速找到value值

**map和multimap的区别：**

- map不允许容器中有重复key值元素
- multimap允许容器中有重复key值元素

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698149231012-1afd41fd-aa38-4d03-ac0a-b0a9dd6e0f21.png#averageHue=%23f9fcf9&clientId=u8da1d6aa-2818-4&from=paste&height=327&id=u4dec4d77&originHeight=409&originWidth=817&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=219106&status=done&style=none&taskId=u838799f2-d091-44e9-b6e4-4593ee6f1f4&title=&width=653.6)
<a name="NpXRK"></a>
### 大小和交换size，empty，swap
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698149314038-f85060a9-7f0a-45fd-8497-d550a0005645.png#averageHue=%23f7faf7&clientId=u8da1d6aa-2818-4&from=paste&height=203&id=u7c9aaf09&originHeight=254&originWidth=594&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=147545&status=done&style=none&taskId=uab50ea1f-6e63-419e-9a27-f8b794a5f83&title=&width=475.2)
<a name="D2Ow1"></a>
### 插入和删除insert，erase，clear
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698149371208-5c515b44-ebe1-4143-bcfb-94798384d54a.png#averageHue=%23f8fbf8&clientId=u8da1d6aa-2818-4&from=paste&height=311&id=uede8e811&originHeight=389&originWidth=1178&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=376707&status=done&style=none&taskId=u187bd910-c324-4a87-9094-610a20149fa&title=&width=942.4)
```cpp
map<int, int> m;
m.insert(pair<int, int>(1, 10));
m.insert(make_pair(3, 10));
m.insert(map<int, int>::value_type(3, 30));
m[4] = 40;

m.erase(m.begin());
m.erase(3);
```
通过[]访问不存在的键值，会创建新的键值对，键值为该访问的键值，实值为0
<a name="CsHgh"></a>
### 查找和统计find，count
**函数原型：**

- find(key); 	//查找key是否存在，若存在，返回该键的元素的迭代器；若不存在，返回map.end();
- count(key); 	//统计key的元素个数
```cpp
map<int, int> m;
m.insert(pair<int, int>(1, 10));
m.insert(pair<int, int>(2, 10));
m.insert(pair<int, int>(3, 10));

map<int, int>::iteraror pos = m.find(3);
if(pos != m.end())
{
    cout << "yes";
}
else
{
    cout << "no";
}
```
<a name="e9CDq"></a>
### 排序
依旧利用仿函数，更改为自动排序为从大到小排序
```cpp
class MyCompare()
{
public:
    bool operator()(int v1, int v2)
    {
        return v1 > v2;
    }
}

...

for(map<int, int, MyCompare>::iterator it = m.begin(); it != m.end(); it++)
{
	cout << it -> first << it -> second;
}
```
<a name="NKDXM"></a>
# STL函数对象
<a name="sV5Oh"></a>
## 函数对象
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698151161434-32c8ff1f-c42c-44da-98a3-04f5f6de44bc.png#averageHue=%23fafdf9&clientId=u8da1d6aa-2818-4&from=paste&height=277&id=u7391e251&originHeight=346&originWidth=863&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=235736&status=done&style=none&taskId=u5576adee-6186-4d6e-b4fd-afcce20c376&title=&width=690.4)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698151141462-bd04f795-c5c8-49e1-b8e3-feffbe8b0bcf.png#averageHue=%23f9fcf9&clientId=u8da1d6aa-2818-4&from=paste&height=210&id=ubfaaafa7&originHeight=263&originWidth=1056&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=223547&status=done&style=none&taskId=u70b83be7-86a9-4a3f-9f98-cba25980b7c&title=&width=844.8)
```cpp
class MyAdd
{
public:
	int operator()(int v1, int v2)
	{
        return v1 + v2;
    }
}

MyAdd myAdd;
cout << myAdd(1, 2);

class MyPrint
{
public:
	MyPrint()
	{
        count = 0;
    }
	void operator()(string s)
	{
        cout << s << endl;
        count++;
    }

	int count; 	//内部自己的状态
}

Myprint myPrint;
myPrint("纯血的魔女久远寺有珠");
cout << myPrint.count;

void Test(MyPrint& mp, string s)
{
    mp(s);
}

MyPrint mp;
Test(mp, "不要小看人类的觉悟啊");
```
<a name="rdGu1"></a>
## 谓词
<a name="qdP5X"></a>
### 一元谓词
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698152170748-eeb84d28-45f8-43e7-87c7-000d46a93a1c.png#averageHue=%23f7faf7&clientId=u8da1d6aa-2818-4&from=paste&height=205&id=ucda7eeba&originHeight=256&originWidth=734&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=197210&status=done&style=none&taskId=ua94f17d6-659a-402a-9bfc-e2777a842ea&title=&width=587.2)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698152375657-06bd3508-3058-41df-a74e-a4f21ab84ccb.png#averageHue=%23f3f6f3&clientId=u8da1d6aa-2818-4&from=paste&height=177&id=u144cdbc2&originHeight=221&originWidth=467&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=93855&status=done&style=none&taskId=u6dd45b66-937e-4bd4-abf4-64e94dc4a40&title=&width=373.6)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698152400317-c40e9858-f4ae-4b83-b4c8-0efbc1130f2a.png#averageHue=%23f4f7f4&clientId=u8da1d6aa-2818-4&from=paste&height=474&id=uafdc7c08&originHeight=592&originWidth=1058&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=285378&status=done&style=none&taskId=uf4413800-acd0-4c0b-8921-82d263c8200&title=&width=846.4)<br />使用find_if需要#include<algroithm>
<a name="ziBBe"></a>
### 二元谓词
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698152660872-fc62f08d-a953-4258-a3b0-9507111a7b2d.png#averageHue=%23f3f6f3&clientId=u8da1d6aa-2818-4&from=paste&height=182&id=u995b87e1&originHeight=227&originWidth=485&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=89175&status=done&style=none&taskId=u95abc93a-89f8-464e-8c4d-31a7d1cda61&title=&width=388)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698152680558-babc2f53-1fc3-4a2f-91a1-23d8bf5b526e.png#averageHue=%23f3f6f2&clientId=u8da1d6aa-2818-4&from=paste&height=82&id=u3442bea2&originHeight=102&originWidth=587&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=108173&status=done&style=none&taskId=uf3ddea2c-dcb3-406e-9000-df7496372f4&title=&width=469.6)
<a name="GmPC8"></a>
## 内建函数对象
<a name="tphUB"></a>
### 内建函数对象的意义
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698152842435-807f168f-af82-4c48-beb9-6b779ed984c2.png#averageHue=%23fafdfa&clientId=u8da1d6aa-2818-4&from=paste&height=378&id=uc9b917f7&originHeight=617&originWidth=942&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=300605&status=done&style=none&taskId=u9848d55c-4167-475b-b805-cd35c10ab1d&title=&width=577.4000244140625)
<a name="zdc5r"></a>
### 算数仿函数
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698153206522-c86aa8ff-0a96-4692-a7ad-4dd6a67fe05e.png#averageHue=%23f5f8f5&clientId=u8da1d6aa-2818-4&from=paste&height=415&id=u11b79b63&originHeight=519&originWidth=813&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=450568&status=done&style=none&taskId=u69381762-ec54-4594-850b-4a99ce9a76c&title=&width=650.4)
<a name="cSSN2"></a>
### 关系仿函数
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698153378610-b81f2c3a-9d0e-4a91-88f5-b6f4258112b6.png#averageHue=%23f7faf7&clientId=u8da1d6aa-2818-4&from=paste&height=416&id=u13e3d50f&originHeight=520&originWidth=908&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=408546&status=done&style=none&taskId=ub5ad587a-75c2-47bf-8558-ee16c3f65d7&title=&width=726.4)
```cpp
#include<vector>
#include<functional>
#include<algorithm>

vector<int> v;
sort(v.begin(), v.end(), greater<nt>());
```
<a name="txwyZ"></a>
### 逻辑仿函数
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698153628330-1874e44e-283c-41a6-b52b-f6ba667090ed.png#averageHue=%23f6f9f6&clientId=u8da1d6aa-2818-4&from=paste&height=206&id=u7b67eb91&originHeight=258&originWidth=877&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=222042&status=done&style=none&taskId=u7cce11a8-f7ac-4301-bfbc-098325d4298&title=&width=701.6)
```cpp
vector<bool> v1;
v1.push_back(false);
v1.push_back(true);

vector<bool> v2;
v2.resize(v1.size());

//全部取反了
transform(v1.begin(), v1.end(), v2.begin(), logical_not<bool>());
```
<a name="KFamP"></a>
# 常用算法
<a name="K5fqi"></a>
## 概述

- 算法主要是由头文件<algorithm> <functional> <numeric> 组成
- <algorithm> 是所有STL头文件中最大的一个，范围涉及比较、交换、查找、遍历操作、复制、修改等等
- <numeric> 体积很小，只包括几个在序列上面进行简单数学运算的函数模板
- <functional> 定义了一些模板类，用以声明函数对象
<a name="kGsEB"></a>
## 遍历算法
<a name="bF7gm"></a>
### for_each 遍历
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698238816697-66b8d448-d58e-4257-a3d4-9c2adcccbc65.png#averageHue=%23f9fcf9&clientId=u0e3c3fb9-d500-4&from=paste&height=297&id=u0be635c3&originHeight=371&originWidth=737&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=203994&status=done&style=none&taskId=u6dca7dec-de13-49a9-b27e-681e9bbb5b2&title=&width=589.6)
```cpp
void Print1(int val)
{
    cout << val;
}

class Print2
{
public:
	void operator()(int val)
	{
        cout << val;
    }
}

for_each(v.begin(), v.end(), Print1);
for_each(v.begin(), v.end(), Print2());
```
<a name="hH6ae"></a>
### transform 搬运
搬运<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698239116321-cc5ef8da-5e9f-4b54-ba19-782669da67a2.png#averageHue=%23fafdfa&clientId=u0e3c3fb9-d500-4&from=paste&height=356&id=u75100032&originHeight=445&originWidth=1022&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=270829&status=done&style=none&taskId=ud575eaa6-381a-4ab1-947c-e574691a58d&title=&width=817.6)
```cpp
class Transform
{
public:
	int operator()(int val)
	{
        return val;	//除了返回值外，还可以附加其他操作
	}
}

vecter<int> v;
v.push_bacK(1);
v.push_back(2);

vector<int> v2;
v2.resize(v.size());	//提前开辟空间
transform(v.begin(), v.end(), v2.begin(), Transform());
```
<a name="RVgNw"></a>
## 查找算法
<a name="SqsHy"></a>
### find 按值查找
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698239540236-f4b48a0a-90f7-407d-bbbc-1036fe6941bd.png#averageHue=%23fafdfa&clientId=u0e3c3fb9-d500-4&from=paste&height=490&id=uc1299f2e&originHeight=613&originWidth=1053&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=395659&status=done&style=none&taskId=ua479bd5a-0e9c-4159-8f61-f2b28c8a891&title=&width=842.4)
<a name="BrL3B"></a>
### find_if 按条件查找
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698239577052-ea52379a-592b-4de8-ae3e-718c7ccf467d.png#averageHue=%23f9f9f8&clientId=u0e3c3fb9-d500-4&from=paste&height=421&id=ud17eff17&originHeight=526&originWidth=1055&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=313131&status=done&style=none&taskId=u72806f01-d9b8-49ae-b03a-257f5e4fa7c&title=&width=844)
```cpp
class Compare
{
public: 
	bool operator()(int val)
	{
        return val > 5;
	}
}
find_if(v.begin(), v.end(), Compare())
```
<a name="hDu4m"></a>
### adjacent_find 查找相邻重复
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698239748755-e7d7e963-251f-4b27-aa28-9e184cdc7474.png#averageHue=%23f9fcf9&clientId=u0e3c3fb9-d500-4&from=paste&height=422&id=udd6ca3f1&originHeight=528&originWidth=843&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=269347&status=done&style=none&taskId=ua0e2e0b0-0c18-43d3-a7c1-a9dfe6c9efc&title=&width=674.4)
<a name="chnaY"></a>
### binary_search 查找某值是否存在，返回布尔值
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698239926843-a00ec648-611a-4278-bf6c-6ec549e13c06.png#averageHue=%23fafdfa&clientId=u0e3c3fb9-d500-4&from=paste&height=524&id=u0ad0f0bb&originHeight=655&originWidth=909&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=344670&status=done&style=none&taskId=u7bb251d7-8209-4c44-85cb-ddbf681b232&title=&width=727.2)
<a name="vOCLp"></a>
### count 统计个数
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698239962013-f339adec-a940-4e94-8438-1ddcb18e0cf3.png#averageHue=%23f9fcf9&clientId=u0e3c3fb9-d500-4&from=paste&height=477&id=u850f76fa&originHeight=596&originWidth=708&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=241298&status=done&style=none&taskId=u11380068-cf54-4b13-b0f7-c9df860a5de&title=&width=566.4)
```cpp
class Person
{
public:
	Person(string _name)
	{
        name = _name;
    }

	bool operator==(const Person& p)
	{
        if(name == p.name)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
	string name;
}

vector<Person> v;
Person p1("阿尔托莉雅");
Person p2("冠位人偶师");
Person p3("草食狼");

v.push_back(p1);
v.push_back(p2);
v.push_back(p3);

Person p4("咕哒");

int num = count(v.begin(), v.end(), p4);
cout << "num = " << num;
```
<a name="cViFu"></a>
### count_if 按条件统计个数
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698240445077-8032df8b-ec46-47fe-8e23-2467930d42f2.png#averageHue=%23f9fcf9&clientId=u0e3c3fb9-d500-4&from=paste&height=430&id=u99acffcf&originHeight=537&originWidth=762&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=242025&status=done&style=none&taskId=u649fe6b6-dbb0-4cd1-9cf5-2b35bbf6eef&title=&width=609.6)
<a name="b2omj"></a>
## 排序算法
<a name="PVYCM"></a>
### sort 排序，默认小到大
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698325467970-5889097a-6bb1-4569-80a5-b7db4205225e.png#averageHue=%23f9fcf9&clientId=u8e36b7a9-a1d6-4&from=paste&height=294&id=u6bac8768&originHeight=368&originWidth=1044&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=244797&status=done&style=none&taskId=uce3b50c0-d832-4c4e-b4fe-864dfaf109d&title=&width=835.2)
<a name="KgloJ"></a>
### random_shuffle 随机打乱顺序
记得加随机数种子<br />srand((unsigned int)time(NULL));<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698325518064-8b61ecb4-9b76-42cb-946b-f66ad6f605cc.png#averageHue=%23f9fcf9&clientId=u8e36b7a9-a1d6-4&from=paste&height=270&id=uf3aea4cc&originHeight=337&originWidth=725&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=180550&status=done&style=none&taskId=u84b95f1b-9e83-4679-973d-9cafa5c48a0&title=&width=580)
<a name="UNf8W"></a>
### merge 合并容器
合并完的容器仍然是有序的<br />目标容器需要提前开辟空间，大小为两个源容器大小的和<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698325636629-9a29c710-78ea-434f-a709-aa1b632c9024.png#averageHue=%23fafdfa&clientId=u8e36b7a9-a1d6-4&from=paste&height=405&id=u11503d43&originHeight=506&originWidth=1297&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=409702&status=done&style=none&taskId=uc5d63fb6-490b-4e41-87ec-6667ec45aea&title=&width=1037.6)
<a name="l9S7z"></a>
### reverse 反转
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698325805849-322739eb-42e2-4dac-b7a6-ebf69a08611a.png#averageHue=%23f9fcf9&clientId=u8e36b7a9-a1d6-4&from=paste&height=272&id=u88b69977&originHeight=340&originWidth=671&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=156819&status=done&style=none&taskId=u58d02b23-3341-42e4-80c9-dc4d74260f3&title=&width=536.8)
<a name="GvUQf"></a>
## 拷贝和替换算法
<a name="gDq9p"></a>
### copy 范围内元素拷贝
目标容器需要提前设置大小<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698325955528-2dc80e73-b9f8-431c-8ddb-419c03322fb7.png#averageHue=%23f9fcf9&clientId=u8e36b7a9-a1d6-4&from=paste&height=321&id=u42055af7&originHeight=401&originWidth=1042&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=287440&status=done&style=none&taskId=ua935a924-cb4b-4b19-af96-48aabfaf9b5&title=&width=833.6)
<a name="fNReW"></a>
### replace 范围内某元素替换
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698326009339-bc280b79-2f11-40fd-b434-2edcbfc52ccf.png#averageHue=%23fafafa&clientId=u8e36b7a9-a1d6-4&from=paste&height=358&id=u9a804b34&originHeight=447&originWidth=925&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=226247&status=done&style=none&taskId=u9387e5ff-37dd-45d7-9fbc-0462fed1318&title=&width=740)
<a name="d53uF"></a>
### replace_if 满足条件的范围内元素替换
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698326107536-19e1952e-7ad6-4105-95cd-c3d465a886db.png#averageHue=%23f9fcf9&clientId=u8e36b7a9-a1d6-4&from=paste&height=356&id=ue699648b&originHeight=445&originWidth=928&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=284215&status=done&style=none&taskId=u933d13a9-0900-42b3-bb63-67c4631fe1e&title=&width=742.4)
<a name="aagrm"></a>
### swap 交换
交换的容器要同种类型<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698326176886-dd52c8e9-5153-4a31-bc2a-337241e05ad9.png#averageHue=%23f9fcf9&clientId=u8e36b7a9-a1d6-4&from=paste&height=259&id=ua8c82072&originHeight=324&originWidth=647&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=123551&status=done&style=none&taskId=ue5e68c9c-0a54-4da6-91a9-10fbfbf93d7&title=&width=517.6)
<a name="b7fCE"></a>
## 算数生成算法
**使用头文件#include <numeric>**
<a name="HykSr"></a>
### accumulate 计算容器元素累计总和
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698326393774-1e25b36c-4ca8-41c4-9244-f58141c5bd19.png#averageHue=%23f9fcf9&clientId=u8e36b7a9-a1d6-4&from=paste&height=306&id=u0c485b21&originHeight=383&originWidth=785&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=201362&status=done&style=none&taskId=u432991e7-baf2-4dbf-9cee-b1fd82a2144&title=&width=628)
<a name="YUY6y"></a>
### fill 向容器中添加元素
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698326623058-1a807fcb-62dc-4423-872f-96ae4ffc7935.png#averageHue=%23f9fcf9&clientId=u8e36b7a9-a1d6-4&from=paste&height=327&id=ufe16b16a&originHeight=409&originWidth=714&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=185014&status=done&style=none&taskId=u332bee74-f449-4b41-a76e-097ce8e4173&title=&width=571.2)<br />一般用来进行后期填充
<a name="XCm7M"></a>
## 集合算法
<a name="jUqtz"></a>
### set_intersection 求交集
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698394856734-aa55c90a-55fa-4798-8e29-a569d41124d8.png#averageHue=%23fafdfa&clientId=u19205b11-a732-4&from=paste&height=401&id=u0a79b7a5&originHeight=501&originWidth=1471&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=425027&status=done&style=none&taskId=u83cdd933-35ac-40ba-b17f-b69e9b6043b&title=&width=1176.8)
```cpp
vTarget.resize(min(v1, v2));
vector<int>::inerator itEnd = set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin())
for_each(vTarget.begin(), itEnd, myPrint());
```
重设大小为两个容器中较小的那个即可
<a name="ZO35S"></a>
### set_union 求并集
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698395150912-ab50938d-296b-4a93-bb77-a1811c4718bb.png#averageHue=%23fafdfa&clientId=u19205b11-a732-4&from=paste&height=399&id=u70a4ad0f&originHeight=499&originWidth=1379&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=392335&status=done&style=none&taskId=u36c835f7-ada1-4356-8621-9aff736af28&title=&width=1103.2)
```cpp
vT.resize(v1.size() + v2.size());

vector<int>::iterator itEnd = set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());

for_each(vT.begin(), itEnd, myPrint);
```
<a name="TmDXg"></a>
### set_difference 求差集
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1698395832217-2ebb5e83-60cd-42a4-8546-0e53080b8b7a.png#averageHue=%23f9fcf9&clientId=u19205b11-a732-4&from=paste&height=402&id=u04a80115&originHeight=502&originWidth=1438&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=417349&status=done&style=none&taskId=ued9d9c0d-4cc9-4b87-bc87-464f5d01f70&title=&width=1150.4)
```cpp
//v1 和 v2 的差集
vT.resize(max(v1.size(), v2.size()));
vector<int>::iterator it = set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());

//v2 和 v1 的差集
vector<int>::iterator it2 = set_difference(v2.begin(), v2.end(), v1.begin(), v1.end(), vTarget.begin()); 
```
