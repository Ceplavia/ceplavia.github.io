---
title: Minecraft 聯機入門篇：合格的伺服端
categories:
  - 教程
tags:
  - minecraft
date: 2018-06-25 16:50:47
---

> 原開頭：
>
> 最近在准备服务器的10周目，然后想起了之前写过的联机篇。
>
> 说是过几天发，实际有2个月了吧？答辩真是一个很好用来消磨(?)时间的东西。

* * *

本文介紹我製作伺服端的方式。

<!-- more -->

### 伺服端使用

#### 小知識

伺服端包括原版、Bukkit系、Bukkit+Forge系、Forge系；

原版自然不必多説，就是官方提供的版本。

Bukkit 系的伺服端除了將地圖的文件結構進行修改以方便備份外，還可以讓玩家安裝插件。最早的 Bukkit Server 現在通常默認 Spigot 是接班人。

而 Forge 系則是在官方的版本上安裝 Forge API，以使用 Mod 來進行聯機。

Bukkit+Forge 系是一個盛行於 1.7.10，發源於 beta 1.7 年代的東西，從 MCPC 到 Cauldron ，再到 Kcauldron，停於 Thermos。看名字就知道，是可以同時安裝 mod 與插件的伺服端。

#### 原版生存

原版生存意味着不安裝任何 Mod，我推薦使用 [Spigot](https://www.spigotmc.org/) 的分支 [PaperSpigot](https://papermc.io/)(aka. PaperMC)，原因就是優化，以及非常快的更新速度。

#### Mod服

若是使用遊戲版本 1.7.10，建議使用 [Thermos](https://github.com/CyberdyneCC/ThermosServer)。但 Thermos 已經很久遠，也沒有多少人維護，最好不要使用此遊戲版本。

若是 1.10.2~1.12.2，可以使用 [Forge](https://files.minecraftforge.net/)+[Sponge](https://www.spongepowered.org/)。Sponge 是一套基於 Forge 的 API。

1.12 以上，如 1.14.4、1.15.2，則建議使用 [Forge](https://files.minecraftforge.net/)。

部署起來相當簡單，例如 Forge 服就是將 Forge 對着官方伺服端安裝。有自己動手裝過 Forge 的應該能理解。

#### 不同版本的差異？

在插件年代，有一個比較出名的插件系列叫做 Essentials，出名不是因為權限管理或是自帶經濟之類的設定，而是因為它提供了諸如`/home`、`/back`、`/tpa`之類的指令。

到了沒有辦法使用插件的年代，就只能用 Mod 來實現。可以直接使用的有 [ForgeEssentials](https://www.curseforge.com/minecraft/mc-mods/forge-essentials-74735)(~1.12.2)，和 [Thut Essentials](https://www.curseforge.com/minecraft/mc-mods/thut-essentials)(~1.15.2)，這些比較適合親友服，人數不多的聯機，大家聯機只為遊玩，不需要太多權限設置等。

如果需要一些更多的管理，就需要使用 Sponge 的 Mod 了，包括：

- Nucleus - 一個可以代替 Essentials 的 Mod，提供非常多的設置（包括權限分組等）及指令；
- LuckPerms - 權限管理，用來管理 Nucleus 接管的權限；
- TotalEconomy - 管理 Nucleus 接管的經濟

但當然也有缺點，第一當然是只支持到 1.12.2，第二是當時我使用 Nucleus 時發現權限是加在玩家身上的，即使是在伺服端控制台也沒有權限執行特殊指令。因為接管了所有權限，我亦不知道對方塊直接操作（如變動）的權限是如何處理。部分會對方塊直接操作的 Mod 就沒有權限操作。例如 Project E 的賢者之石直接對方塊進行變換。以及伺服端配置中的`spawn-protection`對重生點進行保護，被保護的方塊沒有任何人可以破壞。

#### 伺服端啟動

首先無論是上文提及的何種系伺服端，**其啟動方式都是使用一句指令來啟動最核心的 .jar 檔案**，具體到實際使用就變成了寫成 .bat 或 .sh 文件來偷懶。

給出我常用的啟動指令：

```
java -d64 -server -XX:+UseG1GC -XX:MaxGCPauseMillis=100 -XX:+UseStringDeduplication -Xms1G -Xmx2560M -XX:hashCode=5 -Dfile.encoding=UTF-8 -jar server.jar nogui
```

Xms 參數是初始分配記憶體。Xmx 參數是最大分配記憶體。jar 參數是指 Minecraft 的伺服器文件。

此指令保存為 .bat (windows) 或 .sh (linux) 文件，保存在與核心 .jar 同一目錄下，執行即可啟動伺服端。

#### 伺服端文件結構

通常，伺服端的資料夾內包含很多文件，要關心的不多：

- plugins/mods 資料夾 - 插件或 Mod 擺放的地方
- world 資料夾 - 默認的地圖命名，可以修改
- logs 資料夾 - 用來保存 log
- server.properties - 伺服端設置

我電腦上未有保存原版伺服器的設置文件，於是請查看[官方百科](https://minecraft-zh.gamepedia.com/Server.properties)。此處只列出少數我覺得比較重要並可能會修改的：

```
# 地圖的隨機種子，若使用特定的就要輸入
level-seed=
# 新玩家進入伺服器時的默認模式
gamemode=survival
# 伺服器使用的連接埠
server-port=25565
# 是否啟用命令方塊
enable-command-block=false
# 世界名稱，通常不變動，但若使用其它存檔則需要修改
level-name=world
# 此處的 MOTD 可以在 https://minecraft.tools/en/motd.php 製作
motd=A Minecraft Server
# 難度
difficulty=easy
# 世界類型
level-type=default
# 重生點保護，具體為以重生點為中心的正方體
spawn-protection=16
# 若有玩家使用非正版啟動遊戲，則需要設置為 false
online-mode=true
```

其餘設置項可以在上方給出的連結處查看。

#### 其它注意

另外，也有一些 Gamerule，單機玩家應該很熟悉，無非是 keepInventory 之類作弊指令。提出要注意的一點：只有 Bukkit 系的伺服端會將地圖按維度拆分，故每個維度都需要重複執行。

### 插件 / Mod 選用

#### 插件

插件通常是一些微改動遊玩方式，包括但不僅限於提供傳送指令等。自己挑選喜歡的即可。

如果是插件生存，我通常只會使用提供 Essentials 系列指令的插件。前文提到用 PaperSpigot，但 Spigot 的插件也可使用，可[在此](https://www.spigotmc.org/resources/categories/spigot.4/)找到。需要注意遊戲版本。

#### Mod

如果是 Mod 生存，就有比較多需要顧慮的。

首先要注意一點：提供多方向遊玩。

以實際舉例：若安裝了某工業 Mod，則要注意遊玩不應圍繞此工業 Mod，可以提供其它玩法的 Mod 也應一同考慮。可以使用主遊玩 Mod 搭配多個副 Mod 的方式。

其次就是一些小的娛樂 Mod / 裝飾 Mod。

內容太多，暫無法盡列，下方內容先保留。

---



首先有一点要注意：每一个MC玩家，或者说所有游戏的玩家，都是倾向当一个守财奴的。在这里并不是贬义，而是指玩家倾向于收集物品，在物品数量不多的时候更是如此，只有在物品多了的时候才不会那么注重收集这一点，而开始转向建筑或是其他的什么方面。

我将一个好的modpack需要的mod分为以下几类： 主游玩mod，这个通常是一些科技或魔法mod，不仅限于1个，可以是2个或者3个，它们之间最好是有相关联动。否则玩起来会太过于消耗精力。

副游玩mod，可以是科技mod的附属，也可以是提供给不玩主游玩mod的人的一些选择。

比如level up!这种比较像以前mcmmo插件的mod。总之是作为一种备用选择，不要逼着所有人都去玩上面的主游玩mod，尽管除了原版之外的内容不多。

娱乐性mod，通常是一些小mod，比如mccrayfish的家具mod系列，pan's的食物mod系列，openblock之类的。

优化或辅助mod，例如小地图、内存回收、地形修改之类的mod。

于是首先的重中之重就是要有一个主游玩mod，要说区分的话其实就是科技和魔法了，再有的大mod不多，这个可以看玩家群体倾向喜欢哪种。

在采用多个这类大mod的时候要注意，有加新矿的，注意这个矿会不会占掉其他矿的生成。以及多个mod的矿是否通用，要是mod的矿物不在矿物词典里就最好不要采用。

其次要注意对原版素材的需求是否大，若好几个大mod都大量需求铁矿，就要考虑去掉哪个mod，或是使用生成控制的mod将矿脉生成规则进行修改。这些都看玩家倾向。

* * *

其次，副游玩mod。我的服里会有比较不敢独自下矿的玩家，也会有不喜欢玩主游玩mod，不碰科技不碰魔法就到处起建筑的人。

这样就要从其他方面考虑。 对于不太敢下矿的，可以解释为不太喜欢战斗的，或是不太喜欢单人在矿洞里有种压抑感觉的。可以考虑添加一些宠物mod，或是增强一些战斗力的mod。比如额外增加护甲，武器等（别忘了注意平衡）。目前比较多新增武器和护甲mod的都不注重平衡，用的素材的数量和稀少度可能和铁剑差不多，但可能攻击超过钻石斧。使用这类mod时，要适当考虑把游戏难度改成普通或困难。

对于不是太喜欢玩大mod的，为了公平性起见就要考虑增加原版素材获得的mod，比如ex nihilo这个多数空岛生存包都有的mod。

* * *

最后是一些辅助或优化的mod。 这类mod有些要注意的我尽量写出，一时没想到也没办法…… 不管是否为了他人考虑，内存回收、优化的mod都必须安装。 然后我们再来说别的。例如小地图，在做modpack的时候最好准备几个不同的小地图mod，看玩家喜欢哪种。毕竟这类mod不属于必装的系列。 NEI/JEI，注意在服务端装上计算器。

* * *

最后，请仔细测试这个modpack。有些mod尽管看他更新相当频繁，但还是有一些bug。当中有些包含会导致客户端闪退和服务端崩溃的bug。需要多测试。

比如家具mod用淋浴头会崩，客户端和服务端都崩。

还会有一些bug明明有zh\_CN.lang或者zh\_TW.lang，到了游戏里干脆英文，或是干脆直接显示ModName.item.name。

所以，多测试，多测试，多测试。

很重要，说3次。

* * *

服务端设置

设置方面，装了的mod别忘了仔细看看设置。最好不要mod一放进来马上就开服玩。一些设置为了自己服务器的特色还是要修改的。例如登录的欢迎信息，自己自定义一下更有特色。

通常我在1.7.10时，在服务端上必装的mod是Essentials和WorldEdit。 前者不用多介绍了，/home，/tpa这些常用指令都在里面，WorldEdit则是为了避免某些惨剧，只是放着备用。

到了1.10.2，早就没有插件mod服这一说了，幸好还有一个能用的mod提供了这些功能。是ForgeEssentials。在我开了一次1.12.2的服之后，这个mod简直是救星。

因为1.12.2要使用这类功能，我目前看到的只有Nucleus，这个词我绝对没有去搜，一次就打出来了。（因为我花过几个下午的时间来解决1.12.2用这些命令的问题） 这个mod提供了这些命令，但是，没有权限插件来管理是用不了的。 于是，额外需要装一个LuckyPerms来管理，起一个默认用户组和OP组，把所有命令放进OP组，玩家该用的命令放进默认用户组。

这样才算能在1.12.2中用到这些命令。别忘了这个解决方案和gamerule中的keepInventory冲突，这个选项开着的时候是死亡不掉落，但是实际会变成背包清空。死的地方也没有掉东西。

1.12.2是我强烈不建议采用的游戏版本。

> 19年9月2日修排版：
>
> 后来 ForgeEssentials 升级到了 1.12.2 。

以上。