


## 一. 常用函数：
```csharp
string selectPath = AssetDatabase.GetAssetPath(assetPath);
// 根据路径获取文件夹info
DirectoryInfo dir = new DirectoryInfo(fullPath);
// 获取该文件夹下的所有文件 SearchOption - 可以指定获取类型
FileInfo[] infos = dir.GetFiles("*", SearchOption.AllDirectories);
// 根据assetPath获取，可以设置abName
AssetImporter importer = AssetImporter.GetAtPath(assetPath);
// 获取abName列表
string[] abNames = AssetDatabase.GetAllAssetBundleNames();
// 移除所有没用到的abName
AssetDatabase.RemoveUnusedAssetBundleNames();
// 本地获取依赖，参数2代表是否递归
AssetDatabase.GetAssetBundleDependencies(assetBundleName, recursive);
```

### BuildAssetBundleOptions
- **CompleteAssets**：用于保证资源的完备性，默认开启；
- **CollectDependencies**：用于收集资源的依赖项，默认开启；
- **DeterministicAssetBundle**：用于为资源维护固定ID，默认开启；
- **ForceRebuildAssetBundle**：用于强制重打所有AssetBundle文件，新增；
- **IgnoreTypeTreeChanges**：用于判断AssetBundle更新时，是否忽略TypeTree的变化，新增；
- **AppendHashToAssetBundleName**：用于将Hash值添加在AssetBundle文件名之后，开启这个选项可以直接通过文件名来判断哪些Bundle的内容进行了更新（4.x下普遍需要通过比较二进制等方法来判断，但在某些情况下即使内容不变重新打包，Bundle的二进制也会变化），新增。
- **ChunkBasedCompression**：用于使用LZ4格式进行压缩，5.3新增。

Unity3D引擎为我们提供了三种压缩策略来处理AssetBundle的压缩，即：

- LZMA格式
  - LZMA格式：在默认情况下，打包生成的AssetBundle都会被压缩。在U3D中，AssetBundle的标准压缩格式便是LZMA（LZMA是一种序列化流文件），因此在默认情况下，打出的AssetBundle包处于LZMA格式的压缩状态，在使用AssetBundle前需要先解压缩。
  - 使用LZMA格式压缩的AssetBundle的包体积最小（高压缩比），但是相应的会增加解压缩时的时间。
- LZ4格式
  - LZ4格式：Unity 5.3之后的版本增加了LZ4格式压缩，由于LZ4的压缩比一般，因此经过压缩后的AssetBundle包体的体积较大（该算法基于chunk）。但是，使用LZ4格式的好处在于解压缩的时间相对要短。
  - 若要使用LZ4格式压缩，只需要在打包的时候开启BuildAssetBundleOptions.ChunkBasedCompression即可。
- 不压缩
  - 不压缩：当然，我们也可以不对AssetBundle进行压缩。没有经过压缩的包体积最大，但是访问速度最快。
  - 若要使用不压缩的策略，只需要在打包的时候开启BuildAssetBundleOptions.UncompressedAssetBundle即可。
---

## 二. 打包执行代码
```csharp
[MenuItem("AssetBundle/Bundle AssetBundles")]
    static public void WindowBundle()
    {
        string buildPath = Application.dataPath + "/StreamingAssets";
        // 如果目录不存在，则创建
        if (!Directory.Exists(buildPath))
        {
            Directory.CreateDirectory(buildPath);
        }
        BuildPipeline.BuildAssetBundles(buildPath, BuildAssetBundleOptions.ChunkBasedCompression, BuildTarget.StandaloneWindows64);
        AssetDatabase.SaveAssets();
        AssetDatabase.Refresh();
        Debug.LogWarning("build success!");
    }
```

---

## 三. 设置资源包名
由于简化了代码，工作量就变成需要对资源进行分类设置AssetBundleName
- 两点建议
   1. 提供脚本批量对资源设置assetbundleName
   2. 规划好assetBundle所对应的资源类型，规划好assetBundle的数量

```csharp
static public void SetFileAssetBundleName(string filePath, string assetBundleName)
    {
        filePath = filePath.Replace('\\', '/');
        filePath = filePath.Replace(Application.dataPath, "Assets");
        AssetImporter ai = AssetImporter.GetAtPath(filePath);
        ai.assetBundleName = assetBundleName;
        ai.assetBundleVariant = "unity3d";  // 可以用作后缀，也可用来区分相同资源不同精度
    }
```

