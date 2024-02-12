# C#中级编程

## 创建属性

​	属性与字段的区别：

​	1.属性可以通过设置get与set访问器来控制是只读还是只写的

​	2.属性在访问器内还可以添加其它代码

​	3.属性可以自动实现

```c#
public class Player
{
     //成员变量可以称为
    //字段。
    private int experience;
    //Experience 是一个基本属性
    public int Experience
    {
        get
        {
            return experience;
		}
        set
        {
            experience=value;
        }
    }
    //Level 是一个将经验值自动转换为
    //玩家等级的属性
    public int Level
    {
        get
        {
            return experience/10;
		}
        set
        {
            experience=value*10;
        }
	}
    //这是一个自动实现的属性的
    //示例
    public int Health{get;set;}
}
```



## 三元运算符

​	三元运算符可以用来替代简单的if，else语句。

​	若判断结构过于复杂，使用三元运算符会导致难以理解

```c#
using UnityEngine;
using System.Collections;

public class TernaryOperator : MonoBehaviour 
{
    void Start () 
    {
        int health = 10;
        string message;

        //这是一个三元运算的示例，其中根据
        //变量“health”选择一条消息。
        message = health > 0 ? "Player is Alive" : "Player is Dead";
    }
}
```



## 静态

​	静态是在所有类的实例之间共享的，在某个对象身上改变该静态变量的值，其他对象身上该变量值也会同步变化

​	静态的东西是属于这个类本身的，而不属于实例化的对象

​	静态的变量，方法。可以直接通过类名点的方式使用，而不用先实例化该类的对象

​    静态方法无法访问非静态成员变量。

```c#
using UnityEngine;
using System.Collections;

public class Enemy
{
    //静态变量是在类的所有实例之间
    //共享的变量。
    public static int enemyCount = 0;

    public Enemy()
    {
        //通过递增静态变量了解
        //已创建此类的多少个对象。
        enemyCount++;
    }
}
```



## 方法重载

​	

```c#
using UnityEngine;
using System.Collections;

public class SomeClass
{
    //第一个 Add 方法的签名为
    //“Add(int, int)”。该签名必须具有唯一性。
    public int Add(int num1, int num2)
    {
        return num1 + num2;
    }

    //第二个 Add 方法的签名为
    //“Add(string, string)”。同样，该签名必须具有唯一性。
    public string Add(string str1, string str2)
    {
        return str1 + str2;
    }
}
```

```c#
using UnityEngine;
using System.Collections;

public class SomeOtherClass : MonoBehaviour 
{
    void Start () 
    {
        SomeClass myClass = new SomeClass();

        //具体调用的 Add 方法将取决于
        //传入的参数。
        myClass.Add (1, 2);
        myClass.Add ("Hello ", "World");
    }
}
```



## 泛型

```c#
using UnityEngine;
using System.Collections;

public class SomeClass 
{
    //这是一个通用方法。注意通用
    //类型“T”。该“T”将在运行时替换为
    //实际类型。
    public T GenericMethod<T>(T param)
    {
        return param;
    }
}
```

```c#
using UnityEngine;
using System.Collections;

//这是一个通用类。注意通用类型“T”。
//“T”将被替换为实际类型，同样，
//该类中使用的“T”类型实例也将被替换。
public class GenericClass <T>
{
    T item;

    public void UpdateItem(T newItem)
    {
        item = newItem;
    }
}
```

```c#
using UnityEngine;
using System.Collections;

public class SomeOtherClass : MonoBehaviour 
{
    void Start () 
    {
        SomeClass myClass = new SomeClass();

        //为了使用此方法，必须
        //告诉此方法用什么类型替换
        //“T”。
        myClass.GenericMethod<int>(5);
        
        //为了创建通用类的对象，必须
        //指定希望该类具有的类型。
        GenericClass<int> myClass = new GenericClass<int>();

        myClass.UpdateItem(5);
    }
}

```



## 继承

​	注意子类会优先调用父类的构造函数。如果想要指定调用的构造函数，需要在子类构造函数后面添加	base

```c#

    //这是 Fruit 类的第二个构造函数，
    //不会被任何派生类继承。
    public Fruit(string newColor)
    {
        color = newColor;
        Debug.Log("2nd Fruit Constructor Called");
    }

```



