# [git筆記] 忘記切分支就開始開發怎麼辦!

當小Lisa clone了一個專案下來，開始快樂的改程式。

直到準備commit時，小Lisa才發現自己竟然忘記切branch，直接在develop branch上面開發了！

這時候要怎麼辦呢！？



不慌張不緊張，在commit之前我們的修改都是還沒被記錄到本地儲存庫的，就算commit了我們也可以暴力拆commit，就算真的真的你已經push到遠端了，我們還是可以趕快稱沒人發現時補救一下的。



### 情境1: 還沒add / 還沒commit

這種情況下直接直接切換branch就可以了，所有修改會一起移動到新的branch，然後你就可以commit了。



### 情境2: 已經commit，但還沒push到遠端

先使用reset拆掉commit，把你的最後一筆commit變回工作區內，就可以同情境1切branch再commit了。

```
git reset HEAD^
```





### 情境3: 已經push到遠端了

那如果已經push了怎麼辦呢？這是最下下策！reset再強制push！

先同情境2使用reset把本地儲存庫的資料恢復，然後再硬推到遠端，覆蓋剛剛推上去的commit，告訴遠端剛剛那個commit都是假的！

```
git reset HEAD^
git push -f
```



此時你剛剛推到遠端的commit已經消失，並且工作區中還有你開發的code，這個時候就可以回到情境1，趕快切branch推commit裝沒事了。

(盡量在還沒人發現時使用這招，不然看到commit突然消失會很讓人疑惑的。)



以上就是今天的補救小教學，但最重要的還是每次clone程式下來時，都要記得先切branch再開始開發，養成好習慣最重要！
