# 高阶面向对象解析
## 抽象类与开闭原则
我们应当封装不变的，稳定的，固定的，确定的成员。不确定的，不固定的成员写成abstract类（纯虚类），并留给子类去实现。
纯抽象类在C#中一般作为接口来使用。接口类是隐式的，不用写明abstract public,同样继承的子类重写也不需要写override
```csharp
interface IVehicle
{
    void Run();
    void Stop();
    void Fill();
}
abstract class Vehicle : IVehicle
{
      public void Stop() {}
      public void Run() {}
      abstract  public void Fill(); //未重写成员，继续下推给子类实现
}
class Car : Vehicle 
{
	public override void Run()
    {
        //TODO
    }

}
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683197837770-0e08e560-bb9b-4820-9d61-8e3c74ea3989.png#averageHue=%23ececea&clientId=u1d1c9e7c-2836-4&from=paste&height=145&id=u356a688d&originHeight=702&originWidth=1660&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=1205205&status=done&style=none&taskId=uea39679c-f998-46ba-9981-cada85b2054&title=&width=342.3999938964844)
## 接口，依赖反转，单元测试
接口由抽象类过渡而来，接口相当于一种contract，是双方之间的一种契约。 接口为了松耦合而生！
依赖反转在一定程度上降低了耦合度，也是金字塔式关系的平衡。
单元测试算是依赖反转的进一步实现
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683288759797-7e9f0af0-7a3b-48d4-8225-efbcde59fd29.png#averageHue=%23ece8e5&clientId=ua9e35c0d-139b-4&from=paste&height=181&id=ubb9fc242&originHeight=865&originWidth=1462&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=576560&status=done&style=none&taskId=ue96f30e6-4598-4740-bdb8-9ba7d9d9592&title=&width=305.3999938964844)
### 依赖反转
![紧耦合](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683290080009-75e4fc6f-691a-4d01-9966-460510fc04c8.png#averageHue=%23f3f7f2&clientId=ua9e35c0d-139b-4&from=paste&height=211&id=u6b7ababc&originHeight=386&originWidth=570&originalType=binary&ratio=1.25&rotation=0&showTitle=true&size=114430&status=done&style=none&taskId=u7f385272-3983-4afd-8158-0c3915bd62c&title=%E7%B4%A7%E8%80%A6%E5%90%88&width=312 "紧耦合")![依赖反转](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683290120061-d8f203ed-27e7-4e3e-8500-74109ca53189.png#averageHue=%23fbfbf7&clientId=ua9e35c0d-139b-4&from=paste&height=225&id=u856f0347&originHeight=373&originWidth=626&originalType=binary&ratio=1.25&rotation=0&showTitle=true&size=87915&status=done&style=none&taskId=u5b30e8bd-28da-4be2-b34f-ecb719c2f0f&title=%E4%BE%9D%E8%B5%96%E5%8F%8D%E8%BD%AC&width=376.8000183105469 "依赖反转")
```csharp
using static System.Console;
namespace 面向对象
{
    public class User
    {
        IPhoneBase _phone;

        public User(IPhoneBase phone)
        {
            _phone = phone;
        }

        public virtual string ShowValue()
        {
            return "0";
        }
    }

    public class Teacher : User
    {
        IPhoneBase _phone;
        public Teacher(IPhoneBase phone) : base(phone)
        {
            _phone = phone;
        }

        public override string ShowValue()
        {
            int v = _phone.Value();
            if(v < 300)
            {
                return "便宜";
            }
            else
            {
                return "贵";
            }
        }

    }

    public interface IPhoneBase
    {
        int Value();
    }

    public class Nokia : IPhoneBase
    {
        public int Value()
        {
            return 110;
        }
    }
    public class Program
    {       
        static void Main(string[] args)
        {
            User user = new Teacher(new Nokia());
            WriteLine(user.ShowValue());
        }
    }
}
```
### 单元测试

1. 右键解决方案，添加新项目

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683290651850-8fbb4112-b7ce-485b-9c7f-4b5523948d24.png#averageHue=%232b2a2a&clientId=ua9e35c0d-139b-4&from=paste&height=223&id=u6040770e&originHeight=794&originWidth=1113&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=84753&status=done&style=none&taskId=u68b83537-862e-4676-834a-703408610d5&title=&width=312.4000244140625)

2. 右键依赖项添加项目引用

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683290731605-05764869-6e09-445c-8160-74fc19a7653d.png#averageHue=%232e2c2b&clientId=ua9e35c0d-139b-4&from=paste&height=87&id=ufa109f26&originHeight=109&originWidth=332&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=8052&status=done&style=none&taskId=udb5ea873-3710-48c8-ae77-5f93775b708&title=&width=265.6)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683290742989-926a8ee0-f2b0-4665-9041-03a8eb9ac74f.png#averageHue=%23353535&clientId=ua9e35c0d-139b-4&from=paste&height=196&id=ud33f9a98&originHeight=680&originWidth=978&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29118&status=done&style=none&taskId=u6871800f-abf1-4391-9d1f-ae983728dbb&title=&width=281.4000244140625)

3. 利用mock插件，省区创建子类的过程
   1. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683290838824-a51c3a0b-5aac-4515-a6cf-d8d053a801f1.png#averageHue=%23302620&clientId=ua9e35c0d-139b-4&from=paste&height=86&id=uabea281f&originHeight=108&originWidth=733&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=90437&status=done&style=none&taskId=u3b6bcf63-4baa-47e4-a996-afefae462e5&title=&width=586.4)
   2. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683290850249-07b40022-aa77-41e0-9864-cdddb66f95af.png#averageHue=%232f2620&clientId=ua9e35c0d-139b-4&from=paste&height=125&id=ub18ec428&originHeight=232&originWidth=376&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=87869&status=done&style=none&taskId=u31e75a81-a3a8-473f-9f1e-21c538115d5&title=&width=202.80001831054688)
4. 将单元测试与调试结合一起使用将大大增加检查效率。实际开发过程中小组合作代码提交，也会使用类似单元测试工具检查提交是否正确。

      ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683291073033-540bde3f-9205-4eb5-a7ef-ae7bf3f06cc9.png#averageHue=%232c2b2a&clientId=ua9e35c0d-139b-4&from=paste&height=137&id=u55392f56&originHeight=269&originWidth=562&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=33537&status=done&style=none&taskId=u7b7cc4bb-3626-495c-8deb-1f4adc7c1ef&title=&width=286.6000061035156)
```csharp
using System;
using Xunit;
using Moq;
namespace 面向对象.Tests
{
    public class UserTests
    {
        [Fact]
        public void PhoneTest_OK()
        {
            var mock = new Mock<IPhoneBase>();
            mock.Setup(ps => ps.Value()).Returns(() => 200);
            User user = new User(mock.Object);
            string str = "贵";
            var actual = user.ShowValue();
            Assert.Equal(str, actual);
        }
    }

    //class Banana : IPhoneBase
    //{
    //    public int Value()
    //    {
    //        return 500;
    //    }
    //}

}
```
## 接口隔离，反射，特性，依赖注入
### 接口隔离原则
#### 1.一个接口应该拥有尽可能少的行为，是精简的，单一的。
服务调用方不能多要，例子中即使传入的是Tank1对象，也只能调用Run（）方法
```csharp
public class Program
{       
    static void Main(string[] args)
    {
    	Driver d = new Driver(new Tank1());
        d.Run();
    }
}
class Driver
{
    IMove _m;
    public Driver(IMove m)
    {
        _m = m;
    }
    public void Action()
    {
        _m.Run();
    }
}
interface IMove
{
    void Run();
}
interface IAttack
{
    void Fire();
}
interface ITank : IMove, IAttack {}
class Tank1 : ITank
{
	void Run()
    {
        //TODO
	}
    void Fire()
    {
        //TODO
	}
}
class Car : IMove
{
	void Run()
 	{
        //TODO
  	}
}
```
#### 2.调用者不能多要
sum函数参数为IEnumberable, 而不是ICollection。因为此处只需要形式参数可迭代
```csharp
using System.Collections;
using static System.Console;
namespace 接口隔离原则
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int[] nums1 = { 1, 2, 3 };
            ArrayList nums2 = new ArrayList() {1, 2, 3, 4, 5 };
            var nums3 = new ReadOnlyCollection(nums1);
            WriteLine(Sum(nums3));
        }

        static int Sum(IEnumerable nums) 
        {
            int res = 0;
            foreach(var n in nums)
            {
                res += (int)n;
            }
            return res;
        }
    }

    class ReadOnlyCollection : IEnumerable
    {
        private int[] _array;
        public ReadOnlyCollection(int[] array) 
        {
            _array = array;
        }

        public IEnumerator GetEnumerator()
        {
            return new Enumerator(this);
        }

        public class Enumerator : IEnumerator
        {
            private ReadOnlyCollection? _collection;
            private int _index;

            public Enumerator(ReadOnlyCollection? collection)
            {
                _collection = collection;
                _index = -1;
            }

            public object Current
            {
                get 
                {
                    object o = _collection._array[_index];
                    return o;
                }
            }

            public bool MoveNext()
            {
                if(++ _index < _collection._array.Length) 
                {
                    return true;
                }
                else
                {
                    return false;   
                }
            }

            public void Reset()
            {
                _index = -1;
            }
        }

    }

}
```
#### 3.显式隔离实现
C#特有的功能，一个类继承自多个接口，可以隐藏某些接口中的方法
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683546572466-f8e06195-b2ac-46b0-8f5e-11fe111f5969.png#averageHue=%23393029&clientId=u11f44d19-3e7f-4&from=paste&height=145&id=u61dc9080&originHeight=259&originWidth=705&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=103723&status=done&style=none&taskId=u64a790b8-b1f7-4901-9879-94a3fed8603&title=&width=395)
```csharp
int Main()
{
    var worker = new Worker();
    worker.DoThing();
    Killer kill = worker as Killer;
    kill.Kill();
}

