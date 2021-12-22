# 目录
- [1. 生成属于你自己的宝可梦](#1-生成属于你自己的宝可梦)
  - [1.1. Pokemon Showdown 网站](#11-pokemon-showdown-网站)
  - [1.2. Showdown 格式](#12-showdown-格式)
  - [1.3. 创建属于你自己的宝可梦（以剑盾为例）](#13-创建属于你自己的宝可梦以剑盾为例)
    - [1.3.1. 我不熟悉英文，如何找到想要的宝可梦/持有物/性格/技能？](#131-我不熟悉英文如何找到想要的宝可梦持有物性格技能)
- [2. 使用 Dogeee 连接交换/测坑闪](#2-使用-dogeee-连接交换测坑闪)
  - [2.1. 获取自己的 SID & TID / 测闪帧](#21-获取自己的-sid--tid--测闪帧)
  - [2.2. 通过连接交换生成自ID的宝可梦](#22-通过连接交换生成自id的宝可梦)
- [3. 扩展知识](#3-扩展知识)
  - [3.1. 大脑升级：表表ID/里里ID](#31-大脑升级表表id里里id)
  - [3.2. 附加 showdown 指令](#32-附加-showdown-指令)
    - [3.2.1. Header——请求头](#321-header请求头)
    - [3.2.2. Body——可选指令](#322-body可选指令)
      - [3.2.2.1. 训练师数据](#3221-训练师数据)
      - [3.2.2.2. 球种/闪光类型/地区/亲密度/超级巨化](#3222-球种闪光类型地区亲密度超级巨化)
        - [3.2.2.2.1. 球种翻译](#32221-球种翻译)
        - [3.2.2.2.2. 异色条件自查](#32222-异色条件自查)
        - [3.2.2.2.3. 可超级巨化的宝可梦](#32223-可超级巨化的宝可梦)
  - [3.3. 生成宝可梦蛋](#33-生成宝可梦蛋)
  - [3.4. 生成配信宝可梦](#34-生成配信宝可梦)
  - [3.5. Batch Commands](#35-batch-commands)
    - [3.5.1. 相遇地点代码（SW/SH）](#351-相遇地点代码swsh)
    - [3.5.2. 薄荷代码表](#352-薄荷代码表)
- [4. 其他工具](#4-其他工具)
  - [4.1. PKHeX](#41-pkhex)
  - [4.2. 神奇宝贝百科](#42-神奇宝贝百科)
  - [4.3. 宝可梦育成论-剑盾](#43-宝可梦育成论-剑盾)

<div STYLE="page-break-after: always;"></div>

# 1. 生成属于你自己的宝可梦
## 1.1. Pokemon Showdown 网站
  [Pokemon Showdown](https://play.pokemonshowdown.com/teambuilder) 是一款宝可梦免费且开源的在线对战模拟器，用 JavaScript 和 Node.js 编写，供宝可梦对战粉丝在网络上交流和进行宝可梦对战。鉴于一些宝可梦拥有高种族值或特殊的技能， Pokemon Showdown 根据玩家使用它们的频率对宝可梦进行分级：
- **Uber。** 这一等级的宝可梦并非通过使用频率划分，而是被禁用频率，一般而言在 OU 级中过于强大的宝可梦会被列为 Uber ，就像剑盾级别对战中，部分特别宝可梦 **只允许有1只** 参赛。
- **OU（OverUsed，过度使用）。** 游戏中大多数强大的、出战频率高的宝可梦都被设置在 OU 级中。
- **UU（UnderUsed，较多使用）。** UU 级的宝可梦使用频率比 OU 级少一些。
- **RU（RarelyUsed，较少使用）。** RU 级包含了一些使用次数少于 UU 级的宝可梦。
- **NU（NeverUsed，从未使用）。** 几乎没有上场机会的可怜宝可梦。

## 1.2. Showdown 格式
Showdown Format 是一种文本型格式，就像在宝可梦级别对战中查看队伍详情一样，它可以清晰地表示宝可梦详细信息，其强大的泛用性使其成为目前最常用的表现形式，基本结构如下：   
>   Venusaur (F) @ Life Orb  
    Ability: Chlorophyll  
    Level: 100
    IVs: 0 Atk  
    EVs: 4 Def / 252 SpA / 252 Spe  
    Timid Nature  
    - Solar Beam  
    - Toxic  
    - Swords Dance  
    - Endure
<div STYLE="page-break-after: always;"></div>

字段对应的含义如下（ [  ]为变量 ）:

| 语法                | 含义     | 备注                                 |
| :------------------ | :------- | :----------------------------------- |
| [Pokemon]           | 宝可梦名 | showdown首行必填项     |
| @ [Item]            | 持有物   | showdown首行内容，默认空值         |
| [Ability]           | 特性     |                                |
| Level: [1-100]      | 等级     | 默认值100                  |
| IVs：               | 个体     | 默认值全31（6V）           |
| Evs：               | 努力值   | 默认值0，最大有效值252，总努力值≤510 |
| [Naturename] Nature | 性格     |                                    |
| - Move1             | - 技能1  |                                     
| - Move2             | - 技能2  |                                    
| - Move3             | - 技能3  |                                  
| - Move4             | - 技能4  |                                     |

## 1.3. 创建属于你自己的宝可梦（以剑盾为例）
- 打开  [Pokemon Showdown](https://play.pokemonshowdown.com/teambuilder)
- 点击 New Team图标
  ![图 6](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640088727703.png)  
- 点击 Select a Format
  ![图 7](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640088860066.png)   
- 在 Sw/Sh Single/Double 分类下选择你想要的对战级别，新手建议选择 OU 级
  ![图 11](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640089167922.png)  
- 点击 Add Pokemon ，创建你想要的宝可梦数据
  ![图 12](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640089220518.png)  
  ![图 13](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640089606713.png)  
  <span id="jump2"></span>
- 点击 Import/Export 导出 Showdown 数据
  ![图 14](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640089642699.png)  

### 1.3.1. 我不熟悉英文，如何找到想要的宝可梦/持有物/性格/技能？
- [宝可梦列表（按全国图鉴编号）](https://wiki.52poke.com/wiki/%E5%AE%9D%E5%8F%AF%E6%A2%A6%E5%88%97%E8%A1%A8%EF%BC%88%E6%8C%89%E5%85%A8%E5%9B%BD%E5%9B%BE%E9%89%B4%E7%BC%96%E5%8F%B7%EF%BC%89)
- [第八世代道具列表](https://wiki.52poke.com/wiki/%E9%81%93%E5%85%B7%E7%BC%96%E5%8F%B7%E8%A1%A8%EF%BC%88%E7%AC%AC%E5%85%AB%E4%B8%96%E4%BB%A3%EF%BC%89)，点击道具详情查看英文名称
- 性格对照表
    | 性格   | 日文名     | 英文名  | 容易成长的能力 | 不容易成长的能力 |
    | :----- | :--------- | :------ | :------------- | :--------------- |
    | 勤奋   | がんばりや | Hardy   | —              | —                |
    | 怕寂寞 | さみしがり | Lonely  | 攻击           | 防御             |
    | 固执   | いじっぱり | Adamant | 攻击           | 特攻             |
    | 顽皮   | やんちゃ   | Naughty | 攻击           | 特防             |
    | 勇敢   | ゆうかん   | Brave   | 攻击           | 速度             |
    | 大胆   | ずぶとい   | Bold    | 防御           | 攻击             |
    | 坦率   | すなお     | Docile  | —              | —                |
    | 淘气   | わんぱく   | Impish  | 防御           | 特攻             |
    | 乐天   | のうてんき | Lax     | 防御           | 特防             |
    | 悠闲   | のんき     | Relaxed | 防御           | 速度             |
    | 内敛   | ひかえめ   | Modest  | 特攻           | 攻击             |
    | 慢吞吞 | おっとり   | Mild    | 特攻           | 防御             |
    | 害羞   | てれや     | Bashful | —              | —                |
    | 马虎   | うっかりや | Rash    | 特攻           | 特防             |
    | 冷静   | れいせい   | Quiet   | 特攻           | 速度             |
    | 温和   | おだやか   | Calm    | 特防           | 攻击             |
    | 温顺   | おとなしい | Gentle  | 特防           | 防御             |
    | 慎重   | しんちょう | Careful | 特防           | 特攻             |
    | 浮躁   | きまぐれ   | Quirky  | —              | —                |
    | 自大   | なまいき   | Sassy   | 特防           | 速度             |
    | 胆小   | おくびょう | Timid   | 速度           | 攻击             |
    | 急躁   | せっかち   | Hasty   | 速度           | 防御             |
    | 爽朗   | ようき     | Jolly   | 速度           | 特攻             |
    | 天真   | むじゃき   | Naive   | 速度           | 特防             |
    | 认真   | まじめ     | Serious | —              | —                |
- [特性列表](https://wiki.52poke.com/wiki/%E7%89%B9%E6%80%A7%E5%88%97%E8%A1%A8%EF%BC%88%E6%8C%89%E5%85%A8%E5%9B%BD%E5%9B%BE%E9%89%B4%E7%BC%96%E5%8F%B7%EF%BC%89)，点击对应宝可梦特性详情查看英文名称
- [招式列表](https://wiki.52poke.com/wiki/%E6%8B%9B%E5%BC%8F%E5%88%97%E8%A1%A8#.E7.AC.AC.E5.85.AB.E4.B8.96.E4.BB.A3)
  ⚠ 请勿为宝可梦装备无法在第八世代中获得的或宝可梦本身无法学习的技能

<div STYLE="page-break-after: always;"></div>

# 2. 使用 Dogeee 连接交换/测坑闪
如何鉴别这个宝可梦是否属于自己？此处将引入 ***表ID（Trainer ID/TID）*** 的概念。
<span id="jump1"></span>

当你打开宝可梦背包查看宝可梦的能力，你会发现在基本信息页，除了 ***宝可梦姓名*** 、***属性*** 、***初训家*** 外，还有一个6位数的 ***ID No.*** ，这便是 ***表ID（Trainer ID/TID）*** 。

![图 15](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640089792719.png)  

但是假如遇到一个 ***表ID（Trainer ID/TID）*** 及 ***初训家*** 与自己完全一致的家伙，这个宝可梦将同时属于多个训练师吗？

显然不是，因为还有个不可见的4位数ID帮助辨别身份，我们称之为 ***里ID（Secret ID/SID）*** 。正因为表里ID的存在，在遇到上述这种哪怕是极小概率的情况，游戏仍然会辨认出这只宝可梦来源于哪个训练师。

一般情况下，我们无法看到自己的 ***SID*** ，借助某些作弊工具或计算器来确定它，在这里，我们借助 Dogeee 获取自己的 ***SID*** / ***TID*** 。

## 2.1. 获取自己的 SID & TID / 测闪帧
- 确保自己已开通 **Nintendo Swtich 会员** ，游戏中按 **``Y``** → **``+``** ，直至弹出 **已切换到互联网连接** 。
- 在群聊内输入 **````checkpkm````** ，等待 Dogeee 发送连接密码 
  ![图 17](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640092374595.png)  

- 点击 **连接交换** → **设置密码** ，输入连接密码，等待交换
  ![图 18](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640092391360.png)  

- 发送一只**自己捕获的宝可梦**，Dogeee 接收到宝可梦数据后会自动中断交易，并返回对应数据，红框部分即为我们需要记录的数据。
  ![图 19](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640092408908.png)  
  - 在不更换宝可梦存档的情况下， ***训练师名字*** / ***性别*** / ***TID*** / ***SID*** 均不会发生变化，所以仅需交换一次并保存好训练师数据。
  
## 2.2. 通过连接交换生成自ID的宝可梦
我们将 [Pokemon Showdown 的宝可梦数据](#jump2)与生成的训练师数据结合，得到：
>   Venusaur (F) @ Life Orb  
    Ability: Chlorophyll  
    Level: 100  
    IVs: 0 Atk  
    EVs: 4 Def / 252 SpA / 252 Spe  
    Timid Nature  
    - Solar Beam  
    - Toxic  
    - Swords Dance  
    - Endure
    训练师名字:猫尼玛
    训练师性别:男
    训练师TID:26708
    训练师SID:1450

将上述代码全部复制发送至群聊，等待连接交换，以任意宝可梦进行交换。当交换完成，看到 **“欢迎回来，XXXX！”** 的那一刻，恭喜你，成功了，这是属于你的自ID宝可梦！

**需要注意的是，一次连接交换仅可交换一只宝可梦。**

到这里，你已经完全掌握了最基础的宝可梦交换步骤，愉快地玩耍吧！

下面是一些扩展知识，面向自定义需求更高的玩家们。

---
<div STYLE="page-break-after: always;"></div>

# 3. 扩展知识
## 3.1. 大脑升级：表表ID/里里ID
一些玩家可能会感到疑惑，正如[之前](#jump1)所提到的， ***TID*** / ***SID*** 是一串分别由6/4位数字组成的数值，而使用 Dogeee 会返回一个5位数的 ***训练师TID*** / ***训练师SID*** ，且这个 ***训练师TID*** 与游戏中展现的 ***ID No.*** 并不一致：

> ***ID No.*** = 053908
> ***训练师TID*** = 26708

其实在第7世代以前，***TID*** / ***SID*** 都是5位数（严格来讲是十六进制的四位数，最大值为FFFF，也就是十进制中的65535）。从第7世代开始，虽然游戏仍然会生成5位数的 ***TID*** / ***SID*** ，但最终呈现在我们眼前的数值经过下方运算变成了6/4的结构：

<span id="jump3"></span>

$$
ID_{final} = TID + (SID * 65536) 
$$

我们将 dogeee 返回的 ***训练师TID*** / ***训练师SID*** 代入可得：

$$
ID_{final} = 26708 + (1450 * 65536) = 95,053,908
$$

其中，末6位数 ***053908*** 就是我们在游戏中见到的 ***ID No.*** ，而余下的数字在补足0后形成的4位数字，即 ***0095*** ，才是与游戏中 ***ID No.*** 对应的 ***SID*** 。

也就是说，宝可梦当前有两套表里ID体系，国外将运算后的6/4结构称为 *Display TID/SID* ，译为 ***表·表/里ID*** ，而最初的5/5结构称为 *Hidden TID/SID* , 译为 ***里·表/里ID*** ，当宝可梦从第6世代之前的游戏版本将精灵转移至第7/8世代， ***ID No.*** 不会运算，使用的应是 ***里·表ID*** ，即 ***26708***。

当然我们很难在两个完全不同的游戏版本中获得相同的表ID，如果要通过 Dogeee 生成前代的宝可梦（游戏里描述为“从XXX地区穿越时间和空间，千里迢迢地到来了”），我们仅需注意使 ***训练师TID*** / ***训练师SID*** 满足5/5结构。

## 3.2. 附加 showdown 指令
我们可以通过附加 showdown 指令来实现更多自定义的选项，例如修改宝可梦昵称/性别/球种/闪光类型/国家/亲密度等。**这些指令并不强制要求玩家添加，仅供有特殊自定义需求的玩家使用。**
### 3.2.1. Header——请求头
| 语法       | 含义     | 备注                                       |
| :--------- | :------- | :----------------------------------------- |
| [Nickname] | 昵称     | 可不填写，不得出现改地区不支持的字符         |
| [Pokemon]  | 宝可梦名 | 必填项，存在昵称时嵌套半角括号            |
| (Gender)   | 性别     | 仅有性别的宝可梦可填写 |
| @ [Item]   | 持有物   | 默认空值               |
综合应用示例：
> 蒜头王八 (Venusaur) (M) @ Life Orb

描述：
> 蒜头王八 (妙蛙花) (雄性) @ 生命宝珠

### 3.2.2. Body——可选指令
#### 3.2.2.1. 训练师数据
| 语法                   | 含义       | 备注                       |
| :--------------------- | :--------- | :------------------------- |
| OT:[Name]              | 训练师名称 | 不得出现改地区不支持的字符 |
| OTGender:[Male/Female] | 训练师性别 |                            |
| TID:[000000-999999]    | 表·表ID    |                            |
| SID:[0000-4294]        | 表·里ID    |                            |
如果您在其他地方得知了自己的 ***表·表/里ID*** ，您可以选择在确保 ***OT*** 与 ***训练师名称*** 完全一致的情况下，其他数据胡乱填写不会影响宝可梦生成的结果，上述训练师数据会覆盖中文指令，即：
>Venusaur (M)
OT: 猫尼玛
TID: 53908
SID: 95
OTGender: Male
训练师名字:猫尼玛
训练师性别:女
训练师TID:12345
训练师SID:01234

生成结果：
>宝可梦：妙蛙花
宝可梦性别：雄性
初训家：猫尼玛
初训家性别：男
TID/ID No.: 053908
SID: 0095

#### 3.2.2.2. 球种/闪光类型/地区/亲密度/超级巨化
| 语法                                          | 含义     | 备注                              |
| :-------------------------------------------- | :------- | :-------------------------------- |
| Ball: [Name]                                  | 球种     | [球种翻译](#32221-球种翻译)                                  |
| Shiny: [Yes/No/Star/Square]                   | 闪光类型 | [异色条件自查](#32222-异色条件自查) |
| Language: [ChineseS/Japan/English等] | 语言     |                                   |
| Friendship: [0-255]                           | 亲密度   |                                   |
| Gigantamax: [Yes/No]| 超级巨化|仅限[可超级巨化的宝可梦](#32223-可超级巨化的宝可梦)使用 |

##### 3.2.2.2.1. 球种翻译
- 仅**第4世代后**通过**配信**获得的宝可梦才可设置为**贵重球**。
- **请勿创建无法以该球种在此世代获得的宝可梦**，如： 丰缘地区治愈球梦幻。
- 仅来源于**第8世代**的宝可梦可使用**
    | 中文   | 英文         |引进世代 |
    | :----- | :----------- | :----- |
    | 精灵球 | Poke Ball    | 1 |
    | 超级球 | Great Ball   | 1 |
    | 高级球 | Ultra Ball   | 1 |
    | 狩猎球 | Safari Ball  | 1 |
    | 大师球 | Master Ball  | 1 |
    | 速度球 | Fast Ball    | 2 |
    | 等级球 | Level Ball   | 2 |
    | 诱饵球 | Lure Ball    | 2 |
    | 沉重球 | Heavy Ball   | 2 |
    | 甜蜜球 | Love Ball    | 2 |
    | 友友球 | Friend Ball  | 2 |
    | 月亮球 | Moon Ball    | 2 |
    | 捕网球 | Net Ball     | 3 |
    | 潜水球 | Dive Ball    | 3 |
    | 巢穴球 | Nest Ball    | 3 |
    | 重复球 | Repeat Ball  | 3 |
    | 计时球 | Timer Ball   | 3 |
    | 豪华球 | Luxury Ball  | 3 |
    | 纪念球 | Premier Ball | 3 |
    | 黑暗球 | Dusk Ball    | 4 |
    | 治愈球 | Heal Ball    | 4 |
    | 先机球 | Quick Ball   | 4 |
    | 贵重球 | Cherish Ball | 4 |
    | 竞赛球 | Sport Ball   | 4 |
    | 梦境球 | Dream Ball   | 5 |
    | 究极球 | Beast Ball   | 7 |

##### 3.2.2.2.2. 异色条件自查
- **冠之雪原-极巨大冒险** 中的闪光宝可梦只有星闪，相遇地点为 ***极巨巢穴*** 。
- [锁闪宝可梦——以下精灵在特定版本无法以任何方式遇见异色](https://wiki.52poke.com/wiki/%E7%95%B0%E8%89%B2%E5%AF%B6%E5%8F%AF%E5%A4%A2#.E4.B8.8D.E5.8F.AF.E8.83.BD.E4.B8.BA.E5.BC.82.E8.89.B2.E7.9A.84.E5.AE.9D.E5.8F.AF.E6.A2.A6)
  
- [仅可通过配信获得的异色宝可梦](https://wiki.52poke.com/wiki/%E7%95%B0%E8%89%B2%E5%AF%B6%E5%8F%AF%E5%A4%A2#.E6.B4.BB.E5.8A.A8.E8.B5.A0.E9.80.81.E7.9A.84.E5.BC.82.E8.89.B2.E5.AE.9D.E5.8F.AF.E6.A2.A6)
- [未发现蛋组异色宝可梦获得方式](https://wiki.52poke.com/wiki/%E5%BC%82%E8%89%B2%E7%9A%84%E6%9C%AA%E5%8F%91%E7%8E%B0%E8%9B%8B%E7%BE%A4%E5%AE%9D%E5%8F%AF%E6%A2%A6%E8%8E%B7%E5%BE%97%E6%96%B9%E5%BC%8F)
- 第3至第5世代中，宝可梦的 ***PID（Personality Value）*** 与训练师的 ***TID*** / ***SID*** 甚至宝可梦自身的属性都有关联，这也导致了在第3至第5世代中，想通过作弊软件获得自ID且满足个体要求的闪光宝可梦非常困难，请尽可能创建相遇地为卡洛斯/伽勒尔地区（即第7/8世代）的宝可梦以减少不必要的麻烦。
  
    >  ***PID*** 算法[参考文档](https://bulbapedia.bulbagarden.net/wiki/Personality_value) 

##### 3.2.2.2.3. 可超级巨化的宝可梦
  编号 | 宝可梦 | 英文名
  :----- | :---- | :---- 
  #003 | 妙蛙花 | Venusaur
  #006 | 喷火龙 | Charizard
  #009 | 水箭龟 | Blastoise
  #012 | 巴大蝶 | Butterfree
  #025 | 皮卡丘 | Pikachu
  #052 | 喵喵 | Meowth
  #068 | 怪力 |Machamp
  #094 | 耿鬼 | Gengar
  #099 | 巨钳蟹 | Kingler
  #131 | 拉普拉斯 | Lapras
  #133 | 伊布 | Eevee
  #143 | 卡比兽 | Snorlax
  #569 | 灰尘山 | Garbodor
  #809 | 美录梅塔 | Melmetal
  #812 | 轰擂金刚猩 | Rillaboom
  #815 | 闪焰王牌 | Cinderace
  #818 | 千面避役 | Inteleon
  #823 | 钢铠鸦 | Corviknight
  #826 | 以欧路普 | Orbeetle
  #834 | 暴噬龟 | Drednaw
  #839 | 巨炭山 | Coalossal
  #841 | 苹裹龙 | Flapple
  #842 | 丰蜜龙 | Appletun
  #849 | 颤弦蝾螈 | Sandaconda
  #851 | 焚焰蚣 | Centiskorch
  #858 | 布莉姆温 | Hatterene
  #861 | 长毛巨魔 | Grimmsnarl
  #869 | 霜奶仙 | Alcremie
  #879 | 大王铜象 | Copperajah
  #884 | 铝钢龙 | Duraludon
  #892 | 武道熊师 | Urshifu

## 3.3. 生成宝可梦蛋
Dogeee 同样可以生成闪蛋，可以自定义宝可梦种类/性别/特性/能力/个体值/球种。

**注意**：
- 宝可梦名字应填入未进化的形态。例：可以生成妙蛙种子闪蛋，而不可以生成妙蛙花闪蛋。
- 蛋不可以有训练师信息。
- 蛋不能有持有物。
- 蛋的努力值应为0。
- 蛋不可以指定招式。
- 蛋不能接受奖章。
- 生成无法通过生蛋获得的宝可梦可能会导致坏档或封禁。例：苍响闪蛋，会导致无法孵化/放生。

例：
> Egg(Bulbasaur) (M)
Adamant Nature
Ability: Chlorophyll
Shiny: Square
IVs: 23 HP / 2 Atk / 31 Def / 31 SpA / 31 SpD / 16 Spe
Ball: Safari Ball
训练师名字:
训练师性别:
训练师TID:
训练师SID:

## 3.4. 生成配信宝可梦
如果想生成一个**完全合法**的配信宝可梦，您需要仔细查询宝可梦配信数据库，填写五个关键字段：**宝可梦名称** / **TID** / **球种** / **闪光类型** / **训练师名字** ，而 **训练师名字/性别/TID/SID** 不填写，除非你知道配信宝可梦的 *里TID/SID* 。Dogeee 的合法性检测程序会自动为你生成与请求条件相匹配的精灵，如：
> Mewtwo @ Life Orb
OT: Nintendo HK
TID: 06096
Ball: Cherish Ball
Shiny: Yes
训练师名字:
训练师性别:
训练师TID:
训练师SID:
![图 20](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640095832707.png)  
 

注意，配信数据库中所示的 ***ID*** 均为 ***Display TID*** ，即 [章节3.1](#31-大脑升级表表id里里id) 中讲解的 ***表·表ID*** ，请勿错误地将该数据填写至 ***训练师TID*** 。

参考资料：
- [时拉比网配信数据库](https://www.serebii.net/events/dex/)
- [神百部落配信数据库](https://bulbapedia.bulbagarden.net/wiki/Category:Event_Pok%C3%A9mon)

> **高级训练：这个配信宝可梦合法吗？** 
>Zarude-Dada @ Choice Scarf  
    Ability: Leaf Guard  
    EVs: 4 HP / 252 Atk / 252 Spe  
    Quirky Nature  
    - Jungle Healing  
    - Hammer Arm  
    - Power Whip  
    - Energy Ball
    训练师名字:丛林
    训练师性别:男
    训练师TID:17831
    训练师SID:1666

> **答：非法。**
    将 ***训练师TID*** / ***训练师SID***  带入 [$ID_{final}$ 计算公式](#jump3) 得
    $ID_{final} = 17831 + (1666 * 65536) = 109,200,807$
    可得 ***表TID = 200807*** ，***表SID = 0109***。
    以 ***表TID*** 查询神百部落配信数据库，检索到下方数据：
    ![图 3](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640181298852.png)  
    如图所示，**不合法**的字段为：
    - **Nature（性格）** ，数据库中记录的性格为 ***Sassy*** ，而请求交换的宝可梦性格为 ***Quirky*** 。
    - **Pokemon（宝可梦名/形态）**，数据库中记录的形态为**普通形态**，即 ***Zarude*** ，而请求交换的宝可梦为**阿爸形态**，即 ***Zarude-Dada*** 。
    综上，这是一只非法的宝可梦。


## 3.5. Batch Commands
Batch Commands 是在 PKHeX中编辑宝可梦时的附加选项，它同样适用于 Showdown 格式，你可以通过 Batch Commands 实现更多自定义选项。
- [Batch Commands 参考文档](https://pastebin.com/HixDtDxc)

下面将列举一些玩家常用的 Batch Command 语法：
| 语法                        | 含义             | 备注                               |
| :-------------------------- | :--------------- | :--------------------------------- |
| .PKRS_Infected=[true/false] | 感染宝可病毒     |                                    |
| .PKRS_Cured=[true/false]    | 宝可病毒是否治愈 |                                    |
| .Ribbons=$SuggestAll        | 获取所有合法勋章 |                                    |
| .MetDate=[yyyyMMdd]         | 相遇日期         |                                    |
| .EggMetDate=[yyyyMMdd]      | 蛋获得日期       | 仅通过孵蛋获得的宝可梦可设置       |
| =Generation=[1-8]           | 获得版本         | 默认来源第8世代                                   |
| .BattleVersion=[44-45]      | 对战版本         | 44-剑；45-盾，若宝可梦非源自第8世代，该指令会为宝可梦获得对战塔标记                       | 
| .Met_Location=[VALUE]       | 相遇地点         | [相遇地点代码](#3521-相遇地点代码swsh)                       |
| .Egg_Location=60002         | 从寄放屋获得蛋   | 不填写则显示为“在连接交换得到的蛋” |
| .StatNature=[VALUE]         | 薄荷修正性格     |  [薄荷代码表](#352-薄荷代码表)                                  |

例：
>   Venusaur (M) @ Life Orb
    OT: 猫尼玛
    TID: 53908
    SID: 95
    OTGender: Male
    EVs: 252 SpA / 4 SpD / 252 Spe
    Ability: Chlorophyll
    Ball:Safari Ball
    Shiny: Square
    Modest Nature
    - Weather Ball
    - Giga Drain
    - Sludge Bomb
    - Growth
    Gigantamax: Yes
    .PKRS_Infected=true
    .PKRS_Cured=false
    .Ribbons=$SuggestAll
    .EggMetDate=20211220
    .Egg_Location=60002
    .MetDate=20211220
    .Met_Location=110
    .StatNature=17
    训练师名字:猫尼玛
    训练师性别:男
    训练师TID:26708
    训练师SID:1450

生成结果：
> 宝可梦：妙蛙花（雄性）（超级巨）
> 持有物：生命宝珠
> 初训家：猫尼玛
> 初训家性别：男
> TID/ID No.: 053908
> SID: 0095
> 个体：6v
> 努力值：252 特攻 / 4 特防 / 252 速度
> 特性：叶绿素
> 球种：狩猎球
> 闪光类型：方块
> 性格：内敛
    - 技能1 气象球
    - 技能2 终极吸取
    - 技能3 污泥炸弹
    - 技能4 生长
  感染宝可病毒，未治愈
  已获得所有合法证章
  蛋获得日期：2021年12月20日
  从寄放屋获得的蛋
  在宫门市孵化
  相遇日期：2021年12月20日
  薄荷修正性格：冷静

### 3.5.1. 相遇地点代码（SW/SH）
  ![图 21](https://github.com/Nyaneymar/imgdatabase/blob/master/1640095864718.png?raw=true)  
 

### 3.5.2. 薄荷代码表
| 性格   | 日文名     | 英文名  | 薄荷值 | 容易成长的能力 | 不容易成长的能力 |
| :----- | :--------- | :------ | :----- | :------------- | :--------------- |
| 勤奋   | がんばりや | Hardy   |   —     | —              | —                |
| 怕寂寞 | さみしがり | Lonely  | 1      | 攻击           | 防御             |
| 固执   | いじっぱり | Adamant | 3      | 攻击           | 特攻             |
| 顽皮   | やんちゃ   | Naughty | 4      | 攻击           | 特防             |
| 勇敢   | ゆうかん   | Brave   | 2      | 攻击           | 速度             |
| 大胆   | ずぶとい   | Bold    | 5      | 防御           | 攻击             |
| 坦率   | すなお     | Docile  |   —     | —              | —                |
| 淘气   | わんぱく   | Impish  | 8      | 防御           | 特攻             |
| 乐天   | のうてんき | Lax     | 9      | 防御           | 特防             |
| 悠闲   | のんき     | Relaxed | 7      | 防御           | 速度             |
| 内敛   | ひかえめ   | Modest  | 15     | 特攻           | 攻击             |
| 慢吞吞 | おっとり   | Mild    | 16     | 特攻           | 防御             |
| 害羞   | てれや     | Bashful | —       | —              | —                |
| 马虎   | うっかりや | Rash    | 19     | 特攻           | 特防             |
| 冷静   | れいせい   | Quiet   | 17     | 特攻           | 速度             |
| 温和   | おだやか   | Calm    | 20     | 特防           | 攻击             |
| 温顺   | おとなしい | Gentle  | 21     | 特防           | 防御             |
| 慎重   | しんちょう | Careful | 23     | 特防           | 特攻             |
| 浮躁   | きまぐれ   | Quirky  |  —      | —              | —                |
| 自大   | なまいき   | Sassy   | 22     | 特防           | 速度             |
| 胆小   | おくびょう | Timid   | 10     | 速度           | 攻击             |
| 急躁   | せっかち   | Hasty   | 11     | 速度           | 防御             |
| 爽朗   | ようき     | Jolly   | 13     | 速度           | 特攻             |
| 天真   | むじゃき   | Naive   | 14     | 速度           | 特防             |
| 认真   | まじめ     | Serious | 12     | —              | —                |

# 4. 其他工具
**本章节已完全脱离自定义连接交换教学范畴，仅在文本中作简单介绍与最基础的使用方法，本章节涉及的工具/网站在群内不予解答。**

## 4.1. PKHeX

PKHeX 是一款基于C#开发的宝可梦核心系列游戏存档编辑器，你同样可以使用它在 Windows 上创建自己的宝可梦文件。 PKHeX 可以在[这里](https://projectpokemon.org/home/files/file/1-pkhex)下载。

PKHeX 支持以下文件类型：
- 游戏存档（“main”、\*.sav、\*.dsv、\*.dat、\*.gci）
- 包含 GC Pokémon 存档的 GameCube 存储卡文件（.raw、.bin）。
- 单个宝可梦实体文件 (.pk*)
- 神秘礼物文件（.pgt、.pcd、.pgf、.wc*），可转换为 .pk*
- 从破解的 3DS 战斗视频中导入团队
- 跨时代传输的宝可梦文件

数据显示在可以编辑和保存的视图中。界面可以用资源/外部文本文件进行翻译，以便支持不同的语言。且**可以导入/导出为 Pokémon Showdown 语法和二维码**。

![![图 22](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640102071190.png)   1](https://cdn.jsdelivr.net/gh/Nyaneymar/imgdatabase/1640169372991.png) 

使用基础：
- CTRL+N 打开宝可梦数据库，可以使用数据库中精灵为模板进行创作。
- CTRL+G 打开神秘礼物数据库查看配信宝可梦。
- 不合法的精灵带有 **⚠** 标志，点击图标可以显示不合法字段，**请勿使用不合法的精灵进行交换**。
- 可使用 [自动合法化插件](https://github.com/architdate/PKHeX-Plugins/releases) 进行创作。
- **工具-剪贴板操作-导出宝可梦到剪贴板** 将宝可梦导出为可用于 Dogeee 连接交换的 showdown 格式。

## 4.2. 神奇宝贝百科

[神奇宝贝百科](https://wiki.52poke.com/wiki/%E4%B8%BB%E9%A1%B5) 是一个协作共建的，人人都可以编辑的，关于宝可梦的在线百科全书。神奇宝贝百科同时也是世界在线宝可梦百科全书工程 Encyclopædiæ Pokémonis 的中文成员。神奇宝贝百科不但收录了有关宝可梦游戏、动画、电影、漫画、卡牌游戏、书籍、音乐、周边商品等内容，还收录了和宝可梦相关的工作人员、游戏设备、活动和事件等方面的内容。

您可以通过点击各个页面的链接来浏览神奇宝贝百科的内容，也可以通过搜索关键词的方式来检索神奇宝贝百科的文章。

## 4.3. 宝可梦育成论-剑盾

[宝可梦育成论-剑盾](https://yakkun.com/swsh/theory/) 是一个日本的宝可梦养成攻略网站，它提供了大量符合《宝可梦 剑/盾》级别对战的宝可梦养成数据与对战思路。

