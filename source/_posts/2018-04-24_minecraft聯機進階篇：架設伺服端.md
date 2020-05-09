---
title: Minecraft聯機進階篇：架設伺服端
categories:
  - 教程
tags:
  - Minecraft
date: 2018-04-24 20:40:23
---

> 01/05/20 開始重寫，並準備在近期內補上後期維護篇。
>
> 19年9月2日修排版：
>
> 而维护篇我没写……
>
> 这系列东西最初是友人 [SunnyRx](https://sunnyrx.com/) 想知道怎么在 linux 系统上跑起客户端然后我就写了一点……

打算寫一系列關於 Minecraft (Java Edition) 的聯機教程，包括如何整合 Server Pack 到在伺服器上運行。

分為入門篇/聯機進階篇/後期維護篇。

<!-- more -->

* * *

**本教程適用於有相關基礎知識之人士。**

* * *

### 前期準備

#### 一台可使用 ssh 登入的伺服器

……購買即可。我個人推薦使用 Centos 7 系統。若有 Java Edition 與 Bedrock Edition 同時運營的想法，請使用 Ubuntu。並非 Centos 7 不可以，要在 Centos 7 上運行 Bedrock Edition Server 請參考此[連結](https://minecraft.server-memo.net/bedrock_server_install/)。

之後建議更改為僅密鑰登入，即在登入時必須使用密鑰，不可以使用密碼。

另外，需要安裝 Java 運行環境（JRE）。而對多數人來説，Linux 的文件目錄並不像 Windows 那麼直觀。所以我不推薦使用下載包再解壓縮自行安裝的方式。請善用 yum (or apt-get)，參見此[連結](https://developers.redhat.com/openjdk-install/)。

### 具体步骤

#### 1. 将整個伺服端上傳至伺服器；

> 注意：在上傳前，這個伺服端應在本地可成功運行，並且自己可以登入遊玩。

因為 Server Pack 我打算打包成 tar 後傳輸，所以直接用 pscp 來傳輸。這(算?)是 putty 系的配套軟件。在 Windows 下可以用 PowerShell 或者 CMD 來使用。

```
# 先使用 cd 命令到 pscp 所在的資料夾，或者寫變數也可以，看個人喜好。
pscp [本地文件路徑] [伺服器user]@[伺服器地址]:[路徑]
```

使用這個命令就可以在伺服器和本機之間傳輸文件，以上的例子是將本機文件上傳至伺服器。兩個參數可以反轉，如：

```
pscp [伺服器user]@[伺服器地址]:[路徑] [本地文件路徑]
```

則變成從伺服器下載文件到本機。

在實際使用時，因我的伺服器均採用密鑰登入，故需要一個額外參數，完整命令示例如下：

```
pscp -i G:\mykey.ppk G:\Server.tar root@1.2.3.4:/root
```

> 如果修改了 SSH 的連接埠，需要多一個`-P 123`參數。（123 是使用的連接埠）
>
> 若使用密碼登入，也可直接添加參數`-pw 123`，當然，不添加的話準備開啟連接時會詢問。

上方命令表示上傳至伺服器 1.2.3.4 的 root 用户的 root 目錄。若使用其它用户，請自行定位。因我用來運行 Minecraft 的伺服器只做這一件事，就不使用其它用戶，也不搞複雜的文件結構。

上傳前推薦使用 tar 格式打包，伺服端並不大。

上傳後，使用以下指令解包

```
tar xvf 1234.tar
```

### 在服务器上安装 Screen

Screen 的作用類似 Windows 任務欄，可以在多 Screen 內執行程序。否則直接使用 ssh 登入後啟動遊戲伺服器的話，當 ssh 連接斷開，遊戲伺服器也會關閉。

使用以下指令安裝 Screen：

```
yum install screen
```

根據提示該輸入 y 的地方要輸入 y。

Screen 的相關指令如下：

```
# 新建名為 mc 的 screen
screen -S mc

# (不在對應 screen時) 回到指定 screen
screen -r mc

# 列出所有 screen
screen -ls

# 離開當前 screen，讓其在後台運行
Ctrl+A, D
即先按 Ctrl+A 進入接受指令狀態，再按 D。

# kill 當前 screen （即永久關閉）
Ctrl+A, K
會詢問是否 kill，輸入 y。
```

### 运行

先開一個 screen 來專門運行伺服器：

```
screen -S mcserver
```

之後使用 cd 命令到具體伺服端的資料夾內執行啟動指令/腳本即可。

使用 cd 命令切换到服务端所在的文件夹，使用命令启动服务端即可。

重提：

建議在製作伺服端時就將啟動指令保存為腳本，同樣的指令在 Windows 下用 .bat ，再複製一份保存為 .sh 即可。

啟動時直接執行`./start.sh`。在確保伺服端正常運行後，執行前文提到的 Ctrl+A, D，之後就可以`exit`並愉快地遊玩了。

下方給出一個我常用的啟動指令：

```
java -d64 -server -XX:+UseG1GC -XX:MaxGCPauseMillis=100 -XX:+UseStringDeduplication -Xms1G -Xmx2560M -XX:hashCode=5 -Dfile.encoding=UTF-8 -jar server.jar nogui
```

Xms 參數是初始分配記憶體。Xmx 參數是最大分配記憶體。jar 參數是指 Minecraft 的伺服器文件。

### 其他操作

我用於運營伺服器的是阿里雲，不過其它 VPS 服務也可能需要同樣配置。

需要寫「安全組規則」。

阿里雲上的操作是：查看實例 - 更多 - 安全組配置 - 配置規則。配置時參考其它規則即可。Minecraft 的默認連接埠是 25565。

以及選用伺服器時的參考。若是純粹原版生存，伺服端不安裝任何插件，則 1 Core + 2G RAM + 1M 帶寬夠用，大概可以有 8 人以內的連接。

若是 Mod 服，推薦 2 Core + 4G RAM。

---

進階篇完。