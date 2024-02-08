# c#入门

## 变量类型

1字节=8比特=8位 例：1001 0100

无符号 正整数集 ulong(64位) uint(32位) ushort(16位) byte(8位)

有符号 整数集 long(64位) int(32位) short(16位) sbyte(8位)

浮点数 小数 decimal(128位) double(64位)  float(32位) 

特殊字符 字符串 字符 string(无位数限制) char(16位) bool(8位)

声明常量 变量前加“const”关键字

const float PI = 3.14159265f;

## 转义字符 

```c#
//在字符串中使用 "" '' \ 时要使用转义字符"\"来标识
//使用 "\n" 来完成字符串换行
//或者在字符串前加上"@"来取消转义字符"\"
Console.WriteLine("\"哈哈哈\""); //打印结果为："哈哈哈"
Console.WriteLine(@"哈\哈哈"); //打印结果为："哈\哈哈"
```

## 数据类型转换

### 隐式转换

小范围可以隐式转换大范围

![1829a42d425612adad7fbb1bbfab56e.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/25552567/1655378202640-959f0806-0c66-4ef9-ae68-bd561b72899e.jpeg)



char可以隐式转换成整数和浮点，转换结果为Ascii码。

### 显式转换 

#### 括号强转

```c#
//显示转换 括号强转 高精度转低精度
//（整数可能会出现范围超出 浮点会出现精度问题）
// 格式： 变量 =（转换的类型）变量
int i = 1;
short s = 1;
s = (short)i;

byte b = 1;
uint ui = 1;
b = (byte)ui;

//浮点强转整形时会抛弃小数点后的数 无四舍五入
float f = 18.789f;
i = (int)f;
Console.WriteLine(i);
```

#### Parse法

```c#
//目标变量.Parse(“字符串”)；
string str1 = "123";
int i2 = int.Parse(str1);   //字符串转对应类型 注意值的范围
i2 += 100;                  //可以进行数学计算
Console.WriteLine(i2);

short s3 = (short)int.Parse("4000000"); // 字符串“4000000”转int后转short
Console.WriteLine(s3);
//打印结果为 2304 因为short范围问题
```

字符串转对应类型

#### Convert法

适用性强 什么都能转

```c#
//更准确的将各类型之间转换
//Convert.To对应类型(变量)

//转字符串 
int a = Convert.ToInt32("12");
string str2 = Convert.ToString(321);

//浮点数转换 精度更高四舍五入 
a = Convert.ToInt32(3.6789f);
Console.WriteLine(a);
//打印结果为 4
float f1 = Convert.ToSingle("13.567");
//打印结果为 13.567
Console.WriteLine(a);
```

#### 其它类型转String

拼接打印

```c#
// 作用 拼接打印
// 语法 变量.tostring();
string str3 = 9888.ToString();
Console.WriteLine(str3);
//打印结果为9888
int a4 = 1 ;
str3 = a4.ToString();
bool b1 = true;
str3 = b1.ToString();
Console.WriteLine(str3);
//打印结果为true

//调用打印时 进行拼接时 字符串 + 变量 会默认转换
Console.WriteLine("数字是：" + a4 + b1);
//上下效果相同
Console.WriteLine("数字是：" + a4.ToString() + b1.ToString());
```

## 异常捕获

