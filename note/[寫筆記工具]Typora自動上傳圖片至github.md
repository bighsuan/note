# Typora 自動上傳圖片至github

(本篇文章使用之token為範例token，你實際拿去用是沒辦法用的喔!)



這篇要來介紹我平常使用的markdown筆記軟體 -- Typora，Typora對我來說最大的優點就是畫面乾淨，相比之下Notion的介面就很花俏，使用時會覺得有各種按鈕在干擾我，如果喜歡寫筆記時不要有太多干擾的人可以試試。

今天要特別介紹的圖片上傳功能，則是因為我寫完文章後會貼到wordpress上傳，但若直接複製到wordpress編輯，圖片會壞掉，需要一張一張上傳再插入到文章對應的位置，這個過程很繁瑣，而且我的圖片經常只是畫面截圖，根本沒有特別記檔名是什麼。

所以我今天要來教大家如何在Typora中直接把圖片上傳到github，並自動把markdown中的路徑取代為github圖片網址，讓你貼到wordpress時不用再一次上傳！

順帶一提我選擇github只是因為方便加上我不喜歡大陸的圖庫，不喜歡github的人也可以自行選擇其他圖庫。





## Step 1. 下載PicGo-Core

可以使用npm或yarn安裝[PicGo-Core](https://github.com/PicGo/PicGo-Core)，如果你兩個都沒有的話，先裝個[Node.js](https://nodejs.org/en/download/)就會有npm了。



npm全域安裝picgo

```
$ npm install picgo -g
```



如果是mac使用者的話需要先輸入下面這行開權限才能全域安裝

```
$ sudo chown -R $USER /usr/local/lib/node_modules
```



安裝完路徑會在

```
/usr/local/lib/node_modules/picgo/bin/picgo
```



也可以測試一下有沒有安裝成功

```
$ picgo -v
1.4.19
```





## Step 2. 產生config文件

可以參考[這篇](https://picgo.github.io/PicGo-Core-Doc/zh/guide/config.html#默认配置文件)，大概步驟如下：



1. 用`picgo set uploader`產生config

   選哪一種uploader都可以，反正等等會整個config檔案覆蓋

```
$ picgo set uploader
? Choose a(n) uploader (Use arrow keys)
  smms
  tcyun
❯ github
  qiniu
  imgur
  aliyun
  upyun
(Move up and down to reveal more choices)
```



2. 打開`config.json`

   路徑如下圖，例如我的config檔案位置是在`/Users/lisa/.picgo/config.json`

![截圖 2021-06-04 下午2.54.48](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195348.png)



3. 貼上下面這段

   打開並改成下面這樣，那裡面的repo和token可以先留空，待會我們才要建github repo和產生token來填入這些欄位。

```
{
  "picBed": {
    "github": {
      "repo": "<github-repo>",
      "token": "<github-token>",
      "path": "img/",
      "customUrl": "",
      "branch": "main"
    },
    "current": "github",
    "uploader": "github"
  },
  "picgoPlugins": {}
}
```





## Step 3. 開GitHub repo

開一個新的GitHub儲存庫，記得選public，不然其他人看不到圖片。

建好後剛剛上面config裡的repo就可填入`[username]/[repo name]`，例如我的是 `bighsuan/blog`。

<img src="https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195358.png" alt="截圖 2021-04-19 上午12.04.51" style="zoom:50%;" />





## Step 4. 產生token

從個人資料的 Settings > Developer settings > Personal access tokens，點選Generate new token：

![截圖 2021-04-19 上午12.12.25](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195408.png)

![截圖 2021-04-19 上午12.15.35](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195410.png)



隨意給個token名字，下面勾選repo，就可以產生token了。

![截圖 2021-04-19 上午12.19.35](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195419.png)



記得把token複製起來，貼到剛剛config裡的token那欄（圖片是範例token）。

![截圖 2021-04-19 上午12.23.31](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195428.png)



## Step 5. 最終config

最後的config會長這樣，這裡要稍微注意一下branch是不是和你github上的一樣，還有`"path": "img/"`是指上傳的圖片會傳到bighsuan/blog的img資料夾，若不希望多一層資料夾的可以改成`"path": "/"`

```
{
  "picBed": {
    "github": {
      "repo": "bighsuan/blog",
      "token": "你自己的token",
      "path": "img/",
      "customUrl": "",
      "branch": "main"
    },
    "current": "github",
    "uploader": "github"
  },
  "picgoPlugins": {}
}
```



## Step 6. 測試圖片上傳

從Typora的偏好設定 > 圖片，Image Uploader選擇"Custom Command"

![截圖 2021-04-18 下午11.41.43](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195439.png)



![截圖 2021-06-04 下午3.17.39](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195443.png)



Command的部分填入

```
[node] [picgo] uplaod
```



注意node和picgo須給完整路徑，例如我的是

```
/usr/local/bin/node /usr/local/lib/node_modules/picgo/bin/picgo upload
```

<img src="https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195457.png" alt="截圖 2021-04-19 上午12.50.56" style="zoom: 50%;" />



確定config和command沒問題後可點擊Test Uploader來測試上傳圖片是否成功。

<img src="https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195501.png" alt="截圖 2021-04-19 上午1.02.32" style="zoom:50%;" />





## Step 7. 完成！

可以在圖片點擊右鍵，選Upload Image上傳到github了。

<img src="https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195509.png" alt="截圖 2021-04-19 上午1.52.41" style="zoom:50%;" />



也可以在偏好設定選插入圖片時自動上傳圖片，不過我自己常常會對圖片修修改改，如果自動上傳就會上傳太多沒用到的圖片，所以我比較習慣等整篇文章寫完後一口氣用右鍵上傳。

<img src="https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604195513.png" alt="截圖 2021-04-19 上午1.04.43" style="zoom:50%;" />





## 補充-改字體

另外補充一個我新裝typora時必做的設定--改字體，尤其是在Windows時預設好像是新細明體，簡直讓人一打開就想關掉。

改字體可參考[這篇文章](https://medium.com/ishengp-laboratory/typora-1c244c192d5b)，我是改成[思源黑體](https://fonts.google.com/specimen/Noto+Sans+TC)，並且在header的地方加粗，然後字體大小改18px。

就算你沒那麼在乎字體，我也強力推薦起碼改成微軟正黑體。

