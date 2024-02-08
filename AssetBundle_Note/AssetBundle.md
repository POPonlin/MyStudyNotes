<a name="kraMg"></a>
# AB包理论基础
**什么是AB包？**

- 特定于平台的资产压缩包，有点类似压缩文件
- 资产包括：模型、贴图、预制体、音效、材质球等

**AB包的好处**

1. 相对于Resources下的资源，AB包更好的管理

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1699618814319-a0b86927-3ea0-4727-9e5a-6b33f3ba09fc.png#averageHue=%23f2eeed&clientId=u28e25439-6c4c-4&from=paste&height=232&id=ud198725b&originHeight=854&originWidth=1047&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=388911&status=done&style=none&taskId=u4ae08df9-bcec-4daf-9a07-21003ac473c&title=&width=284.3999938964844)

2. 减小包体大小
   1. 压缩资源，节约外存空间
   2. 减少初始包的大小，后续再额外下载资源
3. 热更新
   1. 资源热更新
   2. 脚本热更新，主要是lua

![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1699619070660-8a36e15c-0699-430f-abfd-caa29c09f2ea.png#averageHue=%23f5f4f3&clientId=u28e25439-6c4c-4&from=paste&height=280&id=ub66595bb&originHeight=740&originWidth=1070&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=321772&status=done&style=none&taskId=u71179aff-b1b5-4dd7-a3a3-310da07fe70&title=&width=405.4000244140625)
<a name="VqniQ"></a>
# AB包资源打包
**生成AB包资源文件**

1. Unity编辑器开发，自定义打包工具       一般公司内有写好的工具
2. 官方工具：Asset Bundle Browser
3. AssetBundleBrowser参数相关
4. AB包生成的文件
   1. AB包文件——资源文件
   2. manifest文件
      1. AB包文件信息
      2. 当加载时，提供了关键信息，资源信息，依赖关系，版本信息
   3. 关键AB包（和目录名一样的包）
      1. 主包
      2. AB包依赖关键信息

选中打包的资源，在inspector面板下，选中new后，填写包名，后边的是扩展名<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1699619941277-393874a8-e427-4c4a-99e1-8db658f0c8cf.png#averageHue=%23cdcbc9&clientId=u28e25439-6c4c-4&from=paste&height=78&id=u6563a4b0&originHeight=97&originWidth=489&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29871&status=done&style=none&taskId=u6703f7c4-e1cb-417c-86d0-b095b205081&title=&width=391.2)<br />将预制体打入AB包，不是把代码打入，而是把关联的数据打入<br />预制体中的组件是通过反射进行获取的<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1699620015287-68555763-0306-4a97-aec1-919d9d821648.png#averageHue=%23c2c2c1&clientId=u28e25439-6c4c-4&from=paste&height=201&id=ua41c05db&originHeight=959&originWidth=1464&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=164609&status=done&style=none&taskId=udef18124-6131-4293-9013-23eae3d68ff&title=&width=306.3999938964844)<br />每次打包都是要指定平台<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1699621579547-d9d45cf2-358d-4b5a-a885-69d89858a52a.png#averageHue=%23d5d3cf&clientId=u28e25439-6c4c-4&from=paste&height=190&id=u38ec4519&originHeight=238&originWidth=599&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=117208&status=done&style=none&taskId=uc6c23605-da0f-4722-9cdf-5a288bc46eb&title=&width=479.2)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1699621167609-3ec206b9-f716-4781-8c38-4d2ca5cf898a.png#averageHue=%23c3c0c0&clientId=u28e25439-6c4c-4&from=paste&height=530&id=uebabd1e0&originHeight=663&originWidth=1465&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=221706&status=done&style=none&taskId=ue948befa-7b4d-4d02-9835-649e728c592&title=&width=1172)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/35758071/1699621438982-ecd62dfa-8eb7-4f4d-a8ee-b9e1782d311b.png#averageHue=%23f8f5f5&clientId=u28e25439-6c4c-4&from=paste&height=323&id=ubbc3759c&originHeight=487&originWidth=821&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=212456&status=done&style=none&taskId=ud648a945-bab5-4b79-a01f-b3aa8a81231&title=&width=543.7999877929688)<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/35758071/1699622648011-55290280-b60e-47be-abd0-d4154ec8a810.jpeg)
<a name="AxOVw"></a>
# AB包资源加载
**注意！AB包不能重复加载！**
```csharp
void Start()
{
    //同步加载资源包，StreamingAsset文件会随unity打包进去。所以从这加载
    //并且该文件下内容不会被压缩加密等操作，在PC上可读可写
    //在安卓和IOS上是只读的
    AssetBundle ab = AssentBundle.LoadFromFile(Application.streamingAssetsPath + "/" + "包名");
	//加载指定内容，有三种形式
    GameObject abj = ab.LoadAsset<GameOject>("Cube");
    //不指定资源的类型，可能会导入出错，比如同名的资源
    GameObject abj = ab.LoadAsset("Cube");
    //指定资源类型，因为返回值类型不一样，所以要转换类型，lua里没有泛型，所以主用此种方式
    GameObject abj = ab.LoadAsset("Cube", typeof(GameObject)) as GameObject;

    Instantiate(obj);

    //异步加载
    StartCoroutine(LoadABRes("head", "23_11100001"));
    
	//卸载某个包，接受一个bool参数，true卸载加载进来的所有资源，false只删包不卸载加载的资源
    ab.Unload(false);
	//卸载全部包，参数跟上边一样
    AssetBundle.UnloadAllAssetBundles(false);
}

IEnumerator LoadABRes(string ABName, string resName)
{
	AssetBundleCreateRequest abcr = AssetBundle.LoadFromFileAsync(Application.streamingAssetsPath + "/" + "包名");
	yield return abcr;
    AssetBundleRequst abq = abcr.assetBundle.LoadAssentAsync(resName, typeof(Sprite));
    yield return abq;
    img.sprite = abq.asset as Sprite;
}
```
<a name="GCzsH"></a>
# AB包依赖
**AB包依赖：**如果一个资源用到了别的AB包上的资源，如果只加载自己包，并创建对象会出现资源丢失，则那个别的包就是该包的依赖包，需要把依赖包加载了才会正常。<br />利用主包，获取该包的依赖信息，manifest中记录了包间的依赖关系<br />注意manifest中记录的是某个包依赖的所有包，即使包中某个资源并不需要其中一些的依赖包，仍会全部获取到
```csharp
AssetBundle ab = AssentBundle.LoadFromFile(Application.streamingAssetsPath + "/" + "包名");
//加载主包
AssetBundle abMain = AssentBundle.LoadFromFile(Application.streamingAssetsPath + "/" + "PC");
//获得主包的manifest文件
AssetBundleManifest abManifest = abMain.LoadAsset("AssetBundleManifest", typeof(AssetBundleManifest)) as AssetBundleManifest;
//从manifest文件中获取某个包的所有依赖信息
string[] strs = abMainifest.GetAllDependencies("mode");
//加载所有依赖包
forreach(string name in strs)
{
    AssentBundle.LoadFromFile(Application.streamingAssetsPath + "/" + name);
}

```
<a name="O7dbv"></a>
# AB包资源管理器
```csharp
public class ABMgr : SingletonManger<ABMgr>
{
	private AssetBundle mainBundle = null;
    private AssetBundleManifest manifest = null;
    private Dictionary<string, AssetBundle> ab Dic = new Dictionary<string, AssetBundle>();

    private PathUrl
    {
        get 
        {
            return Application.streamingAssetsPath + "/";
        }
    }
    private MainBNamme
    {
        get
        {
#if UNITY_IOS
            return "IOS";
#elif UNITY_ANDROID
            return "Android";
#else 
            return "PC";
#endif
        }
    }
    
    private void LoadAB(string abName)
    {
        //加载主包，加载依赖包
        if(mainBundle is null)
        {
            mainBundle = AssetBunld.LoadFromFile(PathUrl + MainBName);
            manifest = mainBundle.LoadAsset("AssetBundleManifest", typeof(AssetBundleManifest)) as AssetBundleManifest;            
        }
        
    	AssetBundle ab = null;
        string[] strs = manifest.GetAllDependenceies(abName);
        forreach(string str int strs)
        {
            if(!abDic.ContainsKey(str))
            {
                ab = AssetBunld.LoadFromFile(PathUrl + str);
                abDic.Add(str, ab);
            }
        }

        if(!abDic.ContainsKey(abName))
        {
            ab = AssetBundle.LoadFromFile(PathUrl + abName);
            Dic.Add(abName, ab);
        }                
    }

    //同步加载，不指定类型
    public obj LoadRes(string abName, string resName)
    {
        LoadAB(abName);

        Object obj = abDic[abName].LoadAsset(resName);
        if(obj is GameObject)
            return Instantiate(obj);
        else 
            return obj;
    }

    //同步加载，指定类型，因为System里边同样有Object，为了防止混乱故用System.Type
    public obj LoadRes(string abName, string resName, System.Type type)
    {
        LoadAB(abName);

        Object obj = abDic[abName].LoadAsset(resName, typeof(type));
        if(obj is GameObject)
            return Instantiate(obj);
        else 
            return obj;
    }	

    //同步加载，泛型指定
    public T LoadRes<T>(string abName, string resName) where T : Object
    {
        LoadAB(abName);

        Object obj = abDic[abName].LoadAsset<T>(resName);
        if(obj is GameObject)
            return Instantiate(obj);
        else 
            return obj;
    }
    
    //卸载单个包
    public void Unload(string abName)
    {
        if(abDic.ContainsKey(abName))
        {
            abDic[abName].Unload(false);
            abDic.Remove(abName);
        }
    }
    
	//卸载全部包
    public void ClearAll()
    {
        AssetBundle.UnloadAllAssetBundles(false);
    	abDic.Clear();
        mainBundle = null;
        manifest = null;
    }
}
```

```csharp
//异步加载，无类型
//异步加载不确定什么时候加载好，所以通过回调函数返回值
public void LoadResAsync(string abName, string resName, UnityAction<Object>  callback)
{
    StratCoroutine(ReallyLoadResAsyncReallyLoadResAsync(abName, resName, callback));
}
private IEnumerator ReallyLoadResAsync(string abName, string resName, UnityAction<Object> callback)
{
	LoadAB(abName);

    AssentBundleRequest abr = abDic[abName].LoadAssentAsync(resName);
    yield return abr;
    
    if(abr.assent is GameObject)
        callback(Instantiate(abr.assent) as Obj);
    else 
        callback(abr.assent);
}

//异步加载，有类型
public void LoadResAsync(string abName, string resName, System.type type, UnityAction<Object>  callback)
{
    StratCoroutine(ReallyLoadResAsyncReallyLoadResAsync(abName, resName, type, callback));
}
private IEnumerator ReallyLoadResAsync(string abName, string resName, System.type type, UnityAction<Object> callback)
{
	LoadAB(abName);

    AssentBundleRequest abr = abDic[abName].LoadAssentAsync(resName, type);
    yield return abr;
    
    if(abr.assent is GameObject)
        callback(Instantiate(abr.assent) as Obj);
    else 
        callback(abr.assent);
}

//异步加载，泛型类型
public void LoadResAsync<T>(string abName, string resName, UnityAction<T>  callback) where T : Object
{
    StratCoroutine(ReallyLoadResAsyncReallyLoadResAsync<T>(abName, resName, callback));
}
private IEnumerator ReallyLoadResAsyncM<T>(string abName, string resName, UnityAction<Object> callback) where T : Object
{
	LoadAB(abName);
	
    AssentBundleRequest abr = abDic[abName].LoadAssentAsync<T>(resName);
    yield return abr;
    
    if(abr.assent is GameObject)
    	callback(Instantiate(abr.assent) as T);
    else 
		callback(abr.assent as T);
}
```
```csharp
SingletonManger.GetInstance().LoadResAsync<GameObjec>("modle", "Cube", (obj) => {
   //对obj的操作 
});
```