![b78e4fee0516a944f26c5fe50cc25bd.png](https://cdn.nlark.com/yuque/0/2022/png/25552567/1655689182460-5de19ecd-62b0-461b-b332-af10d3a63bb0.png)



```c#
try
{
    string str = Console.ReadLine();
    int.Parse(str);
}
catch
{
    Console.WriteLine("请输入合法数字");
}
```

## 自增运算符

![419482fad0cb86bde74410b1137d2e2.png](https://cdn.nlark.com/yuque/0/2022/png/25552567/1655690363660-31ed3ab2-3fc9-4323-a9db-33475e3115af.png?x-oss-process=image%2Fresize%2Cw_462%2Climit_0)

## 逻辑运算符的短路规则

与：&&   有假则假 同真为真

或：||  有真为真 同假为假

非：！   反转真假

运算优先级： ! > && > ||



i3打印结果为1

![b9af060679f6f1a2d46185d34ec8c6d.png](https://cdn.nlark.com/yuque/0/2022/png/25552567/1655714715943-a44ca710-030b-4ae8-842c-01307c85297a.png?x-oss-process=image%2Fresize%2Cw_439%2Climit_0)

## 位运算符

![23c4da148546d6f24aa6c8af2b9f855.png](https://cdn.nlark.com/yuque/0/2022/png/25552567/1655716508185-407871b2-ffbc-4fc1-bf42-923bab950325.png)

![96314a0dadc51c22c36c34ef9cd2921.png](https://cdn.nlark.com/yuque/0/2022/png/25552567/1655716510749-f4c435a8-f7f4-49a9-afb6-6bd87225fc79.png)

![3be9bcb1c4464534de49d2c499eede3.png](https://cdn.nlark.com/yuque/0/2022/png/25552567/1655716513078-52e0b505-cc51-4ae3-a45d-600058189046.png)

![f763bac3287b655c9f7d3312e055761.png](https://cdn.nlark.com/yuque/0/2022/png/25552567/1655717846632-6905aa3a-f46a-4e31-a091-9b25afa315c3.png)

## 三目运算符

可以实现简单的逻辑运算   if和else 青春版

![a3b31d6cbd6ddd0f8493139937930fd.png](https://cdn.nlark.com/yuque/0/2022/png/25552567/1655719464221-f9af3d6d-4dbe-4299-9b75-c29d854de87e.png)

## 分支语句

### if

```c#
//三种写法
if (bool值)
{
    如果为真则执行此语句块的代码
}

//——————————————————————————————

if (bool值)
{
    如果为真则执行此语句块的代码
}


else (bool值)
{
    否则执行此段语句块代码
}

//————————————————————————————————

if (bool值)
{
    如果为真则执行此语句块的代码
}
else if (bool值)
{
    否则执行此段语句块代码
}
else if (bool值)
{
    否则执行此段语句块代码
}
//..........可以 else if 下去
else (bool值)
{
    否则执行此段语句块代码
}

//————————————————————————————————————
//if嵌套
if (bool值)
{
    if(bool值)
    {
    
    }  
    else if (bool值)
    {
    
    }    

}
else if (bool值)
{

} 

//if扩展写法
if(!bool值)
{
    如果为真则执行此语句块的代码
}
```



### switch

用于枚举

```c#
switch (变量)
{
    //变量==常量时执行case和break之间的代码     
    case 常量:    //常量，只能写固定的值 不能写范围 逻辑运算符等
        满足条件所执行的代码
        break;
        
    case 常量:
        满足条件所执行的代码
        break;
        
    case 常量:
        满足条件所执行的代码
        break;    
    
    default:
        如果上面的条件都不满足 就会执行default
        break;
}    
//-------------------------------------------------
switch (变量)
{   
    //贯穿 变量 == 常量1 || 常量2  时执行break前的代码
    case 常量1:  
    case 常量2:
        满足条件所执行的代码
        break;    
    
    default:
        如果上面的条件都不满足 就会执行default
        break;

//——————————————————————————————————————————————————————————————————
int a = 0;
switch
{
    case 1:
        Console.WriteLine("a为1")
        break;
        
    case 2:
        Console.WriteLine("a为2")
        break;  
        
    case 3:
        Console.WriteLine("a为3")
        break; 
        
    default:
        Console.WriteLine("a不满足条件")
        break;
}
```

## 循环语句 

### for

使用较多

![caadddb782e31d1939c0ac5407fefaa.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/25552567/1655808783235-f7853137-30cc-4f1b-9969-0ca847122a6c.jpeg)

```c#
//常用在计数循环
for(int i =0; i<10; ++i)
{
    Console.WriteLine(i); 
}

//————————————————————————
//死循环
for(;;)
{
    Console.WriteLine("你好"); 
}

```

### while

```c#
while(bool值)
{
    满足条件时就会执行该语句块
    执行玩会回到while开头再次判断    
}    

//-------------------------------------

//累加i到10
int i = 0 ;
while(i<10)
{
    ++i;
}
```



### do while

```c#
do
{
   判断条件在后面 所以先执行一次本语句块代码再做判断

}while(bool值);
```

用的比较少

### break

```c#
while(true)
{
    Console.WriteLine("break之前的代码");  
    break;
    Console.WriteLine("break之后的代码 不会被执行");  
}    
Console.WriteLine("循环外的代码"); 

//----------------------------------------------

//累加i到10
int i = 0 ;
while(true)
{
    ++i;
    if(i==10)
    {
        break; //这个时while的break
    }
}

```

### continue

```c#
int i = 0 ;
while(true)
{
    Console.WriteLine("continue之前的代码"); 
    continue;
    Console.WriteLine("continue之后的代码不会执行"); 
}
Console.WriteLine("循环外的代码不会执行"); 

```

## 练习

https://github.com/suxiaocong2020/c-

## 补充内容

```c#
//输入一个键并赋值
char c = Console.ReadKey().KeyChar;
//输入一个键并赋值 且不在控制台显示
char c = Console.ReadKey(true).KeyChar;
//清除控制台
Console.Clear();
//设置光标位置 控制台左上角为0，0 右侧是x正方向 下方是y正方向
Console.SetCursorPosition(10,5);
//设置文字颜色
Console.ForegroundColor = ConsoleColor.Red;
//设置背景色 配合Clear使用 填充窗口颜色
Console.BackgroundColor = ConsoleColor.White;
Console.Clear();
//光标显隐
Console.CursorVisible = false;
//关闭控制台
Environment.Exit(0);
```