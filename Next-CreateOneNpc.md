# Next / VNext CreateOneNpc

## 指令格式

```text
CreateOneNpc*类型#流派#境界#性别#正邪
```

## 示例

```text
CreateOneNpc*0#33#6#2#1
```

对应：

类型 = 0
流派 = 33
境界 = 6
性别 = 2
正邪 = 1

底层对应：

```text
VTools.CreateNpc(0, 33, 6, 2, 1)
```

## 注意

CreateOneNpc 是 Next / VNext 的剧情 / 事件指令，不是普通聊天框命令。

创建成功后可以获得 roleID 和 roleName，其中 roleID 就是新 NPC 的 ID。
