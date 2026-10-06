# 《觅长生》NPC 创建方法

本仓库记录《觅长生》Mod 环境下已经实际验证过的 NPC 创建方法。

## 核心方法

### VTools

当前环境确认存在：

```text
Ventulus.VTools
```

创建 NPC：

```text
VTools.CreateNpc(type, liuPai, level, sex, zhengXie)
```

实际验证：

```text
VTools.CreateNpc(0, 33, 6, 2, 1)
```

返回：

```text
NPC ID = 20955
```

随后：

```text
GetNPCName(20955)
```

得到：

```text
朱代萱
```

## Next / VNext

剧情指令：

```text
CreateOneNpc*类型#流派#境界#性别#正邪
```

例如：

```text
CreateOneNpc*0#33#6#2#1
```

其底层对应：

```text
VTools.CreateNpc(0, 33, 6, 2, 1)
```

## NPC 改名

当前确认存在：

```text
SkySwordKill.NextMoreCommand.Utils.NpcUtils.SetNpcName(Int32 npcId, String name)
```

例如：

```text
SetNpcName(20955, "冉汐")
```

## 创建机制

VTools 并不是完全自由地从零生成 NPC。

它会读取游戏已有 NPC 模板，根据类型、流派、境界等条件筛选，然后随机选择符合条件的模板，并通过 NPCFactory.AfterCreateNpc 完成初始化。

因此：

```text
已有 NPC 模板
    ↓
条件筛选
    ↓
随机选择模板
    ↓
NPCFactory.AfterCreateNpc
    ↓
真实 NPC
```

## 结论

如果只是根据已有条件创建 NPC，优先使用 VTools.CreateNpc 或 Next/VNext 的 CreateOneNpc。

如果需要游戏运行中通过自定义 UI 输入参数创建 NPC，才需要额外开发自己的 Mod UI。

## Next Data Mod 创建纯 NPC

- [Next-NPC跟随系统.md](./Next-NPC跟随系统.md) —— 使用物品让 NPC 来/去切换。已验证玄影、素影、幽荧召返石三个实例。

## 完整示例

- [示例/](./示例/) —— 玄影、素影两个已实测通过的完整 NPC 数据，可直接复制改造
