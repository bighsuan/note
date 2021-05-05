# 網頁快取: Cache-Control



### 為什麼要有Cache

假設今天server要透過API回傳資料給前端頁面，但回傳的資料很大，為了避免一直重複呼叫API但實際上拿回來的資料根本沒變化，我們會把資料暫存在前端，當資料在server有變動時才讓前端重新呼叫後端。



使用Cache的原因是Cache可以節省流量，避免大量使用者塞爆後端，也可以減少等待資料回傳顯示頁面的時間，提升使用者體驗。



注意這篇要講的的是前端瀏覽器的快取，跟後端快取是不同的東西，不過兩邊快取的目的都是為了節省流量及加快反應時間，只是實作的方法不同。





### Expire, Pragma

在講`Cache-Control`之前想先提到另外兩個Header：`Expires`和`Pragma`。

這兩個是HTTP/1.0的用法，基本上就是Cache-Control的前身，`Expires`後面接Cache過期時間，`Pragma`後面固定為no-cache。

目前瀏覽器還是有支援這兩個Header，但若Header中有Cache-Control，這兩個就會被忽略。

```
Expires: Wed, 21 Oct 2015 07:28:00 GMT   // 若有Cache-Control: max-age=[second]則忽略Expires
Pragma: no-cache                         // 等同於Cache-Control: no-cache
```



### Cache-Control

`Cache-Control`是HTTP/1.1的新Header，把`Expires`和`Pragma`融合並且增加一些新的cache指令。



`Cache-Control`有分為客戶端用於request的、伺服器端用於response的、和兩邊通用的指令。不過實際應用中較少會從客戶端帶`Cache-Control` ，只有特定情況例如用`Cache-Crotrol: no-cache`要求一定要跟伺服器驗證，確保資料是從伺服器來的。

<table style="text-align:center">
    <thead>
        <tr>
            <th>Request</th>
            <th>Response</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td colspan=2>max-age=[seconds]</td>
        </tr>
        <tr>
            <td colspan=2>no-cache</td>
        </tr>
        <tr>
            <td colspan=2>no-store</td>
        </tr>
        <tr>
            <td colspan=2>no-transform</td>
        </tr>
        <tr>
            <td>max-stale=[seconds] (延長cache時效)</td>
            <td>must-revalidate</td>
        </tr>
        <tr>
            <td>min-fresh=[seconds] (縮短cache時效)</td>
            <td>public</td>
        </tr>
        <tr>
            <td>only-if-cached (只要cache資料)</td>
             <td>private</td>
        </tr>
    </tbody>
</table>





表格並未完整列出所有指令，我只列了我覺得比較重要的，完整版可到[官方文件](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)看。

下面重點介紹幾個伺服器端常用的Cache-Control指令，<u>**須特別注意no-cache和must-revalidate，這兩個的行為和字面意義有差異**</u>。



#### max-age

* 表示cache保存時間，單位為秒。

```
Cache-control: max-age=86400
```



* 須注意max-age的時間是從得到response後開始計算，因此即使同時呼叫多份檔案(例如foo.html, foo.css, foo.js)，過期時間也會有些微差距。
* max-age的最大值依使用者習慣、瀏覽器設定而不同，若是重度使用者Cache儲存空間就會較快滿，當空間滿了，即使還沒到過期時間Cache可能也會被清除。



#### no-cache

<font color=#FF0000>每個response都會被cache，只是cache資料被使用前都要和Server驗證。</font>



#### no-store

實際意義上的不做cache。



#### must-revalidate

<font color=#FF0000>Cache過期的話要重新驗證才可使用，沒過期就可以直接回傳cache。</font>

這個指令的意義比較像「絕不回傳過期cache」，用來阻擋某些可回傳過期cache的狀況，例如離線副本。



#### public

每個使用者來要資料都可給cache，一般可省略。(e.g. 存放在Proxy的資料)



#### private

只有同個使用者可以拿到cache。(e.g. 避免Proxy儲存個人資料、無痕視窗)





### 小學堂

請問下面這些需求要使用哪個指令呢？



Q: 我希望不要cache

A: 請使用**no-store**



Q: 我希望每次用cache前都要跟server驗證

A: 請用**no-cache**





