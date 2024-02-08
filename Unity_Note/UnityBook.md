# Unity基础知识和概念

## 工程文件夹

1.Assets：工程资源文件夹（美术资源，脚本等等）

2.Library：库文件夹（Unity自动生成管理）

3.Logs：日志文件夹，记录特殊信息（Unity自动生成管理）  

4.obj：编译产生中间文件（Unity自动生成管理）

5.Packages：包配置信息（Unity自动生成管理）

6.ProjectSettings：工程设置信息，删除再生成原先设置会丢失（Unity自动生成管理）

## 资源类型

图片格式：jpg、png、tga

模型格式：fbx、max、maya

音效：wav、mp3、ogg

文本：txt、json、bytes

视频：mp4 

## Unity的工作机制

本质就是利用反射，动态地创建GameObject对象并且关联各种C#脚本对象在其上让不同的GameObject对象各司其职扮演好自己的角色，根据Unity提供以及我们书写的脚本，实现所需的游戏内容。

GameObject和Transform是必不可缺少的关键因素

游戏场景文件后缀为.unity 本质是一个配置文件，Unity有自己的一套识别处理机制。但是本质是把场景对象相关信息读取出来，通过反射来创建各个对象关联各个脚本对象

# Unity脚本基础知识

在Unity中打印信息的两种方式

1.没有继承MonoBehaviour类的时候

```c#
Debug.Log("***");//普通控制台打印
Debug.LogError("");//打印一个错误
Debug.LogWarning("");//打印一个警告
```

2.继承了MonoBehaviour，有一个直接使用的方法

```c#
print("");//直接进行打印，方便进行调试，但是没有警告错误的分类
```

**Unity里的Object，不是指的万物之父object；Unity里的Object，命名空间在UnityEngine中的Object类，也是继承万物之父的一个自定义类；C#中的Object，命名空间是在System中的。**

补充知识，Unity有调试画线的功能，直接在脚本中写

```c#
//参数一为起始位置，参数二为目标位置，参数三是给线加一个颜色，方便我们调试
//此方法为调试画线
Debug.DrawLine(this.transform.position, this.transform.forward + this.transform.position + new Vector3(10,10,10), Color.red);

//参数一为起点，参数二为方向，参数三为这条线加一个颜色，方便调试
//此方法为发射一条射线，但不是无限延长的射线
Debug.DrawRay(this.transform.position, this.transform.forward, Color.green);
```

## MonoBehaviour基类

1.创建的脚本默认都继承MonoBehaviour，继承了这个基类才能够挂载在GameObject上

2.继承了MonoBehaviour的脚本不能new进行实例化，只能挂载在对象上。

3.继承了MonoBehaviour的脚本不要去写构造函数，因为不能new进行实例化，所以构造函数没有意义。

4.继承了MonoBehaviour的脚本可以在一个对象上挂多个（如果没有加DisallowMultipleComponent特性）。

5.继承MonoBehaviour的类也可以再次被继承，**遵循面向对象**的基本特性。

不继承MonoBehaviour的类：

​	1.不继承Mono的类，不能挂载在GameObject上。

​	2.不继承Mono的类，使用需要自己new。

​	3.不继承Mono的类，一般是单例模式的类（用于管理模块）或者数据结构类（用于存储数据）

​	4.不继承Mono的类，不用保留默认的生命周期函数。

###默认脚本内容，保存在Editor\Data\Resources\ScriptTemplates

## 生命周期函数及帧知识

### 帧的概念

游戏的本质就是一个死循环，每一次循环处理游戏逻辑就会更新一次画面，之所以能看到画面在动是因为切换画面的速度到达一定时人眼就认为画面是流畅的，一帧就是执行一次循环。

fps(Frames Per Second)：即每秒钟帧数，一般60帧、30帧意思是1秒更新60次、30次画面 1s = 1000ms。人眼舒适放松时可视帧数是每秒24帧。

游戏卡顿的原因：跑1帧游戏逻辑中的计算量过大，或者CPU不给力，不能在1帧的时间内处理完所有游戏逻辑。

### 生命周期函数的概念

所有继承MonoBehaviour的脚本，最终都会挂载到GameObject游戏对象上，生命周期函数就是该脚本对象依附的GameObject对象从出生到消亡整个生命周期中会通过反射自动调用的一些特殊函数。

Unity帮助我们记录了一个GameObject对象依附了哪些脚本，会自动的得到这些对象，通过反射去执行一些固定名字的函数。

### 生命周期函数

生命周期函数的访问修饰符一般为private和protected，因为不需要在外部自己调用生命周期函数，都是Unity自己帮助我们调用的。

### 常用的生命周期函数

Awake：出生时调用，类似于构造函数，一个对象（自己这个类对象，即脚本对象）只会在生成时调用一次。

OnEnable：依附的GameObject对象每次激活时调用。创建时也调用

Start：从自己被创建出来后，第一次帧更新之前调用，一个对象只会调用一次。

FixedUpdate：物理帧更新，固定间隔时间执行，间隔时间可以设置。主要用于进行物理更新，是每一帧的执行，不同于逻辑帧，这个时间间隔可以在project setting中的Time去设置。

Update：逻辑帧更新，每帧执行。主要用于处理游戏核心逻辑更新的函数。

LateUpdate：每帧执行，于Update之后执行。这个更新一般是用来处理摄像机位置更新内容的，因为在Update和LateUpdate之间Unity进行了一些处理，处理我们动画相关的更新。

OnDisable：依附的GameObject对象每次失活时调用。

OnDestory：对象销毁时调用，依附的GameObject对象被删除时。先调用OnDisable

生命周期函数也遵循继承多态。

如果不打算在生命周期函数中写逻辑，那就不要写这些生命周期函数，因为会造成额外开销。

如果依附的物体失活，生命周期函数不会执行

## Inspector显示的可编辑内容

Inspector显示的可编辑内容就是脚本的成员变量

1.私有的和保护的变量无法在可视化界面（引擎中）显示编辑

2.让私有的和保护的变量也可以被显示：

​	加上强制序列化字段特性[SerializeField];所谓序列化就是把一个对象保存到一个文件或数据库字段中去。

3.公共的可以显示编辑

4.公共的也不让其显示编辑：

​	在变量前加上特性[HideInInspector]

5.大部分默认类型都支持显示编辑，字典和自定义类型等变量（枚举，类等）不支持

6.让自定义类型可以被访问：

​	加上序列化特性[System.Serializable]；字典怎样都不行，但是自定义类型可以

7.一些辅助特性：

1. 分组说明特性Header，为成员分组，写法[Header("分组说明")]；

2. 悬停注释Tooltip，为变量添加说明，写法[Tooltip("说明内容")]；

3. 间隔特性Space()，为两个字段间出现8  v   ，写法[Space()]；

4. 修饰数值的滑条范围Range，为变量值添加范围，写法[Range(min, max)]；

5. 多行显示字符串，默认不写参数显示3行，写参数就是对应行，写法[Multiline(x)]；

   不用的话，inspector面板字符串输入框默认只显示一行

6. 滚动条显示字符串，默认不写参数就是超过3行显示滚动条，写法为[TextArea(a,b)]，最少显示a行，最多显示b行，超过b行就显示滚动条；

7. 为变量添加快捷方法，写法[ContextMenuItem("显示按钮名","方法名")]，方法中不能有参数；

   在inspector面板上右键变量名，会弹出按钮，点击就执行方法

8. 为方法添加特性能够在Inspector中执行，写法[ContextMenu("测试函数")]

   点击inspector脚本右边三个点，最下边就是新增的方法，点击执行。运行时无意义

注意：

​	1.Inspector窗口中的变量关联的就是对象的成员变量，运行时改变他们就是在改变成员变量；

​	2.拖曳到GameObject对象后，再改变脚本中变量默认值，可视化界面上不会改变；

​	3.运行中修改的信息不会保存

## MonoBehaviour的内容

1.重要成员：

1. 获取依附的GameObject					this.gameObject
2. 获取依附的GameObject的位置信息						transform.position/eulerAngles/lossyScale
3. 获取脚本是否激活		this.enable

2.重要方法：

​	得到依附对象上挂载的其它脚本	只要能道德场景中别的对象或者对象依附的脚本，那么就能获取到它的所有信息

1. 得到自己挂载的单个脚本
   1. 根据脚本名获取		this.GetComponent("脚本名")	这个获取脚本的方法，如果获取失败，就是没有对应的脚本，会默认返回空
   2. 根据Type获取           this.GetComponent(typeof(脚本类型))
   3. 根据泛型获取			this.GetComponent<脚本类型>()
2. 得到自己挂载的多个脚本					this.GetComponents<脚本类型>()
3. 得到**子对象**挂载的脚本（它默认会找自己身上是否挂载要寻找的脚本）；函数是有一个参数的，默认不传就是false，意思是，如果子对象失活，那么就不会去找这个对象上是否有某个脚本的，如果传true，即使失活也会去找
   1. 得到子对象的单个挂载脚本		this.GetComponentInChildren<脚本类型>()
   2. 得到子对象的多个挂载脚本		this.GetComponentsInChildren<脚本类型>()声明数组存储或者先声明列表写成如下形式：this.GetComponentsInChildren<脚本类型>(true,列表名)
4. 得到**父对象**挂载的脚本（它默认也会找自己身上是否挂载要寻找的脚本）；写法：this.GetComponentInParent<脚本类型>()同样分为可以得到单个脚本或多个脚本，与得到子类对象脚本同理。父对象失活，子对象一定失活
5. 尝试获取脚本			写法this.TryGetComponent<脚本类型>(),相当于提供了一个更安全的，获取单个脚本的方法，如果得到了，会返回true，然后再进行逻辑处理即可。

# Unity重要组件和API

## GameObject中的成员变量

1.GameObject中的成员变量

1. 名字，this.gameObject.name，得到的是一个字符串
2. 是否激活，this.gameObject.activeSelf，得到的是一个bool
3. 是否是静态，this.gameObject.isStatic，得到的是一个bool
4. 层级，this.gameObject.layer，得到的是一个int
5. 标签，this.gameObject.tag，得到的是一个字符串
6. 位置信息，this.gameObject.transform.position，得到一个三维数据

2.GameObject中的静态方法

1. 创建自带几何体，只要得到了一个GameObject对象，就可以得到它身上挂载的所有脚本信息，使用Component方法来得到GameObject.CreatePrimitive(PritiveType.Cube/...)
2. 查找对象相关的知识
   1. 查找单个对象
      1. 通过对象名查找，这个查找效率比较低下，因为会在场景中的所有对象去查找，没有找到就会返回null，写法：GameObject.Find("对象名字")
      2. 通过tag来查找对象，写法：GameObject.FindWithTag("标签名")/GameObject.FindGameObjectWithTag("标签名")
      3. 两种找单个对象方法的共同点：1.无法找到失活的对象；2.如果场景中存在多个满足条件的对象，无法准确知道找到的是谁
   2. 查找多个对象 找多个对象的API，只能是通过tag去找，同样是只能找到激活的对象
      1. 通过tag找到多个对象，写法：GameObject.FindGameObjectsWithTag("标签名")，使用一个GameObject数组来储存找到的结果
   3. 另外的查找对象的方法，较少用，是GameObject父类Object提供的方法
      1. 它可以找到场景中挂载的某一个脚本对象，写法：GameObject.FindObjectOfType<脚本类型>(),效率更低；之前的GameObject.Find和通过FindWithTag找，只是遍历对象，这个方法不仅要遍历对象，还要遍历对象上挂载的脚本
