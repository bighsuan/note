#  Windows下安裝Phalcon及PhpStorm設定

前言: 這是和Phalcon奮鬥了3個月，經歷了各種環境的安裝，以及各種安裝方法後，終於成功在Windows中把Phalcon裝起來，不過之後換電腦要裝在mac上又是另一場戰爭了...





## 第一步: 安裝PHP

第一步是安裝PHP，若還沒安裝請參考另一篇文章「windows環境安裝Nginx+MySQL+PHP」的PHP部分，這裡就不細講了。



安裝好後請確認自己的三個資訊，這些資訊很重要，會影響後面其他extension會不會裝錯導致一直鬼打牆。 

1. PHP版本和位元版本
2. Thread Safe或Non Thread Safe
3. 安裝路徑



#### 1. PHP版本和位元版本

```
>php -v
```

![image-20210324142537401](https://i.loli.net/2021/03/24/XAOgT9xN76iuwDs.png)



#### 2. Thread Safe或Non Thread Safe

```
>php -i|findstr "Thread" 
//disabled就是Non Thread Safe
```

![image-20210324141443883](https://i.loli.net/2021/03/24/GZcT4eq3gkIBiob.png)



#### 3. PHP安裝路徑

```
>where php
```

![image-20210324141753906](https://i.loli.net/2021/03/24/TpgKzFOewfuUsCX.png)





## 第二步: 安裝PSR和Phalcon的Extension

下載psr和phalcon，分別解壓縮，並找到其中的php_psr.dll和php_phalcon.dll，兩個dll檔案都放進剛剛第一步查的php路徑裡面的ext資料夾。



### 1.下載psr

下載網址https://pecl.php.net/package/psr/1.0.0/windows

以剛剛第一步的資訊為例，我的PHP版本是PHP 7.4、64位元、Non Thread Safe，所以我下載7.4 Non Thread Safe (NTS) x64。

![image-20210324143111821](https://i.loli.net/2021/03/24/MVqBeHk4cYwhINx.png)



### 2.下載Phalcon

下載網址https://github.com/phalcon/cphalcon/tags

挑一個喜歡的Phalcon版本，點進去確認有支援自己使用的PHP版本，就可拉到最下面下載，注意下載有x64、php7.4、nts這幾個字的版本。

![image-20210324143319874](https://i.loli.net/2021/03/24/KMXUTeO64muy2js.png)

![image-20210324143431636](https://i.loli.net/2021/03/24/u1oWyNdMniT5g2h.png)

![image-20210324143526602](https://i.loli.net/2021/03/24/3FcMIQKNfCXvhmG.png)





### 3. 解壓縮放進ext資料夾

把剛剛下載的兩個zip解壓縮，裡面會有dll檔案和一些使用權限文件，文件可以不用管，把php_psr.dll和php_phalcon.dll兩個檔案解出來就好，然後放進第一步查詢的php路徑的ext資料夾。

![image-20210324144119832](https://i.loli.net/2021/03/24/WrQ1p8ksfwvehM5.png)





### 4. 修改php.ini

找到extension_dir = "..."，改成你剛剛放dll檔的ext資料夾路徑。

```
extension_dir="C:\wnmp\php\ext"
```



找個位置放下面兩行，位置沒有一定，我選擇放在其他extension的附近。

```
extension=php_phalcon.dll
extension=php_psr.dll
```

![image-20210324144501336](https://i.loli.net/2021/03/24/ShUN8geX6xVZFq9.png)



### 5. 檢查是否成功

可用`php -m`確認是否有成功

![image-20210324144736888](https://i.loli.net/2021/03/24/9oucPJ3CnA7GhVd.png)



若一直出現以下錯誤還附帶亂碼的話，通常是版本裝錯了，請再檢查一遍自己的PHP版本和載下來的psr、phalcon版本是否都相同。

```
PHP Warning: PHP Startup: Unable to load dynamic library 'php_phalcon.dll'
```





## 第三步: 安裝phalcon-devtools

這裡我們要用Composer安裝devtools，網站上是有說可以直接git clone下來，但我失敗了，所以還是乖乖請Composer幫我安裝。



### 1. 安裝Composer

下載網址: https://getcomposer.org/download/

![image-20210324145228745](https://i.loli.net/2021/03/24/pjyFvGXQOrWJkEn.png)





### 2. 下載devtools

```
composer require "phalcon/devtools:dev-master"
```

安裝完記得看一下裝去哪了，以我為例我的路徑在C:\Users\pchome\AppData\Roaming\Composer





### 3. 修改devtools中的phalcon.bat

到剛剛Composer安裝路徑，Composer安裝路徑\vendor\phalcon\devtools\，最後在devtools資料夾裡找到phalcon.bat，修改他。



把路徑改成你找到bat檔案的這個路徑，記得最後要加斜線。

![image-20210324145849559](https://i.loli.net/2021/03/24/U51t4KShBz7XOe3.png)





### 4. 增加環境變數

把路徑也加到環境變數裡，注意這次不用加斜線。

例如我是把 C:\Users\pchome\AppData\Roaming\Composer\vendor\phalcon\devtools 加進環境變數。



### 5. 測試

試著建一個project看看

``` 
phalcon create-project blog
```

![image-20210324150551748](https://i.loli.net/2021/03/24/MfT9Vwn5gNcs4BF.png)





## 附錄: PhpStorm中使用phalcon

直接使用的話PhpStorm會經常跟你說`namesapce not found`，這個時候請安裝 Phalcon 4 Autocomplete 這個Plugin。



在PhpStorm中，File>Settings>Plugins，搜尋 Phalcon 4 Autocomplete 並安裝，重新啟動PhpStorm就可以了。

![image-20210324151231376](https://i.loli.net/2021/03/24/q4QxETpk8S5hjLF.png)

