# HTML Entity

簡單紀錄一下遇到的編碼問題。



## 問題

例如使用者姓名是`凃大宣`，但「凃」這個字可能是從別的網站複製過來的，而別的網站使用了 HTML entity 顯示難字，因此複製過來存到我們資料庫中的值是`&#20931;大宣`，而非中文字。



### string -> HTML entity

`mb_convert_encoding`，簡單好用但好像某些環境下不能使用

```
mb_convert_encoding($string, 'HTML-ENTITIES', 'utf-8');
```

用`htmlentities`，但要加上很多層 decode

```
htmlspecialchars_decode(utf8_decode(htmlentities($string, ENT_COMPAT, 'utf-8', false)))
```



### HTML entity -> string

```
html_entity_decode($string)
```





---



## 什麼是 HTML Entity 

在 HTML 中使用的編碼，用某個數字對應到某個文字或符號，類似 ASCII 的`65`對應到大寫`A`，而 HTML Entity 中 `&#20931;`對應`凃`這個字。

這個entity對應文字跟當前編碼方式無關，即使是在希伯來文的編碼下，`&#20931;`依舊對應`凃`。





## HTML Entity 的格式

是一段字串，格式是`&`開頭`;`結尾，中間是 entityName 或是 `#`加數字。

```
&entityName;   // 例如 &lt;
```

```
&#entityNumber;  // 例如 &#38988;
```





## 常見使用時機

1. 顯示 HTML 保留字符，如 <、>、&、 " 等（可以防禦 XSS 攻擊）*(XSS這個之後有空再補)*
2. 表示難以用常規輸入設備輸入的字符，如 ©、®、± 等
3. 表示非當前編碼支援的字符





## HTML Entity 和一般輸入中文字比較

例如下面這張圖，標題就是用 HTML Entity 寫的，因此不管瀏覽器用什麼編碼都可以正常顯示，甚至選希伯來文也可以；但表格的內容就是單純輸入中文，因此會被編碼影響。

![img](https://6.share.photo.xuite.net/tolarku/16987de/1585902/1094092016_l.jpg)