3. 实例化对象（克隆对象）的方法，作用是根据一个GameObject对象，创建出一个和它一模一样的对象，写法：GameObject.Instantiate(对象)，如果继承了MonoBehaviour，可以不用写GameObject，也同样可以使用该方法，因为这个方法是Unity中的Object基类提供的
4. 删除对象的方法
   1. GameObject.Destroy(对象，int参数表示延迟几秒执行，可省略)，作用是删除一个指定的游戏对象也同样可以删除脚本对象；这个方法，不会马上移除对象，只是给这个对象加了一个移除标记，一般情况下，会在下一帧时把这个对象移除并从内存中移除，是异步的一个方法，降低卡顿的几率。
   2. GameObject.DestroyImmediate(对象)，这个是同步的方法，立刻移除一个对象。
5. 过场景不移除，默认情况下，切换场景时，场景中对象都会被自动删除掉，如果希望某个对象，过场景不被移除，就使用此方法，写法：GameObject.DontDestroyOnLoad(对象)

3.GameObject中的成员方法

1. 创建空物体
   1. GameObject 对象名 = new GameObject();new一个GameObject就是在创建一个空物体
   2. GameObject 对象名 = new GameObject("加入一个字符串表示改名字");
   3. GameObject 对象名 = new GameObject("加入字符串，并且还可以添加脚本",typeof(脚本对象)为变长参数，可以继续添加...);
2. 为对象添加脚本
   1. 继承MonoBehaviour的脚本，不能够new，如果想要动态地添加继承MonoBehaviour的脚本，在某一对象上，直接使用GameObject提供的方法即可。
   2. GameObject对象.AddComponent(typeof(脚本对象))/GameObject对象.AddComponent<脚本对象>()  动态添加脚本 第一种方法如果有变量接收还可能需要类型转换
   3. 通过返回值，可以得到加入的脚本信息，来进行一些处理
3. 得到脚本的方法，和继承Mono的类得到脚本的方法一模一样
4. 标签比较
   1. gameObject.CompareTag("标签名")，返回是一个bool
   2. gameObject.tag == "标签名"，返回同样是一个bool
5. 设置激活失活，GameObject对象.SetActive(bool)，true就是激活，false就是失活。
6. 一些方法，建议了解，效率较低
   1. 通过广播或者发送信息的形式，让自己或者别人，执行某些行为方法，
      1. 发送消息的行为，gameObject.SendMessage("方法名"，object类型参数，如果无参，可省略，有参则可传任意类型参数)，会让自己或别人在自身挂载的所有脚本中去找传入名字的函数，然后执行，效率较低
      2. 广播行为，gameObject.BroadcastMessage("函数名"，参数)，让自己和自己的子对象执行
      3. gameObject.SendMessageUpwards("函数名"，参数)，向自己和父对象发送消息，并执行

总结：GameObject的常用内容：

1. 基本成员变量：名字、失活激活状态、标签、层级等等。
2. 静态方法相关：创建自带几何体、查找场景中对象、实例化对象、删除对象、过场景不移除
3. 成员方法：为对象动态添加指定脚本、设置失活激活状态、和Mono中相同的，得到脚本相关的方法

## 时间相关Time

时间相关内容，主要用于游戏中参与位移、计时、时间暂停等

1.**时间缩放比例**

1. 时间停止：Time.timeScale = 0;
2. 时间恢复正常：Time.timeScale = 1;
3. 二倍速：Time.timeScale = 2;

2.**帧间隔时间**：最近的一帧用了多长时间（秒），主要用来计算位移，位移 = 时间 * 速度

scale就是时间缩放比例，unity在编辑模式下，**不限制帧率**

1. 受scale影响的帧间隔时间，写法：Time.deltaTime
2. 不受scale影响的帧间隔时间，写法：Time.unscaledDeltaTime
3. 根据需求，选择参与计算位移的间隔时间，如果希望游戏暂停时同时不动，就使用deltaTime；如果希望不受游戏暂停影响，就使用unscaledDeltaTime（联想子弹时间）

3.游戏开始到现在的时间

1. 主要用来计时，一般是**单机**游戏中计时
2. 受scale影响，写法：Time.time
3. 不受scale影响，写法：Time.unscaledTime

4.物理帧间隔时间FixedUpdate

1. 受scale影响，写法：Time.fixedDeltaTime
2. 不受scale影响，写法：Time.fixedUnscaledDeltaTime

5.帧数，从游戏**开始到现在**跑了多少帧（次循环），写法：Time.frameCount

总结：

​	Time相关的内容最常用的就是帧间隔时间（用来计算位移相关内容）、时间缩放比例（用来游戏暂停、倍速）、帧数（帧同步）

## Transform相关内容

游戏对象（GameObject）位移、旋转、缩放、父子关系、坐标转换等相关操作都由它处理，transform是Unity提供的极其重要的类

1.**Vector3**基础

1. Vector主要是用来表示三维坐标系中的一个点或者一个向量

2. 声明（无参构造，有参构造）
   1. Vector3 v = new Vector3();v.x = x;v.y = y;v.z = z是一种声明方法
   2. Vector3 v = new Vector3(x,y);只传xy，默认z是0
   3. Vector3 v = new Vector3(x,y,z)
   
3. vector的基本运算（+ - * /），一一对应，进行运算，支持四则运算

4. 常用：Vector3.zero（000）、Vector3.right（100）、Vector3.left（-100）、Vector3.forward（001）、Vector3.back（00-1）、Vector3.up（010）、Vector3.down（0-10）、Vector3.one(111)

   三个正方向：right--x	up--y	forward--z	

   三个负方向：left	down	back

   这里的Vectoor3.right, Vector3.up, Vector3.forward，是固定值，坐标系xyz方向，根据所处的坐标系不同，朝向可能不同

5. 常用方法
   1. 计算两个点之间的距离的方法，写法：Vector3.Distance(v1,v2)

2.**位置**

1. **相对世界坐标系**，写法：this.transform.position/this.gameobject.transform.position，通过position得到的位置，都是相对于世界坐标系的原点位置，如果有父子关系，可视化界面(inspector)上显示的是相对于父对象的相对位置。

   除非父对象也在原点的位置。但实际上这只是巧合，无论如何，它的值都是相对世界坐标系的原点的位置！

2. **相对父对象**，写法：this.transform.localPosition，得到的是相对于父对象的坐标

3. 不能直接单独对transform.position.x/y/z进行改变赋值，只能对整体的Vector3进行赋值,虽然不能单独赋值，但是可以先取出来，让其成为Vector3再单独进行赋值

   Vector3 v = transfrom.localPostion;	v.x = 1;	transform.localPostion = v;

4. **对象当前的各朝向**

   1. 对象当前的**面朝向**，写法：this.transform.forward	z	蓝色轴（轴向是局部坐标系下）

   2. 对象当前的**头顶朝向**，写法：this.transform.up      y     绿色轴

   3. 对象当前的**右手边**，写法：this.transform.right      x       红色轴

      此时是相对与世界坐标系的叠加或者说偏移

3.**位移**，坐标系下的位移计算公式，**路程 = 方向 * 速度 * 时间**

1. 方式一：自己计算，想要变化的就是position，用当前的位置 + 我要动多长距离，得出最终所在的位置，写法：this.transform.position = this.transform.position + this.transform.forward * 1 * Time.deltaTime;

   **this.transform.postion += this.transform.forward * 1 * Time.deltaTime;**(全局坐标系)

   其实是通过连续改变位置实现的移动效果，所以说要叠加上次移动的位置

2. 方式二：API，写法：**transform.Translate()**，参数一：表示位移多少；参数二：表示，相对坐标系，默认该参数是相对于自己坐标系的
   1. transform.Translate(Vector3.forward * 1 * Time.deltaTime,Space.World)，是相对于世界坐标系的Z轴去动，始终朝着世界坐标系的Z轴正方向移动

   2. transform.Translate(this.transform.forward * 1 * Time.deltaTime,Space.World)，是相对于世界坐标系，自己的面朝向去动，始终朝着自己的面朝向移动

   3. transform.Translate(this.transform.forward * 1 * Time.deltaTime,Space.Self)，相对于自己的坐标系下的自己的面朝向向量移动（一定不会让物体这样移动），不可用

      在自己的坐标系下，继续叠加

   4. transform.Translate(Vector3.forward * 1 * Time.deltaTime,Space.Self)，相对于自己的坐标系下的Z轴正方向移动，始终朝自己的面朝向移动

总结：

​	Vector3如何声明、提供的常用静态属性和一个计算距离的方法、位置、相对于世界坐标系和相对于父对象这两个坐标的区别、不能够单独修改xyz，只能统一改、位移、自己如何计算来进行位移、API是哪个方法来进行位移，使用时要注意不能错误坐标系下的错误移动

4.角度相关

1.  **相对世界坐标角度**，写法：this.transform.**eulerAngles**	不是rotation, 它是四元数类型
2. **相对父对象角度**，写法：this.transform.localEulerAngles
3. 注意：设置角度和设置位置一样，不能单独设置xyz，要一起设置，如果我们希望改变的角度是可视化界面上显示的角度，那一定是改变相对父对象的角度

5.旋转相关

1. 自己计算，和位置一样，不断改变角度即可

   ​	this.transform.eulerAngles += new Vector(11, 0, 0) * Time.deltaTime;

   ​	别忘了 矢量 * 时间

2. API计算
   1. **自转**，写法：this.transform.Rotate(new Vector3(x,y,z) * Time.deltaTime)，函数有两个参数，第一个参数，相当于是每一帧旋转的角度；第二个参数默认不填就是相对于自己坐标系进行的旋转。绕着世界坐标来自转，可以写为：this.transform.Rotate(new Vector3(x,y,z) * Time.deltaTime,Space.World)
   
   2. 相对于**某个轴**，转多少度，写法：this.transform.Rotate(Vector3.right,x * Time.deltaTime)，函数有三个参数，参数一是相对哪个轴进行转动；参数二是转动的角度是多少；参数三默认不填就是相对于自己的坐标系进行旋转，也可以填写相对于世界坐标系进行旋转
   
   3. 相对于**某一个点**转，写法：this.transform.RotateAround(相对于哪一个点,相对于点的哪一个轴,旋转的角度)
   
      也可以理解成，绕世界中某个轴转，点是为了确定轴的位置

总结：角度相关和位置相关类似，如何得到角度、通过transform可以得到相对于世界坐标系和相对于父对象的角度、如何自转和绕着另外的点转、Rotate、RotateAround方法

6.缩放相关

1. 相对世界坐标系
   1. 写法：this.transform.lossScale
2. 相对本地坐标系（父对象）
   1. 写法：this.transform.localScale
