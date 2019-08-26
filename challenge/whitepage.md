## 綜合能力測驗

[綜合能力測驗](http://mentor-program.co/huli/game/index.php)

---


一進入後為全白畫面，開啟 Chrome Tools 或檢視原始碼可看見被註解的文檔
```php=
    // 這頁的 PHP 部分原始碼

    require_once('./conn.php');

    $mode         = isset($_GET["mode"]) ? htmlspecialchars($_GET["mode"]) : "";
    $restriction  = isset($_GET["norestriction"]) ? "" : "LIMIT 1";

    if ( $mode == "start" )
    {
      echo "<h1>歡迎來到綜合能力測驗</h1>";
      $instructions = array();
      $sql  = "SELECT * FROM huli_instructions ORDER BY number $restriction";
      $res  = $conn->query($sql);
      while ( $data = mysqli_fetch_array($res) )
      {
        $instructions[] = ($data['number'] > 2 ) ? "<p class='hidden'>提示 #". $data['number'] . " : " . $data['text'] . "</p>": "<p>提示 #". $data['instruction_number'] . " : " . $data['number'] . "</p>";
      }
      if ( !empty($instructions) )
      {
        echo("說明： " . join("", $instructions));
      }
    }

    //此遊戲改寫自：https://pretapousser.fr/devtest/
```
1. 從 4、5 行兩個 Get method與 8 行可看見判斷 mode 為 start 則進入 function，更改網址為
`http://mentor-program.co/huli/game/index.php?mode=start`
得到含提示`#1`的文字頁面  
2. 從 12 行 sql 中可以發現在尾端有 `$restriction`變數，接著注意 6行，當 Get method 有 norestrction 時，會使`$restriction`為`LIMIT 1`，更改網址為
`http://mentor-program.co/huli/game/index.php?mode=start&norestriction`
得到含提示`#2`的文字頁面
3. 16 行可以得知當提示大於 2 時會被隱藏，原始碼如下 

```htmlmixed=
<h1>歡迎來到綜合能力測驗</h1>說明： 
<p>提示 #1 : 你必須想辦法列出所有步驟才能繼續</p>
<p>提示 #2 : 看一下 CSS 不會少塊肉</p>
<p class='hidden'>提示 #3 : 你有看到我的按鈕嗎？</p>
<p class='hidden'>提示 #4 : 看一下 JavaScript 搞不好會有什麼發現</p>
<p class='hidden'>提示 #5 : 別忘了遺漏的變數</p>
<input type="submit" id="goForIt" value="Go">
```
照著提示可以找到 input css`display=none`，更改為 block 後即可顯示按鈕，此時再看下 js

```javascript=
$(document).ready(function() {

  $('#goForIt').on('click', function() {
    console.log("你成功按下按鈕了！");
    if ( typeof myMissingNumberToSetToMakeTheRequest !== 'undefined') {
      $.ajax({
        url : './ajax.php',
        type : 'GET',
        data : 'number='+myMissingNumberToSetToMakeTheRequest,
        dataType : 'json',
        success : function(data){
          console.log("成功發送 ajax request");
          console.log(data);
          
          if ( data['error'] === false ) {
            var result = data['s'];
            console.log("Result: " + result);
          }
          else {
            console.log("error: " + data['error']);
          }
        }
      });
    }
    else {
      console.log("少了些什麼...");
    }
  });

});
```
* Chrome tools 中修改 css、js 後記得存檔

判斷式需有參數`myMissingNumberToSetToMakeTheRequest`，沒有我們就給他，隨便打個`let myMissingNumberToSetToMakeTheRequest = 'test'`，再按一下按鈕 Go，在 Console 中可以看到以下訊息

```
你成功按下按鈕了！                           script.js:4 
成功發送 ajax request                      script.js:13 
error: 必須是 1 到 100 之間的數字            script.js:21 
```
他既然都說了，那就更改吧，試著改為 1 後，訊息變成如下
```
{hint: "54ceb91256e8190e474aa752a6e0650a2df5ba37", error: "數字錯誤"}
error: "數字錯誤"
```
...挖勒，是要猜數字的意思嗎？試著改成 2，發現 hint 值皆為`54ceb91256e8190e474aa752a6e0650a2df5ba37`，直接丟去 google，找到為 SHA-1 加密，解密後值為 56，改為 56 再按下 Go 按鈕，此時訊息變為如下
```
Result: 恭喜破關！flag: m3nT0rPr0GRAm666
```
恩...我怎覺的那個 flag 後續還有東西啊(?)