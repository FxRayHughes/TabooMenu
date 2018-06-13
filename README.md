
# TabooMenu
一款高自由，高效率，功能丰富的菜单插件。  
部分代码来自 **filoghost** 的开源项目 **ChestCommands**。

---
### 目录

+ [插件功能](#插件功能)
+ [插件配置](#插件配置)
+ [插件命令](#插件命令)
+ [插件权限](#插件权限)
+ [创建菜单](#创建菜单)
+ [菜单节点](#菜单节点)
+ [物品示范](#物品节点（示范）)
+ [物品节点](#物品节点（详细）)
  + [序号及坐标](#序号及坐标)
  + [材质及数量](#材质及数量)
  + [名称及描述](#名称及描述)
  + [损伤值](#损伤值)
  + [物品克隆](#物品克隆)
  + [附魔效果](#附魔效果)
  + [熟悉隐藏](#熟悉隐藏)
  + [头颅皮肤](#头颅皮肤)
  + [皮革颜色](#皮革颜色)
  + [查看权限](#查看权限)
  + [点击权限](#点击权限（保留字）)
  + [物品动作](#物品动作（点击命令）)
  + [物品条件](#物品条件（表达式）)
+ [开发者 API](#开发者 API)
  + [事件](#事件)
  + [方法](#方法)
  + [命令注册](#命令注册)

---
### 插件功能

+ 菜单中所有节点均忽略大小写判断。
+ 删除 open-with-item 节点，推荐使用 CustomJoinItem 或 BaseItem 当做菜单物品。
+ 使用 ChestCommands 格式载入，复制文件便可直接使用。
+ 保留 ChestCommands 物品配置中的大部分节点。
+ 保留 ChestCommands 的 51 项物品别名。
+ 保留 ChestCommands 的 cc 命令别名，直接替换也不会影响其他插件打开菜单。
+ 更简单的物品位置写法并保留 POSITION-X/Y 节点。
+ 更简单的损伤值写法并保留 DATA-VALUE 节点。
+ 更简单的附魔写法并保留 ENCHANTMENT 节点。
+ 更强大的指令写法并保留 COMMAND 节点。
+ 新增 ATTRIBUTE-HIDE 节点用于决定是否隐藏属性。
+ 新增 SLOT-COPY 节点用于复制该物品到其他位置。
+ 新增 SHINY 节点用于决定是否显示附魔特效。
+ 新增 REQUIREMENT 节点并保留 PRICE, POINTS, LEVEL 节点。
+ 新增 PERMISSION-BYPASS 节点用于跳过页面的权限判断。
+ 强过 DeluxeMenu 无数倍的条件表达式写法。
+ 物品 ID 容错。
+ 7 种命令触发方式。
+ 11 种命令执行方式。
+ PlaceholderAPI 支持。
+ 更智能的界面刷新方式。
+ 更智能的命令系统。
+ ...

---
### 插件配置

```yaml
Settings:
  # 默认颜色
  DefaultColor:
    Name: '&f'
    Lore: '&7'
  # 相似度匹配阈值
  SimilarDegreeLimit: 0.7
  # 物品点击间隔
  AntiClickApamDelay: 200
```

---
### 插件命令

TabooMenu 保留了 ChestCommands 插件的 `cc` 指令别名，所以就算直接替换插件也不会出问题。

> 你要是之前用 /chestcommands open 就当我开玩笑

|  命令  |  作用  |
| --- | --- |
|  /taboomenu open [菜单] <玩家>  |  打开菜单  |
|  /taboomenu list  |  列出菜单  |
|  /taboomenu reload  |  重载插件  |

如果菜单名含有空格，则使用双下划线 `__` 代替，且允许使用 `TAB` 补全菜单名。

---
### 插件权限

|  权限  | 作用 |
| --- | --- |
| taboomenu.command.help | 查看命令帮助 |
| taboomenu.command.open | 使用打开菜单命令 |
| taboomenu.command.open.other | 使用打开菜单命令（向其他玩家） |
| taboomenu.command.list | 使用列出菜单命令 |
| taboomenu.commsnd.reload | 使用重载插件命令 |

---
### 创建菜单
```yaml
在路径 /plugins/TabooMenu/menu/ 内创建任意以 yml 结尾编码为 UTF-8 的文件。
```

---
### 菜单节点
```yaml
# 菜单设置节点名
menu-settings:
  # 菜单名
  name: '测试菜单'
  # 菜单行数
  rows: 1
  # 菜单打开所需要的命令，不同指令使用 ";" 符号区分
  command: 'menu;help'
  # 菜单自动刷新时间，单位：秒
  auto-refresh: 1
  # 菜单打开执行动作
  open-action: 'sound: BLOCK_CHEST_OPEN-1-1'
  # 菜单打开是否跳过权限判断
  permission-bypass: false
```

---
### 物品节点（示范）
```yaml
# 物品位置，如果该名称非数字，则需要使用 POSITION-X/Y 节点。
0:
  # 物品名称，如果你不知道你要使用的物品名称，你可以写上印象中大概的英文名。
  # TabooMenu 会为你自动联想最相似的物品。
  id: dixxxx sword
  # 物品名称
  name: '&b钻石剑'
  # 物品描述
  lore:
  - ''
  - '&7使用 &e100 &7点券购买'
  # 物品命令
  command: 
  - ‘console: points take %player_name% 100'
  - 'console: give %player_name% diamond_sword 1'
  # 物品条件
  requirement:
  # 物品条件表达式
  - expression: '%playerpoints_points% > 100'
    # 物品条件显示物品（如果条件达成则显示该物品）
    item:
      id: barrier
      name: '&b钻石剑'
      lore:
      - ''
      - '&7使用 &e100 &7点券购买'
      - '&4你的点券不足'
      command: 'tell: &7你的点券不足无法购买'
```
你可以使用最少的配置做出最丰富的内容，我相信 TabooMenu 的自由度可以媲美市面上任何一款菜单插件。

---
### 物品节点（详细）

#### 序号及坐标

```yaml
# 序号方式
0:
  id: diamond
# 坐标方式
item:
  id: diamond
  position-x: 0
  position-y: 0
```

可以使用以上两种方式设定物品位置，用过 ChestCommands 的话肯定对第二种方式深有感触。

**序号图**  
![](https://i.loli.net/2018/06/13/5b20bd91b2355.png)

**坐标图**  
![](https://i.loli.net/2018/06/13/5b20be2fc6b4c.png)

#### 材质及数量

```yaml
0:
  id: iron chestplate
1:
  id: 276
2:
  id: wool:14
  amount: 1
3:
  id: diamond
  amount: 10
```

> 材质节点继承了 ChestCommands 的优良传统，你可以使用数字或是英文名称，甚至是别名。

TabooMenu 的联想判断是智能的，而不是简单的判断 `golden apple` 与 `golden` 的区别，你可以将 `diamond sword` 写成 `sword with diamond` 都可以。

```yaml
0:
  id: sword with diamond
```

你可以在配置文件中更改相似度判断的阈值，过低或者过高都会导致判断出现误差，所以最好不要过于依赖 TabooMenu 的材质联想。

```yaml
Settings:
  # 当相似度达到 70% 时结束判断
  SimilarDegreeLimit: 0.7
```

如果你记不住材质名称或是数字ID，你可以在配置文件中添加新的材质别名，但是要注意这里别名对应的材质名称必须要准确无误。

```yaml
Aliases:
  钻石剑: DIAMOND_SWORD
```

#### 名称及描述

```yaml
0:
  name: '物品名称'
  lore:
  - '第一行描述'
  - '第二行描述'
```

这个不用我多讲了吧，菜单插件都能做到。  
当然你还可以使用 Yaml 的特殊格式：

```yaml
0:
  name: '物品名称'
  lore: ['第一行描述', '第二行描述']
```

#### 损伤值

```yaml
0:
  id: wool:14
1:
  id: wool
  data-value: 14
```

这两种写法都是可以的，但是我不建议使用第二种，那是原插件的保留字而且过于繁琐。

#### 物品克隆

```yaml
0:
  id: diamond
  slot-copy:
  - 1
  - 2
  - 3
  - 4
  - 5
  - 6
  - 7
  - 8
9:
  id: gold ingot
  slot-copy: [10, 11, 13, 14, 15, 16, 17]
```

用于将该物品克隆到其他位置，各项节点相同。  
似乎 DeluxeMenu 也有这个功能？

#### 附魔效果


```yaml
0:
  id: diamond sword
  shiny: true
1:
  id: diamond sword
  enchantment: 'anything'
```

和 CheatCommands 一样附魔内容不再显示，仅仅显示附魔特效，以至于原插件的保留字... 里面写啥都行。

#### 属性隐藏

```yaml
0:
  id: wood sword
  hide-attribute: false
```

和 ChestCommands 不同，TabooMenu 允许使用者决定是否隐藏物品属性，这个值默认是开启的。

#### 头颅皮肤

```yaml
0:
  id: head:3
  skull-owner: 'BlackSKY'
1:
  id: head:3
  skull-owner: '%player_name%'
```

如果你的服务器安装了 `PlaceholderAPI` 插件，你可以在这里使用其他变量。

#### 皮革颜色

```yaml
0:
  id: leather chestplate
  color: 255,255,0
```

用于设置皮革护甲的颜色

#### 查看权限

```yaml
0:
  id: book and quill
  permission-view: ‘rule.view.1‘
1:
  id: bokk and quill
  view-permission: ’rule.view.2‘
```

> 第二种写法是旧插件的保留字，因为和 `permission` 不对称所以调换了两个单词的顺序。
  
如果玩家拥有该权限，则可以查看到该物品。  
没有权限时点击对应的位置也是无效的。

#### 点击条件（保留字）

```yaml
0:
  id: diamond
  # 点击所需权限
  permission: 'diamond.click'
  # 点击所需权限提示
  permission-message: ’你没有权限这么做‘
1:
  id: diamond
  # 点击所需金币（需要经济插件）
  price: 100
2:
  id: diamond
  # 点击所需权限（需要 PlayerPoints 插件）
  points: 100
3
  id: diamond
  # 点击所需经验
  level: 100
```

旧插件的保留字，用过 ChestCommands 的朋友应该都了解，以上条件不会影响玩家查看物品。

#### 物品动作（点击命令）

```yaml
0:
  id: diamond
  name: '回城'
  command: 'spawn'
```

保留 ChestCommands 的原本命令格式，你可以在原本格式的基础上做到这种效果。

```yaml
1:
  id: diamond
  name: ’左键创造，右键生存‘
  command:
  - type: LEFT
    list: 'op: gamemode 1'
  - type: RIGHT
    list: 'op: gamemode 0'
```

通过 `type` 节点可以判断点击类型，多个类型可以用 "|" 符号区分如：`LEFT|RIGHT`

> 子节点 `list` 与 `command` 意义相同。

**可用点击类型:**

```yaml
# 左键
LEFT
# 右键
RIGYT
# 潜行左键
SHIFT_LEFT
# 潜行右键
SHIFT_RIGHT
# 鼠标中键
MIDDLE
# 丢弃
DROP
# 全部丢弃
CONTROL_DROP
# 全部
ALL
```

一拳一个 dm 怪，按在地上锤。

```yaml
# DeluxeMenu 格式
items:
  a:
    slot: 0
    name: '左键生存，右键创造'
    material: diamond
    left_click_commands:
    - '[player] gamemode 1'
    right_click_commands:
    - '[player] gamemode 0'
```

与 ChestCommands 相同，你可以在单行写多条指令用 ";" 符号区分。也可以写成多行：

```yaml
0:
  id: diamond
  command: 'give %player_name% diamond;give %player_name% iron'
1:
  id: diamond
  command:
  - 'give %player_name% diamond'
  - 'give %player_name% iron'
2:
  id: diamond
  command:
  - type: ALL
    list:
    - 'give %player_name% diamond'
    - 'give %player_name% iron'
```

与 ChestCommands 相同，你可以在指令中使用某些字符，来改变执行方式。


```yaml
0:
  id: diamond
  command: 'tell: 点击提示'
```

**可用执行方式:**

```yaml
# 发送信息
tell, send, message
# 发送公告
broadcast
# 执行管理员命令
op
# 执行后台命令
console
# 执行玩家命令
player 或 省略
# 传送服务器（需要 BungeeCord）
server
# 播放音效
sound
# 打开菜单
open
# 打开菜单（跳过权限判断）
open-force
# 延迟执行（单位：ticks）
delay
```

以下几种特殊方式需要注意使用规范，其他类型按照正常格式执行。

**特殊方式规范:**

```yaml
# 播放音效
command: 'sound: BLOCK_ANVIL_USE-1-1'
# 打开菜单
command: 'open: example.yml'
# 延迟执行（仅允许整数）
command: 'delay: 20'
```

指令执行过成为从上到下，在中间添加 `delay` 类型则会延迟执行接下来的动作。

```yaml
0:
  id: diamond
  command:
  - 'tell: 三秒后回城'
  - 'delay: 90'
  - 'op: spawn'
```

#### 物品条件（表达式）

你可以使用 `requirement` 节点来添加自定义条件表达式：

```yaml
0:
  id: diamond
  requirement:
  - expression: '!player.hasPermission("diamond")'
    item:
      id: barrier
```

节点 `expression` 为表达式文本，并且添加了以下可用参数：

**玩家对象**

```JavaScript
player.getLevel() > 100
```

**服务器对象**

```JavaScript
bukkit.getOnlinePlayers().size() > 10
```

**点击方式文本**

```JavaScript
clickType == "RIGHT"
```

这里有一点需要注意，当玩家查看物品或是点击物品时都会执行表达式。但是在玩家查看物品时，`clickType` 则为 `VIEW`。所以在判断点击方式时，应该使用以下写法：

```JavaScript
expression: 'clickType == "VIEW" || clickType == "RIGHT"'
```

> 如果你不了解编程语言中的逻辑符号，不建议在表达式中判断点击方式。

如果条件表达式返回 `true`，则为玩家展示子节点 `item` 中的物品：

```yaml
0:
  id: diamond
  command: 'tell: 权限符合'
  requirement:
  - expression: '!player.hasPermission("diamond")'
    item:
      id: barrier
      command: 'tell: 权限不足'
```

> 你可以在 `item` 节点中的物品使用除 `requirement` 外的任何节点。

重写物品嫌麻烦？你可以省略 `item` 直接使用 `command` 代替：

```yaml
0:
  id: diamond
  command: 'tell: 权限符合'
  requirement:
  - expression: '!player.hasPermission("diamond")'
    command: 'tell: 权限不足'
```

> 猜一猜会显示什么物品？

你还可以将多个表达式叠加使用：

```yaml
0:
  id: diamond
  command: 'tell: 基础权限'
  requirement:
  - expression: 'player.hasPermission("vip3")'
    command: 'tell: vip3 权限'
  - expression: 'player.hasPermission("vip2")'
    command: 'tell: vip2 权限'
  - expression: 'player.hasPermission("vip")'
    command: 'tell: vip 权限'
```

> 比起某 menu 是不是更加直观而简单？

使用 `priority` 节点可以为表达式设置优先级，判断顺序为从小到大。

```yaml
0:
  id: diamond
  command: 'tell: 基础权限'
  requirement:
  - expression: 'player.hasPermission("vip")'
    command: 'tell: vip 权限'
    priority: 3
  - expression: 'player.hasPermission("vip2")'
    command: 'tell: vip2 权限'
    priority: 2
  - expression: 'player.hasPermission("vip3")'
    command: 'tell: vip3 权限'
    priority: 1
```

> 省略优先级则以从上到下的顺序判断

表达式里可以使用 `PlaceholderAPI` 变量，但是会降低运行速度。  
如果不需要变量识别，可以启用预编译功能来提前编译表达式以提升运行速度。

```yaml
0:
  id: diamond
  command: 'tell: 权限符合'
  requirement:
  - expression: '!player.hasPermission("diamond")'
    command: 'tell: 权限不足'
    precompile: true
1:
  id: diamond
  command: 'tell: 等级符合'
  requirement:
  - expression: '%player_level% < 10'
    command: 'tell: 等级不足'
    precompile: false
```

>  如果启用预编译，则无法使用 `PlaceholderAPI` 变量

---
### 开发者 API

#### 事件

TabooMenu 提供了三种关于菜单的事件。

```java

/**
 * 菜单物品点击事件
 */
 
@Override
public void onClick(IconClickEvent e) {
    e.getPlayer().sendMessage("点击菜单：" + e.getMenu().getName());
}


/**
 * 菜单物品查看事件
 */
 
@Override
public void onView(IconViewEvent e) {
    e.getPlayer().sendMessage("查看菜单：" + e.getMenu().getName());
}


/**
 * 菜单打开事件
 */
 
@Override
public void onOpen(MenuOpenEvent e) {
    e.getPlayer().sendMessage("打开菜单：" + e.getMenu().getName());
}
```

如果你在 `IconViewEvent` 中取消了事件。那么物品将不会展示给玩家，但是玩家依旧可以点击对应位置来触发效果，所以还需要在 `IconClickEvent` 中进行二次判断。

#### 方法

```java
TabooMenuAPI.openMenu(Player 玩家, String 菜单名, Boolean 是否跳过权限判断)
TabooMenuAPI.getCurrentMenu(Player 玩家)
TabooMenuAPI.getMenus()
```

TabooMenu 提供了以上 3 种可供直接调用的方法，用于打开菜单和获取菜单。

```java
// 菜单已打开
OPENED
// 权限不足
NO_PERMISSION
// 菜单不存在
NOT_FOUND_MENU
// 其他错误
UNKNOWN
```

openMenu 方法会返回 `MenuState` 枚举类，来判断方法的执行结果。

#### 指令注册

你可以通过 `IconCommandSerializer` 注册自定义的指令执行方式。

**创建指令执行类**

```java
public class TitleIconCommand extends AbstractIconCommand {

    public TitleIconCommand(String command) {
        super(command);
    }

    @Override
    public void execute(Player player) {
        TitleUtils.sendTitle(player, command, "", 10, 40, 10):
    }
}
```

**注册指令执行类**

```java
public class Main extends JavaPlugin {

    @Override
    public void onEnable() {
        IconCommandSerializer.registerCommand("title:", TitleIconCommand.class);
    }
}
```
