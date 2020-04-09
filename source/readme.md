## 你好

Hi，我是 Ceplavia 。

本 repo 用來放我第二次搬遷後的 blog。

在重新部署 blog 的過程中，學到了好多東西，因此我決定把整個過程，以及一些容易犯的錯誤點記下，充當教程吧。讓更多人可以利用 Github Pages 這個服務記錄下自己的想法。

若有閒餘，不妨來一看 -> [Ceplavia's Gaming Blog](www.ceplavia.com)

> Readme & Tutorial update record:
>
> 13/02/20 刪去大量廢話
>
> 06/04/20 語言修改為繁體中文，並再次刪去大量廢話，全文重寫



---

在我所認為的全文完成之後，我才會為 Github repo 裏的 readme 更新目錄。

另外，因為各應用的平台關係，文中部分內容或已不再是最新，但讀者若動手能力足夠，也能跟着做，目前我時間不夠，**之後一定**會重寫有些部分。

[TOC]

## -1. 聲明

本文所使用之單詞或短語或並不專業，惟不接受所謂「名詞警察」之指正。若有此能力請你也寫一篇文章。

此外，在本文仍在編寫的期間，部分內容或會被重寫、刪減，或調換順序。

本文操作均基於 Windows 10，使用 Hexo 引擎。

## 0. 準備工作

### 0.0. 本文是在做什麼？

本文是一篇在 Windows 10 上使用 Hexo 生成靜態頁面，並使用 [Github Pages](https://pages.github.com/) 進行頁面託管。

### 0.1. What is Hexo

Hexo 是一個生成靜態網頁的引擎。它的文章可以支援 [Markdown](https://zh.wikipedia.org/wiki/Markdown) 語法。

動態網頁與靜態網頁的區別在於：

靜態網頁所顯示的內容是預先生成好的。內容在更新上或會有一些延遲，這源自於瀏覽器或 CDN（若有）的快取。但對服務器幾乎沒有性能需求，只有流量產生。

動態網頁所顯示的內容是伺服器實時生成的，因而在大量訪問時，伺服器可能會有較高的佔用。同時，生成內容後才會再返回到瀏覽器，中間也需要一些時間。

除了 Hexo 本身所提供的功能外，本文同時使用名為 NexT 的 Theme，此 Theme 不僅提供外觀美化這種低級功能，同時整合了大量插件，我比較建議使用。

### 0.2. 環境

為了使用 Hexo，首先需要安裝 [Node.js](https://nodejs.org/en/)。官方網站上給出的下載有 LTS 與 Current，我傾向於使用 Current。

之後為了方便部署到 Github，安裝 [Git](https://git-scm.com/downloads)。

最後，當然是需要一個 Github 帳號，請自行註冊。


### 0.3 新建一個 repo（倉庫）

在 Github 上選擇 New repository，Repository name 填寫 `你的ID.github.io`，ID是此處顯示的 Owner 的 lowercase。

舉例，我的 ID 是 Ceplavia，那麼 Repository name 就是 `ceplavia.github.io` 。

如下圖，紅色三角警告是因為我已有同名 repo，不必理會。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/newrepo.png)

完成以上步驟後，你的 blog 鏈接便為 ID.github.io。

> 從此處開始，下文經常出現 ID.github.io，都是指你自己的 ID。


### 0.4. 驗證（Optional）與開啟 HTTPS

可以透過幾個簡單的步驟來驗證上方操作有無錯誤，但，這些步驟想出錯也很難，因此這步驟為 Optional。

在 repo 頁面，上方有一排菜單，進入 Settings，找到名為 Github Pages 的設置項，按 [Choose a theme]，隨便挑選一個後按 [Select theme]，之後會跳轉到讓你編輯 index.md，直接向下拉到底，按 [Commit changes]。

然後，若訪問你的 blog 時能看到之前選擇的 theme，以上步驟即沒有出錯。

在這下方有一個 Enforce HTTPS 選項。如果不打算自行使用其它憑證，勾上即可，使用的是 Github 為你註冊的 Let's Encrypt 憑證。

但我使用 CloudFlare CDN，就可以使用 CF 的憑證而無需勾。

> Wikipedia: [HTTPS]([https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%AE%89%E5%85%A8%E5%8D%8F%E8%AE%AE](https://zh.wikipedia.org/wiki/超文本传输安全协议))
>
> 以前的網站使用 HTTP 協定，而現代瀏覽器不會讓你訪問非 HTTPS 協定的網站。

### 0.5 在本地安装 Hexo

在電腦上建立一個資料夾，建議不要建立在桌面。為了方便導航，我把這個資料夾命名為 _BLOG。

若前文的 Git 安裝順利，那麼在資料夾內 right click，可以看到 Git 的相關菜單，此時選擇 [Git Bash Here]。

> 這是一個方便地快速使用命令提示字元的方式，以後所提到「輸入命令」的行為都在這個窗口內進行。為了偷懶，之後就叫命令窗口。

執行`npm install hexo-cli -g`，等待安裝完成。可能會出現兩行 npm warn，不必在意。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/warn1.png)

執行`npm install hexo --save`，再次出現一些 warn，仍然不必在意。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/warn2.png)

執行`hexo -v`，若能正確輸出一些版本號，即安裝完成。


### 0.6 小結

第 0 章是必須的安裝。

為了方便書寫文章，我建議使用 [Typora](https://typora.io/) 作為 Markdown 編輯器。

為了方便編輯 config，我建議安裝 [Notepad++](https://notepad-plus-plus.org/)。


## 1. Hexo 的基本設置與使用

### 1.1. Hexo 初始化

安裝好 Hexo 後不能直接執行，需要指定一個目錄作為 blog 的目錄，在 blog 的目錄中才可以執行 Hexo 相關的指令。

執行 `hexo init ID.github.io`，這樣會在之前創建的資料夾當中再創建一個名為 ID.github.io 的資料夾，並將該資料夾初始化成 hexo 能認可的 blog。

完成後，會看到一句提示：

```
INFO  Start blogging with Hexo
```

再執行`npm install`，這個步驟會補充一些必要的依賴。

至此，已在本地生成了一個使用 Hexo 默認配置的 blog，但目前只有 blog 的「源文件」。


### 1.2. 本地伺服器測試

在對 blog 進行一些修改後，若是部署到 repo 後再通過訪問的方式來查看效果，未免太麻煩。Hexo 可以在本地開啟伺服器，方便進行 debug 與預覽修改。

上文提過，已在本地生成了 blog 的源文件，要看網站部署後的效果，首先要生成靜態頁面。

此處開始，所有指令，都需要在 ID.github.io 這個資料夾內執行。若你熟悉 linux，可以使用 cd 命令來切換資料夾。否則，你可以在 Windows 的檔案總管進入該資料夾後再右擊選擇 [Git Bash Here]。

執行`hexo g`生成靜態網頁。

執行`hexo s`在本地開啟伺服器。

使用瀏覽器訪問 localhost:4000，即可訪問生成的靜態網頁。可以看到一篇自帶的文章，上面寫的是指令用法。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/hexopage.png)

g for generate，s for server，d for deploy。

看得差不多之後，在命令窗口內按 Ctrl+C 即可關閉伺服器。

### 1.3 部署到 Github 的設定

為了安全地部署到 Github 倉庫，下面將進行一些設置。

執行以下指令：

```
git config --global user.name "ID"
git config --global user.email "郵箱"
```

上面要填寫的是託管頁面網站的資料，本文使用 Github，所以是 Github 的註冊資料。

部署時不使用密碼驗證的方式，使用 SSH key 。執行`ssh-keygen -t rsa -C "郵箱"`。

執行後會先問在何處保存 key。默認在 C:\Users\你的賬戶\\.ssh，要使用默認位置的話直接回車。之後會再問是否在使用 key 的時候要輸入密碼，根據個人喜好來決定輸入密碼還是直接回車。

> 密鑰都是一對的，每對密鑰包含公鑰與私鑰。

如果上方提到的 .ssh 資料夾中原本在有你的其它 key，那你應當很熟悉，可以跳過下面這段。

> 上方生成的密鑰，名為 id_rsa，在對應資料夾內有 id_rsa（私鑰）和 id_rsa.pub（公鑰）。若資料夾中沒有多對密鑰時，系統通常默認使用名為 id_rsa 的密鑰，但有多對密鑰時，就需要使用 ssh-agent。
>
> 舉個例子，ssh-agent 像是一個智能的鎖匙釦，當你要開不同的門，會自動彈出對應的鎖匙。

執行以下指令，把剛才生成的密鑰添加到 ssh-agent。如果剛才生成的時候輸入了密碼來保護，會提示要求輸入密碼。之後，我建議備份這對密鑰。

執行以下兩條指令：

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

回到 Github 的網站，點擊右上角自己的頭像，進入 Settings，左側選擇 SSH and GPG keys，按綠色的 [New SSH keys] ，Title 隨意寫，只是為了辨識。

找到 C:\Users\你的賬戶\\.ssh 这里，找到 id_rsa.pub（又或是你使用的其它密鑰），用 Notepad++之類的程式打開，將內容全選複製到 Key 這欄，按 [Add SSH key] 。

再回到命令窗口，執行 `ssh -T git@github.com`，應該會見到一句 Hi 信息。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/sshtest.png)

> 當每一次用密鑰對登入到新的地方時，會提示是否要將站點加入 known hosts，輸入 yes 即可。

在 Hi 後面能看到自己的 ID，就沒有問題了。

### 1.4. blog 的設置項

### 1.4.1. 部署設置項

打開 blog 資料夾內的 \_config.yml，下拉到最後一行，內容應該是這樣：

```
deploy:
  type:
```

修改一下，改成這樣：

```
deploy:
  type: git
  repository: git@github.com:ID/ID.github.io.git
  branch: master
```

這裏的設定將生成的靜態頁面部署到 master 分支。另外，填寫內容的兩個 ID，前者應該是你自己的 Github ID，也就是有大小楷區別。後者是 repo name 中的 ID，應當是小楷。

> 這是第一次修改 blog 的 \_config.yml，請注意類似「type: git」這種設置項，半型冒號後必定有一個半型空格，否則在使用`hexo g`生成頁面時必定會報錯。如果在生成頁面時遇到錯誤，最好先回想一下是否剛修改過設置。

### 1.4.2. 個人設置項

在這個 \_config.yml 的開頭有一些專屬於你的 blog 的設置，如 title、description 等。

在下方的 \# URL 分類中有一個 `url:`的設置項，這裏若你打算使用自己的域名，就填寫`https://www.yourdomain.com`，若是打算使用 Github Pages 提供的域名，就寫`https://ID.github.io`。

再往下有一個 \# Writing 的分類，在 `post_asset_folder`這個設置項，把後面的 false 更改成 true。原因在下文會提及。

剩餘部分的設置項就可以看個人喜好來更改，但更改前最好參考 Hexo 的官方 [Docs](https://hexo.io/docs/configuration.html)。

### 1.5. 寫一篇
執行`hexo new post "文章标题"`就可以新建一篇文章，指令中的雙引號最好保留。文章存放在 blog 中的 .\source\\_posts，現在裏面除了一篇 hello-world.md 之外，就是你剛新建的新文章。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/newpost.png)

> 自己在 \_posts 資料夾內創建 md 文件也可以，但開頭的參數就要自己寫了。

兩個「---」中間是這篇文章的參數，包括時間、分類、標籤等。第二個「---」下方就是正文。

> 開頭的date: 後面沒有寫時間的話，就使用這個文件的修改日期當發佈時間，例如 hello-world.md。

### 1.5.1. 插入圖片

文章正文可以使用 Markdown 的語法來插入圖片，但這個語法在 Hexo 中並不支持，Hexo 使用另一句語法來引用圖片。

當然，有一個插件可以輕鬆地解決這個問題，在 generate 的時候把 Markdown 的語法轉換成 Hexo 的，但我不會推薦，也建議不要使用。因為就我多次試圖使用的經歷來看，這個插件有嚴重的 BUG，以及用法指引可以在 Google 上搜出好幾個版本。

使用 Hexo 引用圖片的方式，能夠引用 .\ID.github.io\source\\_posts\ 下的圖片，但若是所有圖片都放在此處，就容易引起混亂。前文 1.4.2. 設置了`post_asset_folder: true`，這是在創建新文章時，為文章自動建立同名資料夾，並只引用此資料夾內的圖片。

當然可以不止引用圖片，其它媒體資源也可以放在此資料夾內。

在文章中使用以下語句來引用圖片：

```
{% asset_img 圖片文件名 圖片標題 %}
```

例如：

```
{% asset_img test.png This is a test png. %}
```

注意這其中的空格。

在 Hexo 的 [Docs](https://hexo.io/zh-tw/docs/asset-folders.html) 裏有可以引用的媒體資源列表，但我一向認為 Hexo 寫 Docs 的人沒打算寫得簡單易懂。

### 1.5.2. 插入音頻

外鏈音頻比較簡單，複製提供的 Html code，粘貼到文章中即可。

而引用自己上傳的音頻，同樣需要將音頻文件放在媒體資源資料夾當中，然後用 Html 引用。

在文章中插入以下語句：

```html
<audio controls src="filename.mp3"></audio>
```

即可。

**引用時請注意版權問題。**

另外，在 Typora 中編輯時音頻不能播放，但在 deploy 後正常。亦有另一種寫法在編輯時也可播放，但該寫法太舊，且我認為沒有必要。

### 1.5.3. 補充事項

1. 若標題帶半型冒號「:」，則需要在文章的 .md 文件中用半型雙引號「""」括起。
2. 在文章中添加`<!--more-->`，在主頁顯示文章時則只會顯示此語句前面的部分。並添加一個 [Read more>>] 按鈕。當文章很多時，這能有效加快頁面載入速度。

### 1.6. 草稿

建立草稿的方法有兩種，一種是將現有 \_posts 資料夾內的文章變成草稿，另一種是直接新建草稿。

對於現有文章，將其移動到 source 目錄下的 \_drafts 資料夾即可。

新建草稿則使用以下指令：

```
hexo new draft "標題"
```

草稿寫完後，使用以下指令發佈：（發佈只是將其移動到 \_posts 資料夾）

```
hexo p "標題"
```

> P for Publish
>


## 2. 第三方服務

### 2.1. 域名（Optional）

若希望使用自己的域名，在 Google 應該能找到不少服務商，此處不提。

購買域名後，需要進行以下設置：

Github repo - Settings，就在 0.4. 提到的地方，再往下。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/customdomain.png)

將你希望使用的域名填入此處。

之後在購買 domain 的地方，添加 CNAME 類型解析。將 www 及 @ 指向 ID.github.io 即可。

再在 .\ID.github.io\source 下新建一個 CNAME 檔案（無擴展名），裏面直接寫你的域名，與上方填寫的一致。

這樣設置之後就可以訪問自己的域名跳轉到 blog 了。

> 以上默認你使用一級域名，即 www.domain.com。若使用二級域名，如 blog.domain.com 也是同樣的操作。

### 2.1.1. 關於域名的小問題

在跟隨本文操作時，或許並不想購買域名，但之後又想使用自己的域名。在使用自己的域名之後，注意一些服務都可能要重新綁定。比方說下文會介紹使用的 Disqus 評論系統。評論綁定在文章鏈接上，鏈接變更可能會丟失評論。

此外，blog 的 \_config.yml 內容也記得要改。這些需要改的地方可以記在一個 txt 中，就放在 \_BLOG 這個資料夾中，方便查找。

### 2.2. 使用 CLOUDFLARE CDN（Optional）

此步驟僅在有個人域名時才可進行。

使用 CDN 的優點是可以提高在全球訪問的速度（中國大陸除外），這一步我在操作的時候沒有截圖，只能憑回憶寫。

Cloudflare 註冊後，會讓你填寫需要託管的域名，之後會讓你修改這個域名的 NS (Name Server)，在域名註冊商處可以修改。

修改完成之後，域名解析就託管在 Cloudflare 了。它應該會自動掃描域名上的解析項並自動添加，如果沒有，記得自行添加。Proxy status 默認應該是 Proxied，如果不是，可以點擊切換。

之後在上方的 SSL/TLS 選項卡，可以更改設置。默認是 Full，這樣訪問 blog 時的憑證就是由 CloudFlare 簽發。不再需要使用 Github 代註冊的 Let's Encrypt 了。

### 2.3. NexT Theme

Hexo 的 Theme 不僅是外觀上的改變，也有提供許多第三方插件的 Theme。

Next Theme 的優點是提供了許多第三方插件，可以按需啟用。

缺點是有的人覺得這樣十分臃腫。但我覺得便利性比較重要。

執行以下指令，從 NexT 的 [repo](https://github.com/theme-next/hexo-theme-next) 中拉取一份複製到本地：

```
git clone https://github.com/theme-next/hexo-theme-next themes/next
```

> 更新 Theme 的部分會在下文提及。

找到 blog 的 \_config.yml，找到`theme:`設置項，更改之：

```
theme: next 
```

即可。

注意，從現在開始，引入 Theme 的 \_config.yml。之後的操作多數是對 Theme 的 \_config.yml 進行操作。

路徑是 .\ID.github.io\themes\next\\_config.yml，注意區分。

### 2.4. 評論系統（Disqus）

我使用的服務是 [Disqus](https://disqus.com) 。

創建帳號後，在[主頁](https://disqus.com/)按 [Get Started] -> [I want to install Disqus on my site]。

中間有一個 Website Name，這個並不是填寫你的域名。填寫自己覺得好記的即可。例如可以填寫 yourname+blog。

> Disqus 默認將 Website Name 作為 Shortname，而我們配置時需要用到 Shortname。
>
> 但，如果 Shortname 與其它人的重複，不會有提示，但 Disqus 會略改一下你填寫的 Shortname。
>

Catagory 和 Language 就按自己喜好，之後按 [Create Site] 。

選 PLAN，向下拉，有免費。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/disqus.png)

然後，左上角有步驟，直接點 [Configure Disqus]，第二步不必理會。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/disqusconf.png)

這個頁面中填寫 Website URL，URL需要帶 https:// ，我的截圖中沒有，下一步就會有警告。

之后就完成了 Disqus 帐号的配置部分。

然後找到主題的 \_config.yml，更改對應的設置項：

```
disqus:
  enable: true
  shortname: use your own shortname!!!
  count: true
  lazyload: false
```

> 找設置項可以使用 Ctrl+F 來搜尋。

### 2.5. 統計訪問數（Google Analytics）

若使用 CloudFlare CDN，可以查看到訪問數，但那個有些假，連 Python 爬蟲可能也計算在內。於是我選擇使用 Google Analytics。

先到 [Google Analytics](https://marketingplatform.google.com/about/analytics/) 新增資源，類型選擇互聯網。

資源設定需要填寫的是網站名稱、產業類別、報表時區。

资源设定要填写的东西是网站名称、网站网址、产业类别。

網站名稱可以自己隨便填，另外兩個認真寫。

> 域名有变更的话记得回来改网址。

之後會得到一個 UA-xxxxxxxx-x ，在主題的 \_config.yml 修改對應設置項即可。

```
# Google Analytics
google_analytics:
  tracking_id: use your own tracking id!!!
  only_pageview: false
```

### 2.6. 投放廣告（Google Adsense）

### 2.6.1. 申請

注意：要使用這項服務，你的 blog 需要有一定數量的文章，如果沒有，就幾乎不會過審。

> 有人說要有 5 篇文章，也有人說要有 10 篇，也有人說要運營半年以上。但都要看 Google 心情。

在 [Google Adsense](https://www.google.com/adsense/start/#/?modal_active=none) 註冊，就是簡單的填網址，填郵箱地址。直到他給你一段類似這樣的程式碼：

類似這樣的程式碼：

```
<script data-ad-client="!!!!!!!!!!!!!!!!!!" async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
```

Google 需要使用者對網站的 HEAD 部分進行一點修改，來在網站投放廣告，並認證你是網站的擁有者。要修改的文件是 .\ID.github.io\themes\next\layout\\_partials\head\head.swig。

把以上程式碼粘貼到這個文件的底部。之後將網站部署，也就是執行`hexo g`和`hexo d`。

> 若你決定使用 Google Adsense，在修改完此文件後，請看一下 4.2. 的部分，再回來繼續閱讀。

回到 Google Adsense 網站，勾選「我已将程式码贴进网站中」後按完成，等待審核。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/gad.png)

我的 blog 申請時文章較多，大概等了不到一天。

### 2.6.2. 廣告設置

過審之後，其實應該就會開始自動投放廣告了。不需要特別設置。

本文第一次書寫時，Google 給出的程式碼非常長，並只作驗證效果。要添加單元廣告的話，需要自己再修改 layout，但後來又不再需要這麼繁複的步驟了。所以我就只把當時的圖片放在這裏。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/ad.png)

### 2.6.3. 注意事項

不要試着點自己網站上的廣告，Google 非常聰明。

另外，我以前用過 Blogspot 的 Google Adsense 服務，並且沒寫什麼文章。再打開 Google Adsense 網站時會提示以前的 Blogspot 上沒什麼內容，要修正，blablabla。

在帳戶設定中取消帳戶，然後等一段時間再重新註冊此服務即可。如果取消帳戶的按鈕是灰色，也是等一段時間就可以取消了。

### 2.7. Sitemap

## 3. blog 的個人化

### 3.1. 新的 page

### 3.1.1. about

About 頁面應該是每個人都想要首先添加的，所以我放在第一個。

執行以下指令來添加 about頁面：

```
hexo new page "about"
```

然後就可以在 .\ID.github.io\source\about 見到一個 index.md。

> 如果在 1.4.2. 中設置了`post_asset_folder: true`的話，此處會有一個名為 index 的資料夾，同樣是引用媒體資源用的。其它 page 也是如此。

要控制 blog 主頁的菜單中顯示的頁面項，則可以在主題的 \_config.yml 中修改：

而控制有什么页面要显示的设定则是在主题的 \_config.yml 里，在约 152 行附近就能看到：

```
menu:
  home: / || fa fa-home
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  tags: /tags/ || fa fa-tags
  about: /about/ || fa fa-user
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat
```

> 這是我的設置項，和默認的或許有些許不同。此處的規則是按順序排列，每項則為：
>
> `顯示名稱: /引用目錄/ || fa 圖標名稱`
>
> 名稱似乎是自動首字母轉大楷。引用目錄即為 .\ID.github.io\source 中的目錄，也就是新建 page 時使用的名稱，雙豎線之後是 FontAwesome 的圖標。NexT Theme 在 v7.8 版本開始使用 FontAwesome 5，因此引用方式需要注意。圖標名稱可以在 [Font Awesome](https://fontawesome.com/icons?d=gallery) 找到。

### 3.1.2. 類別（categories）和標籤（tags）

這兩個頁面和 about 頁面在操作上有些許的區別。

執行以下兩條指令來新增這兩個頁面。

```
hexo new page "categories"
hexo new page "tags"
```

之後定位到這兩個頁面的 index.md，在 date 項下方新起一行分別寫上下面的語句即可：

```
type: categories
```

```
type: tags
```

要啟用在主頁菜單上顯示，操作和 3.1.1. 一樣。

要給文章標記上類別或標籤，可以在兩行「---」當中添加。

添加類別使用如下語句：

```
categories:
  - 類別1
```

同一篇文章可以添加多個類別：

```
categories: 
  - [類別1, 類別2]
```

另外，這樣的寫法的話，類別 2 是類別 1 的子類別：

```
categories: 
  - 類別1
  - 類別2
```

添加標籤簡單許多，這樣寫即可：

```
tags: 
  - 標籤1
  - 標籤2
```

这样就可以了。

### 3.2. 範本（scaffolds）

看完 3.1.2. 之後，你應該意識到每篇文章都應該包含 categories 和 tags。

每次新增文章都要重新寫上的話，有點麻煩，於是可以修改生成空文章時的範本。

位置是 .\\_BLOG\ID.github.io\scaffolds，現在只講如何修改 post.md，其餘兩個 draft.md 和 page.md 的修改方式沒有太大變化。

post.md 我修改成了這樣：

```
---
title: {{ title }}
date: {{ date }}
categories: 
  - 
tags:
  -  
---
```

可以按照自己喜好更改。

### 3.3. 文章文件的命名格式

在文章多了之後，有時可能會對特定的一篇文章進行修改，找起來可能有點麻煩。本節中可以修改新建文章時默認的命名方式。

修改的位置是 blog 的 \_config.yml，修改`new_post_name`項，這個項默認是`new_post_name: :title.md`

Hexo 的 [Docs](https://hexo.io/zh-tw/docs/configuration) 很白癡，我不知道他想說明什麼。

![](https://github.com/Ceplavia/ceplavia.github.io/blob/master/readmeimg/newpostconf.png)

這個設置項可以使用的名稱如下：

| 名稱     | 註釋                    |
| -------- | ----------------------- |
| :title   | 標題（空格替換成「-」） |
| :year    | 4 位數年份              |
| :month   | 2 位數月份，7 月即 07   |
| :i_month | 月份，没有0，7月就是7   |
| :day     | 2 位數日子，7 號即 07   |
| :i_day   | 日子，没有0，7号就是7   |

我使用的格式是：

```
:year-:month-:day_:title.md
```

使用此格式的文件名是「2020-02-20_標題」。

### 3.4. blog 圖標

ID.github.io\themes\next\source\images 裏可以看到兩張圖片，分別是favicon-16x16-next.png`和`favicon-32x32-next.png`。

我想要怎麼修改已經很明顯了……

### 3.5 友鏈

設置項在 theme 的 \_config.yml，設置項是 `links: `，可以通過搜尋`# Blog rolls`這句註釋來定位。

添加友鏈就像示例一樣簡單：

```
#Title: http://example.com
```

## 4. 後期維護

### 4.1. Hexo 的更新

Hexo 的更新非常簡單，由於 Hexo 也算是一個 Node.js 組件，因此只要執行以下指令即可。

```
npm update
```

### 4.2. NexT Theme 的更新

在更新 NexT Theme 之前，需要簡單介紹一下 Git 的工作方式。

在 2.3. 中，「安裝」NexT 的指令是`git clone`，這意味着從 repo 複製了一份最新的主題。但本文在 2.6. 部分教過修改 layout，並且對 theme 的 \_config.yml 也進行了修改。這意味着本地用的是經過修改的版本。當然也可以重新拉取一份最新版，然後自己再修改這些部分。但這相當煩。

實際上在修改完文件後，應當將修改 commit，之後再執行操作時就可以免去一些麻煩。執行以下指令來將修改的文件 commit。

```
git add "修改的文件"
git commit -m "message"
```

> 以上的 message 實際上是給自己看的，如果懶，隨便寫一句「modified xxx file」也可。
>
> 另外，指令要指向文件，請注意當前的操作目錄是否文件所在目錄，若非，則需要自行 cd 到對應的目錄再執行，或是寫上路徑。

（按照我的理解）Commit 之後就意味着在本地使用着自己的版本，而拉取新版本時執行的操作為「合併」。

在 ID.github.io\themes\next 這個資料夾呼出指令窗口，再執行以下指令

```
git pull
```

這個指令會拉取一份最新的版本並自動與本地合併。但有的時候自動合併會出錯，因為有的地方並不是簡單的設置項變更。

先只介紹如何處理。簡單講的話，提示有衝突的文件當中，會找到如下一段內容

```
<<<<<<< HEAD
CODE A
=======
CODE B
>>>>>>> (一段看似亂碼的內容)
```

此處的 A 是原本文件的內容，而B是從 repo 新拉取的內容。簡單來說就是兩者擇一，但當然沒有這麼簡單。新的內容當然是要合併到這個文件的，但 CODE A 的部分可能有一些自己的修改內容，只要將其中修改的內容提取出來，再寫在對應的位置即可。

說起來或許有些抽象……或許我要再舉個例子。下面這段 code 是我實際更新 NexT 時遇到的（記憶中是這樣），仔細看的話，pull 的內容中，雙豎線後的內容發生了變化，在這類設置項中，多了空格就意味着是多一個參數（所以有些設置項在填寫時需要添加雙引號來表明是同一項），自動合併就理解不了，此時只要自行合併一下即可。

```
<<<<<<< HEAD
menu:
  home: / || home
=======
menu:
  home: / || fa fa-home
>>>>>>> (一段看似亂碼的內容)
```

合併好，並移除那些「<<<===>>>」的部分，只留下 pull 到的部分和自己原本或有修改的部分，再重新執行以下指令。

```
git add "修改的文件"
git commit -m "message"
git pull
```

應該就會成功更新。

### 4.3 更換電腦？



## 5. 警告，警告，警告！

这个部分用来写我遇到的坑，虽然不知道有没有人会看，但写下来，也很重要。

一个小忠告，在跟着任何人的教程做任何事之前，最好自己也把步骤写下来，不要嫌麻烦，在出现了什么坏事之后这就是最好的帮助文件。当然用版本管理会更简单，但我不喜欢这东西。

### 5.1 千万不要去动Permalink

这是一个在博客本体的 `_config.yml` 里的一项设置。这个可以改动文章的链接。

默认应该是 `:year/:month/:day/:title/` ，千万不要去动！至于为什么：

注意前面的三个参数是为了区分文章的（区分什么我也不知道）。我曾经试过把前面的去掉，只留 `:title/` ，就出事了，每篇文章的格式都变掉了。

比方说，一篇文章链接应该是 `xxx.com/2019/09/15/标题标题` ，在没有前面的部分之后，会**永久**变成 `xxx.com/2019/09/15/5_标题标题` ，多出来日期的最后一位和一个下划线。

**啊我他🐴就是不知道为什么。**

总之这是永久变更的，会逼死强迫症，我就因为这样重置了几次整个博客目录，很庆幸我把整个过程都写了下来，否则我真是没法修，因为碰过的地方太多，我绝对是记不清的。

### 5.2 慎用 HTML 嵌入

实际上我也不知道在 markdown 里插入 HTML 的语句是不是叫嵌入，我就先这么写了。

我在我博客的 `/about` 页面用过一些 HTML 语句。例如 `<center></center>` ，因为 markdown 没有置中嘛。

但就弄出来了一个 Bug ，就是这个页面只有底部的 Google AD 和 「Powered by Hexo」 这部分会载入，其它部分完全空白。

要是在这个页面按 F12 的话，能看到一条红色报错……我忘记截图了……

但页面内容是都载入了的，在该有文字的地方双击甚至能选中文字。但他就是一片白。

如果发现有页面一片白，请注意是否写了一些 HTML 语句。

### 5.3 东西可以乱吃，插件不要乱装

插件不要乱装，真的。

昨天我忘了是在搞一个什么功能了，总之跟着一个 17 年的教程，让我装插件，装完了之后其实多数插件都会有很多依赖，而那教程让我来一句 `npm audit fix` ，也就是更新装上的插件的依赖。

啊不要乱装，再次重申。

我完全没有发现，它竟然把 hexo-asset-image 这个垃圾给我装回来了，虽说被我发现了，但这也是一个要注意的地方。

### 5.4 更多想要的功能

事实上能搜到的不少美化教程之类的，都是 17 年前后的，比方说我之前试着在文章末尾添加一个版权声明，也就是 license 。

按照搜来的一个鬼教程，是要自己去改网页 layout 的，前后涉及超过 3 个文件。

但后来我发现……这东西在 Next 主题的设置里就有，搜 `license` 搜到位置之后，把 `post:` 设置成 `post: true` 就好了。

很多想要的功能事实上在主题里都已经自带，不再需要自己修改太多的东西。这个可以关注一下主题更新。

---

<center><- 工事中 工事中 工事中 工事中 工事中 工事中 工事中 工事中 工事中 -></center>>
---

<center><- CEPLAVIA LINE DO NOT CROSS CEPLAVIA LINE DO NOT CROSS -></center>
---

## 杂谈

最初，我只是打算在 Github Page 放一个 MC 服的更新页面（原本以为只能放点简单的页面）。但后来仔细看了好久的教程之后，我决定把我的博客搬过来。

因为我觉得自己从头搞的话，可定制度更高。我很不喜欢 WordPress 那种万事包办的感觉，况且主机壳那有储存空间限制，我偶尔写文章比较喜欢放图和音乐。对于我来说，WordPress 魔改几乎靠插件，而且有些插件看着评分高、使用人数多，自己装上了又没用，搜教程又搜不到，很烦躁。此外，原因还有主机壳的 host 托管也是没有太多可以可以调的设置。比方说 PHP7 永远是测试版，问题多，而 WordPress 后来的版本想要升级就一定要使用 PHP7 。

最初搜 Github Page 的教程时几乎都没有多少会教你从头开始的。也没有人会说 Jekyll 和 Hexo 到底有什么区别。

而对我来说，Hexo 可以在本地更改各种布局，并测试完成好再部署到 Github Page ，因而就决定是它了。跟着他人的「教程」来使用 Jekyll 的印象见「-1.1」。

> 几乎没有人提到 Jekyll 的文件目录结构这回事，那真的是从头开始。添加文章直接在 Github 仓库操作添加文件到底是什么骚操作……

决定使用 Hexo 之后，在搜索教程时并没找到多少好的，原因有几个：

一是不知道部分写教程的人有什么毛病喜欢照搬官方文档，的那种语气，一句人话不写；

二是时间太久远，有些路径都是错的；

三是写什么都不详细，写在简书知乎这种屑地方看起来一千字不到就没了。

于是决定这次把过程详尽地记录下来，算是充当一个教程吧。因为我很讨厌在跟着教程做的时候，步骤完全一样，却出来不一样的结果，所以我会写得很详细，务求将所有的坑都踩一遍，让更多想搭博客的人少走点弯路。