---

## 四. 加载资源
- LoadAsset：从资源包中加载指定的资源
- LoadAllAsset：加载当前资源包中所有的资源
- LoadAssetAsync：从资源包中异步加载资源
- Unload：该方法会卸载运行时内存中包含在bundle中的所有资源。
   - 当传入的参数为true，则不仅仅内存中的AssetBundle对象包含的资源会被销毁。根据这些资源实例化而来的游戏内的对象也会销毁。
   - 当传入的参数为false，则仅仅销毁内存中的AssetBundle对象包含的资源。

### 加载代码
```csharp
// 存储加载过的资源
static public Dictionary<string, AssetBundle> loadPathList = new Dictionary<string, AssetBundle>();

	static public GameObject LoadResource(string assetBundleName, string resourceName)
    {
        AssetBundle ab = AssetBundle.LoadFromFile(Application.streamingAssetsPath+"/StreamingAssets");
        if (ab == null)
            return null;
        AssetBundleManifest abm = ab.LoadAsset<AssetBundleManifest>("AssetBundleManifest");
        ab.Unload(false);
        if (abm == null)
            return null;
        string[] paths = abm.GetAllDependencies(assetBundleName);
        for (int i = 0; i < paths.Length; i++)
        {
            LoadFromFile(paths[i]);
        }
        AssetBundle reab = AssetBundle.LoadFromFile(Application.streamingAssetsPath + "/" + assetBundleName);
        GameObject go = reab.LoadAsset<GameObject>(resourceName);
        
        reab.Unload(false);
        return go;
    }

    static public bool LoadFromFile(string path)
    {
        if (loadPathList[path])
        {
            return true;
        }
        AssetBundle ab = AssetBundle.LoadFromFile(Application.streamingAssetsPath + "/" + path);
        if(ab == null)
        {
            return false;
        }
        loadPathList.Add(path, ab);
        return true;
    }
```

### 测试代码
```csharp
GameObject go = LoadAssetBundle.LoadResource("prefabs.unity3d", "Dog");
if (go != null) {
    Instantiate(go);
}
```

### 通过服务器加载资源代码
```csharp
private AssetBundleManifest MainManifest = null;
private AssetBundle MainAB = null;
// 游戏启动时可以先加载MainManifest
IEnumerator LoadMainManifest()
{
    string url = @"http://localhost:81/StreamingAssets/StreamingAssets";
    UnityWebRequest request = UnityWebRequest.GetAssetBundle(url);
    yield return request.Send();
    if (string.IsNullOrEmpty(request.error))
    {
        MainAB = (request.downloadHandler as DownloadHandlerAssetBundle).assetBundle;
        MainManifest = MainAB.LoadAsset<AssetBundleManifest>("AssetBundleManifest");
    }
    else
    {
        Debug.Log(request.error);
        yield break;
    }
}

IEnumerator LoadDependencies(string assetName)
{
    // 获取该资源所有依赖
    string[] dependencies = MainManifest.GetAllDependencies(assetName);
    foreach (var name in dependencies)
    {
        string url = @"http://localhost:81/StreamingAssets/" + name;
        UnityWebRequest request = UnityWebRequest.GetAssetBundle(url);
        yield return request.Send();
        if (string.IsNullOrEmpty(request.error))
        {
            AssetBundle ab = (request.downloadHandler as DownloadHandlerAssetBundle).assetBundle;
        }
        else
        {
            Debug.Log(request.error);
            yield break;
        }
    }
}

IEnumerator LoadAssets(string assetName, string resourceName)
{
    yield return StartCoroutine(LoadDependencies(assetName));
    string url = @"http://localhost:81/StreamingAssets/" + assetName;
    UnityWebRequest request = UnityWebRequest.GetAssetBundle(url);
    yield return request.Send();
    if (string.IsNullOrEmpty(request.error))
    {
        AssetBundle ab = (request.downloadHandler as DownloadHandlerAssetBundle).assetBundle;
        GameObject go = ab.LoadAsset<GameObject>(resourceName);
        Instantiate(go);
    }
    else
    {
        Debug.Log(request.error);
        yield break;
    }
}
```
### 测试代码：
```csharp
StartCoroutine(LoadMainManifest());
StartCoroutine(LoadAssets("prefabs.unity3d", "Role"));
```
