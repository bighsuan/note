# Solr查詢的q和fq



### q (Query)

* 依據條件查詢對應資料

* 會對結果做相關性評分，例如關鍵字查詢時計算相關性來決定結果的排序

  

### fq (Filter Query)

* 單純依條件篩選資料，不管相關性或其他
* 會暫存篩選結果的dataset



這裡要注意每個fq的篩選結果會被暫存，方便下次查詢時可以重複使用，提升搜尋速度，但過多瑣碎的fq暫存會塞爆你的記憶體。

例如我要利用會員名稱查詢會員資料，我使用了`fq = name: Lisa`或`fq = name: Jamie`這個條件下去查，這就會造成`name:Lisa`是一筆暫存、`name:Jamie`也是一筆暫存，每個會員的查詢都是一筆暫存，然後你的記憶體就爆掉了，因此這裡應該使用`q = name:Lisa`會比較洽當。





### q和fq的查詢順序

1. 在fq cache中尋找是否有對應DocSet，如果有則回傳DocSet
2. 如果cache中沒有的話，則從Solr查詢並將結果存入cahce中
3. 將多個fq的DocSet取集合
4. 依據q的條件查詢
5. 將q和fq的結果取集合

以上的流程圖如下，其中2和4的執行順序會比較相近，也就是fq和q的查詢會是同時進行的。

另外注意q和fq都是對全部資料做查詢的，並沒有先後順序，q的查詢範圍並不是fq的DocSet範圍。



![Untitled Diagram](https://raw.githubusercontent.com/bighsuan/note/main/img/Untitled%20Diagram.png)





### 使用一個fq或多個fq

查詢時，q參數只能有一個，條件之間就是用AND、OR連結，但fq可以多個。

當你有一個多條件的查詢要寫在fq裡，你可以選擇寫成一個長的fq或是拆成多個小的fq。



這裡的範例是我要用類別和年份查詢對應書籍：

1. `fq = category:art AND year:2021`
2. `fq = category:art & fq = year:2021`



上面兩個寫法的查詢結果會相同，但產生的cache不同，第一個寫法是用一個Filter Query同時篩選category和year，第二個寫法則是拆成兩個Filter Query。簡單來說一個Filter Query會產生一筆cache，因此第一種寫法只會有一筆cache叫做category:art AND year:2021，而第二種則會有兩個cache，其結果是兩個cache的集合。



```
fq = category:art AND year:2021
```



![Untitled Diagram-Page-3](https://raw.githubusercontent.com/bighsuan/note/main/img/Untitled%20Diagram-Page-3.png)



```
fq = category:art & fq = year:2021
```



![Untitled Diagram-Page-2](https://raw.githubusercontent.com/bighsuan/note/main/img/Untitled%20Diagram-Page-2.png)





講到這裡，那到底我們寫Filter Query時應該要選擇哪一種呢？這就要看你的查詢條件的性質。

如果是很常一起出現的查詢條件，那可以寫在一起變成一個cache，如果是沒什麼相關的兩個條件，那就寫成兩個Filter Query。





---

經過這篇文章的介紹，希望大家以後可以更清楚的使用q和fq，不要濫用fq造成記憶體爆炸，若是想優化查詢速度的話也比較有方向。

另外這裡沒有提到的是Filter Query其實有參數讓他不要Cache，或是只有在特定條件下不Cache，不過這就是比較高級的用法了，有興趣的人可以自行去查詢。

```
fq={!cache=false}id:Lisa
```





