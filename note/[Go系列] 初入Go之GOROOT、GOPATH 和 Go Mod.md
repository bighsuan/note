# [Go系列] 初入 Go 之 GOROOT、GOPATH 和 Go Mod

剛接觸 Ｇo 時會對 GOROOT 和 GOPATH 這兩個東西感到非常困惑，因為當你載好 Go，打開 IDE 想來寫個 Hello World 時，IDE 做的第一件事就是問你的GOROOT 和 GOPATH 路徑是什麼，所以這兩個東西到底是什麼呢？還有這個標題上的 Go mod 又是什麼？跟 GOPATH 有關係嗎？如果你有這些問題的話就繼續看下去吧。



## GOROOT

GOROOT 其實就是你的 Go 的安裝路徑，這是個環境變數，通常在你安裝 Go 時就會自動幫你設定好，Go 內建的 package 就會放在這個地方。



## GOPATH

是另一個環境變數，有點類似你的 go 專案目錄，預設是在你的使用者目錄下的`/go`，而所有你的程式碼、使用到的第三方 package 都要放在這。



以上兩個變數可以用下面這個指令來查看：

```bash
go env
```



會印出所有 Go 的環境變數：

```bash
GOPATH="/Users/lisa/go"
GOROOT="/usr/local/Cellar/go/1.16.4/libexec"
......(以下省略)
```



也可以直接指定要看的變數，這樣就不會有其他不相關的資訊了

```
go env GOPATH
或
go env GOROOT
```





## import package

到這裡我們知道 GOROOT 裡會有官方 package，GOPATH 裡會有第三方 package，那假設我要引用一個 package，go 在執行時會做什麼呢？

首先打開一個資料夾，新增一個 main.go 檔案，並在內貼上：

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	router := gin.Default()
	router.Run()
}
```



上面這段程式碼表示我要引用一個 package 叫做`github.com/gin-gonic/gin`，這是一個第三方的 package，接著執行它：

```bash
go run main.go
```



會看到以下錯誤：

```bash
main.go:3:8: cannot find package "github.com/gin-gonic/gin" in any of:
        /usr/local/Cellar/go/1.16.4/libexec/src/github.com/gin-gonic/gin (from $GOROOT)
        /Users/lisa/go/src/github.com/gin-gonic/gin (from $GOPATH)
```



從這三行可以很清楚看出，go 在找這個 package時，會先到 GOROOT 底下的 src 資料夾去找，找不到後接著會到 GOPATH 底下的 src 資料夾找，兩個地方都沒有的話就會跳錯誤顯示找不到 package。





## GOPATH的缺點

從上面這個範例我們能看出go對你的程式擺放路徑有很強烈的規定，例如你的 package 就是必須放在 GOPATH 裡的 src 資料夾，其他地方都沒用（go 還有一些其他固定資料夾結構例如/pkg和/bin，這個有空的話會再補上），那這時很多問題就來了：

1. 我自己寫的 package 也必須和第三方 package 一樣，統一放在 GOPATH/src，這樣當你要推程式到 github 上時，會把你自己的程式和別人的 package 一起混著推上去。
2. 如果我有兩個專案怎麼辦，直接大雜燴，全部的專案全部的package都放在同樣的 GOPATH？但如果不同專案要用到不同版本的 package 要怎麼分辨？



先說說上面第二個問題，多專案時有個比較常見的做法是每次換專案時同時更換 GOPATH，這樣就可以各自裝 package 到自己的 GOPATH下，但這樣還是很麻煩，因為每次換專案就要重新設環境變數，而且這樣常用的 package 會被裝好幾份在不同路徑裡，感覺就還是個不夠好的做法。

總之 GOPATH 是不是讓你覺得很奇怪呢？感覺他就是想讓你把程式全部放在同一個地方，沒有多專案也沒有多版本 package 的問題？其實不能這樣說，依我查到的資料，也許是因為 Google 本身是 Ｍono Repo 的模式，他們將所有的共用程式、各個產品、各個專案全部都放在一起管理，程式之間共用度很高，所以才會設計出 GOPATH 這種管理 package 的方式。不過這顯然並不適合大部分的公司，因為還是有不少公司是走 Poly Repo 的模式，不同專案間並不會去共用 package，因此 GOPATH 就變得很難以使用。



## Go Modules 介紹

為了解決 GOPATH 的問題，在 Go 1.11 時 (2018/8月推出)，官方新增了 Go Modules 的功能，你的第三方 package 依舊放在 GOPATH 裡，不過你的專案程式碼可以放到別的地方，並且新增 go.mod 檔案來紀錄專案使用到的 package。



Go Modules 的使用方式首先需要設定 GO111MODULE 這個環境變數，有三種值：

* on：使用 Go Modules

* off：使用 GOPATH

* auto：若符合下面兩種條件其一時使用Go Modules，都不合則依舊使用 GOPATH
  1. 專案目錄不在 GOPATH/src/ 下
  2. 資料夾內有 go.mod 檔案



來看個範例，首先我們建立一個新的資料夾，注意這個資料夾要在 GOPATH 以外的地方，並設定 GO111MODULE 變數：

```shell
export GO111MODULE=on
```



建立新專案後，在資料夾內執行：

```bash
go mod init <module name>
```



資料夾內會出現一個 go.mod 檔案，內容如下：

```
module <module name>

go 1.16
```



接著一樣開個 main.go 並貼上剛剛上面用過的 import gin 範例：

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	router := gin.Default()
	router.Run()
}
```



再次執行

```
go run main.go
```



go 會自動偵測到你的程式用到了 gin 這個 package，並且叫你下載：

```
main.go:3:8: no required module provides package github.com/gin-gonic/gin; to add it:
        go get github.com/gin-gonic/gin
```



照著執行 `go get`：

```
go get github.com/gin-gonic/gin
```



會發生三件事：

1. GOPATH/pkg/mod 裡多了 `github.com/gin-gonic/gin` 的資料夾
2. go.mod 裡多了一行：

```
module gin-app

go 1.16

require github.com/gin-gonic/gin v1.7.2
```

3. 多一個 go.sum 檔案，這個會紀錄 package 的 hash 值並用來驗證，通常可以不用理會。



這時再次執行 `go run main.go`也會可以順利運作了。



簡單來說 GOPATH 和 Go Modules 的差別大概如下圖，就是把你的和第三方的package分開來了。

![Untitled Diagram-2](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210621180703.png)





## Go Modules 常用指令



在當前資料夾初始化 module，同時建立 go.mod

```
go mod init <module name>
```



加入用到的package ＆ 清除沒用到的 package，使用情境是可在開發時邊更新你的 go.mod

```
go mod tidy
```



下載 go.mod 中的 package 到GOPATH/pkg/mod，當你 clone 了一個帶 go.mod 的專案下來時可使用這個下載需要的 package

```bash
go mod download
```



直接下載 package 並更新 go.mod

```bash
go get <package name>
```





## 後記

package 該翻成什麼實在苦手，說套件嘛又讓我很不直覺，軟體包嘛又好像沒人這麼用，乾脆就直接用英文囉，希望大家不會覺得中英穿插很煩呀！





## 參考資料

1. [Golang-GOROOT、GOPATH、Go-Modules-三者的關係介紹](https://blog.kennycoder.io/2019/12/23/Golang-GOROOT、GOPATH、Go-Modules-三者的關係介紹/)

2. [Go Modules 三兩事](https://medium.com/@hieven/go-modules-三兩事-cd5964f7c50)