3. 注意
   1. 同样缩放不能只改xyz，只能一起改（相对于世界坐标系的缩放大小只能得，不能改），所以一般更改缩放大小都是相对于父对象的localScale，写法：this.transform.localScale = new Vector3(x,y,z);
   2. Unity没有提供关于缩放的API，想要实现缩放变化的效果，只能自己算

7.看向相关

1. 让一个对象的面朝向，可以一直看向某一个点或者某一个对象，写法：this.transform.LookAt(一个vector3/一个对象的Transform)，会将自己的Z轴指向看向的点/对象

总结：

​	缩放相关：相对于**世界坐标系的缩放只能得**不能改、只能去修改相对于本地坐标系的缩放（相对于父对象）、没有提供对应的API来缩放变化，只能自己算

​	transform.localScale +=  Vector3.one * Time.deltaTime;

​	看向相关：**LookAt**方法看向一个点或者一个对象，一定要写在Update中，才会不停变化

​	transform.LookAt(Vector3.zero);	transform.LookAt((transform)lookObj);

8.父子关系

1. 获取和设置父对象
   1. 获取父对象，写法：this.transform.parent
   2. 设置父对象，取消父子关系，写法：this.transform.parent = null
   3. 设置父对象，建立父子关系，写法：this.transform.parent = GameObject.Find("对象名").transform;只是一种写法，也可以直接声明一个public的Transform，然后通过拖拽赋值
   4. 通过API来进行父子关系的设置
      1. this.transform.SetParent(null);取消父子关系
      2. this.transform.SetParent(GameObject.Find("对象名").transform);建立父子关系
      3. 参数一，父对象；参数二，是否保留世界坐标的位置，角度，缩放，信息，默认为true，会保留世界坐标下的状态和父对象进行计算，得到本地坐标系的信息(为了保持所处位置不变)；若设置为false，不会保留，会直接把世界坐标系下的位置角度缩放直接赋值到本地坐标系下（所处位置会变）
2. 和自己的所有子对象（第一层）断绝联系
   1. 写法：**this.transform.DetachChildren();**只能和自己的子对象断绝联系，无法影响子对象和子对象自身的子对象的联系
3. 获取子对象
   1. 按照名字查找子对象，写法：**this.transform.Find("对象名");**找到子对象的transform信息；Find方法可以找到失活的对象，而GameObject相关的查找是不能找到失活对象的；只能找到自己的子对象，无法找到子对象的子对象；虽然它的效率比GameObject.Find方法高一些，但是前提是必须要知道父对象是谁，才能进行查找
   2. 遍历子对象
      1. 如何得到有多少个子对象，写法：**this.transform.childCount；**此方法，失活的子对象也同样会算数量；只能找到子对象，找不到子对象的子对象
      2. 通过索引号，去得到自己相应的子对象，如果索引号超出子对象数量范围，会报错，写法：**this.transform.GetChild(索引号);**返回值是transform，可以得到对应子对象的位置相关信息
4. 子对象可以进行的操作
   1. 一个对象判断自己是不是另一个对象的子对象，写法：this.transform.IsChildOf(传入一个transform)，判断是不是传入的GameObject的子对象
   2. 得到自己作为子对象的编号，写法：this.transform.GetSiblingIndex()
   3. 将自己设置为父对象的第一个子对象（编号），写法：this.transform.SetAsFirstSibling()
   4. 将自己设置为父对象的最后一个子对象（编号），写法：this.transform.SetAsLastSibling()
   5. 将自己设置为父对象的指定编号的子对象，写法：this.transform.SetSiblingIndex(编号)，如果传入的编号越界（正越界或者负越界）不会报错，会直接设置成最后一个编号

总结：设置父对象相关的内容、获取子对象、和所有子对象断绝联系、子对象可以进行的操作

```
transform.parent
transform.parent = null
transform.parent = GameObject.Find("xxx").transform
transform.SetParent(Transfom, bool)
transform.DetachChildren()
transform.Find(string 对象名)
transform.childCount
transform.GetChild(int 索引号)
transform.IsOfChild(Transform)
transform.GetSiblingIndex()
transform.SetAsFirstSibling(int)
transform.SetAsLastSibling(int)
transform.SetSiblingIndex(int)
```

9.坐标转换

1. 世界坐标转本地坐标，可以帮助我们大概判断一个相对位置（此时Vector3.forward在世界坐标系下，不是本地）
   1. 世界坐标系的点，转换为相对本地坐标系的点，受到缩放影响，直接调用API，写法：this.transform.InverseTransform**Point**(Vector3.forward)
   
      就是该点的位置从相对于全局坐标系，转换到了相对于本地坐标系
   
   2. 世界坐标系的方向转换为相对本地坐标系的方向
      1. 不受缩放影响，写法：this.transform.InverseTransform**Direction**(Vector3.forward)
      2. 受缩放影响，写法：this.transform.InverseTransform**Vector**(Vector3.forward)
   
2. 本地坐标转世界坐标（此时Vector3.forward在本地坐标系下，不是世界）
   1. 本地坐标系的点转换为相对世界坐标系的点，受到缩放影响，写法：this.transform.TransformPoint(Vector3.forward)
   2. 本地坐标系的方向转换为相对世界坐标系的方向
      1. 不受缩放影响，写法：this.transform.TransformDirection(Vector3.forward)
      2. 受缩放影响，写法：this.transform.TransformVector(Vector3.forward)

总结：本地坐标系的点转世界坐标系的点：比如，现在玩家要在自己面前x距离放置一个物体，不用关心世界坐标，只需要用相对本地坐标转为世界坐标即可。

```
transform.InverseTransformPoint()
transform.InverseTransformDirection()
transform.InverseTransformVector()
transform.TransformDirection()
transform.TransformPoint()
Transform.TransformVector()
```



## Input和Screen

1.输入相关Input

​		输入相关内容是写在Update中的内容

1. 鼠标在屏幕位置
   1. 屏幕坐标的原点是在屏幕左下角，右方为x轴正方向，上方为y轴正方向
   2. 得到鼠标位置，写法：Input.mousePosition，返回值是Vector3，但是z轴没有值，因为屏幕本身是2D的。
2. 检测鼠标输入，可以使用相关检测，做一些处理，比如攻击或者摄像机的转动
   1. 鼠标按下的一瞬间，进入，0左键，1右键，2中键，写法：Input.GetMouseButtonDown(int型参数（0，1，2）)
   2. 鼠标抬起的一瞬间，进入，写法：Input.GetMouseButtonUp(int型参数，同上)
   3. 鼠标长按按下抬起都会进入，就是长按按键时，会一直进入该函数，写法：Input.GetMouseButton(参数同上)
   4. 中键滚动，返回值是一个Vector2，中键滚动会改变y的值，(0,-1,0)是向下，(0,0,0)是静止，(0,1,0)是向上，写法：Input.mouseScrollDelta
3. 检测键盘输入
   1. 键盘按下，写法：Input.GetKeyDown(KeyCode.键（此方法是一个枚举，是封装好的枚举类型）)
   2. 传入字符串的重载，写法：Input.GetKeyDown("键位名")，这里传入的字符串不能是大写，只能传入小写，不然报错
   3. 键盘抬起，写法：Input.GetKeyUp(KeyCode.键)
   4. 键盘长按，写法：Inpyt.GetKey(KeyCode.键)，长按时会一直执行
4. 检测默认轴输入
   1. 鼠标键盘输入，主要是用来控制玩家，比如旋转，位移等，所以Unity提供了更方便的方法来帮助我们控制对象的位移和旋转。
   2. 键盘AD按下时，返回-1到1之间的变换，相当于得到这个值就是我们的左右方向，我们可以通过它来控制对象左右移动或者左右旋转，写法：Input.GetAxis("Horizontal")
   3. 键盘SW按下时，返回-1到1之间的变换，得到的是垂直方向的变换，写法：Input.GetAxis("Vertical")
   4. 鼠标横向移动时，-1到1，从左到右，写法：Input.GetAxis("Mouse X")，不动时为0
   5. 鼠标纵向移动时，-1到1，从下到上，写法：Input.GetAxis("Mouse Y")，不动时为0
   6. Input.GetAxisRaw方法和GetAxis使用方法相同，只是GetAxisRaw的返回值只会是-1 0 1之间变化，没有中间渐变
5. 其它
   1. 是否有任意键或鼠标长按，写法：Input.anyKey，只要有一个键长按，就会一直执行
   2. 是否有任意键或鼠标按下，写法：Input.anyKeyDown，只要有一个键按下，就会执行一次
   3. 这一帧的键盘输入，写法：Input.inputString
   4. 手柄输入相关
      1. 得到连接的手柄的所有按钮名字，写法：string[] strs = Input.GetJoystickNames();
      2. 某一个手柄键按下，写法：Input.GetButtonDown("按键名")
      3. 某一个手柄键抬起，写法：Input.GetButtonUp("按键名")
      4. 某一个手柄键长按，写法：Input.GetButton("按键名")
   5. 移动设备触摸相关
      1. Input.touchCount，有几个触点在接收信息
      2. Touch 变量名 = Input.touches[下标],触点接收的信息
      3. 位置，写法Touch.position
      4. 相对于上次位置的变化，写法：Touch.deltaPosition
      5. 是否启用多点触控，写法：Input.multiTouchEnable = true/false
      6. 陀螺仪（重力感应）
         1. 是否开启陀螺仪，必须开启才能正常使用，写法：Input.gyro.enable = true/false
         2. 重力加速度向量，写法：Input.gyro.gravity
         3. 旋转速度，写法：Input.gyro.rotationRate
         4. 陀螺仪，当前的旋转四元数，比如用这个角度信息来控制场景中一个物体受到重力影响，随手机变动而变动，写法：Input.gyro.attitude

​		总结：Input类提供了大部分和输入相关的内容，鼠标、键盘、触屏、手柄、重力感应

```
MouseButton -- 鼠标
Key	-- 键盘，可传枚举，可传字符串
Button -- 按键，只能传字符串
```

2.屏幕相关Screen

1. 静态属性
   1. 常用属性
      1. 查看**当前屏幕分辨率**，写法：Resolution r = Screen.currentResolution，Resolution有width，height等属性
      
      2. 查看**屏幕当前宽高**，写法：Screen.width/height，是当前窗口的宽高，不是设备分辨率
      
         用窗口宽高做计算时，用的是Screen.width/height
      
      3. 屏幕休眠模式，写法：Screen.sleepTimeout = SleepTimeOut.NeverSleep;
      
   2. 不常用属性
      1. 运行时是否全屏模式，写法：Screen.fullScreen = true/false;
      
      2. 窗口模式，分为独占全屏(FullScreenMode.ExclusiverFullScreen)、全屏窗口(FullScreenMode.FullScreenWindows)、最大化窗口(FullScreenMode.MaximizedWindows)、窗口模式(FullScreenMode.Windowed)，写法：Screen.fullScreenMode = FullScreenMode.属性（此为一个枚举类型，直接选择即可）
      
      3. 移动设备屏幕转向相关
         1. 允许自动旋转为左横向，Home在左，写法：Screen.autorotateTolandscapeLeft = true/false
         
         2. 允许自动旋转为右横向，Home在右，写法：Screen.autorotateTolandscapeRight = true/false
         
         3. 允许自动旋转到纵向，Home在下，写法：Screen.autorotateToPortrait = true/false
         
         4. 允许自动旋转到纵向的反向，Home在上，写法：Screen.autorotateToPortrairUpsideDown = true/false
         
         5. 指定屏幕显示方向，写法：Screen.orientation = ScreenOrientation.属性(Landscape横屏)
         
            portrait竖屏
   