interface People
{
    void DoThing();
}
interface Killer
{
    void Kill();
}
class Worker : People, Killer
{
    public void DoThing()
    {
        throw new NotImplementedException();
    }

    void Killer.Kill()
    {
        throw new NotImplementedException();
    }
}
```
## 反射
反射是.net框架的功能，任何使用.net框架的语言都可以使用反射
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683548599777-f08ad5b2-9a16-443e-8087-4f4dcb4e4ecb.png#averageHue=%23ebebe3&clientId=u11f44d19-3e7f-4&from=paste&height=156&id=u199deedf&originHeight=356&originWidth=1056&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=318273&status=done&style=none&taskId=u06eb4b31-075a-4f1d-9020-88efde41c7a&title=&width=461.4000244140625)
## 依赖注入
## SDK开发案例

---

# 预处理器指令
## 什么是编译器
## ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683598910007-c5cdf105-20db-4abe-a811-d724ca0b57d3.png#averageHue=%233b5535&clientId=u9d193690-2f92-4&from=paste&height=102&id=u35902605&originHeight=195&originWidth=1088&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=213265&status=done&style=none&taskId=uad731e59-40f3-45b8-9a69-e73a6bf3602&title=&width=568.4000244140625)
## 什么是预处理指令
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683599011592-08cb0159-df7b-4e5f-84fe-8b69f78b8152.png#averageHue=%23364b33&clientId=u9d193690-2f92-4&from=paste&height=144&id=ub4548415&originHeight=240&originWidth=924&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=271542&status=done&style=none&taskId=uea06fc30-0707-4612-be22-dd6c1f8eb06&title=&width=556.2000122070312)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683599757327-3a055e30-53bf-422a-8a8b-19e048dcef4c.png#averageHue=%23365133&clientId=u9d193690-2f92-4&from=paste&height=117&id=u83f35786&originHeight=163&originWidth=777&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=181498&status=done&style=none&taskId=uc673c459-b4f2-4756-8d19-5c53983c896&title=&width=555.6000366210938)
## 常见的预处理指令
```
1.
#define 定义一个符号，类似于没有值的变量
#undef 取消定义的符号，让其失效
两者都是写在脚本文件的最开始 一般配和if指令 或者配合特性
2.
#if
#elif
#else
#endif
和if语句的规则一致，一般配合#define定义的符号使用
用于告诉编译器编译代码的流程控制
#if 和 #endif 成对出现，因为#endif能关闭前面的条件编译
3.
#warning 警告
#error 错误 如果此条指令可以执行，则程序会直接报错并且不能运行
一般配合if指令使用
4.
#region
#endregion 
折叠范围内的代码块

```
```csharp
#define Unity5
#define Unity2017
#define Unity2020
#define IOS
#define Androad
#undef IOS
...
#if IOS
	error 不支持此系统
static int Cal(int x, int y)
{
#if Unity5
    return x + y;
#elif Unity2017
    return X * y;
#elif Unity2020
    return x - y;
#else
    warning 不支持当前版本
    return 0;
#endif
}

```
# 简单数据结构类
## ArrayLIst
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707554902794-955d00d3-7133-4b33-9ef5-d57346094d33.png#averageHue=%23334930&clientId=u12e6cca2-e0ad-4&from=paste&height=130&id=ub3e4af0f&originHeight=203&originWidth=573&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=251238&status=done&style=none&taskId=ua44a76bc-7e08-4b61-9750-a43a81db71c&title=&width=366.72)
using System.Collections;
```csharp
ArrayList arr = new ArrayList();
//增
arr.Add(1);
arr.Add(2);
arr.Insert(2, 4);
//删
arr.Remove(1);	//按元素删
//arr.RemoveAt(xxx);	//按位置删
//查
if (arr.Contains(2))
{
    WriteLine("查找到2");
}
WriteLine(arr.Capacity);
WriteLine(arr.Count);
int index = arr.IndexOf(1); //从前往后查，查找到返回下标，查不到返回-1
//改
arr[0] = 3;
WriteLine(arr[0]);
int lastIndex = arr.LastIndexOf(3); //从后往前查
WriteLine(lastIndex);
//装箱拆箱
int v = 5;
arr[1] = v; 
v = (int)arr[1];
//遍历
for(int i = 0; i < arr.Count; ++i)
{
    WriteLine(arr[i]);
}
foreach (object item in arr)
{
    WriteLine(item);
}
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707555926237-2a086a23-d82b-4db6-ac7d-092d0a70b624.png#averageHue=%23374e32&clientId=u12e6cca2-e0ad-4&from=paste&height=129&id=u9e564d3a&originHeight=202&originWidth=1219&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=432066&status=done&style=none&taskId=ua03ced3b-7cca-455a-a13e-c225a616fb9&title=&width=780.16)
## Stack
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707557066238-506dbde8-cb3f-4fda-916d-21c2d7d31d1e.png#averageHue=%233e5538&clientId=u12e6cca2-e0ad-4&from=paste&height=164&id=ue1fc4488&originHeight=256&originWidth=814&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=317547&status=done&style=none&taskId=u0aab8cfa-f3ad-499d-8846-6f79f35abee&title=&width=520.96)
using System.Collections;
```csharp
Stack stack = new Stack();
//增
stack.Push(1);
//取
WriteLine(stack.Pop());
//查
stack.Push(2);
WriteLine(stack.Peek());    //查看栈顶元素
if (stack.Contains(2))
{
    WriteLine("存在2");
}
//改
//无法直接改，先全部清空，再重新写
stack.Clear();
stack.Push(3);
stack.Push(4);
stack.Push(5);
//遍历，无法直接for循环
WriteLine("______________");
foreach (object item in stack)
{
    WriteLine(item);
}
//先转成普通列表再for循环
object[] arr = stack.ToArray();
for(int i = 0; i < arr.Length; ++i)
{
    WriteLine(arr[i]);
}
//循环弹栈
while(stack.Count > 0)
{
    WriteLine(stack.Pop());
}
WriteLine(stack.Count);
```
## ![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707557639742-e5115110-14bc-4fe4-9bf0-0713d85aa9f7.png#averageHue=%23364d33&clientId=u12e6cca2-e0ad-4&from=paste&height=108&id=uf5fc7356&originHeight=169&originWidth=682&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=238435&status=done&style=none&taskId=u3da5a441-5233-4199-83b3-147b3cb1615&title=&width=436.48)
## Queue
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707558254687-765db4e0-9805-4082-8f12-915f244f8ba8.png#averageHue=%23405338&clientId=u12e6cca2-e0ad-4&from=paste&height=180&id=u123d2a67&originHeight=282&originWidth=806&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=308844&status=done&style=none&taskId=ue0743719-9ecc-4e0a-9b4b-be58cfadff8&title=&width=515.84)
using System.Collections;
```csharp
Queue queue = new Queue();
//增
queue.Enqueue(1);
//取
WriteLine(queue.Dequeue());
//改
queue.Clear();
queue.Enqueue(2);
queue.Enqueue(3);
queue.Enqueue(4);
//查
if (queue.Contains(3))
{
    WriteLine("存在3");
}
WriteLine("队头元素" + queue.Peek());
//遍历
foreach (object item in queue)
{
    WriteLine(item);
}

object[] arr = queue.ToArray();
for(int i = 0; i < arr.Length; ++i)
{
    WriteLine(arr[i]);
}

while(queue.Count > 0)
{
    WriteLine(queue.Dequeue());
}

WriteLine(queue.Count);
```
## Hashtable
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707559139381-9d96b7af-c33f-4dd4-9b48-96d15256e11a.png#averageHue=%233e5538&clientId=u12e6cca2-e0ad-4&from=paste&height=109&id=u1edf9400&originHeight=171&originWidth=975&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=252359&status=done&style=none&taskId=u7e94bdff-8317-4c12-9e9d-6a8a35a997a&title=&width=624)
using System.Collections;
```csharp
Hashtable hash = new Hashtable();
//增
hash.Add("123", 123);
hash.Add(12, "12");
hash.Add(1, 2);
//删
hash.Remove(1); //只能按键删
//改
hash[12] = "1234";  //只能按键改
//查            
if (hash.Contains(12)) { }
if (hash.ContainsKey(12)) { }
if (hash.ContainsValue(123)) 
{
    WriteLine("存在值123");
}        
//遍历
foreach(DictionaryEntry entry in hash) 
{
    WriteLine(entry.Key + " " + entry.Value);
}
//按键遍历
foreach(object key in hash.Keys)
{
    WriteLine(key + " " + hash[key]);
}
//按值遍历
foreach(object value in hash.Values) 
{
    WriteLine(value);
}
//迭代器遍历
IDictionaryEnumerator ide = hash.GetEnumerator();
bool flag = ide.MoveNext();
while(flag)
{
    WriteLine(ide.Key + " " + ide.Value);
    flag = ide.MoveNext();
}
```
# 泛型
## 泛型概述
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707300016826-8ae2fb37-0527-4b99-9724-c1bcf275f516.png#averageHue=%23364e33&clientId=u8d2e1a43-415c-4&from=paste&height=154&id=ub5f5780e&originHeight=241&originWidth=709&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=300255&status=done&style=none&taskId=ub2b7abf0-62c8-4453-8065-dda70355d8c&title=&width=453.76)
## 泛型分类
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707300081047-945d6a44-dbc9-4221-8f7a-efb6f271bb7e.png#averageHue=%23405539&clientId=u8d2e1a43-415c-4&from=paste&height=225&id=u4aef3772&originHeight=352&originWidth=685&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=320238&status=done&style=none&taskId=u3dceb46b-c747-4f0c-a167-c8caa2778be&title=&width=438.4)
```csharp
class TestClass<T>
{
    public T value;
}

class TestClass2<T1, T2, T3>
{
    //TODO:
}

class TestClass3
{
    public void TestFun<T>()
    {
        T t = default(T);
    }

    public T TestFun2<T>(string s)
    {
        return default(T);
    }
}

class TestClass3<T>
{
    public T value;
    //不是泛型方法，T是泛型类申明的时候就 已经指定了 在使用这个函数的时候不能再进行动态地变化
    public void TestFun(T t) {}
}

class Program
{
    static void Main(string[] args)
    {
        TestClass<int> t = new TestClass<int>();
        TestClass3 t2 = new TestClass3();
        t2.TestFun<int>();
    }
}
```
## 泛型作用

