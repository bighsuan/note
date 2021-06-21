# picgo上傳圖片時自動命名

這是typora上傳圖片的第二篇，[第一篇在這裡](https://blog.codefarm.com.tw/2021/04/19/typora-自動上傳圖片至github/)，還沒看過的要先去跟著第一篇設定picgo喔，這篇只會介紹如何自動命名上傳的圖片。

照著第一篇流程上傳的圖片，檔名會是你電腦上的命名，若有重複的檔名就會上傳失敗，而且這樣檔名也會很亂，因此補充如何以時間自動命名。今天要使用的是[picgo-plugin-super-prefix](https://www.npmjs.com/package/picgo-plugin-super-prefix)這個plugin。



## Step 1. 安裝picgo-plugin-super-prefix

用node執行picgo安裝picgo-plugin-super-prefix

```
node picgo install picgo-plugin-super-prefix
```



注意不是`npm install picgo-plugin-super-prefix`，用npm裝的話會在跟picgo同層，我們要的是picgo-plugin-super-prefix裝在picgo裡面。



安裝後目錄會像這樣：

```
/Users/lisa/.picgo/
|-- node_modules
|   |-- dayjs
|   |   `-- other files...
|   `-- picgo-plugin-super-prefix
|       `-- other files...
|-- config.json
|-- package-lock.json
|-- package.json
`-- picgo.log

```



在.picgo目錄中多了node_modules和package.json、package-lock,json，picgo-plugin-super-prefix則在node_modules中。



## Step 2. 修改config檔

config中的picgoPlugins裡增加一行，表示要使用picgo-plugin-super-prefix

```
"picgo-plugin-super-prefix": true
```



picgoPlugins同層下面增加picgo-plugin-super-prefix的設定

```
"picgo-plugin-super-prefix": {
    "prefixFormat": "pic_",
    "fileFormat": "YYYYMMDDHHmmss"
  }
```

其中prefixFormat就是檔案前綴，fileFormat就是你的檔名要改成什麼，以我上面的設定來說，我的圖片就會命名為pic_20210604193603.png這種格式



要注意的是prefixFormat裡也是會先抓時間格式，我原本想用`img_`作為前綴，結果plugin把m當成分鐘，產出下面這樣的檔名

![截圖 2021-06-04 下午7.41.21](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604204111.png)



總之最後的config長這樣

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
  "picgoPlugins": {
    "picgo-plugin-super-prefix": true
  },
  "picgo-plugin-super-prefix": {
    "prefixFormat": "pic_",
    "fileFormat": "YYYYMMDDHHmmss"
  }
}
```



最後儲存，收工！

就可以開始用了



---

### 後記

今天在用plugin時發現沒辦法完全照著自己希望的格式去命名，例如那個img變成i3g，因此之後若有機會，希望能自己嘗試著修改plugin，不過那大概是很久以後的事了~

