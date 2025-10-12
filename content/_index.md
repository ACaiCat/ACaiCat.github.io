---
title: "Cai's Blog"
---
<div style="text-align: left;">

# 你好呀 ~

我是Cai，一条咸鱼，**福州大学**2025届本科生，**给排水科学与工程专业**，~~也许要转专业跑路~~。喜欢玩**Terraria**和**StardewValley**，写一些Terraria相关的小项目。

## CaiのProject
### CaiBot
基于NoneBot2框架的**QQ官方Bot**，用于管理**TShock**和**tModLoader**服务器的，日活300人，目前大概900群，5000+用户。
[![Repo Card](https://opengraph.githubassets.com/githubcard/UnrealMultiple/CaiBotLite)](https://github.com/UnrealMultiple/CaiBotLite)

### TShockPlugin
TShock插件仓库，虽然说代码质量有点堪忧，但是**Can run is OK！**
[![Repo Card](https://opengraph.githubassets.com/githubcard/UnrealMultiple/TShockPlugin)](https://github.com/UnrealMultiple/TShockPlugin)


## CaiのSkill
### Git
```shell
git push --force
```

### C#
```csharp
// 学的第一门语言
// TShock插件开发的主语言，目前桌面完全不会喵~
Console.WriteLine("TShock, tModLoader, linq2db, Json.NET")
var anon= "爱音"
Console.WriteLine($"Suki！有内插字符串，内存占用又小，语法和{anon}一样糖~")
```

### Python
```python
# 学的第二门语言
# Bot开发的主语言，因为写Bot才入的Python...
print("nonebot2, fastapi, sqlalcomy, beautifulsoup4")
print("Life is short, you need Python")
```

### Go📋 
```go
// 大概是我的方向是后端开发...
fmt.Println("Hello World!")
```

### Kotlin🚧
```kotlin
// 主要是为了写个神秘想法的APP
println("Hello World?")
```

### GitHub Actions
```yaml
name: Grow cotton

on:
  push:
    branches: [ "main" ]

permissions:
  contents: write

jobs:
  grow:
    runs-on: ubuntu-latest

    strategy:
        matrix:
            ai: ["deepseek.v3", "gpt-4", "claude-4.5"]

    steps:
    - name: Grow cotton
      uses: ACaiCat/cotton@v114514
      with:
        worker: ${{ matrix.ai }}
        go_work: true
        output_path: output/

    - name: Upload cottons
      uses: actions/upload-artifact@v4
      with:
        name: cotton-${{ matrix.ai }}
        path: output/

```

</div>