1. 不同类型对象，使用相同的逻辑处理就可以选择泛型
2. 使用泛型可以一定程度避免装箱拆箱

总结

1. 申明泛型时 它只是一个类型的占位符
2. 泛型真正起作用的时候 是在使用它的时候
3. 泛型占位字母可以有n个 用逗号隔开
4. 不确定泛型类型时 获取默认值 用default(占位字母)
5. 泛型占位字母 一般是大写字母
6. 看到<>包裹的字母 肯定是泛型
## 泛型约束
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707301426038-e5216d07-4947-416e-be78-163bc54189d2.png#averageHue=%233a5134&clientId=u8d2e1a43-415c-4&from=paste&height=228&id=uda49408b&originHeight=356&originWidth=1077&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=534598&status=done&style=none&taskId=u86fd09a8-fca5-41f6-a5b9-31ab334fc0d&title=&width=689.28)
```csharp
class Test1<T> where T : struct
{
    //T只能值类型
}
class Test2<T> where T : class
{
    //T只能引用类型
}
class Test3<T> where T : new()
{
    //传进去的类型必须有公共无参构造函数
}
class Test {}
class Test4<T> where T : Test 
{
}
interface IFly {}
class Test5<T> where T : IFly
{
}
class Test6<T, U> where T : U
{
}
class Test7<T> where T : class, new() {}
class Test8<T, U> where T : class where U : class, new() {}
```
# 常见泛型数据结构类
## List
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707565586411-c0ec60e4-5edd-4688-aa1e-6012068eba5b.png#averageHue=%23334930&clientId=u12e6cca2-e0ad-4&from=paste&height=130&id=u55528f93&originHeight=203&originWidth=542&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=228513&status=done&style=none&taskId=u3ed15742-b586-47f8-9849-e2a46394f3a&title=&width=346.88)
using System.Collections.Generic
```csharp
List<int> intList = new List<int>();
List<string> strList = new List<string>();
//增
intList.Add(1);
intList.Add(1);
strList.Add("123");
List<string> str = new List<string> {"666"};            
strList.AddRange(str);  //范围添加
//删
intList.Remove(1);
intList.RemoveAt(0);
//改
intList.Add(1);
intList[0] = 2;
//查
if (intList.Contains(2))
{
    WriteLine("intList中存在2");
}
int index = intList.IndexOf(2);
int lastIndex = intList.LastIndexOf(2);
//遍历
for(int i = 0; i < intList.Count; ++i)
{
    WriteLine(intList[i]);
}
```
## Dictionary
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707567553299-93a8577d-2ff5-4103-b547-bb45b48ed399.png#averageHue=%23364c32&clientId=u12e6cca2-e0ad-4&from=paste&height=106&id=u4e793bf3&originHeight=165&originWidth=858&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=264832&status=done&style=none&taskId=u84296925-9faf-4b35-a233-65dcc4fedee&title=&width=549.12)
using System.Collections.Generic
```csharp
Dictionary<int, int> dic = new Dictionary<int, int>();
//增
//键不能重复
dic.Add(1, 1);
dic.Add(2, 2);
dic.Add(3, 3);
//删
dic.Remove(1);
//查
if (dic.ContainsKey(1)) { }
if (dic.ContainsValue(1)) { }            
WriteLine(dic[2]);  //访问不存在的下标会直接报错，而ArrayList返回空实现
//改
dic[1] = 4;
//遍历
foreach (KeyValuePair<int, int> item in dic)
{
    WriteLine(item.Key + " " + item.Value);
}

foreach (int key in dic.Keys)
{
    WriteLine(dic[key]);
}

foreach(int value in dic.Values)
{
    WriteLine(value);
}
```
## 顺序存储和链式存储
## ![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707568700799-5b9287b4-bf75-4199-b3d8-d4a7844ee4d4.png#averageHue=%233b5236&clientId=u12e6cca2-e0ad-4&from=paste&height=275&id=u17ad8498&originHeight=429&originWidth=1023&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=642341&status=done&style=none&taskId=u971e8ec5-ac60-4c1f-8599-5c14ed577b1&title=&width=654.72)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707568772625-ddb95576-495f-4fee-bc0f-656405eb90fc.png#averageHue=%2344593e&clientId=u12e6cca2-e0ad-4&from=paste&height=338&id=u539c5b7d&originHeight=528&originWidth=951&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=636902&status=done&style=none&taskId=uf6592b89-8db8-45c7-90ae-1f302fca7cb&title=&width=608.64)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707568840085-81e82650-9fb6-4efe-97bd-d90ff322bf5e.png#averageHue=%23394f35&clientId=u12e6cca2-e0ad-4&from=paste&height=113&id=uf10a198a&originHeight=177&originWidth=739&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=219677&status=done&style=none&taskId=u1916ef2f-3511-4348-a245-c2754d5be3d&title=&width=472.96)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707569417732-834fe89c-8463-4535-ab46-acc558ccd23d.png#averageHue=%23374d34&clientId=u12e6cca2-e0ad-4&from=paste&height=156&id=ufff3cf0b&originHeight=244&originWidth=1237&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=567404&status=done&style=none&taskId=uabc75afe-69a3-4603-8644-2f4b63a54fe&title=&width=791.68)
using System.Collections.Generic
```csharp
public class LinkNode<T>
    {
    public T value;
    public LinkNode<T>? next;
    public LinkNode(T _value) { value = _value; }
}

public class LinkList<T>
    {        
    public LinkNode<T>? head;  //头结点
    public LinkNode<T>? last;  //尾结点
    public void Add(T _value)
    {
        LinkNode<T> tmp = new LinkNode<T>(_value);
        if (head == null)
        {
            head = tmp;
            last = tmp;
        }
        else
        {
            last.next = tmp;
            last = tmp;
        }
    }

    public void Remove(int _value)
    {
        if (head == null)
        {
            return;
        }
        if (head.value.Equals(_value))
        {
            if (head.next == null)
            {
                last = last.next;
            }
            head = head.next;
        }
        LinkNode<T> tmp = head;
        while (tmp.next != null)
        {
            if (tmp.next.value.Equals(_value))
            {
                tmp.next = tmp.next.next;
                break;
            }
            tmp = tmp.next;
        }
    }
}


internal class Program
{
    //#nullable enable
    static void Main(string[] args)
    {
        LinkList<int> link = new LinkList<int>();
        link.Add(1);
        link.Add(2);
        link.Add(3);
        LinkNode<int> node = link.head;
        while(node != null)
        {
            WriteLine(node.value);
            node = node.next;
        }
        link.Remove(1);
        link.Remove(2);
        node = link.head;
        while (node != null)
        {
            WriteLine(node.value);
            node = node.next;
        }
    }
}
```
## Linkedlist
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707571228382-88f98ce0-6430-401d-a730-7027828e51a8.png#averageHue=%23374c34&clientId=u12e6cca2-e0ad-4&from=paste&height=84&id=uf2466e54&originHeight=131&originWidth=597&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=156825&status=done&style=none&taskId=u0079b851-34d5-4083-bfbb-52213e3ff30&title=&width=382.08)
```csharp
LinkedList<int> list = new LinkedList<int>();
//增            
list.AddFirst(1);   //在头结点插入
list.AddLast(2);    //在尾结点插入
list.AddLast(3);
list.AddFirst(4);
list.AddAfter(list.First, 5);   //在某个结点后插入
list.AddBefore(list.First, 0); //在某个结点前插入            
//删
list.Remove(3);
list.RemoveFirst(); //删除头结点
list.RemoveLast();  //删除尾结点
//改
//先获得结点，再修改值
list.First.Value = 10;
//查
WriteLine(list.First.Value);    //头结点值
WriteLine(list.Last.Value); //尾结点值;
LinkedListNode<int> node = list.Find(1);    //找不到会直接返回空
if(list.Contains(1))
{
    WriteLine(node.Value);
}
//遍历
WriteLine("**********************");
list.AddLast(1);
LinkedListNode<int> tmp = list.FindLast(1); //从尾开始找
list.AddBefore(tmp, 9);

foreach (int item in list)
{
    WriteLine(item);
}
//从头到尾遍历
LinkedListNode<int> head = list.First;
while(head != null)
{
    WriteLine(head.Value);
    head = head.Next;   //指向下一个元素
}
//从尾到头遍历
LinkedListNode<int> last = list.Last;
while(last != null) 
{
    WriteLine(last.Value);
    last = last.Previous;   //指向前一个元素
}
```
## 泛型栈和队列
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707573072762-71adfcb3-2046-47e9-a2e6-239a331f5a26.png#averageHue=%233f5338&clientId=u12e6cca2-e0ad-4&from=paste&height=237&id=ue41c0b5c&originHeight=370&originWidth=416&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=201135&status=done&style=none&taskId=ua88ee4a3-4215-4b77-b052-1cff05ca95d&title=&width=266.24)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707573106320-b8e09cea-6ae2-4b3b-bdd4-e33cd8f1f0f4.png#averageHue=%23494f42&clientId=u12e6cca2-e0ad-4&from=paste&height=126&id=u841d4dd7&originHeight=197&originWidth=645&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=153203&status=done&style=none&taskId=u9ef44227-2778-40d5-bf2d-ab548e5cd07&title=&width=412.8)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707573151593-b0b7a2d9-7f66-4f3c-8af4-3adbc4033636.png#averageHue=%233c5137&clientId=u12e6cca2-e0ad-4&from=paste&height=181&id=u2bda5295&originHeight=283&originWidth=463&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=202133&status=done&style=none&taskId=u5df6ef8b-8f77-4795-8327-27cccb7adb8&title=&width=296.32)
using System.Collections.Generic
```csharp
Stack<int> stack = new Stack<int>();
Queue<int> queue = new Queue<int>();
```
# 委托
## 委托概述

	委托提供与**函数指针**相似的功能，将对方法的引用作为实参传给另一个方法。
	
	存在委托数据类型
	
	委托是强类型的，对委托的调用也是强类型的，_如果实参数据类型不兼容，编译器会报错_
	
	委托实际上是特殊的类，所有委托都不可变。
	
	编译器不允许声明直接或间接从System.Delegate或System.MulticastDelegate派生的类