2. 静态方法
   1. 设置分辨率，一般移动设备不使用，写法：Screen.SetResolution(Resolution.height，Resolution.width，true/false(是否全屏))

## Camera相关

1.Camera可编辑参数

​		高亮为重点，暗色为了解，较少使用

1. ==Clear Flags==
   1. 如何清除背景
      1. skybox天空盒
      2. Solid Color颜色填充
      3. Depth only只画该层，背景透明
      4. Don't Clear不移除，覆盖渲染
   
2. ==Culling Mask==
   
   1. 选择性渲染部分层级，可以指定只渲染对应层级的对象
   
3. ==Projection==
   
   1. Perspective透视模式
      1. FOV Axis视场角，决定摄像机横纵向的角度，轴，决定光学仪器的视野范围
      2. Field of view视口大小
      3. Physical Camera做了解
         1. 物理摄像机，勾选后可以模拟真实世界中的摄像机，焦距、传感器尺寸、透镜移位等
         2. Focal Length焦距
         3. Sensor Type传感器类型
         4. Sensor Size传感器尺寸
         5. Lens Shift透镜移位
         6. Gate Fit闸门配合
   2. orthographic正交摄像机，一般用于2D游戏制作，Size，摄制范围
   
4. ==Clipping Planes==：裁剪平面距离，渲染范围，Near开始，Far结束，只有在范围内才能看到

   ​									可能会造成物体被裁剪

5. Viewport Rect：视口范围，屏幕上将绘制该摄像机视图的位置
   1. 主要用于双摄相机游戏
   2. 0-1相当于宽高百分比	xy相当于视图的坐标原点位置，wh视图的宽高
   3. 例如主机游戏，可以看到多个玩家同时游玩，比如双人成行

6. ==Depth==：渲染顺序上的深度，越小越先被渲染，先被渲染的物品会被后渲染的遮挡，一般会和Depth only和Culling Mask配合使用，完成场景的图层融合

   在上层的摄像机选择，Depth Only 并选择渲染层，设置较大渲染深度，去叠加画面

   一般UI用一个摄像机渲染，场景对象用另外一个摄像机渲染

7. Rendering path：渲染路径

8. ==Target Texture==：渲染纹理

   1. 可以把摄像机画面渲染到一张图上，主要用于制作小地图，也可用来做瞄准镜等
   2. 在Project右键创建Render Texture

9. ==Occlusion Culling==：是否启用剔除遮挡，被挡住的物体是否要渲染，会节省一部分性能，默认勾选

10. Allow HDR：是否允许高动态范围渲染

11. Allow MSAA：是否允许抗锯齿

12. Allow Dynamic Resolution：是否允许动态分辨率呈现

13. Target Display：将摄像机渲染的图像显示在哪个显示器，主要用来开发有多个屏幕的平台游戏

14. Target Eye：做VR游戏时使用，眼睛的开启  

2.Camera代码相关

1. 重要的静态成员
   1. 获取摄像机
      1. 主摄像机的获取，Camera.main，使用这个方法的前提是，场景中有摄像机的Tag是选择的MainCamera，可以有多个选择此标签的摄像机，但不建议，因为不能确定找到的是哪一个摄像机，所以建议主摄像机只有一台
      2. 获取摄像机的数量，写法：Camera.allCamerasCount
      3. 得到所有摄像机，写法：Camera[] allCamera = Camera.allCameras;
   2. 渲染相关委托
      1. Camera.onPreCull += (c) =>{};摄像机剔除前处理的委托函数
      2. Camera.onPreRender += (c) => {};摄像机渲染前处理的委托
      3. Camera.onPostRender += (c) => {};摄像机渲染后处理的委托
   
2. 重要成员
   1. 界面上的参数，都可以在Camera中获取到，例如：Camera.main.depth = x;
   
      得到的是主摄像机上的对象，是成员对象
   
   2. 世界坐标转屏幕坐标，写法：Camera.main.WorldToScreenPoint(传入物品的transform)，转换过后，x和y就是对应的屏幕坐标，z就是物品相对于摄像机的距离，多是用来做头顶血条相关的功能
   
   3. 屏幕坐标转世界坐标，写法：Camera.main.ScreenToWorldPoint(Input.mousePosition)，Z轴默认为0，转换过去的世界坐标系的点，永远都是一个点，可以理解为视口相交的焦点；如果改变了Z，那么转换过去的世界坐标的点就是相对于摄像机前方多少的单位的横截面上的世界坐标点
   
   3. 屏幕坐标系的原点就在屏幕左下角

# 核心系统 —— 光源系统

## 光源组件

烘焙：预先算好的，不会被影响，节约性能

1.面板参数

1. ==Type==：光源类型
   1. Spot聚光灯
      1. Range：发光范围距离
      2. Spot Angle：光锥角度
   2. Directional：方向光（环境光）
   3. Point：点光源
   4. Area：面光源（只在烘焙下有用）
2. ==Color==：颜色
3. ==Mode==：光源模式
   1. Realtime：实时光源：每帧实时计算，效果好，性能消耗大
   2. Baked：烘培光源：实现计算好，无法动态变化
   3. Mixed：混合光源：预先计算 + 实时计算
4. ==Intensity==：光源亮度
5. Indirect Multiplier
   1. 改变间接光的强度
   2. <1，每次反弹会使光更暗；>1，每次反弹会使光更亮
6. ==Shadow Type==
   1. NoShadows：关闭阴影
   2. HardShadows：生硬阴影
   3. SoftShadows：柔和阴影，更消耗性能
7. RealtimeShadows
   1. Strength：阴影角度0-1之间，越大越黑
   2. Resolution：阴影贴图渲染分辨率，越高+越逼真，消耗越高
   3. Bias：阴影推离光源的距离
   4. Normal Bias：阴影投射面沿法线收缩距离
   5. Near Panel：渲染阴影的近裁剪面
8. ==Cookie==：投影遮罩
9. Cookie Size：投影遮罩大小，方向光组件上有，一般方向光不用投影遮罩，但可以用来做云彩
10. ==Draw Halo==：球形光环开关，绘制光晕
11. ==Flare==：耀斑，光晕
    1. 如果想要在**Game窗口能够看到耀斑**，需要在摄像机加上一个**Flare Layer**脚本
12. Render Mode：渲染优先级
    1. Auto：运行时确定
    2. Important：以像素质量为单位进行渲染，效果逼真，消耗大，比如汽车的远光灯
    3. Not Important：以快速模式进行渲染
13. ==Culling Mask==：剔除遮罩层，决定哪些层的对象受到该光源影响

2.代码控制

​	声明变量Light light，然后使用变量直接调用即可。

## 光面板相关知识点

1. Environment环境相关设置
   1. Skybox Material天空盒材质：可以改变天空盒
   
   2. Sun Source太阳来源：不设置会默认使用场景中最亮的方向光代表太阳
   
   3. Environment Lighting环境光设置
      1. Source环境光光源颜色
         1. Skybox天空盒材质作为环境光颜色
         2. Gradient可以为天空、地平线、地面单独选择颜色和他们之间混合
         2. Color 单独的一种颜色
         
      2. Intensity Multiplier环境光亮度
      
      3. Ambient Mode全局光照
      
         只有启用了实时全局和全局烘培时才有用
         1. Realtime已弃用
         2. Baked
   
2. OtherSettings其它设置
   1. Fog雾开关
      1. Color雾颜色
      2. Mode雾计算模式
         1. Linear随距离线性增加
            1. Start离摄像机多远开始有雾
            2. End离摄像机多远完全遮挡
         2. Exponential随距离指数增加
            1. Density强度
         3. Exponential Quare随距离比指数更快地增加
            1. Density强度
   2. Halo Texture光源周围围着光环的纹理，光晕的呈现效果
   3. Halo Strength光环可见性
   4. Flare Fade Speed耀斑淡出时间，最初出现之后淡出的时间
   5. Flare Strength耀斑可见性
   6. Spot Cookie聚光灯剪影纹理，默认的纹理，全部探照灯都设为这个纹理，cookie还能继续设

# 核心系统——物理系统中碰撞检测

## 刚体

1. 碰撞产生的必要条件：**两个物体都有碰撞器，至少一个物体有刚体**

   **刚体会利用体积进行碰撞计算模拟真实的碰撞效果，产生力的作用**

2. RigidBody组件信息
   1. Mass质量，默认为**千克**；质量越大惯性越大
   
   2. Drag空气阻力，根据力移动对象时影响对象的空气阻力大小；0表示没有空气阻力
   
   3. Angular Drag扭矩阻力，根据扭矩旋转对象时影响对象的空气阻力大小；0表示没有空气阻力
   
   4. Use Gravity是否受重力影响
   
   5. Is Kinematic如果启用此选项，则对象将不会被物理引擎驱动，只能通过（Transform）对其进行操作，对于移动平台，或者如果要动画化附加了HingeJoint的刚体，此属性将非常有用
   
   6. Interpolate插值运算，让刚体物体移动更平滑
   
      **物理帧更新时间直接影响物理系统**
   
      1. None不应用插值运算
      2. Interpolate根据下一帧的变换来平滑变换
      3. Extrapolate差值运算，根据下一帧的估计变换来平滑变换
   
   7. Collision Detextion（碰撞检测模式），用于防止快速移动的对象穿过其它对象而不检测碰撞
      1. Discrete离散检测；对场景中的所有其它碰撞体使用离散碰撞检测，其它碰撞体在测试碰撞时会使用离散碰撞检测，用于正常碰撞，是默认使用的模式，也是最不消耗性能的模式
      
      2. Continuous连续检测；对动态碰撞体（具有刚体）使用离散碰撞检测，并对静态碰撞体（没有刚体）使用连续碰撞检测；设置为连续动态（Continuous Dynamic）的刚体将在测试与该刚体的时使用连续碰撞检测。（此属性对物理性能有很大影响，如果没有快速对象的碰撞问题，请将其保留为Discrete设置），其他刚体将使用离散碰撞检测。
      
      3. Continuous Dynamic连续动态检测；性能消耗高；对设置为连续（Continuous）和连续动态（Continuous Dynamic）碰撞的游戏对象使用连续碰撞检测，还将对静态碰撞体（没有刚体）使用连续碰撞检测；对于所有其他碰撞体，使用离散碰撞检测，用于快速移动的对象。
      
      4. Continuous Speculative连续推测检测；对刚体和碰撞体使用推测性连续碰撞检测，该方法通常比连续碰撞检测的成本更低
      
      5. 性能消耗：**Continuous Dynamic > Continuous Speculative > Continuous > Discrete**
      
      6. 连续推测检测的优先级比较高，只要连续推测检测和别的碰撞检测发生碰撞，都是使用连续推测检测
      
         ![image-20231202153337788](Unity学习.assets/image-20231202153337788.png)
      
   8. Constraints约束，对刚体运动的限制
      1. Freeze Position有选择地停止刚体沿**世界**X、Y和Z轴的移动
      2. Freeze Rotation有选择地停止刚体围绕**局部**X、Y和Z轴的旋转

