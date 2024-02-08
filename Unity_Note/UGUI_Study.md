# UI系统

## UGUI

1. UGUI是什么？
   1. UGUI是Unity引擎内自带的UI系统，官方称之为：Unity UI，是目前Unity商业游戏开发中使用最广泛的UI系统开发解决方案，它是基于Unity游戏对象的UI系统，只能用来做游戏UI功能，不能用于开发Unity编辑器中内置的用户界面
2. 六大组件
   1. Canvas对象上依附的：
      1. Canvas：画布组件，主要用于渲染UI控件
      2. Canvas Scaler：画布分辨率自适应组件，主要用于分辨率自适应
      3. Graphic Raycaster：射线事件交互组件，主要用于控制射线响应相关
      4. RectTransform：UI对象位置锚点控制组件，主要用于控制位置和对齐方式
   2. EventSystem对象上依附的：
      1. EventSystem和Standalone Input Module：玩家输入事件响应系统和独立输入模块组件，主要用于监听玩家操作
3. Canvas组件
   1. Canvas组件用来做什么
      1. Canvas的意思是画布，它是UGUI中所有UI元素能够被显示的根本，它主要负责渲染自己的所有UI子对象
      2. 如果UI控件对象不是Canvas的子对象，那么控件将不能被渲染，我们可以通过修改Canvas组件上的参数修改渲染方式
   2. 场景中可以有多个Canvas对象
      1. 场景中允许有多个Canvas对象，可以分别管理不同画布的渲染方式，分辨率适应方式等参数
      2. 如果没有特殊需求，一般情况场景上一个Canvas即可
   3. Canvas组件的3种渲染方式
      1. Render Mode渲染模式
         1. Screen Space - Overlay：屏幕空间，覆盖模式，UI始终在前
            1. Pixel Perfect：是否开启无锯齿精确渲染（性能换效果）
            2. SortOrder：排序层编号（用于控制多个Canvas时的渲染先后顺序）
            3. TargetDisplay：目标设备（在哪个显示设备上显示）
            4. Additional Shader Channels：其他着色器通道，决定着色器可以读取哪些数据
         2. Screen Space - Camera：屏幕空间，摄像机模式，3D物体可以显示在UI之前
            1. Pixel Perfect：是否开启无锯齿精确渲染（性能换效果）
            2. RenderCamera：用于渲染UI的摄像机（如果不设 置将类似于覆盖模式，不建议使用主摄像机，不易管理UI和3D物体的渲染，建议加入一个新的摄像机管理UI）
            3. Plane Distance：UI平面在摄像机前方的距离，类似整体Z轴的感觉
            4. Sorting Layer：所在排序层
            5. Order in Layer：排序层的序号
         3. World Space：世界空间，3D模式，可以把UI对象像3D物体一样处理，常用于VR或者AR
            1. Event Camera：用于处理UI事件的摄像机（如果不设置，不能正常注册UI事件）

## NGUI

1. NGUI是什么
   1. NGUI全称 下一代用户界面 它是第三方提供的Unity付费插件 专门用于制作Unity中游戏UI的第三方工具，相对于GUI它更适用于制作游戏UI功能，更方便使用，性能和效率更高
   2. Unity插件：是一种基于Unity规范编写出来的程序，主要用于拓展功能，简单理解就是别人基于Unity写好的某种功能代码，我们可以直接用来处理特定的游戏逻辑。