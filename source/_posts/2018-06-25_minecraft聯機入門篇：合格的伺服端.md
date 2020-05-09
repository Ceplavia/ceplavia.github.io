---
title: Minecraft 聯機入門篇：合格的伺服端
categories:
  - 教程
tags:
  - Minecraft
date: 2018-06-25 16:50:47
---

> 原開頭：
>
> 最近在准备服务器的10周目，然后想起了之前写过的联机篇。
>
> 说是过几天发，实际有2个月了吧？答辩真是一个很好用来消磨(?)时间的东西。

* * *

本文介紹我製作伺服端的方式。本文重寫於 2020 年 5 月初。

<!-- more -->

### 伺服端使用

#### 小知識

伺服端包括原版、Bukkit系、Bukkit+Forge系、Forge系；

原版自然不必多説，就是官方提供的版本。

Bukkit 系的伺服端除了將地圖的文件結構進行修改以方便備份外，還可以讓玩家安裝插件。最早的是 Bukkit Server，現在通常默認 Spigot 是接班人。

而 Forge 系則是在官方的版本上安裝 Forge API，以使用 Mod 來進行聯機。

Bukkit+Forge 系是一個盛行於 1.7.10，發源於 beta 1.7 年代的東西，從 MCPC 到 Cauldron ，再到 Kcauldron，停於 Thermos。看名字就知道，是可以同時安裝 mod 與插件的伺服端。

#### 原版生存

原版生存意味着不安裝任何 Mod，我推薦使用 [Spigot](https://www.spigotmc.org/) 的分支 [PaperSpigot](https://papermc.io/) (aka. PaperMC)，原因就是優化，以及非常快的更新速度。

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

### 個人探討

下方內容算是早期試圖探討玩家心理學的內容。但我重寫了一遍，略作了修改。

首先需要注意一點：每一個玩家都傾向於做一個守財奴。此處並不是貶義，而是指玩家傾向於收集，在物品數量不多時更是如此。在物品充足後才會開始轉向建築（Mod 玩家）或是其它方面。

我認為的好 Mod Pack 需要有以下幾類 Mod：

- 主遊玩 Mod - 通常指一些科技或魔法 Mod，不僅限於一個，也可以是 2 個或 3 個，前提是 Mod 之間物品有聯動，否則遊玩起來非常消耗精力，尤其是在目前的大 Mod 發展線都相對繁複的情況下。
- 副遊玩 Mod - 可以是主遊玩 Mod 的 addon，也可以是提供給不玩主遊玩 Mod 的人的其它選擇。比方説像是「Level Up!」這類比較像以前叫做「mcMMO」的插件的 Mod。總之是可以作為備用選擇，沒有必要逼着所有人都去玩前面睇到的主遊玩 Mod，儘管原版內容並不算多，但玩家應當自己選擇想要遊玩的方向。
- 娛樂 Mod，通常都是小 Mod，添加一些裝飾物品或是實用物品，如 McCrayFish 的 Furniture Mod，以及 Pam 的 HarvestCraft，OpenBlock 之類。
- 優化或輔助 Mod，小地圖，內存回收，地形修改等。

#### Main Mod

主遊玩 Mod 類型實際上並不多，幾乎可以説是除了科技就只有魔法。要注意的地方是，有沒有添加新的礦物，這個礦物會否佔去其它礦物生成，以及多個 Mod 的金屬礦物是否通用（如銅錫等），若 Mod 的礦物並不在礦物詞典中則不建議使用。

> 不過現在幾乎所有 Mod 的礦物都在礦物詞典中。那就要注意多個 Mod 共存時會否在地下生成過多的同一金屬，影響平衡性。

此外，還需注意會否有 Mod 對原版素材的需求過大。若同時遊玩數個需求鐵礦都多的 Mod，則要考慮修改生成規則等。

我曾經玩過一個做鐵桶需要 72 個鐵礦的 Mod。

#### Sub Mod

有關副遊玩 Mod，我的伺服器有時會有些不敢獨自去挖礦的玩家，也會有不喜歡主遊玩 Mod，只喜歡造建築的玩家。

針對前者，我會添加一些寵物 Mod，或是增強角色戰力的 Mod，比如額外增加護甲和武器等（記得考慮平衡！）。

若是此類 Mod 可以輕鬆地獲取高傷害武器，則記得適當調高遊戲難度。

針對後者，為了公平性起見，我會考慮增加原版素材獲取途徑，例如 Ex Nihilo 這個 Mod。

#### Optimal Mod

內存回收的 Mod 沒有什麼好提的，幾乎每次我都會安裝。無論是為他人電腦着想或是為自己電腦着想，這類 Mod 並沒有壞處。

至於地圖 Mod，要看個人喜好，最好多備幾個地圖 Mod 讓玩家自行挑選，畢竟在聯機時這不算是必用 Mod。

另外還有要使用 [Just Enough Items](https://www.curseforge.com/minecraft/mc-mods/jei) 的話，就最好順便加上 [Just Enough Calculation](https://www.curseforge.com/minecraft/mc-mods/just-enough-calculation)。

#### Test

測試非常重要。

在 Minecraft 中測試 Mod 並不像現實的測試職位一樣有多種方法，只能循固定思路去做。

除了要測試每一個機器的功能之外，還要注意一些方塊的功能。還有 Mod 間的衝突。

要注意的方面非常多，不能盡列。總結：測試每一個可以交互的方塊。

那些不會導致 crash 的 Bug 就無法避免，只能個人思考取捨。

Anyway, 多做測試。

---

入門篇完。