## 碰撞器

**碰撞器是用来表示物体的体积（形状）**；刚体会利用体积进行碰撞计算，模拟真实的碰撞效果，产生力的作用

1. 3D碰撞器种类
   
   1. 盒状：Box Collider
   2. 球状：Sphere Collider
   3. 胶囊：Capsule Collider
   4. 上述三种碰撞器性能消耗较低，但是不够精准，下列碰撞器则相反
   5. 网格：Mesh Collider
   6. 轮胎：Wheel Collider
   7. 地形：Terrain Collider
   
2. 共同参数
   1. Is Trigger是否是触发器，如果启用此属性，则该碰撞体将用于触发事件，并被物理引擎忽略，主要用 于进行没有物理效果的碰撞检测
   2. Material物理材质，可以确定碰撞体和其它对象碰撞时的交互（表现）方式
   3. Center碰撞体在对象局部空间中的中心点位置
   
3. 常用碰撞器
   1. Box Collider盒状碰撞器
      1. Size碰撞体在X、Y、Z方向上的大小
   2. Sphere Collider球状碰撞器
      1. Radius球形碰撞体的半径大小
   3. Capsule Collider胶囊碰撞器
      1. Radius胶囊体的半径
      2. Height胶囊体的高度
      3. Direction胶囊体在对象局部空间中的轴向
   
4. 异形物体使用多种碰撞器组合
   1. 刚体对象的子对象碰撞器信息参与碰撞检测
   
5. 不常用碰撞器
   1. Mesh Collider网格碰撞器
      1. Convex勾选此复选框可启用Convex；如果启用此属性，该Mesh Collider将与其它Mesh Collider发生碰撞。Convex Mesh Collider最多255个三角形，性能低
      
         加了刚体，必须勾选此选项
      
      2. Cooking Options启用或禁用影响物理引擎对网格处理方式的网格烹制选项
         1. None禁用下方列出的所有Cooking Options
         2. Everything启用下方列出的所有Cooking Options
         3. Cook for Faster Simulation使物理引擎烹制网格以加快模拟速度。启用此设置后，这会运行一些额外步骤，以保证生成的网格对于运行时性能是最佳的。这会影响物理查询和接触生成的性能。禁用此设置后，物理引擎会使用更快的烹制速度，并尽可能快速生成结果。因此，烹制的Mesh Collider可能不是最佳的
         4. Enable Mesh Cleaning使物理引擎清理网格。启用此设置后，烹制过程会尝试消除网格的退化三角形以及其他几何瑕疵。此过程生成的网格更适合于在碰撞检测中使用，往往可生成更准确的击中点
         5. Weld Colocated Vertices使物理引擎在网格中删除相等的顶点。启用此设置后，物理引擎将合并具有相同位置的顶点.这对于运行时发生的碰撞反馈十分重要
      
      3. Mesh引用需要用于碰撞的网格
      
   2. Wheel Collider环状碰撞器
      1. Mass车轮的质量
      2. Radius车轮的半径
      3. Wheel Damping Rate这是应用于车轮的阻尼值
      4. Suspension Distance车轮悬架的最大延伸距离（在局部空间中测量）。悬架始终向下延伸穿过局部Y轴
      5. Force App Point Distance此参数定义车轮上的受力点。此距离应该是距车轮底部静止位置的距离（沿悬架行程方向），以米为单位。当forceAppPointDistance = 0时，受力点位于静止的车轮底部。较好的车辆会使受力点略低于车辆质心。
      6. Suspension Spring悬架尝试通过增加弹簧力和阻尼力来到达目标位置（Target Position）
      7. Forward Friction车轮向前滚动时轮胎摩擦的特性
      8. Sideways Friction车轮侧向滚动时轮胎摩擦的特性
      9. 注意：不必通过转动或滚动Wheel Collider对象来控制汽车；附加了Wheel Collider的对象应始终相对于汽车本身固定
      10. **应该和父对象配合使用，根对象加刚体，而且刚体质量要足够大（例如mass1500），不然汽车会被轮胎弹飞**
      
   3. Terrain Collider地形碰撞器
      1. Terrain Data地形数据
      2. Enable Tree Colliders选中此属性时，将启用树碰撞体

## 物理材质

可以让两个物体之间碰撞表现出不同效果

1. 创建物理材质
   1. 直接创建Physic Material
2. 物理材质参数
   1. Dynamic Friction已在移动时使用的摩擦力。通常为0到1之间的值。值为零就像冰一样，值为一将使对象迅速静止（除非用很大的力或重力推动对象）
   2. Static Friction当对象静止在表面上时使用的摩擦力。通常为0到1之间的值。值为零就像冰一样，值为一将导致很难让对象移动
   3. Bounciness表面弹性如何；值为0将不会反弹，值为1将在反弹时不产生任何能量损失，预计会有一些近似值，但可能只会给模拟增加少量能量
   4. Friction Combine两个碰撞对象的摩擦力的组合方式
      1. Average对两个摩擦值求平均值
      2. Minimum使用两个值中的最小值
      3. Maximum使用两个值中的最大值
      4. Multiply两个摩擦值相乘
   5. unce Combine两个碰撞对象的弹性的组合方式。其模式与Friction Combine模式相同

## 碰撞检测函数

碰撞和触发响应函数属于特殊的生命周期函数，也是通过反射调用；在生命周期中介于物理帧(FixedUpdate)和逻辑帧(Update)之间，并且会回馈到物理帧

1. 物理碰撞检测响应函数
   1. 碰撞触发接触时会自动执行这个函数OnCollisionEnter(Collision collision)
   2. 碰撞结束分离时会自动执行这个函数OnCollisionExit(Collision collision)
   3. 两个物体相互接触摩擦时会不停调用这个函数OnCollisionStay(Collision collision)
   4. Collision类型的参数包含了碰到自己的对象的相关信息
      1. 关键参数
         1. 碰撞到的对象碰撞器的信息collision.collider
         2. 碰撞对象的依附对象（GameObject）collision.gameObject
         3. 碰撞对象的依附对象的位置信息collision.transform
         4. 触碰点数相关collision.contactCount
      2. 只要得到了碰撞到的对象的任意一个信息，就可以得到它的所有信息
2. 触发器检测响应函数
   1. 触发开始的函数，当第一次接触时会自动调用OnTriggerEnter(Collider other)
   2. 结束触发分离的时候，会自动调用OnTriggerExit(Collider other)
   3. 当两个对象一直接触的状态，会自动调用OnTriggerStay(Collider other)
   4. Collider是所有碰撞器的基类，都继承于Collider
3. 要明确什么时候会响应函数
   1. 只要挂载的对象能和别的物体**产生碰撞或者触发**，那么对应的这些函数就能够被**响应**
   2. 如果是一个异形物体，刚体在父对象上，如果你想要通过子对象上挂脚本检测碰撞是行不通的；必须要挂载到这个具有刚体组件的父对象上才可以
   3. 要明确，物理碰撞和触发器响应的区别
4. 碰撞和触发函数都可以写成虚函数，在子类去重写逻辑
   1. 一般会把想要重写的碰撞和触发函数写成保护类型的，没有必要写成public类型，因为不会自己手动调用，都是Unity通过反射帮助我们调用

## 刚体加力

1. 刚体自带添加力的方法
   1. 给刚体加力的目标就是让其有一个速度，朝向某一个方向移动
      1. 首先应该获取刚体组件，使用GetCompoment<RigidBody>()方法
      
      2. 添加力
         1. 相对世界坐标，RigidBody下有一个方法AddForce(方向和大小，第二个参数为力的模式)；加力过后，对象是否停止移动是由阻力决定的；如果阻力为0，那给了一个力过后，始终不会停止；如果你希望即使有阻力，对象也可以一直动，可以选择一直给对象一个力，也可以实现
         
            刚体.AddForce(Vector3方向 * 力大小);
         
         2. 相对本地坐标，RigidBody下有一个方法AddRelativeForce(方向和大小，第二个参数为力的模式)
         
            rigidBody.AddRelativeForce(Vector3.forward * 10);
         
         3. 如果想要在世界坐标系方法中，让对象相对于自己的面朝向动，方法：RigidBody.AddForce(this.transform.forward * 10)，相当于相对本地坐标方法Vector3.forward
         
            这一块类比Transform中的位移
         
      3. 添加扭矩力，让其旋转
         1. 相对世界坐标，在RigidBody下有一个方法AddTorque(方向大小) rigidBody.AddTorque(Vector3.up * 10);
         
         2. 相对本地坐标，在RigidBody下有一个方法AddRelativeTorque(方向大小)
         
            rigidBBody.AddRelativeTorque(Vector3.up * 10);
         
      4. 直接改变速度，RigidBody.velocity = 方向 * 大小，但是速度方向朝向**世界坐标系**
      
      5. 模拟爆炸效果，RigidBody.AddExplosionForce(参数一爆炸力度大小，参数二位置，参数三爆炸半径)；如果想要得到这个效果，就需要想要实现效果的物体的刚体都执行这个方法
   
2. 力的几种模式
   1. 主要的作用就是计算方式不同，由于4种计算方式不同，最终的移动速度就会不同
   
   1. 动量定理：Ft = mv
   
      ​	v = F / t	F:力 t:时间 m:质量 v:速度
   
   1. Acceleration加速度模式；给物体增加一个持续的加速度，**忽略其质量**；计算时时间就是物理帧时间，质量默认为1
   
   1. Force加力模式；给物体添加一个持续的力，**与物体的质量有关**；计算中时间为物理帧时间，质量代入即可算出速度
   
   1. Impulse瞬间力模式；给物体添加一个瞬间的力，与物体的**质量有关，忽略时间**，默认为1
   
   1. VelocityChange瞬时速度模式；给物体添加一个瞬时速度，**忽略质量，忽略时间**，默认都为1，只和力的大小有关
   
3. 力场脚本，在Unity中提供着一个现成的脚本（Constant Force），作用是给物体一个持续的力，添加方法为直接在物体上添加脚本组件

   - Force
   - Relatice Force
   - Torque
   - Relative Torque

4. 刚体休眠：Unity为了节约性能，在一定情况下，刚体会发生休眠，而导致刚体不会实现原有功能，这时候只能改变一些物体的位置才能继续行使刚体的能力，称之为刚体休眠。解决方法：RigidBody中有一个方法可以判断是否休眠，那么解决刚体休眠的方法就可以直接使用if(RigidBody.IsSleeping()){RigidBody.WakeUp();}来唤醒刚体

------

此处我们可以做一个总结，想让一个物体移动

1. 通过transform.Translate(方向 * 速度大小 * 时间，坐标系)

2. 通过 transform.postion += 方向 * 速度大小 * 时间 

