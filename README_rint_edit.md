# rint 改動紀錄  
紀錄在ruyo的基礎上做出的改動  
因為上遊仍然在更新，為了方便同步上遊的更新，不能想改啥就改啥  

| UE5版本   | 插件版本     |
|-----------|-------------|
| UE5.5     | VRM4U_UE5.5 |
| UE5.6     | master分支  |

## 改動  
- ~~將所有UTF-8 with BOM檔案改為使用UTF-8編碼~~  
- ~~將所有Shift-JIS檔案改為使用UTF-8編碼~~  
- 修復導入pmx時的預設值  
- 修復導入pmx時的透明度問題  
- ~~完善bonemap over!警告訊息~~  
- 修改ImportPriority無須重啟UE5  

以下是只改了一行代碼的整理  
```
"EnabledByDefault": true,
int32 ImportPriority = 120;
TArray<FString> extList = {TEXT("pmx")};
// AutoExpandCategories = (FTransform, Mesh, AdvancedDisplay) // 好像沒效果
```

修改的源代碼文件整理  
- VrmRuntimeSettings.h
- VRM4UImporterFactory.cpp
- VrmConvertTexture.cpp

## 代碼風格  
使用.clang-format文件  
為了合併上遊的分支只對貼上的代碼格式化`"editor.formatOnPaste": true`，不執行`Alt+Shift+F`  
文件是從 https://github.com/TensorWorks/UE-Clang-Format 下載的  
並做出以下改動  
```
AlignConsecutiveDeclarations: false
```

## 不改動  
其他問題  
Warning(為了方便同步上遊的更新，不影響運作盡量不修復)  
```
// AllowedClasses名稱問題
LogClass: Warning: Property StructProperty UVrmRuntimeSettings::AssetListObject defines MetaData key "AllowedClasses" which contains short type name "Object". Suggested pathname: "/Script/CoreUObject.Object". Module:VRM4U File:Public/VrmRuntimeSettings.h

// VrmAssetList添加了不存在的東西的問題
LogPackageName: Warning: GetLocalFullPath called on FPackagePath ../../../../../../Users/user/Desktop/MyProject/Content/MyContent/Model/test which has an unspecified header extension, and the path does not exist on disk. Assuming EPackageExtension::Asset.
LoadErrors: Warning: While trying to load package /Game/MyContent/Model/VA_test_VrmAssetList, a dependent package /Game/MyContent/Model/test was not available. Additional explanatory information follows:
FPackageName: Skipped package /Game/MyContent/Model/test has a valid, mounted, mount point but does not exist either on disk or in iostore. The uncooked file would be expected on disk at 'C:/Users/user/Desktop/MyProject/Content/MyContent/Model/test.uasset'. Perhaps it has been deleted or was not synced?
```
PMX Editor保存空白模型，導入UE5會閃退，可能是因為沒有Mesh  

## 其他要註意的地方  
如果有開啟Support 16-bit Bone Index，則可以無視bonemap over!警告訊息  

VrmConvertTexture.cpp  
這個檔案中的大部分代碼其實是在處理Material Instance的設置，而不僅僅是Texture的轉換  
