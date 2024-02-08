# **Unity常用API解析**

## 打印函数

​	print()函数与Debug.Log()函数

​	print()函数属于MonoBehaviour类

​		在其他类中无法调用print（）函数，可以使此类继承MonoBehaviour类进行调用

​	Debug是一个封装好的类，Log()是其中一个静态方法，因此可以通过类名.方法名进行调用

​	这两种打印函数常用来进行标记或者查找错误

## 生命周期函数

![生命周期函数](C:\Users\胡桃单推人\Downloads\素材，资料（api）\生命周期函数.jpg)

### Reset

​	调用时间：脚本第一次挂载到对象身上，或者执行了Reset命令之后

​	调用次数：会执行一次，并初始化所有值。

### Awake

​	在加载场景时初始化包含脚本的激活状态的GameObject时

​	GameObject首次从非激活状态转变为激活状态时，后续无论失活激活都不会调用

​	在初始化使用Instantiate创建的GameObject时

​	

​	在脚本的生存期内，Awake（）函数**只调用一次。**

​	Unity中调用每个GameObject的Awake（）函数的顺序是不一样的。可以人为设计顺序

​	Awake（）代替构造函数进行初始化，在Unity里，组件的初始化不使用构造函数



​	可以理解成在物体实例化时进行调用

### OnEnable

​	游戏物体被激活，或脚本组件被激活

​	每次激活都会调用一次

​	可以用来重复赋值，转变为初始状态	

### Start

​	物体或脚本组件被激活时调用

​	但**只会调用一次**

​	在脚本实例激活时在第一帧的Update之前调用



​	可以在Awake中加载素材，Start中实例。

![image-20220605163146078](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220605163146078.png)

### Update

![image-20220605163746835](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220605163746835.png)

### LateUpDate

​	后于Update执行

### OnDisable

​	游戏物体，脚本组件失活时，游戏物体被摧毁时。均调用一次

​	用于一些对象的状态重置，资源回收与清理

### OnApplicationQuit

![image-20220605165011919](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220605165011919.png)

### OnDestroy

![image-20220605165215214](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220605165215214.png)

## GameObject物体

### 	创建游戏物体

​		1.通过构造函数进行创建一个空的游戏对象

```c#
GameObject game =new GameObject("NewGameObject");
```

​		2.根据现有预制体（游戏物体）资源或者场景已有的物体来实例化

```c#
public GameObject game;
Instantiate(game);//静态方法
```

​		3.使用特别的API创建一些基本的游戏物体

![image-20220605171500479](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220605171500479.png)

### 物体查找

![image-20220605172244792](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220605172244792.png)

![image-20220605172653263](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220605172653263.png)

![image-20220605173008889](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220605173008889.png)

## MonoBehaviour基类

​	继承自Behaviour。Unity所用的东西来自MonoBehaviour。

## component组件

## Transform变换组件

## Vector向量和点

​	Vector2

### 	静态变量

​		![image-20220607125952151](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220607125952151.png)

### 构造函数

```c#
Vector2 vec2=new Vector2(2,2);
```

### 成员变量

​	求模长：vec2.magnitude

​	求模长平方：vec2.sqrMagnitude

​	求单位向量：vec2.normalized			vec2的值不变  vec2.Normalize()直接将vec2单位化

​	访问x，y：vec2.x     vec2.y

​	访问x，y（索引器形式）：vec2[0]     vec2[1]

### 公共函数

```c#
bool equ=vec2.Equals(new Vector2(1,1));	//比较两个向量是否相等
vec2.Normalize();	//直接将vec2单位化
vec2.Set(5,6);	//重新设置值
```

### 修改位置

```c#
trasform.position=new Vector2(3,3);
Vector2 vector2=trasform.position;
vector2.x=2;
trasform.position=vector2;
```

不能直接更改transform.position的位置。属于结构体类型。unity将其封装成了{get;set;}属性，不能只改变x或y的值。

### 静态函数