3. 通过rigidBody.AddForce(方向 * 大小，力模式) 或 rigidBody.AddRelativeForce(方向 * 大小，力模式)

4. 通过rigidBody.velocity = 方向 * 大小

   联想一下某些游戏，在设置里提供了很多种移动类型设置，比如固定距离，距离加速等等

让一个物体旋转

1. transform.eluerAngles += new Vector(1, 0, 0) * 大小 * 时间
2. transform.Rotate(new Vector(1, 0, 0) * 大小 * 时间，坐标系 );
3. transform.Rotate(轴，力大小 * 时间，坐标系)
4. rigidBody.AddTorque(方向 * 大小) 或 rigidBody.AddRelativeTorque(方向 * 大小);

# 补充知识

## 协程

协程在Unity中是一个很重要的概念，我们知道，在使用Unity进行游戏开发时，一般（注意是一般）不考虑多线程，那么如何处理一些在主任务之外的需求呢，Unity给我们提供了协程这种方式

在Unity中一般不考虑多线程。因为在Unity中，只能在主线程中获取物体的组件、方法、对象，如果脱离这些，Unity的很多功能无法实现，那么多线程的存在与否意义就不大了
既然这样，线程与协程有什么区别呢：

对于协程而言，同一时间只能执行一个协程，而线程则是并发的，可以同时有多个线程在运行
两者在内存的使用上是相同的，共享堆，不共享栈
其实对于两者最关键，最简单的区别是微观上线程是并行（对于多核CPU）的，而协程是串行的

1. 什么是协程
   1. 协程，从字面意义上理解就是协助程序的意思，我们在主任务进行的同时，需要一些分支任务配合工作来达到最终的效果；稍微形象的解释一下，想象一下，在进行主任务的过程中我们需要一个对资源消耗极大的操作时候，如果在一帧中实现这样的操作，游戏就会变得十分卡顿，这个时候，我们就可以通过协程，在一定帧内完成该工作的处理，同时不影响主任务的进行
2. 协程的原理
   1. 首先需要了解协程不是线程，协程依旧是在主线程中进行
   2. 然后要知道协程是通过迭代器来实现功能的，通过关键字IEnumerator来定义一个迭代方法，注意使用的是IEnumerator，而不是IEnumerable：
      1. IEnumerator：是一个实现迭代器功能的接口
      2. IEnumerable：是在IEnumerator基础上的一个封装接口，有一个GetEnumerator()方法返回IEnumerator
   3. 在迭代器中呢，最关键的是yield 的使用，这是实现我们协程功能的主要途径，通过该关键方法，可以使得协程的运行暂停、记录下一次启动的时间与位置等等
3. 协程的使用
   1. 首先通过一个迭代器定义一个返回值为IEnumerator的方法，然后再程序中通过StartCoroutine来开启一个协程即可
   2. 需要了解StartCoroutine的两种重载方式
      1. StartCoroutine（string methodName）：这种是没有参数的情况，直接通过方法名（字符串形式）来开启协程
      2. StartCoroutine（IEnumerator routine）：通过方法形式调用
      3. StartCoroutine（string methodName，object values)：带参数的通过方法名进行调用
   3. 在一个协程开始后，同样会对应一个结束协程的方法StopCoroutine与StopAllCoroutines两种方式，但是需要注意的是，两者的使用需要遵循一定的规则，在介绍规则之前，同样介绍一下关于StopCoroutine重载：
      1. StopCoroutine（string methodName）：通过方法名（字符串）来进行
      2. StopCoroutine（IEnumerator routine）:通过方法形式来调用
      3. StopCoroutine(Coroutine routine)：通过指定的协程来关闭
   4. 如果我们是使用StartCoroutine（string methodName）来开启一个协程的，那么结束协程就只能使用StopCoroutine（string methodName）和StopCoroutine(Coroutine routine)来结束协程
4. 关于yield
   1. 首先解释一下位于Update与LateUpdate之间这些yield 的含义：
      1. yield return null; 暂停协程等待下一帧继续执行
      2. yield return 0或其他数字; 暂停协程等待下一帧继续执行
      3. yield return new WairForSeconds(时间); 等待规定时间后继续执行
      4. yield return StartCoroutine("协程方法名");开启一个协程（嵌套协程)
   2. 协程虽然是在Update中开启，但是关于yield return null后面的代码会在下一帧运行，并且是在Update执行完之后才开始执行，但是会在LateUpdate之前执行
   3. 接下来看几个特殊的yield，他们是用在一些特殊的区域，一般不会有机会去使用，但是对于某些特殊情况的应对会很方便
      1. yield return GameObject; 当游戏对象被获取到之后执行
      2. yield return new WaitForFixedUpdate()：等到下一个固定帧数更新
      3. yield return new WaitForEndOfFrame():等到所有相机画面被渲染完毕后更新
      4. yield break; 跳出协程对应方法，其后面的代码不会被执行
   4. 通过上面的一些yield一些用法以及其在脚本生命周期中的位置，我们也可以看到关于协程不是线程的概念的具体的解释，所有的这些方法都是在主线程中进行的，只是有别于我们正常使用的Update与LateUpdate这些可视的方法
5. 内部原理
   1. 当执行到yield时会将协程的代码权限交给父线程，这个协程的代码被挂起，直到父线程使用MoveNext，才会继续执行yield后面的代码
   2. unity中有限制：仅能够从主线程中访问有关Unity中的对象，任何从第二个线程中访问的都将会失败并且引发错误。Unity中使用的多线程也是因为有C#，使用了C#的多线程。相关Unity的则一般使用协程，他从属于主线程，不用考虑同步锁等问题。Unity中每帧的工作是：每帧执行协程的MoveNext（）方法，当为true时继续往下执行

## 坐标系

### 世界坐标系

​	原点：世界的中心点

​	轴向：世界坐标系的三个轴向是固定的

### 物体坐标系

​	原点：物体的中心点

​	轴向：

- 物体右方为x轴正方向
- 物体上方为y轴正方向
- 物体前方为z轴正方向

### 屏幕坐标系

原点：屏幕左下角

轴向：

- 向右为x轴正方向
- 向上为y轴正方向

最大宽高：Screen.width Screen.height

### 视口坐标系

原点：屏幕左下角

轴向：

- 向右为x轴正方向
- 向上为y轴正方向

特点：左下角（0，0）右上角（1，1） 

和屏幕坐标系类似，将坐标单位化

### 相关代码总结

```c#
//修改他们，会是相对世界坐标系的变化
transform.postion;
transform.eulerAngles;
transform.rotation;
transform.lossyScale;

//是相对父对象的物体坐标系的位置 本地坐标 相对坐标
//修改他们，会是相对父对象物体坐标系的变化
transform.localPostion;
transform.localEulerAngles;
transform.localRotation;
transform.localScale;

//屏幕坐标系
Input.mousePostion;
Screen.width;
Screen.height;

//视口坐标系
//视口范围

//世界坐标系转本地坐标系
transform.InverseTransformPoint();
transform.InverseTransformDirection();
transform.InverseTransformVector();
//本地坐标系转世界坐标系
transform.TransformDirecton();
transform.TransformVector();
transform.TransformPoint();

//世界坐标系转屏幕坐标系
Camera.main.WorldToScreenPoint();
//屏幕坐标系转世界坐标系
Camera.main.ScreenToWorldPoint();

//世界坐标系转视口坐标系
Camera.main.WorldToViewportPoint();
//视口转世界
Camera.main.ViewportToWorldPoint();
//视口转屏幕
Camera.main.ViewportToScreenPoint();
//屏幕坐标系转视口坐标系
Camera.mian.ScreenToViewportPoint();
```

## Draw Call

CPU准备好渲染数据（顶点，纹理，法线，shader等等）后，告知GPU开始渲染（将命令放入命令缓冲区）的命令

一次DrawCall就是 CPU准备好渲染数据通知 GPU渲染的过程

如果游戏中DrawCall数量较高会影响CPU的效率，最直接的感受就是 游戏卡顿

每次DrawCall，CPU都需要准备很多数据发送给GPU，那么如果DrawCall越多，那么额外开销就越大。GPU的渲染效率是很强大的，往往影响渲染效率的都是CPU提交命令的速度，CPU把大量时间花费在提交DrawCall上，造成CPU过载，游戏卡顿

在UI层面上，小图合大图，即多个小DrawCall变一次大DrawCall

## InputSystem

InputSystem是Unity提供的一种新的输入系统，需要Unity2019.4以上版本 + .NET 4 runtime

相对于老的输入系统更具拓展性和可自定义的替代方案

InputManger：需要写各种检测来判断设备输入，并处理对应逻辑

InputSystem：不仅可以像老输入系统一样使用，还增加了输入配置的概念，新输入系统将输入操作进行封装，让我们可以在Unity内进行输入配置文件编辑，不需要写代码来判断设备输入，只需要把工作重心放在逻辑处理上

PackageManger中导入Input System

Project目录下，右键创建Input Action

在GameObject下添加组件Player Input，并关联Input Action

切换输入系统：File -> Build Setting -> Player Setting -> Other -> Active Input Handing

## ScriptableObject

是Unity

提供的一个数据配置存储基类，可以用来保存大量数据的数据容器。就像是可以自定义的数据资源文件

类似MonoBehavior的基类，需要继承它来进行使用

作用：

1. 数据复用（多个对象用同一个数据）
2. 配置文件（配置游戏中的数据）
3. 编辑模式下的数据持久化

在发布运行时，ScriptableObject并不具备持久化特性（修改数据对象，并不会保存在本地）

1. 可以直接在Inspector窗口编辑配置数据，可以里利用它来做配置文件
2. 处理重复 数据，减少数据看看拷贝时造成的内存占用，可以利用它来做公共数据
3. 可以更方便的处理数据带来的多态行为

# 3D数学知识点

## Mathf知识点

1. Mathf和Math
   1. Math是C#中封装好的用于数学计算的工具类，位于System命名空间中
   2. Mathf是Unity中封装好的用于数学计算的工具结构体，位于UnityEngine命名空间中；它们都是提供来用于进行数学相关计算的
   
2. 它们的区别
   1. Mathf和Math中的相关方法几乎一样；Math是C#自带的工具类，主要就提供一些数学相关计算方法；Mathf是Unity专门封装的，不仅包含Math中的方法，还多了一些适用于游戏开发的方法
   
3. Mathf中的常用方法，一般计算一次
   1. Π（PI）：Mathf.PI
   2. 取绝对值（Abs）：Mathf.Abs()
   3. 向上取整（CeilToInt）：Mathf.CeilToInt()
   4. 向下取整（FloorToInt）：Mathf.FloorToInt()
   5. 夹紧函数（Clamp）：Mathf.Clamp(min,x,max):如果x<min值为min;如果x>max值为max;如果min<x<max值为本身；min和max参数位置可以改变，会自动计算最大和最小值
   6. 获取最大值（Max）：Mathf.Max(参数一，参数二，······)
   7. 获取最小值（Min）：Mathf.Min(参数一，参数二，······)
   8. 一个数的n次幂（Pow）：Mathf.Pow(底数，指数)
   9. 四舍五入（RoundToInt）：Mathf.RoundToInt()
   10. 返回一个数的平方根（Sqrt）：Mathf.Sqrt()
   11. 判断一个数是否是2的n次方（IsPowerOfTwo）：Mathf.IsPowerOfTwo();返回值为true和false
   12. 判断正负数（Sign）：Mathf.Sign();返回值为1和-1
   
