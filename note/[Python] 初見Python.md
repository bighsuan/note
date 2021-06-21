## Python版本選擇

基本上分Python 2和Python 3，語法上會有小差異，但因為有些套件不再支援Python2，所以建議學Python3。

### 安裝

建議安裝Anaconda3 (Python3版本的Anaconda)
下載網址：https://www.anaconda.com/products/individual

Anaconda優點：

* 直接幫你裝好各種常用套件 (e.g. Numpy, Pandas, Matplotlib)
* 自帶Jupyter Notebook，一個高互動、方便視覺化的開發環境
* 環境管理，若有多專案且不同Python版本需求的話很方便



簡單來說Anaconda就是一個懶人包，安裝以後就可以直接開始寫Python，但缺點是容量很大，因為把套件都自動裝上去了。若真的電腦空間不足也可選擇安裝miniconda，再自行安裝需要的套件。



## 執行Jupyter Notebook

在工作列直接搜尋Jupyter Notebook 並執行，會跳出一個黑色視窗，並且隨後彈出瀏覽器畫面。

黑色這個是你的kernal，kernal是什麼這裡暫時不提，你只要記得這是你notebook的本體，你的程式碼其實都是靠他在執行，只是視覺化呈現在瀏覽器上，而你也透過瀏覽器告訴kernal要執行什麼程式。

所以注意使用Jupyter Notebook 期間，kernal不可以關掉，一關掉你的瀏覽器畫面也會斷線。

![](https://blog.codefarm.com.tw/wp-content/uploads/2021/04/1.png)

![](https://blog.codefarm.com.tw/wp-content/uploads/2021/04/2.png)

## Hello world

點右上角 New &gt; Python 3 開啟一個新的notebook檔案，這個檔案會自動存到jupyter預設的根目錄 `/Users/使用者名稱/文件`

![](https://blog.codefarm.com.tw/wp-content/uploads/2021/04/3.png)


每個輸入框就是一個程式碼區塊(Cell)，Jupyter Notebook我覺得最方便的地方就是他可以選擇單一區塊執行，在開發時可以一小段一小段的測試，而不需要到處註解。


<br>
在第一個cell中輸入

``````

``````



```
print('Hello World')
```



然後按上面功能列的`Run`，或是直接`Ctrl`+`Enter`執行，就可以看到結果輸出在cell的下面。

![](https://blog.codefarm.com.tw/wp-content/uploads/2021/04/4.png)



## 快捷鍵 

`Ctrl` + `Enter` 執行當前選擇Cell

`Ctrl`+ `Shift` 執行當前選擇Cell，並選擇下一個Cell

`Esc` + `A`  在上方插入cell

`Esc` + `A`  在下方插入cell

`Esc` + `D` 按兩次  刪除選擇cell

`Esc` + `M`  切換cell到markdown模式，可以用來寫備註或分類cell



![](https://blog.codefarm.com.tw/wp-content/uploads/2021/04/5.png)

剪刀: 剪下cell，被我當成刪除cell使用

Run: 等於`Ctrl`+ `Shift` 

cell的模式，可在Code和Markdown之間切換





## 後記

下一篇會介紹我推薦的Extension，例如行號、執行時間計時，還有預設瀏覽器、根目錄怎麼設定。