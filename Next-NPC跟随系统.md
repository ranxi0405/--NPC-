# Next Data Mod｜NPC 跟随系统

> 验证日期：2026-10-06
> 状态：✅ 已在游戏内实测通过

## 一、目标

让自定义 NPC 支持"来 / 去"两态切换：

- 玩家使用一个物品 → NPC 出现并跟随
- 再次使用 → NPC 离开
- 可以反复切换

不依赖剧情、不依赖 FungusPatch、不改动原版任何文件。

## 二、核心机制

### 2.1 跟随命令

| 命令 | 作用 |
|---|---|
| `SetNpcFollow*NPC_ID` | 让 NPC 开始跟随 |
| `SetNpcRemoveFollow*NPC_ID` | 取消跟随 |

`NPC_ID` 用**模板 ID**（如 4126 / 4200），不是运行时 ID。

### 2.2 触发方式：DialogTrigger（推荐）

玩家使用物品 → DialogTrigger 检查 itemID 和状态变量 → 分派到两个事件之一 → SetNpcFollow / SetNpcRemoveFollow

**关键**：不是靠事件内部的 `Trigger*AfterUseItem` 自注册，而是靠 DialogTrigger 的 `type: 使用物品`。

## 三、文件清单

| 文件 | 作用 |
|---|---|
| `Data/ItemJsonData/XXX.json` | 召返物品 |
| `NData/DialogEvent/XXX.json` | 来/去 两个事件 + 发放事件 |
| `NData/DialogTrigger/XXX跟随触发器.json` | 使用物品 → 分派 |
| `NData/DialogTrigger/XXX-发放.json` | 一次性发放物品 |

## 四、物品格式（已验证）

```json
{
  "id": 9994200,
  "ItemIcon": 1017,
  "maxNum": 1,
  "name": "玄影信物",
  "FaBaoType": "",
  "Affix": [],
  "TuJianType": 0,
  "ShopType": 99,
  "ItemFlag": [],
  "WuWeiType": 0,
  "ShuXingType": 0,
  "type": 16,
  "quality": 6,
  "typePinJie": 0,
  "StuTime": 0,
  "seid": [],
  "vagueType": 1,
  "price": 0,
  "desc": "让玄影进行跟随",
  "desc2": "使用后她会自动回到你的身边",
  "CanSale": 1,
  "DanDu": 0,
  "CanUse": 1,
  "NPCCanUse": 0,
  "yaoZhi1": 0,
  "yaoZhi2": 0,
  "yaoZhi3": 0,
  "wuDao": []
}
关键字段说明
type: 16：特殊类型，使用后不消耗（可重复使用）

seid: []：留空。不要写 seid: [51] —— 会报"物品XXX定义了seid51，但是物品seid51表中没有此物品的对应数据"

ItemFlag: []：留空。幽荧原版用 [53]，那是她特有的标记

CanUse: 1：让玩家能主动"使用"

maxNum: 1：不可堆叠

五、DialogTrigger 格式（已验证）
json
[
  {
    "id": "玄影跟随-切换",
    "type": "使用物品",
    "condition": "itemID==9994200 && GetInt(\"玄影跟随\")<=0",
    "triggerEvent": "玄影跟随-用信物",
    "once": false
  },
  {
    "id": "玄影离开-切换",
    "type": "使用物品",
    "condition": "itemID==9994200 && GetInt(\"玄影跟随\")>=1",
    "triggerEvent": "玄影离开-用信物",
    "once": false
  }
]
关键点
type: "使用物品"（不带"后"）—— 带"后"会导致游戏启动失败

condition 里用 itemID==XXX 和 GetInt("变量名") 组合判断状态

once: false —— 需要可反复触发

triggerEvent 指向 DialogEvent 里的事件 ID

六、DialogEvent 格式（已验证）
json
[
  {
    "id": "玄影跟随-用信物",
    "character": { "旁白": 0, "主角": 1, "玄影": 4200 },
    "dialog": [
      "旁白#黑玉信物微微发烫，一道玄色身影无声出现在你面前。",
      "玄影#……",
      "SetNpcFollow*4200",
      "SetInt*玄影跟随#1"
    ],
    "option": []
  },
  {
    "id": "玄影离开-用信物",
    "character": { "旁白": 0, "主角": 1, "玄影": 4200 },
    "dialog": [
      "玄影#（停下脚步，回眸看你）",
      "SetNpcRemoveFollow*4200",
      "SetInt*玄影跟随#0"
    ],
    "option": []
  }
]
关键点
character 里定义说话人：旁白: 0、主角: 1、NPC 用模板 ID

dialog 数组每项两种格式：角色名#文本（对话）或 命令*参数1#参数2（命令）

SetInt*变量名#值 是状态记录的关键 —— 触发器靠它判断

七、状态变量命名建议
用唯一前缀：玄影跟随、素影跟随、幽荧召返状态

不要用通用名如 跟随状态、flag1

发放标记位用 XXX已发放

八、已验证的三个 NPC
NPC	模板 ID	信物 ID	状态变量	发放触发器
玄影	4200	9994200	玄影跟随	After(1,1,1)
素影	4201	9994201	素影跟随	After(1,1,1)
幽荧召返石	4126（借用）	9994913	幽荧召返状态	GetInt("幽荧召返已发放")<=0
九、发放方式的两种写法
写法 A：按时间（适合新档）
json
{
  "type": "时间变化",
  "condition": "After(1,1,1)",
  "triggerEvent": "XXX-信物发放",
  "once": true
}
缺陷：老档已经过了第 1 年，不会触发。

写法 B：按标记位（新档老档都行，推荐）
json
{
  "type": "时间变化",
  "condition": "GetInt(\"XXX已发放\")<=0",
  "triggerEvent": "XXX-发放",
  "once": false
}
发放事件里：

text
SetInt*XXX已发放#1    ← 必须先设标记
AddItem*物品ID#1#1
ShowTip*获得XXX#7
顺序：SetInt 要在 AddItem 之前，否则会重复发放。

十、踩坑记录
坑	表现	原因	解法
物品引用了未定义的 seid	游戏启动失败	seid 写了 ID 但没定义	seid: [] 留空
type: 使用物品后	游戏启动失败	Next 不支持"后"后缀	改成 type: 使用物品
事件内 Trigger*AfterUseItem	用了物品无反应	该命令不是注册语义	用 DialogTrigger 分派
新 mod enable 默认 false	mod 数据不被加载	Next 默认禁用新目录	手动改 nextModSetting.json
发放触发器 once:true + After	老档不触发	老档已过该时间点	用标记位写法
幽荧跟随石无法切换	只能跟随不能离开	原版无状态判断	另做独立召返石
十一、向幽荧 mod 借用 NPC 的注意
如果召返石的目标 NPC 是别的 mod 提供的（如幽荧 4126）：

SetNpcFollow*4126 只在玩家装了幽荧 mod 时有效

不装的话会静默失败

建议在物品描述里注明前置依赖

十二、实测通过的完整链路
新档第 1 年 → 发放触发器 → AddItem 信物 + SetInt 已发放=1 → 玩家使用信物 → DialogTrigger 检查状态 → SetNpcFollow / SetNpcRemoveFollow → 状态切换