```c#
Vector2 a=new Vector2(1,0);
Vector2 b=new Vector2(0,1);
Debug.Log("a到b夹角"+Vector2.Angle(a,b));
Debug.Log("a到b距离"+Vector2.Distance(a,b));
Debug.Log("a与b的点积"+Vector2.Dot(a,b));

Vector2 a1 = new Vector2(2,1);
Vector2 b1= new Vector2(1,5);
Debug.Log(Vector2.Max(a1,b1));	//a1和b1在各个方向上最大分量组成的新向量
Debug.Log(Vector2.Min(a1, b1));	//a1和b1在各个方向上最小分量组成的新向量
Vector2.Lerp(a,b,t);	//t是插值，计算公式为a+(b-a)*t，新产生的向量只能在b-a向量的模长线段
Vector2.LerpUnclamped(a,b,-1);	//无限制插值。产生的新向量不局限于b-a
int maxDistance;
Vector2.MoveTowards(a,b,maxDistance);	//以最大步频为maxDstance从a点向b点移动
Vector2.SignedAngle(a,b);	//a和b间有符号角度（单位为度，逆时针为正方向）
Vector2.Scale(a,b)	//a和b在各个方向上的分量相乘得到的新向量
Vector2.SmoothDamp(a,b,ref c,t)	//t是时c是vector2类型。a是起始，b是结束
```

## 值类型和引用类型

​	值类型：

```c#
public struct MyStruct
{
    public int age;
}

MyStruct my=new MyStruct();
my.age=20;

MyStruct your=my;
your.age=18;

print(my.age);
print(your.age);
```

**结果：**20	18				值不一样。公共结构体为值类型。my,your相当于两块内存空间

引用类型：

```c#
public class MyClass
{
    public int age;
}

MyClass my=new MyClass();
my.age=20;

MyClass your=my;
your.age=18;

print(my.age);
print(your.age);
```

**结果：**18    18                 两个属于同一块内存。引用类型

## Input访问输入系统的接口类

## Message消息

## Animator动画事件

​	

## Time时间接口类

​	

```c#
Time.deltaTime	//完成上一帧所用的时间（秒为单位）
Time.fixedDeltaTime	//执行物理或者其他固定帧率更新的时间间隔
Time.fixedTime	//总时间，固定帧率
Time.time	//总时间
Time.realtimesSinceStartup	//游戏开始以来的实际时间
Time.smoothDeltaTime	//经过平滑处理的Time.deltaTime时间
Time.timeScale	//时间流逝刻度，可以放慢，放快动作
Time.timeSinceLevelLoad	//自加载上一个关卡以来的时间
```



## Mathf数学函数类

​	![image-20220702135531882](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220702135531882.png)

## Random生成随机数的类

​	![image-20220702142020693](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220702142020693.png)

![image-20220702154706774](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220702154706774.png)

Random.InitState(t)	t为种子数，任意一个整数。此行代码作用是在任何一台机器上都产生相同的随机数

## OnMouseEnventFunction鼠标回调事件

​	  ![image-20220705210233120](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220705210233120.png)

![image-20220705210308584](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220705210308584.png)

![image-20220705210345966](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20220705210345966.png)

## Coroutine携程

​	携程的两种表示方法:	

```c#
StartCoroutinue("test");	//1开启			开启和关闭只能采用同一种形式
StartCoroutinue(test());	//2开启

IEnumerator i=test();
StopCoroutine(i);	//2停止
StopCoroutine("test");	//1停止
StopAllCoroutines()	//停止所有携程

IEnumerator test()
{
    yield return null;	//停止一帧，null也可以换成数字。
    yield return new WaitForSecond(1)	//等待一秒
    yield return new WaitForEndOfFrame()	//在本帧末执行
    
}
```

​	携程中可以嵌套携程

​	携程内可以加入while循环

## Invoke延时调用 

​	InvokeRepeating("Test",1,2);	第一次间隔1秒，以后都是间隔2秒

​	CancelInvoke("Test");

​	IsInvoking("Test");	查看此函数是否调用，返回true或false

## Rigidbody刚体