4. Mathf中的常用方法，一般不停计算
   1. 插值运算（Lerp）
   
   2. Lerp函数公式：result = Mathf.Lerp(start,end,t)
   
   3. t为插值系数，取值范围为0-1；result = start + (end - start) * t
   
   4. 插值运算用法一：每帧改变start的值，变化速度先快后慢，位置无限接近，但是不会得到end位置；start = Mathf.Lerp(start,end,t)
   
      start = Mathf.Lerp(start, 10, Time.deltaTime);
   
   5. 插值运算用法二：每帧改变t的值，变化速度匀速，位置每帧接近，当t >= 1时，得到结果；result = Mathf.Lerp(start,end,t匀速变化)
   
      time += Time.deltaTime;
   
      result = Mathf.Lerp(start, 10, time);

## 三角函数

1. 角度和弧度
   1. 角度和弧度都是度量角的单位，1°/1radian(rad)
   
   2. Π rad = 180°
   
      1rad = （180 / Π）° => 1rad = 180 / 3.14 ≈ 57.3°
   
      1° = （Π / 180）rad => 1° = 3.14 / 180 ≈ 0.01745 rad
   
   3. 弧度、角度相互转化：
   
      - 角度 = 弧度 * Mathf.Rad2Deg；  弧度转角度，2是to的简写
   
      - 弧度 = 角度 * Mathf.Deg2Rad；
   
2. 三角函数
   
   Unity中没有度的符号，所以传入弧度值
   
   1. Mathf中的三角函数相关函数，传入的参数需要是弧度值：Mathf.Sin(度数 * Mathf.Deg2Rad)/Mathf.Sin(度数 * Mathf.Deg2Rad)
   
3. 反三角函数
   1. 反三角函数作用是得到正弦或余弦值对应的弧度：Mathf.Asin(正弦或余弦值)
   
      rad = Mathf.Acos(0.5f);
   
      print(rad * Mathf.Rad2Deg);

## 向量模长和单位向量

向量：有数值大小，有方向的**矢量**

1. 向量
   1. 三维向量Vector3有**两种**几何意义
      1. 位置，代表一个点
      2. 方向，代表一个方向
2. 两点决定一向量
   1. 声明两个Vector3代表两个点，然后进行运算，即可得到一个向量
3. 零向量和负向量
   1. 零向量是唯一一个大小为0的向量 Vector3.zero
   2. 负向量：负向量和原向量大小相等，方向相反
4. 向量的模长
   1. 向量的模长就是向量的长度
   2. 向量是由两个点算出，所以向量的模长就是两个点的距离
   3. 模长公式为√x^2^ + y^2^ + z^2^
   4. Vector3中提供了获取向量模长的成员属性，向量名.magnitude；如果是一个点的magnitude属性，得到的是该点相对于原点的距离
   4. 两向量之间的距离：Vector3.Distance(A, B);
5. 单位向量
   1. 模长为1的向量为单位向量，任意一个向量经过归一化就是单位向量，只需要方向，不想让模长影响计算结果时使用单位向量
   2. **归一化**公式：单位向量 = (x / 模长,y / 模长,z / 模长)
   3. Vector3中提供了获取单位向量的成员属性，向量名.normalized
6. 总结
   1. Vector3这个变量，可以表示一个点，也可以表示一个向量，具体用法可以根据需求和逻辑决定
   2. 终点-起点就可以在Unity得到一个向量，一个点也可以代表向量，表示从该点到原点的距离
   3. 得到了向量就可以利用Vector3中提供的成员属性得到模长和单位向量
   4. 模长相当于可以得到两点之间的距离；单位向量主要是用来进行移动计算的，它不会影响我们想要的移动效果

## 向量加减乘除运算

1. **向量相加**，首尾相连；**向量和位置**相加，相当于平移位置
2. 向量减法
   1. **位置之间相减**，得到一个向量；两点决定一向量，终点-起点
   2. **向量之间的减法**。得到一个向量；向量相减，头连头，尾指尾，A - B = B头指A头
   3. **位置减向量**相当于加负向量，得到一个位置；位置减向量 = 平移位置
3. 向量乘除
   1. 向量只会和标量进行乘除法运算
   2. 向量 * / 标量 = 向量
   3. 向量 * / 正数，方向不变，放大缩小模长
   4. 向量 * / 负数，方向相反，放大缩小模长
   5. 向量 * 0，得到零向量

- 向量加减法——位置平移和向量计算
- 向量乘除法——模长的放大或缩小

## 向量点乘

1. 点乘计算公式
   1. A·B = Xa * Xb + Ya * Yb + Za * Zb；向量·向量 = 标量

2. 点乘的几何意义

   可以得到一个向量在自己向量上投影的长度

   - 点乘结果**>0**两向量夹角为**锐角**

   - 点乘结果**<0**两向量夹角为**钝角**

   - 点乘结果**=0**两向量夹角为**直角**

     可以用来判断敌方的大致位置，或者判断视野范围等等操作

3. 通过点乘判断对象方位

   1. Vector3提供了计算点乘的方法：Vector3.Dot(乘数，被乘数);结果为一个float，可以通过判断此float，来判断位置

   2. ```c#
      //举例说明，参数一为自身面朝向，即a向量，参数二为观测物体位置 - 自身位置的向量，即BA 
      float res = Vector3.Dot(this.transform.forward, cb1.transform.position - this.transform.position);
              if (res >= 0) print(cb1.name + "在" + this.name + "前方");
              else print(cb1.name + "在" + this.name + "后方");
      ```

4. 公式推导

   ![image-20231202195733755](Unity学习.assets/image-20231202195733755.png)

5. 通过点乘推导公式算出夹角

   1. 用单位向量算出点乘结果

      1. ```c#
         //举例说明，参数一为自身面朝向，即a向量，参数二为观测物体位置 - 自身位置的向量，即BA,此处参数二需要是单位向量，不然可能会出现错误情况
         float res = Vector3.Dot(this.transform.forward, (cb1.transform.position - this.transform.position).normalized);
         ```

   2. 用反三角函数得出角度

      1. ```c#
         print("角度为" + Mathf.Acos(res) * Mathf.Rad2Deg);
         //如此即可得到观测物体相对于自身的角度
         ```

6. Vector3中提供了方法，可以直接调用算出夹角

   1. ```c#
      //举例，参数一为自身朝向，参数二为观测物体位置 - 自身位置的向量
      print("角度为" + Vector3.Angle(this.transform.forward, cb1.transform.position - this.transform.position));
      ```

## 向量叉乘

1. 叉乘计算公式

   ![image-20231202201556005](Unity学习.assets/image-20231202201556005.png)

2. 叉乘计算

   1. 在Unity中有现有方法

      1. ```c#
         //参数一为A，参数二为B；两个向量计算叉乘
         print(Vector3.Cross(this.transform.position, cb1.transform.position));
         ```

3. 几何意义

   1. A * B得到的向量同时垂直A和B，A * B向量垂直于A和B组成的平面，即法向量；A × B = -(B × A)

      1. 假设向量A和B都在XZ平面上，向量A叉乘向量B；得到的结果Y > 0则证明B在A右侧，若Y < 0则证明B在A左侧

      2. ```c#
         //参数一为物体A，参数二为物体B
         Vector3 C = Vector3.Cross(this.transform.position, cb1.transform.position);
                 if (C.y > 0) print(cb1.name + "在" + this.name + "右侧");
                 else print(cb1.name + "在" + this.name + "左侧");
         ```

点乘配合叉乘，判断位置

Angle 加 叉乘，可以判断角度范围

# 2D相关

## 图片导入设置

### 概述

Unity支持的图片格式有很多

- BMP：是Windows操作系统的标准图像文件格式，特点是几乎不压缩，占磁盘空间大
- TIF：基本不损失图片信息的图片格式，缺点是体积大
- JPG：一般指JPEG格式，属于有损压缩格式，能够让图片压缩在很小的存储空间，一定程度上会损失图片数据，无透明通道
- PNG：无损压缩算法的位图格式，压缩比高，生成文件小，有透明通道
- TGA：支持压缩，使用不失真的压缩算法，还支持编码压缩。体积小，效果清晰，兼备BMP的图像质量和JPG的体积优势，有透明通道
- PSD：是PS图形处理软件转用的格式，通过一些第三方工具或自制工具可以直接将PSD画面转为UI画面
- EXR、GIF、HDR、IFF、PICT等等

Unity最常用的三种图片格式是JPG、PNG、TGA

### 纹理类型设置

Texture Type 

1.  ==Default==：默认纹理，大部分导入的模型贴图都是该类型

   1. sRGB（Color Source）：启用可以将纹理存储在伽马空间中（对每一个像素做一次幂函数运算）默认勾选

   2. ==Alpha Source==：指定如何生成纹理的Alpha通道（透明度通道）

      1. ==None==：无论输入纹理是否有Alpha通道，导入的纹理都没有Alpha通道

      2. ==Input Texture Alpha==：输入纹理中的Alpha

      3. ==From Gray Scale==：从输入纹理RGB值的平均值生成Alpha（用的比较少）

         图片每个像素存储RGBA四个信息，红，绿，蓝，透明度

         <img src="Unity学习.assets/image-20231203200732408.png" alt="image-20231203200732408" style="zoom:50%;" />

   3. Alpha Is Transparency：启用避免边缘上的过滤瑕疵

2. ==Normal map：法线贴图格式（法线贴图就是在原物体的凹凸表面的每个点上均作法线，法线就是垂直于某个点的切线的方向向量）在一个更粗糙的模型上，用更精致模型的法线贴图渲染出更好的效果

   1. ==Create From Grayscale==：启用此属性可以从灰度高度贴图创建法线贴图
   2. ==Bumpiness==：控制凹凸程度，值越大凹凸感越强
   3. ==Filtering==：如何计算凹凸值 Smooth：使用标准算法生成法线贴图 Sharp：生成比标准模式更锐利的法线贴图

3. ==Editor GUI and Legacy GUI==：一般在编辑器中或者GUI上使用的纹理

4. ==Sprite（2D and UI）==：2D游戏或者UGUI中使用的格式

   1. ==Sprite Mode==：图像中提取精灵图形的方式
      1. Single：按原样使用精灵图像
      2. Multiple：瓦片模式，如果是图集，可以在Sprite Editor编辑窗口自定义图片
      3. Polygon：网格精灵模式
   2. ==Pixels Per Unit==：世界空间中的一个距离单位对应多少像素（一米对应多少像素）在分辨率自适应的时候会参与一些计算
   3. ==Mesh Type==：网格类型，只有Single和Multiple模式支持
      1. Full Rect：创建四边形，将精灵显示在四边形上
      2. Tight：基于像素Alpha值来生成网格，更加贴合精灵图片的形状。任何小于32*32的精灵都使用FullRect模式，即使设置成Tight模式也是。生成的面片数量可能更多
   4. Extude Edge：使用滑动条确定生成的网格中精灵周围溢出的区域大小
   5. ==Generate Physics Shape==：启用，Unity会自动根据精灵轮廓生成默认物理形状
   6. ==Sprite Editor==：编辑Sprite，需要安装2D Sprite包
   7. ==Pivot==：精灵图片的轴心点，Single模式才有此选项。对应九宫格布局的九个点，还可以自定义

