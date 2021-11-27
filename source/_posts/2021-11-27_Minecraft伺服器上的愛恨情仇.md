---
title: Minecraft 伺服器上的愛恨情仇
date: 2021-11-27 17:15:53
categories:
  - 視點
tags:
  - Minecraft
  - 伺服器
---

架設 Minecraft 伺服端其實沒有看起來那麼簡單， 儘管大部分人真的覺得很簡單。

在 1.18 (aka 1.17 part 2) 即將發佈之際，寫下一點重大變更。

<!-- more -->

### 找數

還記得當初我説一定要補上多節點 MC 的方法嗎？答案就是流量轉發。只要轉發端到 MC 伺服端是直連，或是路由相對簡單，延遲又低，就可以做到。對於我自己的伺服器，為了大部分人易於訪問而部署在上海，然後使用香港、日本、台北的 VPS 進行轉發，就可以做到各地都是低延遲的體驗。

流量轉發我用 NGINX。不用防火牆來轉發的其中一個原因是性能不足，使用很簡單，只要下面幾行。

```
stream {
    server {
        listen 25565;
        proxy_pass YOURMC.DOMAIN.COM:25565;
    }
}
```

很簡單。

### Hybrid Server 的愛恨情仇

有一天偶然看到這一篇，當初是為了性能而選用的 Mohist，但沒有注意到 Mohist 會這樣做。

https://essentialsx.net/do-not-use-mohist.html

> It has come to our attention that as of [10th April 2021](https://github.com/MohistMC/Mohist/commit/58bbb1c8a13dcbf764c11668287e6fb85a884b3a), **Mohist tricks users into deleting official plugin jars and installing unofficial modified builds.** Not only is this behaviour shady, but it also poses significant risk to users who don't know what software they're running. 

已經説得很清楚了。自此之後再運營 Mod 服，大概我也不會再考慮 Mohist。

### JVM?

自 MC 的 1.17 發佈以來，當時我就留意到官方要求使用 JDK17 來進行遊戲，……其實也不算官方要求，而是低版本 JDK 會報錯。然後我就開始留意各種 JVM 的性能，有好有壞的就不説了，只拿好的來説，就是 Azul Zulu 和 Alibaba Dragonwell。前者相對更優，後者很棒，不用多説了。後者甚至完美兼容 Hotspot JVM。

為了表達對 Oracle 的感謝，我決定在 1.18 的原版生存服中使用 Alibaba Dragonwell JVM。

不過 Dragonwell 有個小缺點，就是對於新 JDK 版本的發佈較慢，但相對於它對 CPU 和 RAM 的使用優化，值了。

### Server?

原本我對插件伺服器的認知還停留在 Bukkit -> Spigot/Paper Spigot(aka PaperMC)，不過後來又多了 Purpur，Yatopia 等，在研究了很多對比後，在即將到來的 1.18 原版生存服，我決定使用 Purpur。

https://purpur.pl3x.net/downloads/

### OS

CentOS 大抵最終還是死了（嘆氣）。

説起來很有趣，我一直喜歡 CentOS 的原因有兩個，一個當然是它可以縮寫成 CTOS，嗯……

另一個就是 CentOS 的命令在我看來很「合理」，比如直接 yum 而不是 apt-get 還要按個「-」。

最後決定用 Debian，百年老店，值得信賴。CentOS Stream 又或者 Rocky 不過是狗尾續貂。

不過我還是懷念 CentOS 那很久不更一次版本號的操作。