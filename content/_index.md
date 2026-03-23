---
title: "Cai's Blog"
---
<div style="text-align: left;">

## 你好呀 ~

我是Cai，一条咸鱼，**福州大学**2025级本科生，**软件工程专业**。玩**Terraria**和一堆乱七八糟的游戏，写一些Terraria相关的小项目。

## Org

### [@Pryaxis](https://github.com/Pryaxis)

维护TShock

### [@west2-online](https://github.com/west2-online)

福uu开发组打杂

## Project

### TShock

强大的反作弊、可扩展Terraria服务端

[![Repo Card](https://opengraph.githubassets.com/githubcard/Pryaxis/TShock)](https://github.com/Pryaxis/TShock)

### CaiBotLite

基于NoneBot2框架的**QQ官方Bot**，用于管理**TShock**和**tModLoader**服务器的，日活300人，目前大概900群，5000+用户。
[![Repo Card](https://opengraph.githubassets.com/githubcard/UnrealMultiple/CaiBotLite)](https://github.com/UnrealMultiple/CaiBotLite)

### TShockPlugin

TShock插件仓库，虽然说代码质量有点堪忧，但是**Can run is OK！**
[![Repo Card](https://opengraph.githubassets.com/githubcard/UnrealMultiple/TShockPlugin)](https://github.com/UnrealMultiple/TShockPlugin)

## Skill

### Git

```shell
# 什么时候被队友草飞...
git push --force
```

### CSharp

```csharp
// 学的第一门语言
// TShock插件开发的主语言，目前桌面完全不会喵~
Console.WriteLine("TShock, tModLoader, linq2db, Json.NET")
Console.WriteLine($"Suki！有内插字符串，内存占用又小，性能又高")
await DoAnything("超级强的异步支持!")
```

### Python

```python
# 学的第二门语言
# Bot开发的主语言，因为写Bot才入的Python...
print("nonebot2, fastapi, sqlalchemy, beautifulsoup4")

# 语法简洁，糖完了
print("Life is short, you need Python")
```

### Go

```go
// 想去学后端，狠狠的赚米
if err != nil {
    return fmt.Errorf("语法好简洁，一点糖都没有: %w", err)
}

```

### Kotlin

```kotlin
// 学的是Android Compose开发，纯兴趣
data class BigSugar(
  val hello: String = "200%糖"
)
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
            ai: ["deepseek.v3", "gpt-5", "claude-4.6"]

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