:::info
  委托属于类型
:::
## 委托的声明

	delegate 返回值类型 委托方法名（参数列表）；_注：一般命名时后跟Delegate表示为委托类型_
  委托可定义在类内，结构体内，接口内。
  定义在函数内就会报错。
```csharp
public delegate void IntMethedDelegate(int x);
```
## 委托的不同使用方法
使用的话Action a = () => WriteLine("esfdefs"); 此种写法目前不受限制
```csharp
//1.
IntMethedDelegate temp = new IntMethedDelegate(Test);
//2.
IntMethedDelegate temp = Test;
//使用例：
temp(2);

//增加
temp += Work;

//减少
temp -= Work;

//清空
temp = null;
```
只引用一个方法的称为单播委托，引用多个方法的称为多播委托
## 委托的实例化
```csharp
static void Sort<T>(T[] data, Func<T, T, bool> compare)
{
    ......
}
Sort<Employee>(employees, Employee.PriceCompare);
```
	委托是引用类型，但不需要用new实例化。从方法组（为方法命名的表达式）向委托类型的转换会自动创建新的委托对象。
## 常用委托类型System.Func和System.Action

:::info
	这两种委托类型均为泛型，C#3.0起包含的一组常规用途的委托。不必自己去声明委托。
:::

1.Func 系列委托代表有返回值的方法，尖括号内先填参数类型，最后填返回值类型
		格式：Func<T1, T2, T3， ...> 方法名 = 注册的方法;
2.Action 系列委托代表返回值为void的方法，尖括号内填参数类型
     	  格式：Action<T1, T2, T3, ...> 方法名 = 注册的方法; 
	以上两种类型均支持最多16个参数，Func最后一个类型为返回值类型（声明中为out TResult）
3.public delegate bool Predicate (T obj);
	若用一个Lambda返回bool，则该Lambda称为**谓词**。通常用谓词筛选或识别集合中的数据项。向谓词传递一个数据项，它返回true或false指出该项是否符合条件。 
```csharp
static void HHH<T>(Func<T> c)
{
    
}
static void MMM(Action a)
{
    
}
```
## 嵌套委托类型

	如果委托声明出现在另一个类中，此委托类型成为嵌套类型。如果仅在包容类中有用，就应该考虑嵌套。
```csharp
class DelegateSample
{
    public delegate bool ComparisonHandler(int first, int second);
}
//DelegateSample.ComparisonHandler
```
## 匿名方法
### 用法
	C#2.0不支持Lambda表达式.
	匿名方法是实例化委托的一种简化方式,值得注意的是匿名方法允许完全省略参数列表
	例如:	`delegate { return Console.ReadLine() != " "; }`
```csharp
//一般作为函数参数传递 或者作为函数返回值
delegate bool TestDelegate(int x, int y);
TestDelegate tmp = delegate (int x, int y)
{
    return x > y;
}
//无参情况下可以省略（）
delegate bool TestDelegate1();
TestDelegate1 tmp1 = delegate { return true; }
//实际上还能进一步简化

//作为返回值
public Action Test()
{
    return delegate 
    {
        WriteLine("返回了一个例子");
    }
}

int main()
{
    ...
    //第一种调用方法
    Action a = t.Test();
    a();
    //第二种调用方法
    t.Test()();
}
```
### 缺点
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683811875331-8ddde394-a850-4581-a432-978b57df588d.png#averageHue=%23334c31&clientId=ue2c0479f-af42-4&from=paste&height=41&id=u2ffb8314&originHeight=51&originWidth=701&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=58836&status=done&style=none&taskId=u25c2f108-4625-45c2-a1df-01ee4ef3cd4&title=&width=560.8)
```csharp
Action ac = delegate
{
    WriteLine("匿名函数一");
}
ac += delegate
{
    WriteLine("匿名函数二");
}
//以上添加进的两个方法没有办法单独移除，只能一次性全部移除,如下操作
ac = null;
```
# lambad
## 什么是lambad
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683894013051-3dd1bde1-5991-4f0e-b1e5-f46ad8281122.png#averageHue=%23355435&clientId=u687389d0-8d72-4&from=paste&height=103&id=u8ff464f5&originHeight=129&originWidth=464&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=87388&status=done&style=none&taskId=u51fde8e8-0d80-408c-a735-0c9c52bc61f&title=&width=371.2)
## lambad表达式语法
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683894050756-556daf20-1716-47e5-bc3a-a0288d1a9d99.png#averageHue=%23292b23&clientId=u687389d0-8d72-4&from=paste&height=122&id=u34e9aebe&originHeight=152&originWidth=269&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29658&status=done&style=none&taskId=u4a091f18-8728-467c-8afb-8bccccb7707&title=&width=215.2)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683894059485-df9f96e2-d9a5-431d-8d6a-a91de96027c5.png#averageHue=%23354930&clientId=u687389d0-8d72-4&from=paste&height=120&id=ub230eb46&originHeight=150&originWidth=199&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=34455&status=done&style=none&taskId=u36e59120-703f-46cd-8115-af5e0e37f36&title=&width=159.2)
## 使用
```csharp
//无参无返回值
Action a = () =>
{
    //TODO
};
//有参无返回值
Action<int> a = (int value) =>
{
    //TODO
};
//简写,因为参数类型可以推断，所以可以省略
Action<int> a = (value) =>
{
    //TODO
};
//无参有返回值
Func<bool> a = () => 
{
    return true;
};
//有参有返回值
Func<int, int, bool> a = (int a, int b) =>
{
    return a > b;
};
//简写， 函数体内只有一条语句时，可以省略大括号
Func<int, int, bool> a = (a, b) => a > b;
```
## 🌟闭包
##  ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683896202507-1baec5ae-64b0-4773-9651-504f4ff3f215.png#averageHue=%23375232&clientId=u687389d0-8d72-4&from=paste&height=104&id=u39c42a51&originHeight=130&originWidth=715&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=110712&status=done&style=none&taskId=u49c53f45-b95c-47fb-8676-c4e7a3e2de0&title=&width=572)
```csharp
Class Temple
{
	int value = 10; //正常情况下value变量执行过后可能会被GC
	public Temple()
    {
        Action a = () => {WriteLine(value);}; //此处引用了value，不会被GC掉，直到委托列表被刷新
        //一直在占用
        a = null;
        Action b = null;
		for(int i = 0; i < 10; i++)
    	{
            int tmp = i;
        	a += () =>
        	{
            	WriteLine(i); 
        	};
            b += () =>
            {
                WriteLine(tmp);
            };
    	} 
    }

 
 	public void Test()
    {
        a();  //打印结果为10个10；因为10是父函数范围内的最终值      
        b();  //打印结果为0到10；因为每次给tmp赋值都相当于是一个新的
	}
}
```
# List排序
使用List 要先引用System.Collections.Generic命名空间
## List自带的排序方法，自定义类的排序
:::info
  Sort()方法
