## r3:0 異世界網站挑戰

[r3:0 異世界網站挑戰](https://github.com/Lidemy/mentor-program-3rd-Ponchimeow)

### lv0
將 token 傳遞給 server
```
https://r30challenge.herokuapp.com/lv1.php?token=r30:start
```

### lv1

將`100101001001100001110`轉為十八進位
轉為十進位為`1217294`
轉為十八進位則為`bad18`
```
https://r30challenge.herokuapp.com/lv2.php?token=bad18
```

### lv2
提示可以大概猜出是要找出隱藏訊息
使用 chrome tool，找到隱藏 div
```
https://r30challenge.herokuapp.com/lv3.php?token=divsurprise
```

### lv3
再次觀看原始碼，找到註解
```
https://r30challenge.herokuapp.com/lv4.php?token=commentfaker
```

### lv4
與其說解謎不如說..捉迷藏?
在 js 檔案中找到
```
https://r30challenge.herokuapp.com/lv5.php?token=csspersona!
```

### lv5
畫面有先閃過 token
找到 lv5.js 看到`window.location='./lv6.php?token=fail';`也沒答案

後來是在 stackoverflow 上看到這篇[chrome-dev-tools-fails-to-show-response-even-the-content-returned-has-header-con](https://stackoverflow.com/questions/38924798/chrome-dev-tools-fails-to-show-response-even-the-content-returned-has-header-con/46437756)
在 source 的 EventLinstenerBreakpoints -> Load -> 選取 load ，然後再 F8 一步一步往下，然後找到答案

```
https://r30challenge.herokuapp.com/lv6.php?token=windowhack
```

### lv6
於 lv6.js 中找到顏文字，注意空格問題，使用[aadecode - Decode encoded-as-aaencode JavaScript program. (ﾟДﾟ) ['_']](https://cat-in-136.github.io/2010/12/aadecode-decode-encoded-as-aaencode.html)

```
https://r30challenge.herokuapp.com/lv7.php?token=emojicute
```

### lv7
找 cookie，使用 dev tools 於 network 當前頁面的 header 找到

```
https://r30challenge.herokuapp.com/lv8.php?token=cookieyumyum
```

### lv8
於 Network Headers 的 Response Headers 中找到

```
https://r30challenge.herokuapp.com/lv9.php?token=headshot
```

### lv9
太糾結於單一解答了，token 只要符合條件即可
```
https://r30challenge.herokuapp.com/lv10.php?token=qwerasbi
```