5. ==Cursor==：自定义光标

6. ==Cookie==：光源剪影格式

   1. Light Type：应用的光源类型。一般点光源的剪影需要设置为立方体纹理，方向光和聚光灯的剪影设置为2D纹理
      1. Spotlight：聚光灯，需要边缘纯黑色纹理
      2. Direction：方向光，平铺纹理
      3. Point：点光源，需要设置为立方体形状

7. ==Lightmap==：光照贴图格式

8. ==Single Channel==：纹理只需要单通道的格式

   1. Channel：希望将纹理处理为Alpha通道还是Red通道
      1. Alpha：使用Alpha通道，不允许进行压缩
      2. Red：使用红色通道

### 纹理形状设置

纹理不仅可以用于模型贴图，还可以用于制作天空盒和反射探针，纹理形状设置主要就是用于在两种模式之间进行切换

Texture Shape ：纹理形状

1. ==2D==：2D纹理，最常用设置，这些纹理将使用到模型和GUI元素上
2. ==Cube==：立方体贴图，主要用于天空盒和反射探针
   1. Mapping：如何将纹理投影到游戏对象上
      1. Auto：根据纹理信息创建布局
      2. 6 Frames Layout：纹理包含标准立方体贴图布局之一排列的六个图像
      3. Latitude-Longitude Layout：将纹理映射到2D维度/经度
      4. Mirrored Ball：将纹理映射到类似球体的立方体贴图上
   2. Convlution Type：纹理的过滤类型
      1. None：无过滤
      2. Specular：将立方体作为反射探针
      3. Diffuse：将纹理进行过滤表示辐照度，可作为光照探针
   3. Fixup Edge Seams：Convolution Type 为None 和 Diffuse下才有用。解决低端设备上面之间立方体贴图过滤错误

### 纹理高级设置

主要是纹理的一些尺寸规则，读写规则，以及MipMap相关设置

MipMap：

在三维计算机图形贴图渲染中有一个常用的技术被称为Mipmaping

为了加快渲染速度和减少图像锯齿，贴图被处理成由一系列被预先计算和优化过的图片组成的文件，这样的贴图被称为mipmap

mipmap需要占用一定的内存空间

Mipmap中每一个层级的小图都是主图的一个特定比例的缩小细节的复制品

虽然在某些必要的视角，主图仍然会被使用，来渲染完整的细节。但是当贴图缩小或者只需要从远距离观看时，mipmap就会转换到适当的层级

因为mipmap贴图需要被读取的像素远少于普通贴图，所以渲染速度得到了提升，而且操作时间减少了，因为mipmap的图片已经做过抗锯齿处理，从而减少了实时渲染的负担，放大缩小也因为mipmap而变得更有效率。

如果每个贴图的基本尺寸是256 * 2566 像素的话，它的mipmap就会有8个层级，每个层级是上一个层级的四分之一大小

依次层级大小：128 * 128；64 * 64； 32 * 32；16 * 16；8 * 8； 4 * 4；2 * 2；1 * 1（一个像素）

总结：开启MipMap，Unity会帮助我们根据图片信息生成n张不同分辨率的图片，在场景中根据我们离该模型的距离选择合适尺寸的图片用于渲染，提升渲染效率

LOD

Advanced

1. ==Non-Power of 2==：如果纹理尺寸非2的幂如何处理。因为图形学规则，纹理必须是2的幂尺寸
   1. None：纹理尺寸大小保持不变
   2. To nearest：将纹理缩放到最接近2的幂的大小（PVRTC格式要求纹理为正方形）
   3. To larger：将纹理缩放到最大尺寸大小值的2的幂的大小
   4. To smaller：将纹理缩放到最小尺寸大小值的2的幂的大小
2. ==Read/Write Enabled==：启用可以使用Unity中提供的一些方法从纹理中获取到数据（一般需要获取图像数据时才开启）增加内存消耗
3. Streaming Mipmaps：启用则可以使用纹理串流，主要用于在控制加载在内存中的Mipmap级别，用于减少Unity对于纹理所需的内存总量，用**性能换内存**
4. ==Generate Mip Maps==：允许生成MipMap
   1. Border Mip Maps：避免颜色外渗到较低MIP级别的边缘
   2. Mip Map Filering：优化图像质量的过滤方法
      1. Box：随着尺寸减小，级别更平滑
      2. Kaiser：随着Mipmap尺寸大小下降而使用的锐化算法，如果远处纹理太模糊，可使用该算法
   3. Mip Maps Preserve Coverage：Mipmap的Alpha通道在Alpha测试期间保留覆盖值 Alpha Cutoff  Value：覆盖率参考值
   4. Fadeout Mip Maps：级别递减时使Mipmap淡化为灰色

### 纹理平铺拉伸设置

1. ==Warp Mode==：平铺纹理时的方式
   1. Repeat：在区块中重复纹理
   2. Clamp：拉伸纹理边缘
   3. Mirror：在每个整数边界上镜像纹理以创建重复图案
   4. Mirror Once：镜像纹理一次，然后将拉伸边缘纹理
   5. Per-axis：单独控制如何在U轴和V轴上包裹纹理
2. ==Filter Mode==：纹理在通过3D变化拉伸时如何进行过度
   1. Point：纹理在靠近时变为块状
   2. Bilinear：纹理在靠近时变得模糊
   3. Triliner：与Bilinear类似，单纹理也在不同的Mip级别之间模糊
3. Aniso Level：以大角度查看纹理时提高纹理质量，性能消耗高

## Sprite Renderer

Sprite Renderer是精灵渲染器，所有2D游戏中游戏资源（除UI外）都是通过Sprite Renderer让我们看到的。是游戏开发中一个极为重要的组件

创建方式：

- 直接拖入Sprite图片
- 右键创建
- 空物体添加脚本

1. ==Sprite==：渲染的精灵图片
2. Color：定义着色，一般没有特殊需求不会修改
3. Filp：水平或竖直反转精灵图片
4. ==Draw Mode==：绘制模式，当尺寸变化时的缩放方式
   1. ==Simple==：简单模式，缩放时整个图像一起缩放
   2. ==Sliced==：切片模式，9宫格切片模式，**十字区域缩放**，4**个角****不变化**。一般用于变化不大的纯色图（需要把精灵的网格类型设置为Full Rect）通过合适的切割和缩放，小图也能变成大图，做ui底板等等
   3. ==Tiled==：平铺模式，将**十字区域进行平铺**而不是缩放（需要把精灵网格类型设置为Full Rect）
      1. Continuous：当尺寸变化时，中间部分均匀平铺
      2. Adaptive：当尺寸变化时，类似Simple模式，当更改尺寸达到Stretch Value时，中间才开始平铺
5. ==Mask Interaction==：与精灵遮罩交互时的方式
   1. None：不与场景中任何精灵遮罩交互
   2. Visible inside Mask：精灵遮罩覆盖的地方可见，而遮罩外部不可见
   3. Visible Outside Mask：精灵遮罩外部的地方可见，而遮罩覆盖处不可见
6. Sprite Sort Point：计算摄像机和精灵之间距离时，使用精灵中心Center还是轴心点Pivot，一般情况下不用修改
7. Material：材质，可以使用一些自定义材质来显示一些特殊效果一般情况不修改。默认材质不会受到光照影响，如果要受光照影响，可以选择Default-Diffuse
8. ==Additional Setting==：高级设置
9. ==Sorting Layer==：排序层设置
10. ==Order in Layer==：层级序列号，数值越大越会显示在前面

代码设置

```c#
GameObject obj = new GameObject;
SpriteRenderer sr = obj.AddCompment<SpriteRenderer>();
sr.sprite = Resources.Load<Sprite>("图片名");

Sprite[] sprs = Resources.LoadAll<Sprite>("图集名");;
sr.sprite = sprs[10];
print(sprs[10].name);
```

## Sprite Creator

精灵创造者

可以利用Sprite Editor的多边形工具创造出各种多边形

Unity也为我们提供了现成的一些多边形

主要作用是2D游戏资源，在等待美术出资源时，可以用作替代品。有点类似Unity自带的几何体。是demo和学习的必需品

在Project界面下右键，选择Sprite下的不同类型创建。实际上每种都是从一个四边形下切割出来的，选择了不同的渲染区域

## Sprite Mask

精灵遮罩，对精灵图片产生遮罩。制作一些特殊功能，比如只显示图片的一部分让玩家看到

联系Sprite Renderer中的Mask Interaction

1.  ==Sprite==：遮罩图片，显示区域需要有颜色，不显示区域没有颜色
2. ==Alpha Cutoff==：如果Alpha包含透明区域和不透明区域之间的混合（半透明），则可以手动确定所显示区域的分界点（0~1）大于才显示
3. ==Custom Range==：自定义遮罩范围。开启后可以设置遮罩范围，按照排序层来划分
   1. Sprite Renderer中层序需要 >= Back 中的 层序 并且 < Front中的层序，才能在遮罩下常显示
   2. 同样可设置排序层，只要不 >= Front中排序层的层级 和 层序，就都能显示
4. Sprite Sort Point

## Sorting Group

排序分组

Unity会将同一个排序组中的精灵图片一起排序，就好像他们是单个游戏对象一样

主要作用是对于需要分层的2D游戏用于整体排序

子排序组，先排子对象，再按父对象和别人一起排（同层和同层比）

多个 挂载排序分组组件的预制体 之间 通过修改 排序索引号来决定先后顺序

排序是优先于Z轴

## Sprite Atlas

打图集的目的是减少DrawCall，提高性能

**在Unity中打开自带的打图集功能**：

Edit -> Project Setting ->Editor

Project目录下右键选择创建SpriteAtlas

Sprite Packer(精灵包装器，可以通过Untiy自带图集工具生成图集)

1. Disabled：默认设置，不打包图集
2. Enabled For Build：Unity在打包时，打包图集，在编辑器模式下不会打包
3. Always Enabled：Unity在打包时打包图集，在编辑器模式下运行前打包图集
4. Legacy Sprite Packer：传统打包模式，多了设置图片之间的间隔距离
   1. 选择的数字代表2的n次方个像素间隔。打包的精灵之间以及精灵与生成的图集边缘之间的间隔距离

![image-20231205102636189](Unity学习.assets/image-20231205102636189.png)

**观察Draw Call数量：**

![image-20231205102740199](Unity学习.assets/image-20231205102740199.png)

```c#
GameObject obj = new GameObject();
SpriteRenderer sprd = obj.AddCompment<SpriteRenderer>();
SpriteAtlas sa = Resources.Load<SpriteAltas>("图集名");
sprd.sprite = sa.GetSprite("图片名");
```

