# VTools NPC 创建方法

## API

```text
Ventulus.VTools
VTools.Instance
```

确认存在：

```text
Int32 CreateNpc(Int32 type, Int32 liuPai, Int32 level, Int32 sex, Int32 zhengXie)
Int32 CreateNpcByTypeAndLevel(Int32 type, Int32 level, Int32 banLiuPai)
String GetNPCName(Int32 npcId)
```

## 实际验证

```text
CreateNpc(0, 33, 6, 2, 1)
→ NPC ID 20955
→ GetNPCName(20955)
→ 朱代萱
```

## NPCFactory

```text
Int32 AfterCreateNpc(
    JSONObject npcDate,
    Boolean isImportant,
    Int32 ZhiDingindex,
    Boolean isNewPlayer,
    JSONObject importantJson,
    Int32 setSex
)
```

该方法负责完成 NPC 的主要初始化，包括属性、装备、悟道、头像、背包等数据。

## 改名

```text
SkySwordKill.NextMoreCommand.Utils.NpcUtils.SetNpcName(Int32 npcId, String name)
```
