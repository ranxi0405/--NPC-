# 示例 NPC

本目录存放两个已经验证通过的完整 NPC 示例，可以直接复制改造成新 NPC。

## 目录

- `玄影/` —— 模板 ID 4200，信物 ID 9994200，还附带一个"幽荧召返石"（9994913）
- `素影/` —— 模板 ID 4201，信物 ID 9994201

## 怎么用

每个示例的目录结构：
玄影/
├── modConfig.json ← 注意：实际部署时要放到 Config/ 下
├── Data/
│ ├── NPCImportantDate.json
│ ├── NPCLeiXingDate.json
│ ├── AvatarJsonData.json
│ ├── NPCWuDaoJson.json
│ ├── NPCChengHaoData.json
│ ├── NpcTalkSpecialNPCAddress.json
│ └── ItemJsonData/信物.json
└── NData/
├── DialogEvent/跟随事件.json
└── DialogTrigger/
├── 跟随触发器.json
└── 发放触发器.json

text

**实际部署结构**（放到游戏里）：
本地Mod测试/你的NPC名/plugins/Next/mod你的NPC名/
├── Config/modConfig.json ← modConfig 在这里
├── Data/
└── NData/

text

## 改造步骤

1. 复制一份 `玄影/` 或 `素影/` 目录
2. 把所有 ID 替换成新的、不冲突的段位（参考下方"ID 分配表"）
3. 把所有"玄影"或"素影"文字替换成新 NPC 名
4. 调整 `modConfig.json` 的 `Name` 和 `Description`
5. 把 `modConfig.json` 挪到 `Config/` 目录下
6. 部署到 `本地Mod测试/你的NPC名/plugins/Next/mod你的NPC名/`
7. 启动游戏一次，退出，编辑 `nextModSetting.json` 把新 mod 的 `enable` 改成 `true`
8. 再启动游戏，验证

## ID 分配表（不冲突即可）

| 用途 | 玄影 | 素影 | 下一个可用段 |
|---|---|---|---|
| NPC 模板 ID | 4200 | 4201 | 4202 起 |
| 流派 / 类型 ID | 8200 | 8201 | 8202 起 |
| 等级模板 ID | 42001–42015 | 42101–42115 | 42201 起 |
| 称号 ID | 4200 | 4201 | 与模板 ID 同步 |
| 信物物品 ID | 9994200 | 9994201 | 9994202 起 |

幽荧 mod 占用段：4000–4300、8000–8300、12000–12095、999900–999983。**避开这些**。

## 依赖

- `玄影/` 里的 `幽荧召返.json` 和 `幽荧召返-*.json` **依赖幽荧 mod**（引用 NPC 4126）。如果不要这个功能，删掉这三个文件即可。
- 其余文件均自包含，不依赖任何外部 mod。
