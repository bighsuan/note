# HTTP Cache 驗證方法

前一篇提到Cache-control: no-cache是指每次使用cache前都要和Serve確認，那到底要怎麼確認呢？



### Last-Modified 和 If-Modified-Since

![image-20210322181259542](https://i.loli.net/2021/03/22/sdtXfokWHKS2vOE.png)

實際使用中日期格式長這樣: 

```
Last-Modified: Mon Mar 22 2021 16:40:15 GMT
If-Modified-Since: Mon Mar 22 2021 16:40:15 GMT
```



#### API流程:

1. Client 第一次請求資料
2. Server 回傳資料，指定cache 的使用權限是no-cache，並在response 中增加一個<b>Last-Modified</b> Header
3. Client 之後想使用cache 資料，但因為no-cache 設定所以必須詢問server 是否可以用，詢問時在request 中帶<b>If-Modified-Since</b> Header和上次得到的時間
4. Server 依據時間判斷Client 儲存的cache 是否還可用
   * 若資料未被修改，回傳304 Not Modified和空的body
   * 若資料已改變，回傳200 OK 和新的資料



#### 缺點

1. 多台Server 的話須注意時間是否同步
2. 精度只到秒，秒內的多次變更無法被區別。
3. 只看修改時間，若有內容不變但會定期被產生的文件，使用If-Modified-Since會造成Client不能使用Cache





### Etag 和 If-None-Match

![image-20210322181337221](https://i.loli.net/2021/03/22/hPaDMigp1W7QZCG.png)

Etag 可以表示某資源的特定版本，由Server產生，但沒有固定產生方式，常用生成方法為內容或修改時間的hash值、或單純版本號。



#### API流程:

同上面Last-Modified 和If-Modified-Since，第一次請求資料時Server會回傳一個Etag，下次Client要使用cache時就拿這個Etag去詢問Server是否可用，並回傳304 Not Modified或200 OK。



#### 優點

1. 解決Last-Modified 精度只到秒的問題
2. 可實際判斷內容是否變更
3. 可自行設計適合之hash function



#### 缺點

1. 每次查詢都要重新計算hash，造成系統負擔
2. 多台Server可能算出不同的hash值







### 補充

上面說的If-Modified-Since 和 If-None-Match，這兩個header只能用在GET和HEAD method。



若需要用在PUT或POST，例如詢問資料是否有變動，沒變動才可以修改、有變動則不行，那就要使用If-Unmodified-Since 和 If-Match。



#### If-Unmodified-Since

![image-20210322183251351](https://i.loli.net/2021/03/22/oJxW6uQGrPDfhcI.png)

#### If-Match

![image-20210322183312639](https://i.loli.net/2021/03/22/aHVu92QAbKLB1wW.png)





