# Vim使用教學

Vim, Linux的預設編輯器，從cmd編輯文件常常會自動打開vim，不熟指令的人很容易會卡在裡面最後直接關閉cmd強制退出（沒錯就是在說我自己）。



## 三種模式

Vim有三種模式，command(一般)、 insert(編輯) 和 command-line(指令)：



* command mode

  一進入時的模式，簡稱為「一般模式」，據說可以做複製、貼上、刪除，但因為指令稍微不一樣，我建議所有修改一律進到下一個insert mode做操作，把command mode當作是預覽就好。



* insert mode

  我叫他「編輯模式」，要修改文件的話需要切換到這個模式，輸入 i 即可切換，也有其他按鍵也可以切換，不過我覺得i = insert這個最好記。

  切換後畫面的左下方會出現"INSERT" 或"REPLACE"的字，這個時候就可以編輯了。如果要退出回到一般模式的話則輸入 Esc。
  
  ![image-20210604010102421](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210604130952.png)



* command-line mode

  簡單來說，「指令模式」，各種儲存、離開、取代都在這，從一般模式按「 : ? /」三選一都可以進入，長相是你的游標會移動到左下角。
  
  ![image-20210604010043638](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210605004955.png)



## 切換模式

那三奘模式的切換之間要注意 Insert mode 和 Command-line mode兩個不互通，需要先回到Command mode再進行切換。

![image-20210604015939067](https://raw.githubusercontent.com/bighsuan/note/main/img/pic_20210605005018.png)



## 常用指令

- :w   儲存檔案
- :q    退出
- :wq 儲存並退出
- :q!   強制離開，修改後不儲存直接離開的意思