:::
想要使用此方法需要本类型继承IComparable<T>接口，如果没有需要手动实现，并重写CompareTo（T other）函数
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684046172731-bb977e25-ceca-4fec-9413-d3abbd6ff26a.png#averageHue=%23545649&clientId=ue1ef18f6-ac02-4&from=paste&height=81&id=u1a11fc47&originHeight=101&originWidth=557&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=39205&status=done&style=none&taskId=u678a7508-40dd-4514-a27d-f06105dcbcf&title=&width=445.6)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684046193111-92f1a7ac-b88c-43b5-a61e-1f385c085db0.png#averageHue=%233e5337&clientId=ue1ef18f6-ac02-4&from=paste&height=197&id=ud9834ec2&originHeight=364&originWidth=534&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=162341&status=done&style=none&taskId=u12614e4d-592f-4f6e-b5f8-e99d8cc0771&title=&width=289.20001220703125)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684046345518-e24771f7-ddc4-4f4d-977b-541a82f7d850.png#averageHue=%23262626&clientId=ue1ef18f6-ac02-4&from=paste&height=175&id=u9aa770fa&originHeight=344&originWidth=498&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=62129&status=done&style=none&taskId=u489d0902-f941-439e-a1de-b1e0efbf010&title=&width=253.39999389648438)
如果继承的是IComparable接口，CompareTo方法内参数类型为Object
![使用时需要用as转换下类型](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684046532407-72f3d1c9-6652-479b-b88f-1cb292ac75ff.png#averageHue=%23504e48&clientId=ue1ef18f6-ac02-4&from=paste&height=79&id=u33603506&originHeight=99&originWidth=679&originalType=binary&ratio=1.25&rotation=0&showTitle=true&size=44423&status=done&style=none&taskId=u94b9c143-b50b-4d44-9a34-922e0ee10d5&title=%E4%BD%BF%E7%94%A8%E6%97%B6%E9%9C%80%E8%A6%81%E7%94%A8as%E8%BD%AC%E6%8D%A2%E4%B8%8B%E7%B1%BB%E5%9E%8B&width=543.2 "使用时需要用as转换下类型")
## 通过委托函数进行排序
  通过继承接口的方式来写比较麻烦每次新写一个类都要先重写方法
```csharp
public class Data
{
    private int id;
    public Data(int id)
    {
        this.id = id;
    }
}
...
int main()
{
    List<Data> list = new List<Data>();
    List.Add(new Data(1));
    List.Add(new Data(3));
    List.Add(new Data(2));
	//委托写法
    list.Sort(DataSort);
	//匿名函数写法
    list.Sort(delegate(Data a, Data b)
    {
    	if(a.id > b.id)
    	{
        	return 1;
    	}
    	else
    	{		
        	return -1;
    	}
    });
    //lambad表达式写法
    list.Sort((a, b) => a.id > b.id ? 1 : -1);
}

static int DataSort(Data a, Data b)
{
    if(a.id > b.id)
    {
        return 1;
    }
    else
    {
        return -1;
    }
}
```
# 协变逆变
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684048159527-30b0f514-4760-4b8a-ac9f-0971164e1c9d.png#averageHue=%23375133&clientId=ue1ef18f6-ac02-4&from=paste&height=125&id=ufc6d7d63&originHeight=237&originWidth=511&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=143416&status=done&style=none&taskId=u5c2d311e-8415-478e-94e8-25cc8c987a0&title=&width=269.8000183105469)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684048174186-50140fa8-97c5-4460-9f9b-2f19e05960b6.png#averageHue=%233b5335&clientId=ue1ef18f6-ac02-4&from=paste&height=105&id=u79292d21&originHeight=221&originWidth=791&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=173103&status=done&style=none&taskId=u12a49ca8-bfa0-425e-ac80-5486e1c1a6b&title=&width=376.79998779296875)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684048448706-2412f197-46b9-4482-8e35-1bd06a092863.png#averageHue=%23354e32&clientId=ue1ef18f6-ac02-4&from=paste&height=107&id=ufbcf9c74&originHeight=200&originWidth=461&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=130045&status=done&style=none&taskId=u8bc6bae7-a89f-435b-9a27-799d09c90af&title=&width=246.80001831054688)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684048435454-4c06c9a2-daf3-455e-920d-c9f7a8798d09.png#averageHue=%23344f36&clientId=ue1ef18f6-ac02-4&from=paste&height=93&id=u19d66b9d&originHeight=161&originWidth=527&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=134551&status=done&style=none&taskId=ucac234ec-ac0c-4089-9072-b9aa3960ecf&title=&width=303.6000061035156)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684050306812-2a86bf40-4b70-4832-a90b-daf6ec723e99.png#averageHue=%233c4235&clientId=ue1ef18f6-ac02-4&from=paste&height=140&id=ue6692cd7&originHeight=234&originWidth=427&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=72319&status=done&style=none&taskId=u6e167732-3ae2-4e32-bc1a-aa3e0f05f26&title=&width=255.60000610351562)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684050339287-2b665814-57e7-4f44-bef3-4612db1b4d03.png#averageHue=%23494a40&clientId=ue1ef18f6-ac02-4&from=paste&height=228&id=u49cabc99&originHeight=377&originWidth=938&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=248525&status=done&style=none&taskId=ub53bceda-f969-4235-b4d2-07359a3f5e1&title=&width=567.4000244140625)
![只有使用了in关键字iF才能正常转换类型](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684050413787-d8d0e0a9-d4a3-4d08-b3de-e43b64150a96.png#averageHue=%23302f2a&clientId=ue1ef18f6-ac02-4&from=paste&height=241&id=ud0fcfdbc&originHeight=392&originWidth=925&originalType=binary&ratio=1.25&rotation=0&showTitle=true&size=205363&status=done&style=none&taskId=u3a4315cb-c404-470c-b9bc-93d37af4bf9&title=%E5%8F%AA%E6%9C%89%E4%BD%BF%E7%94%A8%E4%BA%86in%E5%85%B3%E9%94%AE%E5%AD%97iF%E6%89%8D%E8%83%BD%E6%AD%A3%E5%B8%B8%E8%BD%AC%E6%8D%A2%E7%B1%BB%E5%9E%8B&width=568 "只有使用了in关键字iF才能正常转换类型")
# 多线程
## 什么是进程
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684051146986-a62cbe30-18b8-4701-853d-685d26d3a1e7.png#averageHue=%23344c31&clientId=ue1ef18f6-ac02-4&from=paste&height=123&id=u81ce3004&originHeight=196&originWidth=941&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=255761&status=done&style=none&taskId=uafc98328-3297-4394-bc09-12df9e189ec&title=&width=589.4000244140625)
## 什么是线程
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684051328229-75b5c9f8-17dc-4fbb-b638-81d956ad1299.png#averageHue=%2333412d&clientId=ue1ef18f6-ac02-4&from=paste&height=158&id=u2362368c&originHeight=282&originWidth=1042&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=250799&status=done&style=none&taskId=uedfa2895-8239-4ebf-8f15-9f96fed420c&title=&width=583.4000244140625)
![](https://cdn.nlark.com/yuque/0/2023/jpeg/35758071/1684052316469-8491d21b-ef79-465b-a9c9-7de9c3a35a8c.jpeg)
## 语法相关
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684051928877-e863dc25-bc52-4811-97f5-5f28b232c0f1.png#averageHue=%233f5639&clientId=ue1ef18f6-ac02-4&from=paste&height=147&id=u07d48c24&originHeight=314&originWidth=853&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=250376&status=done&style=none&taskId=u2f083120-1a49-447f-afca-85634937562&title=&width=399.4000244140625)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684051945587-c0dc7d05-9f62-4673-bb2d-81d397451854.png#averageHue=%23375033&clientId=ue1ef18f6-ac02-4&from=paste&height=92&id=uc71e5a94&originHeight=194&originWidth=1054&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=234535&status=done&style=none&taskId=u90056b85-a540-46e1-abb0-8bb60d1e5ce&title=&width=499.4000244140625)
![isRuning是后台线程里循环的控制条件](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684052757089-11fa967b-fcc2-4ea7-86cc-4f4d8a93d6f6.png#averageHue=%23262525&clientId=ue1ef18f6-ac02-4&from=paste&height=378&id=uc2449d02&originHeight=818&originWidth=1046&originalType=binary&ratio=1.25&rotation=0&showTitle=true&size=364753&status=done&style=none&taskId=u8f7d6312-1526-494c-b4c0-3555dbe97a3&title=isRuning%E6%98%AF%E5%90%8E%E5%8F%B0%E7%BA%BF%E7%A8%8B%E9%87%8C%E5%BE%AA%E7%8E%AF%E7%9A%84%E6%8E%A7%E5%88%B6%E6%9D%A1%E4%BB%B6&width=483.3999938964844 "isRuning是后台线程里循环的控制条件")
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684052860278-8895d345-8614-4363-b11d-f8107056667f.png#averageHue=%23354f32&clientId=ue1ef18f6-ac02-4&from=paste&height=126&id=u16eb4b1f&originHeight=157&originWidth=564&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=112131&status=done&style=none&taskId=u1ab8d630-a9fb-47b2-a300-ec3659d218c&title=&width=451.2)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684053121607-323870b6-fc7e-4369-b6d1-8be5c2141678.png#averageHue=%23365033&clientId=ue1ef18f6-ac02-4&from=paste&height=73&id=u15e6f922&originHeight=131&originWidth=876&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=150095&status=done&style=none&taskId=ua5594ae7-e376-4607-b0c4-17c09125f3a&title=&width=490.79998779296875)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684053432535-f9ff7952-8b88-471d-babb-d27dab4eda18.png#averageHue=%23385133&clientId=ue1ef18f6-ac02-4&from=paste&height=125&id=ueb70c47d&originHeight=156&originWidth=831&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=138013&status=done&style=none&taskId=u3f0e733f-7874-4658-b8c4-362dcbece22&title=&width=664.8)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684053453019-4d11f05b-cbcc-488a-a219-0ad8d75297d5.png#averageHue=%23292928&clientId=ue1ef18f6-ac02-4&from=paste&height=173&id=uf51e748e&originHeight=358&originWidth=861&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=143670&status=done&style=none&taskId=ue468cb51-235d-43bc-95e6-33b50b58d2c&title=&width=416.79998779296875)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684053515520-5c5baf18-bd60-4d0d-a538-47e85d204eef.png#averageHue=%23545246&clientId=ue1ef18f6-ac02-4&from=paste&height=173&id=u88b92318&originHeight=506&originWidth=831&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=276989&status=done&style=none&taskId=u74e84614-dff1-497d-bbaf-1c256f3ca6c&title=&width=283.79998779296875)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684053742622-025b4a5f-fd75-4479-8463-1eb8b3b97989.png#averageHue=%23354f32&clientId=ue1ef18f6-ac02-4&from=paste&height=76&id=u0b22b0c9&originHeight=95&originWidth=613&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=82790&status=done&style=none&taskId=u4ed6f358-6323-44e5-8123-1b2bef619bd&title=&width=490.4)
# 特性
## 特性的定义
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684227110315-7757ea5d-6aa6-4b7f-a8df-d94c64e6afb1.png#averageHue=%23395333&clientId=u32a9e54e-c02e-4&from=paste&height=253&id=u14815a33&originHeight=316&originWidth=1114&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=419018&status=done&style=none&taskId=u41632dbc-6660-44d2-9a7e-dcc7fa6d915&title=&width=891.2)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684229028130-277a3bb1-bbb7-4ad9-aad4-0a02c2e258c6.png#averageHue=%23375133&clientId=u32a9e54e-c02e-4&from=paste&height=119&id=u501eb6bb&originHeight=149&originWidth=899&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=180684&status=done&style=none&taskId=u152667da-49b4-4ecd-a3e2-f74fe50e2b0&title=&width=719.2)
## 自定义特性
自定义特性命名时后边必须跟Attribute
```csharp
    class MySelfAttribute : Attribute
    {
        private string developer;
        private string vesion;
        private string description;
        public MySelfAttribute(string a, string b, string c)
        {
            developer = a;
            vesion = b;
            description = c;
        }
    }
    [MySelf("Nie","1.0.0.0","test")]
    internal class Program
    {
        ...
    }
```
## 特性的搜索
Isdefined() 第一个参数为需要搜索特性的类型，第二个参数为布尔类型表示是否搜索继承链
```csharp
if (typeof(Program).IsDefined(typeof(MySelfAttribute), false))
{
    Write("Get");
}

//获取全部特性信息，通过反射实现
object[] ob = typeof(Program).GetCustomAttributes(true);
for(int i = 0; i < ob.size(); i++)
{
    if(ob[i] is MySelfAttribute)
    {
        WriteLine((ob[i] as MySelfAttribute).info);
    }
}
```
## 限定特性的范围
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684228872156-f66efa6a-5bc7-4097-8b7e-ffe026b441d5.png#averageHue=%233d5238&clientId=u32a9e54e-c02e-4&from=paste&height=128&id=u946878d4&originHeight=160&originWidth=1724&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=314301&status=done&style=none&taskId=ub0e02834-79fe-4b69-b1e6-a312b62b4e9&title=&width=1379.2)
将该特性加在自定义特性上面实现对范围的限定, | 是位运算符，全为0结果才为0
## 系统自带特性
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684229231777-f7ad813f-c98a-499a-a88f-bdd05c3f781b.png#averageHue=%23374d33&clientId=u32a9e54e-c02e-4&from=paste&height=111&id=ue671b35c&originHeight=189&originWidth=946&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=218319&status=done&style=none&taskId=u0888e46c-cc4f-4b70-93ce-9acfa7bae08&title=&width=556.4000244140625)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684229418573-0d7f50a5-789c-4c41-8c4d-bceec9751490.png#averageHue=%23303429&clientId=u32a9e54e-c02e-4&from=paste&height=214&id=ua73cebc4&originHeight=378&originWidth=988&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=253917&status=done&style=none&taskId=u83777524-8f5b-4268-b09d-dc4bea83c21&title=&width=559.4000244140625)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684229436757-9fd4aeb5-e492-4a2e-9ef7-aa13889bc10b.png#averageHue=%23504d42&clientId=u32a9e54e-c02e-4&from=paste&height=150&id=u217a2b1e&originHeight=352&originWidth=1309&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=298514&status=done&style=none&taskId=u0feabef3-aefc-4389-bd7e-ffbe719121d&title=&width=559.4000244140625)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684229565067-a69d69cd-6fc0-4586-8078-fa60ae7ab934.png#averageHue=%233c5235&clientId=u32a9e54e-c02e-4&from=paste&height=216&id=u26ed16c5&originHeight=297&originWidth=773&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=216406&status=done&style=none&taskId=u7b9fb682-19c9-4148-9efe-8d8088c0994&title=&width=562.4000244140625)
![只有#define Fun ，该函数才能执行](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684229582350-080cc1e1-1363-47af-9470-f29072842dc2.png#averageHue=%235a5146&clientId=u32a9e54e-c02e-4&from=paste&height=169&id=ue2c5f0d7&originHeight=273&originWidth=585&originalType=binary&ratio=1.25&rotation=0&showTitle=true&size=96702&status=done&style=none&taskId=u2b4202d5-c702-44b9-826c-5f082521864&title=%E5%8F%AA%E6%9C%89%23define%20Fun%20%EF%BC%8C%E8%AF%A5%E5%87%BD%E6%95%B0%E6%89%8D%E8%83%BD%E6%89%A7%E8%A1%8C&width=362 "只有#define Fun ，该函数才能执行")
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684229778586-f43952b4-ebc7-4cac-9f51-16b15ff2dfe8.png#averageHue=%23395135&clientId=u32a9e54e-c02e-4&from=paste&height=151&id=u89433f41&originHeight=247&originWidth=992&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=279200&status=done&style=none&taskId=u9d36088b-1bab-4f44-817f-f4c50839ff0&title=&width=606.4000244140625)
![括号内填路径名，下面的方法是该包内的同名方法](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684229801656-d45fbc6f-f13e-4d2a-b9ba-2ac9f11f30c9.png#averageHue=%23363730&clientId=u32a9e54e-c02e-4&from=paste&height=102&id=ue5ccfeaf&originHeight=128&originWidth=770&originalType=binary&ratio=1.25&rotation=0&showTitle=true&size=92250&status=done&style=none&taskId=uab12a99f-2f32-41d2-9dce-9b5b9683636&title=%E6%8B%AC%E5%8F%B7%E5%86%85%E5%A1%AB%E8%B7%AF%E5%BE%84%E5%90%8D%EF%BC%8C%E4%B8%8B%E9%9D%A2%E7%9A%84%E6%96%B9%E6%B3%95%E6%98%AF%E8%AF%A5%E5%8C%85%E5%86%85%E7%9A%84%E5%90%8C%E5%90%8D%E6%96%B9%E6%B3%95&width=616 "括号内填路径名，下面的方法是该包内的同名方法")
# 迭代器
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684238737443-3e06d22a-c865-4aa5-8fe9-5acec5186196.png#averageHue=%23364f32&clientId=u32a9e54e-c02e-4&from=paste&height=110&id=u011fefb2&originHeight=167&originWidth=913&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=203143&status=done&style=none&taskId=ub0d7e199-8433-4366-bf63-3722655e48d&title=&width=601.4000244140625)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684238759220-3229c224-7205-406f-bed0-f7c43fcf7849.png#averageHue=%23355032&clientId=u32a9e54e-c02e-4&from=paste&height=119&id=ud988f093&originHeight=167&originWidth=843&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=217845&status=done&style=none&taskId=u2719ce2b-0439-4855-abdb-e013afe9a0d&title=&width=600.4000244140625)
## 迭代器的实现
```csharp
    class Enu : IEnumerable, IEnumerator
    {
        private int index = -1;
        private int[] list;
        public Enu()
        {
            list = new int[] { 1, 2, 3 };
        }
        public object Current
        {
            get { return list[index]; }
        }

        public IEnumerator GetEnumerator()	//获取IEnumerator对象进行迭代，可以不继承IEnumerable接口
        {
            Reset();
            return this;
        }

        public bool MoveNext()	//每次进来先调用，所以最开始index下标要从-1开始
        {
            ++index;
            return index < list.Length;  
        }

        public void Reset()
        {
            index = -1;
        }
    }

...
    
    Enu e = new Enu();
    foreach(int i in e)
    {
        WriteLine(i);
    }
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684239598077-40128700-55dd-4e10-bd6e-aa6898b6e434.png#averageHue=%233a4f33&clientId=u32a9e54e-c02e-4&from=paste&height=236&id=uf379a32e&originHeight=437&originWidth=889&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=365734&status=done&style=none&taskId=u2af4bdee-5cda-466a-aa1e-e781b45901e&title=&width=479.20001220703125)
## 用 yield return 语法糖实现迭代器
语法糖，跟表示实现方法效果一样
```csharp
    class Enu1 : IEnumerable
    {
        private int[] i;
        public Enu1()
        {
            i = new int[] { 1, 2, 3, 4, 5 };
        }
        public IEnumerator GetEnumerator()
        {
            for(int i = 0; i < this.i.Length; i++)
            {
                yield return this.i[i];
            }
        }
    }
```
## 泛型类的迭代器
```csharp
calss Enu<T> : IEnumerable
{
	private T[] array;
    public IEnumerator GetIEnumerator
    {
        for(int i = 0; i < array.Length; i++)
        {
            yield return array[i];
    	}
	}
}
```
# 特殊语法
 ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684241963549-df512cdc-393f-41ad-bb0b-9fc1ec17f707.png#averageHue=%233a5235&clientId=u32a9e54e-c02e-4&from=paste&height=245&id=uf0ac7bcf&originHeight=306&originWidth=773&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=255089&status=done&style=none&taskId=udae9fe50-eceb-4f3a-86d6-db50a814727&title=&width=618.4)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684242521286-c7f214dd-24f9-4f3a-af9e-9cf48ddbe899.png#averageHue=%233a4e37&clientId=u32a9e54e-c02e-4&from=paste&height=108&id=u2ba3f3af&originHeight=208&originWidth=1196&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=269980&status=done&style=none&taskId=ud52173fe-0834-48dd-9deb-5e12bc5f994&title=&width=623.4000244140625)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684242606203-05f56a4e-d960-4c8d-adbf-4a90f1970035.png#averageHue=%234c4740&clientId=u32a9e54e-c02e-4&from=paste&height=303&id=u9b1a2885&originHeight=766&originWidth=1055&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=483016&status=done&style=none&taskId=ucfbe359f-a963-487b-85b4-a00fac3326a&title=&width=417.4000244140625)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684242681479-d2cfd1a7-bcf8-47b9-a288-2355a96cdd0e.png#averageHue=%23414f3b&clientId=u32a9e54e-c02e-4&from=paste&height=149&id=u271f96a8&originHeight=213&originWidth=932&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=215062&status=done&style=none&taskId=ubc67cd7a-8518-45e7-8bd4-b906b13f68d&title=&width=652.6000366210938)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684243035540-e399684d-6fc9-403d-b912-8c37c87c7bc0.png#averageHue=%23484b3c&clientId=u32a9e54e-c02e-4&from=paste&height=297&id=u9005ad35&originHeight=846&originWidth=867&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=561407&status=done&style=none&taskId=uee4474fa-2478-4715-8801-511b0f622b0&title=&width=304.6000061035156)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684243085053-1bb449a2-537e-4560-9903-3bc9903f7e77.png#averageHue=%234e4e41&clientId=u32a9e54e-c02e-4&from=paste&height=236&id=u50f9b476&originHeight=508&originWidth=858&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=313095&status=done&style=none&taskId=ub50915fa-bcd0-4c71-b8e2-6ee9c749521&title=&width=399.4000244140625)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684243216368-af74d587-8927-4712-ad4b-08bc69887882.png#averageHue=%23424f3a&clientId=u32a9e54e-c02e-4&from=paste&height=277&id=u7f0ac25f&originHeight=593&originWidth=819&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=388827&status=done&style=none&taskId=u367a07f3-b2c6-45b1-a7a2-5c936d48361&title=&width=383.20001220703125)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684243248856-48604435-27de-46e3-81ed-8810931dd257.png#averageHue=%2340513a&clientId=u32a9e54e-c02e-4&from=paste&height=202&id=u027f77ab&originHeight=253&originWidth=873&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=224589&status=done&style=none&taskId=u6b9f0ca3-d513-4e99-9226-e947f2b8f00&title=&width=698.4)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684243355670-136a983f-f50e-4afd-aec6-c65ccfa27779.png#averageHue=%23424441&clientId=u32a9e54e-c02e-4&from=paste&height=53&id=uf7c84544&originHeight=66&originWidth=425&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31808&status=done&style=none&taskId=u293f5c2a-17f8-4cf5-ad83-acae712e6b1&title=&width=340)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1684243380081-e7aff16f-7e67-43f1-8934-2afe84728f2f.png#averageHue=%23314b45&clientId=u32a9e54e-c02e-4&from=paste&height=38&id=u97d3fc3d&originHeight=48&originWidth=1007&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=67191&status=done&style=none&taskId=ucf1e00be-7a24-468a-aeb9-046ec02f9b0&title=&width=805.6)
# 事件
## 事件的声明语法
事件只能声明在类内，结构体内，接口内，不能在类外声明
```csharp
even Action e;
```
## 事件的使用
使用上和委托几乎一致
事件不属于类型，是对委托修饰
```csharp
public class MyClass
{
    public MyClass()
    {
        event Action e;
        e = Test();
        e += Test1();
        e -= Test1();
    }
    public void Test() {}
    public void Test1() {}
}
```
## 事件的作用
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683807262096-d2b1f82a-87df-410f-80b9-026e46903e80.png#averageHue=%233b5438&clientId=ue2c0479f-af42-4&from=paste&height=106&id=u9c433c74&originHeight=133&originWidth=699&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=120668&status=done&style=none&taskId=uc1290953-7562-4e83-8ca0-3791b418301&title=&width=559.2)
## 委托和事件的区别
事件不能在类外部使用赋值运算符=，不能在类外部调用，不能作为函数中的临时变量
事件只能作为成员存在于类和接口以及结构体中，不能够在类外声明
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683807407515-dffb1c97-3b48-4e40-b610-540d6174851b.png#averageHue=%233a4e33&clientId=ue2c0479f-af42-4&from=paste&height=252&id=uadb044cf&originHeight=315&originWidth=933&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=278763&status=done&style=none&taskId=u3f920fe8-c717-4f31-bc7a-a9b6ae4db36&title=&width=746.4)
# 反射
## 程序集的概念
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683634113021-07cba86a-90f4-4233-b63d-75efd1b33d78.png#averageHue=%23374d32&clientId=u12fc569d-c90c-4&from=paste&height=231&id=ucb1922a4&originHeight=289&originWidth=1256&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=351399&status=done&style=none&taskId=u3a395a54-305a-46bf-828d-416fb096f59&title=&width=1004.8)
## 元数据
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683634172374-dc69d4b3-d244-4f85-9722-dca4b1fbbf32.png#averageHue=%23385134&clientId=u12fc569d-c90c-4&from=paste&height=187&id=u87643516&originHeight=234&originWidth=872&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=240757&status=done&style=none&taskId=uf58e9020-4952-46fe-a07f-a720f578658&title=&width=697.6)
## 反射的概念
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683634246068-ae761022-04a9-415a-9a90-5289234b0245.png#averageHue=%23395134&clientId=u12fc569d-c90c-4&from=paste&height=199&id=u212d5b21&originHeight=249&originWidth=1043&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=290003&status=done&style=none&taskId=u36d0b2f8-6cc1-490e-8275-c5f8777cdd6&title=&width=834.4)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683634317241-f2c21de4-ec46-454e-ad5c-8d84c24f0e7f.png#averageHue=%23354c32&clientId=u12fc569d-c90c-4&from=paste&height=130&id=ud26a12fe&originHeight=163&originWidth=1020&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=229789&status=done&style=none&taskId=ue1e550b5-0a49-47c1-9464-6fff29add49&title=&width=816)
```csharp
//万物之父object中的GetType()可以获取对象的Type
int a = 666;
Type type = a.GetType();
//通过typeof关键字传入类名
Type type2 = typeof(int);
//通过Type.GetType("完整命名")  类名必须包含命名空间
Type type3 = Type.GetType("System.Int32");
//得到所在程序集信息
WriteLine(type.Assembly);
```
指向同一对象的不同Type类型在栈区拥有不同的地址，但在堆区都指向同一地址
```csharp
Class Test
{
	private _i = 0;
    public _j = 0;
    public Test(int i)
    {
        _i = i;
    }
    public Test(int j, int i) : this(i)
    {
        _j = j;
    }
    public void Speak()
    {
        //TODO
    }
}
```
:::info
使用时需要先引入using System.Reflection命名空间
:::
```csharp
//获取所有信息
Type t = typeof(Test);
MemberInfo[] infos = t.GetMembers(); //只是获得了信息，不是具体对象
for(int i = 0; i < infos.Length; i++)
{
    WriteLine(infos[i]);
}

//获取无参构造函数信息
ConstructorInfo info = t.GetConstructor(new Type[0]);
Test test = info.Invoke(null) as Test; //激活类型并转换为对应类型
WriteLine(test._j); //此时可以访问Test类内的公有信息

//获取有参构造函数信息
Constructor info2 = t.GetConstructor(new Type[] {typeof(int)}); //获取的是Test中的第一个有参构造
//获取其他有参构造，继续添加参数对应类型的typeof
Test test2 = info2.Invoke(2) as Test;
WriteLine(test2._j); //此时打印结果应为2

//获取所有公有成员信息
FieldInfo[] fs = t.GetFields();
for(int i = 0; i < t.Length; i++)
{
    WriteLine(fs[i]);
}
//得到指定名称的公共成员变量
FieldInfo f = t.GetField("_j");
WriteLine(f.GetValue(test));	//此处默认获取的是自身程序集里边的类，所以上边用到了as。实际开发中就用obj
f.SetValue(test, 100); //将_j的值改成100;

//获取公有方法信息，举例获取string里的方法，不能获取到扩展方法
Type str = tyypeof(string);
MethodInfo[] mi = str.GetMMethods("Substring", new Type[] { typeof(int), typeof(int) } );
for(int i = 0; i < mi.Lenght; i++)
{
    WriteLine(mi[i]);
}
string str2 = "awd";
object obj = mi.Invoke(str2, new Type[] {1, 2}); //第一个参数是调用对象，如果是静态方法则写null，后边为传入的参数
WriteLine(obj);
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683721992693-f9692d91-3bb5-4778-8705-3eb7b2427ddc.png#averageHue=%23354832&clientId=u96d9e16b-b323-4&from=paste&height=325&id=u90874c97&originHeight=668&originWidth=255&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=182180&status=done&style=none&taskId=ucd1a21a8-c26a-44ae-9099-c03ca18f792&title=&width=124)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683722025396-6a6cced5-6ed3-44df-947c-538b225c77c7.png#averageHue=%23354932&clientId=u96d9e16b-b323-4&from=paste&height=88&id=uc5cbe6d1&originHeight=136&originWidth=197&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=35517&status=done&style=none&taskId=ua23d02b8-85a8-4bc8-b0b6-d7ee7239468&title=&width=128)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683722034718-639d908b-8350-466e-be3e-641df5831cae.png#averageHue=%23344732&clientId=u96d9e16b-b323-4&from=paste&height=85&id=ubc10fc8d&originHeight=124&originWidth=255&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=40290&status=done&style=none&taskId=ueab715e3-166a-4ea0-9526-f81dfcfdb00&title=&width=174)![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683722062569-1caaf667-8d56-4a30-9a69-7dd7445a2102.png#averageHue=%23344832&clientId=u96d9e16b-b323-4&from=paste&height=87&id=udd5338b4&originHeight=114&originWidth=233&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=38035&status=done&style=none&taskId=u1f1c27e1-3f6f-4a3f-bcd1-9b532dfab39&title=&width=178)
## 反射关键类Assmebly和Activator
### Activator类
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683722951671-585d401f-3e5c-431f-a785-c836e8f7e418.png#averageHue=%23354e31&clientId=u96d9e16b-b323-4&from=paste&height=118&id=u846bdb5b&originHeight=158&originWidth=502&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=116547&status=done&style=none&taskId=ub6e88b0e-1e52-4cb7-bcc3-9ab148bc1e8&title=&width=375)
```csharp
Type test = typeof(Test);
//无参构造
Test t = Activator.CreateInstance(test) as Test;
//有参构造,一个参数
Test t2 = Activator.CreateInstance(test, 1) as Test;
WriteLine(t2._j); //访问得到的类内成员
```
### Assembly类
   ![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683724970965-5147d66d-4894-4350-bd4c-db9264fc8c57.png#averageHue=%23394f32&clientId=u96d9e16b-b323-4&from=paste&height=226&id=u872e32a6&originHeight=537&originWidth=1249&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=642931&status=done&style=none&taskId=u14734ac1-5ee1-4bf3-8958-dd20e252c6c&title=&width=526.4000244140625)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683725024178-1562c6ba-8a1c-4008-986c-3c01e6b4b4fc.png#averageHue=%23554e44&clientId=u96d9e16b-b323-4&from=paste&height=226&id=u3c6b1063&originHeight=282&originWidth=1446&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=270962&status=done&style=none&taskId=u5b24c061-eb39-4862-99cb-c4cc639d222&title=&width=1156.8)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683725079509-3ddb92bb-d922-421b-adbd-b56ed746764e.png#averageHue=%23304431&clientId=u96d9e16b-b323-4&from=paste&height=98&id=u64c5a96d&originHeight=123&originWidth=856&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=166071&status=done&style=none&taskId=u443afc5a-419f-4be0-a9b6-9edc51c457d&title=&width=684.8)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1683725107849-e1f486e1-dcf8-4d68-801c-2f75c63b0ea4.png#averageHue=%23514f40&clientId=u96d9e16b-b323-4&from=paste&height=602&id=u4f712b1b&originHeight=752&originWidth=1263&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=648103&status=done&style=none&taskId=ucf3510d1-5e3e-471f-9559-3c00ade1b85&title=&width=1010.4)
:::info
通过反射可以访问其他工程中的代码，不用拥有某个类，利用Activator类直接快速创建对象。
:::
:::info
通过MethodInfo类创建的对象，调用需要使用.Invoke（）方法，第一个参数填调用该函数的对象
:::
获取dll路径时，填写完整路径信息，不用写.dll后缀。
获取程序集中的某个类的信息时，GetType("") 里边填写完整的命名空间名称点类名称
# 值类型和引用类型
值类型：

- 无符号：byte ushort uint ulong
- 有符号：sbyte short int long
- 浮点数：double float decimal
- 特殊：char bool
- 枚举：enum
- 结构：struct

引用类型：

- string
- class
- interface
- 委托

值类型和引用类型的本质区别：值的具体内容存储在栈内存上，引用的具体内容存储在堆内存上
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707725715359-2574c2c7-16fb-4d42-a0b3-0a34fc18ae8f.png#averageHue=%232b2d26&clientId=u939a244c-6990-4&from=paste&height=521&id=u5084ae01&originHeight=814&originWidth=877&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=703075&status=done&style=none&taskId=u5395c11b-aa84-44b9-9786-8c3e9784578&title=&width=561.28)
语句块执行结束，没有被记录的对象将被回收或变成垃圾。
值类型：系统自动回收
引用类型：在栈上存储地址的内存被自动回收，堆上存储的具体内容变成垃圾，待下次GC回收
想要不被回收或者不变成垃圾：必须将其记录下来
如何记录：在更高层级记录 或 使用静态全局变量记录
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707726715321-37f5608a-a023-4e1d-b235-8936d3d8319f.png#averageHue=%23374f32&clientId=u939a244c-6990-4&from=paste&height=204&id=u8515ebf1&originHeight=318&originWidth=769&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=416734&status=done&style=none&taskId=u8f2d5d04-d221-46b8-843a-27d37e7e0d2&title=&width=492.16)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/35758071/1707726901875-7cdc9d86-6358-4d60-a8ab-0e40ba1c36a3.png#averageHue=%23374e33&clientId=u939a244c-6990-4&from=paste&height=338&id=ubba8998b&originHeight=528&originWidth=578&originalType=binary&ratio=1.5625&rotation=0&showTitle=false&size=530579&status=done&style=none&taskId=u174e75de-4071-4ac6-aab2-879ba857f20&title=&width=369.92)
利用里氏替换原则，用接口容器装载结构体存在装箱拆箱
```csharp
interface ITest
{
    int Value
    {
        get;
        set;
    }
}

struct TestStruct : interface
{
    private int _value;
    int Value
    {
        get
        {
            return _value;
        }
        set
        {
            _value = value;
        }
    }
}	
```
```csharp
//在栈内存储
TestStruct obj1 = new TestStruct();
obj1.Value = 1;
WriteLine(obj1.Value);	//1
TestStruct obj2 = new TestStruct();
obj2.Value = 2;
obj1 = obj2;
//值类型互不影响
WriteLine(obj1.Value);	//1
WriteLine(obj2.Value);	//2

ITest iObj1 = obj1;	//装箱，在堆区重新存储
ITest iObj2 = iObj1;
iObj2.Value = 3;
//指向同一内存地址，都变
WriteLine(iObj.Value);	//3
WriteLine(iObj.Value);	//3

TestStruct obj3 = (TestStruct)iObj1;	//拆箱, 在栈区

```
# 排序进阶
## 插入排序
分为排序区 和 未排序区。每次取未排序区中元素插入到排序区中
```csharp
static void Main(string[] args)
{
    int[] arr = new int[] { 9, 8, 7, 6, 5 };

    for(int i = 1; i < arr.Length; ++i)
    {
        ///排序区最后一个元素
        int sortIndex = i - 1;
        //未排序区第一个元素
        int nSortNum = arr[i];

        while(sortIndex >= 0 && arr[sortIndex] > nSortNum)
        {
            arr[sortIndex + 1] = arr[sortIndex];
            sortIndex--;
        }

        arr[++sortIndex] = nSortNum;
    }

    for(int i = 0; i < arr.Length; i++)
    {
        WriteLine(arr[i]);
    }
}
```
## 希尔排序
按步长分组执行插入排序
```csharp
static void Main(string[] args)
{
    int[] arr = new int[] { 9, 8, 7, 6, 5 };
    //确定步长，步长<=0表示排序完成
    for(int step = arr.Length / 2; step > 0; step /= 2)
    {
        //分组进行排序
        for(int i = step; i < arr.Length; ++i)
        {
            int sortIndex = i - step;
            int nSortNum = arr[i];
            while (sortIndex >= 0 && arr[sortIndex] > nSortNum)
            {
                arr[sortIndex + step] = arr[sortIndex];
                sortIndex -= step;
            }

            arr[sortIndex + step] = nSortNum;
        }
    }

    for(int i = 0; i < arr.Length; ++i)
    {
        WriteLine(arr[i]);
    }
}
```
## 归并排序
递归不停左右分组，分完从最小左右排序归并
```csharp
internal class Program
{
    //#nullable enable
    public static int[] Sort(int[] left, int[] right)
    {
        int[] array = new int[left.Length + right.Length];
        int leftI = 0, rightI = 0;

        for (int i = 0; i < array.Length; ++i)
        {
            if (leftI >= left.Length)
            {
                array[i] = right[rightI++];
            }
            else if (rightI >= right.Length)
            {
                array[i] = left[leftI++];
            }
            else if (left[leftI] < right[rightI])
            {
                array[i] = right[rightI++];
            }
            else
            {
                array[i] = left[leftI++];
            }
        }

        return array;
    }

    public static int[] Merge(int[] arr)
    {
        if (arr.Length < 2)
        {
            return arr;
        }
        //从中间拆分
        int mid = arr.Length / 2;
        int[] left = new int[mid];
        int[] right = new int[arr.Length - mid];
        for(int i = 0; i < arr.Length; ++i)
        {
            if (i < mid)
            {
                left[i] = arr[i];
            }
            else
            {
                right[i - mid] = arr[i];
            }
        }

        return Sort(Merge(left), Merge(right));
    }

    static void Main(string[] args)
    {
        int[] arr = new int[] { 9, 8, 7, 6, 5 };

        arr = Merge(arr);

        for(int i = 0; i < arr.Length; ++i)
        {
            WriteLine(arr[i]);
        }
    }
}
```
