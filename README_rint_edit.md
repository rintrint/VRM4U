# rint 改动纪录  
纪录在ruyo的基础上做出的改动  
因为上游仍然在更新，为了方便同步上游的更新，不能想改啥就改啥  

## 改动  
- 将所有UTF-8 with BOM档案改为使用UTF-8编码  
- 将所有Shift-JIS档案改为使用UTF-8编码  
- 修復导入pmx时的预设值  
- 修復导入pmx时的透明度问题  
- ~~完善bonemap over!警告讯息~~  

以下是只改了一行代码的整理  
```
"EnabledByDefault": true,
int32 ImportPriority = 120;
TArray<FString> extList = {TEXT("pmx")};
// AutoExpandCategories = (FTransform, Mesh, AdvancedDisplay) // 好像没效果
```

## 代码风格  
使用.clang-format文件  
为了合併上游的分支只对贴上的代码格式化`"editor.formatOnPaste": true`，不执行`Alt+Shift+F`  
文件是从 https://github.com/TensorWorks/UE-Clang-Format 下载的  
并做出以下改动  
```
AlignConsecutiveDeclarations: false
```

## 不改动  
其他问题  
Warning(为了方便同步上游的更新，不影响运作尽量不修復)  
```
// AllowedClasses名称问题
LogClass: Warning: Property StructProperty UVrmRuntimeSettings::AssetListObject defines MetaData key "AllowedClasses" which contains short type name "Object". Suggested pathname: "/Script/CoreUObject.Object". Module:VRM4U File:Public/VrmRuntimeSettings.h

// VrmAssetList添加了不存在的东西的问题
LogPackageName: Warning: GetLocalFullPath called on FPackagePath ../../../../../../Users/user/Desktop/MyProject/Content/MyContent/Model/test which has an unspecified header extension, and the path does not exist on disk. Assuming EPackageExtension::Asset.
LoadErrors: Warning: While trying to load package /Game/MyContent/Model/VA_test_VrmAssetList, a dependent package /Game/MyContent/Model/test was not available. Additional explanatory information follows:
FPackageName: Skipped package /Game/MyContent/Model/test has a valid, mounted, mount point but does not exist either on disk or in iostore. The uncooked file would be expected on disk at 'C:/Users/user/Desktop/MyProject/Content/MyContent/Model/test.uasset'. Perhaps it has been deleted or was not synced?
```
PMX Editor保存空白模型，导入UE5会闪退，可能是因为没有Mesh  

## 其他要注意的地方  
如果没有开启Support 16-bit Bone Index，容易出现bonemap over!警告讯息  

VrmConvertTexture.cpp  
这个档案中的大部分代码其实是在处理Material Instance的设置，而不仅仅是Texture的转换  