```c#
 //这是 Apple 类的第二个构造函数。
 //它使用“base”关键字指定
 //要调用哪个父构造函数。
public Apple(string newColor) : base(newColor)
    {
        //请注意，该构造函数不会设置 color，
        //因为基构造函数会设置作为参数
        //传递的 color。
        Debug.Log("2nd Apple Constructor Called");
    }
```



## 多态

​	向上转换：将子类转换成父类对象

​		转化后的父类对象具有父类所有方法，若方法被子类重写override，那么实际调用时，调用的是重	写后的实现。

​		子类实现抽象类，调用方法时，自动找到子类同名方法，执行子类同名方法。向上转型时，转型后	的对象只具有父类方法和子类对父类重新（实现）的方法。

​	向下转换：将父类转换成子类对象

​	**继承自非抽象类：**

```c#
    class Animal
    {
        public void call() { Console.WriteLine("无声的叫唤"); }
    }
    class Dog : Animal
    {
        // new的作用是隐藏父类的同名方法
        public new void call() { Console.WriteLine("叫声：汪～汪～汪～"); }  	
        public void smell() { Console.WriteLine("嗅觉相当不错！"); }
    }

            Animal animal = new Dog();             //向上转型①
 
            animal.call();   //打印出“无声的叫唤”，调用的是父类本身的call方法。
 
            // animal.smell();        //出错，不存在此方法。
```

```c#
Dog dog = (Dog)animal;                      //向下转型②
dog.call();                 //打印出“叫声：汪～汪～汪～”，即调用的是Dog类的call方法。
dog.smell();                //打印出“嗅觉相当不错！”，即子类方法可用。
```

**继承自抽象类**：

```c#
    abstract class Animal_a
    {
        public abstract void call();
    }
    class Dog_a : Animal_a
    {
        public override void call() { Console.WriteLine("叫声：汪～汪～汪～"); }
        public void smell() { Console.WriteLine( "嗅觉相当不错！"); }
    }
Animal_a animal_a = new Dog_a();               //向上转型
animal_a.call();      //打印出“叫声：汪～汪～汪～”
// animal_a.smell();  //不存在此方法
```

​	关于多态，菜鸟上边的多态文章讲的比较详细

​	[C# 多态性 | 菜鸟教程 (runoob.com)](https://www.runoob.com/csharp/csharp-polymorphism.html)

## 成员隐藏

​	有点向上转换的意思。

​	另外如果想要某个类不被继承，就在类定义前面放置关键字 **sealed**，可以将类声明为**密封类**。当一个	类被声明为 **sealed** 时，它不能被继承。抽象类不能被声明为 sealed。

```c#
public class Humanoid
{
    //Yell 方法的基版本
    public void Yell()
    {
        Debug.Log ("Humanoid version of the Yell() method");
    }
}
```

```c#
public class Enemy : Humanoid
{
    //这会隐藏 Humanoid 版本。
    new public void Yell()
    {
        Debug.Log ("Enemy version of the Yell() method");
    }
}
```

```c#

public class WarBand : MonoBehaviour 
{
    void Start () 
    {
        Humanoid human = new Humanoid();
        Humanoid enemy = new Enemy();        

        //注意每个 Humanoid 变量如何包含
        //对继承层级视图中
        //不同类的引用，但每个变量都
        //调用 Humanoid Yell() 方法。
        human.Yell();
        enemy.Yell();
    }
}
```



## 覆盖

​	

```c#
using UnityEngine;
using System.Collections;

public class Fruit 
{
    public Fruit ()
    {
        Debug.Log("1st Fruit Constructor Called");
    }

    //这些方法是虚方法，因此可以在子类中
    //将它们覆盖
    public virtual void Chop ()
    {
        Debug.Log("The fruit has been chopped.");        
    }

    public virtual void SayHello ()
    {
        Debug.Log("Hello, I am a fruit.");
    }
}

```

```c#
using UnityEngine;
using System.Collections;

public class Apple : Fruit 
{
    public Apple ()
    {
        Debug.Log("1st Apple Constructor Called");
    }

    //这些方法是覆盖方法，因此
    //可以覆盖父类中的任何
    //虚方法。
    public override void Chop ()
    {
        base.Chop();
        Debug.Log("The apple has been chopped.");        
    }

    public override void SayHello ()
    {
        base.SayHello();
        Debug.Log("Hello, I am an apple.");
    }
}

```

```c#
using UnityEngine;
using System.Collections;

public class FruitSalad : MonoBehaviour 
{    
    void Start () 
    {
        Apple myApple = new Apple();

        //请注意，Apple 版本的方法
        //将覆盖 Fruit 版本。另外请注意，
        //由于 Apple 版本使用“base”关键字
        //来调用 Fruit 版本，因此两者都被调用。
        myApple.SayHello();
        myApple.Chop();    

        //“覆盖”在多态情况下也很有用。
        //由于 Fruit 类的方法是“虚”的，
        //而 Apple 类的方法是“覆盖”的，因此
        //当我们将 Apple 向上转换为 Fruit 时，
        //将使用 Apple 版本的方法。
        Fruit myFruit = new Apple();
        myFruit.SayHello();
        myFruit.Chop();
    }
}

```



## 接口

​	一般来说，一个脚本里就写一个接口

​	此外，可以用泛型和接口结合，来传输任意数据类型的变量，等等用法

​	接口与继承相比，一个类可以有多个继承，而一个类只能有一个基类

​	接口里的方法不能有实现，可以起到一个协议的作用

```c#
using UnityEngine;
using System.Collections;

//这是只有一个必需方法的基本
//接口。
public interface IKillable
{
    void Kill();
}

//这是一个通用接口，其中 T 是
//将由实现类提供的数据类型的
//占位符。
public interface IDamageable<T>
{
    void Damage(T damageTaken);
}
```

```c#
using UnityEngine;
using System.Collections;

public class Avatar : MonoBehaviour, IKillable, IDamageable<float>
{
    //IKillable 接口的必需方法
    public void Kill()
    {
        //执行一些有趣操作
    }

    //IDamageable 接口的必需方法
    public void Damage(float damageTaken)
    {
        //执行一些有趣操作
    }
}
```



## 扩展方法

​	

```c#
using UnityEngine;
using System.Collections;

//创建一个包含所有扩展方法的类
//是很常见的做法。此类必须是静态类。
public static class ExtensionMethods
{
    //扩展方法即使像普通方法一样使用，
    //也必须声明为静态。请注意，第一个
    //参数具有“this”关键字，后跟一个 Transform
    //变量。此变量表示扩展方法会成为
    //哪个类的一部分。
    public static void ResetTransformation(this Transform trans)
    {
        trans.position = Vector3.zero;
        trans.localRotation = Quaternion.identity;
        trans.localScale = new Vector3(1, 1, 1);
    }
}

```

```c#
using UnityEngine;
using System.Collections;

//创建一个包含所有扩展方法的类
//是很常见的做法。此类必须是静态类。
public static class ExtensionMethods
{
    //扩展方法即使像普通方法一样使用，
    //也必须声明为静态。请注意，第一个
    //参数具有“this”关键字，后跟一个 Transform
    //变量。此变量表示扩展方法会成为
    //哪个类的一部分。
    public static void ResetTransformation(this Transform trans)
    {
        trans.position = Vector3.zero;
        trans.localRotation = Quaternion.identity;
        trans.localScale = new Vector3(1, 1, 1);
    }
}

```



## 命名空间

​	1.通过using引用命名空间

​	2.通过**命名空间.**的形式访问

​	3.通过在想要访问的类上直接加上namespace 命名空间（一般不推荐，除非此类也想放入命名空间当	中）

​	通过namespace 来定义命名空间

​	命名空间可以嵌套

​	

```c#
using UnityEngine;
using System.Collections;

namespace SampleNamespace
{
    public class SomeClass : MonoBehaviour 
    {
        void Start () 
        {

        }
    }
}
```



## 列表和字典

列表中sort插入，removeall移除一个元素。

```c#
using UnityEngine;
using System.Collections;
using System; //这允许 IComparable 接口

//这是您将存储在
//不同集合中的类。为了使用
//集合的 Sort() 方法，此类需要
//实现 IComparable 接口。
public class BadGuy : IComparable<BadGuy>
{
    public string name;
    public int power;

    public BadGuy(string newName, int newPower)
    {
        name = newName;
        power = newPower;
    }

    //IComparable 接口需要
    //此方法。
    public int CompareTo(BadGuy other)
    {
        if(other == null)
        {
            return 1;
        }

        //返回力量差异。
        return power - other.power;
    }
}
```

```c#
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class SomeClass : MonoBehaviour
{
    void Start () 
    {
        //这是创建列表的方式。注意如何在
        //尖括号 (< >) 中指定类型。
        List<BadGuy> badguys = new List<BadGuy>();

        //这里将 3 个 BadGuy 添加到列表
        badguys.Add( new BadGuy("Harvey", 50));
        badguys.Add( new BadGuy("Magneto", 100));
        badguys.Add( new BadGuy("Pip", 5));

        badguys.Sort();

        foreach(BadGuy guy in badguys)
        {
            print (guy.name + " " + guy.power);
        }

        //这会清除列表，使其
        //为空。
        badguys.Clear();
    }
}

```

```c#
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class SomeOtherClass : MonoBehaviour 
{
    void Start ()
    {
        //这是创建字典的方式。注意这是如何采用
        //两个通用术语的。在此情况中，您将使用字符串和
        //BadGuy 作为两个值。
        Dictionary<string, BadGuy> badguys = new Dictionary<string, BadGuy>();

        BadGuy bg1 = new BadGuy("Harvey", 50);
        BadGuy bg2 = new BadGuy("Magneto", 100);

        //可以使用 Add() 方法将变量
        //放入字典中。
        badguys.Add("gangster", bg1);
        badguys.Add("mutant", bg2);

        BadGuy magneto = badguys["mutant"];

        BadGuy temp = null;

        //这是一种访问字典中值的更安全
        //但缓慢的方法。
        if(badguys.TryGetValue("birds", out temp))
        {
            //成功！
        }
        else
        {
            //失败！
        }
    }
}
```



## 协程

​	![image-20221102170827974](C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20221102170827974.png)

### 1.协程的使用

​	(1) 循环在协程中的使用

```c#
    private IEnumerator TestIE()
    {
        for (int i = 0; i < 3; i++)
        {
            Debug.Log("i:" + i);
            yield return i;	//终止该协程并在下一帧执行操作
        }

    }
//如果在协程内写入一个死循环，将会卡死进程
```

​	(2) 协程等待下载

​	(3) 协程实现异步加载

### 2.IEnumerator + yield 强大的迭代器

​	(1) IEnumerator: 是一个函数对象的容器；[函数代码1，函数代码2，...]

​		<img src="C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20221102172230810.png" alt="image-20221102172230810" style="zoom:67%;" />	

​		注：*MoveNext() 负责移动到容器里下一个函数代码块，并返回一个bool值，Current可以显示出				return的值*

​	(2) yield: 关键字，可以抽出函数代码，生成函数，放入IEnumerator容器里边；

​		<img src="C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20221102171902529.png" alt="image-20221102171902529" style="zoom: 50%;" />

​	(3) IEnumerator 依次执行容器内的函数；

​		<img src="C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20221102172119488.png" alt="image-20221102172119488" style="zoom:50%;" />

​		<img src="C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20221102172007056.png" alt="image-20221102172007056" style="zoom:67%;" />

​		ie 容器中会含有提取出的两个函数代码块

### 3.模拟协程的实现过程

​	（1）例如：yield return null; 每隔一帧执行一次

​	<img src="C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20221102175932775.png" alt="image-20221102175932775" style="zoom:50%;" />

​	（2）yield return new WaitForSecond(5);	本质还是一个计时器

​		<img src="C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20221102180246130.png" alt="image-20221102180246130" style="zoom:50%;" />

​		<img src="C:\Users\胡桃单推人\AppData\Roaming\Typora\typora-user-images\image-20221102180053587.png" alt="image-20221102180053587" style="zoom:50%;" />

## 四元数

## 委托

## 特性

## 事件