## xss-game
[xss-game](https://xss-game.appspot.com)
[[偷米騎巴哥]帶你認識XSS攻擊手法](https://www.youtube.com/watch?v=MMMkvHwqPRY)

後半就半看提示半看影片了-.-

1. Level 1: Hello, world of XSS

  利用 input 製造 alert，輸入`<script>alert('balalbala')</script>`，alert 內容隨意

2. Level 2: Persistence is key

  由預設的留言可以看出 attribute tag 並沒有被跳脫成字元，使用`<script>`無效果，使用`<img src="" onerror="alert('aaa')">`

3. Level 3: That sinking feeling...

```javascript
        function chooseTab(num) {
        // Dynamically load the appropriate image.
        var html = "Image " + parseInt(num) + "<br>";
        html += "<img src='/static/level3/cloud" + num + ".jpg' />";
        $('#tabContent').html(html);
 
        window.location.hash = num;
 
        // Select the current tab
        var tabs = document.querySelectorAll('.tab');
        for (var i = 0; i < tabs.length; i++) {
          if (tabs[i].id == "tab" + parseInt(num)) {
            tabs[i].className = "tab active";
            } else {
            tabs[i].className = "tab";
          }
        }
 
        // Tell parent we've changed the tab
        top.postMessage(self.location.toString(), "*");
      }
 
      window.onload = function() { 
        chooseTab(unescape(self.location.hash.substr(1)) || "1");
      }
 
      // Extra code so that we can communicate with the parent page
      window.addEventListener("message", function(event){
        if (event.source == parent) {
          chooseTab(unescape(self.location.hash.substr(1)));
        }
      }, false);
```
`self`指當前窗口本身  
`self.location`為當前窗口的 URL  
`self.location.hash`為取出 URL 自 `#` 開始右側，例如 `example.com/#abcd` 會取出 `#abcd`  
`unescape`(已較少使用) => `unescape(str)` 對 str 進行十六進位解碼

已知會對`#`後字串帶入`chooseTab`中  
`html += "<img src='/static/level3/cloud" + num + ".jpg' />";`  
可以考慮兩個方向，一個前後各包一個 img tag 然後中間再接`<script>`，一個是用`onerror` or `onload`。
`num = '/><script>akert()</script><img src='`
`num = asdf' onerror=alert() '`

4. Level 4: Context matters

倒數計時器，從輸入欄下手，index.html #13 輸入一個 timer  
在 level.py 中找到
```python
if not self.request.get('timer'):
      # Show main timer page
      self.render_template('index.html')
    else:
      # Show the results page
      timer= self.request.get('timer', 0)
      self.render_template('timer.html', { 'timer' : timer })
     
    return
```
當有`timer`時往 timer.html，找到`<img src="/static/loading.gif" onload="startTimer('{{ timer }}');" />`
當頁面讀取時會運行`startTimer('{{ timer }}');`  
`timer` 為 input 輸入值，考慮如何利用這個產生 alrt()  
與 level 3 類似，由於已在 onload 中，使用`');alert('` 產生 `onload="startTimer('{{ '); alert(' }}');"`  
解讀為兩段`startTime('{{');`與`alert('}}');`

Level 5: Breaking protocol

一開始為 welcome.html 點選 Sign up 後進入 signup.html，email 沒有找到傳遞，＃15 `<a href="{{ next }}">Next >></a>`，level.py 中在 #11 - #13 看到在 signup 頁面 request.get('next')，試著修改 URL `next=confirm` 觀察 a href， 輸入`javascript:alert()`，也說將 URL 更改為`https://xss-game.appspot.com/level5/frame/signup?next=javascript:alert()`

[超連結href=#和href=javascript:void(0)差別 & href和src不一樣的地方](https://malagege.github.io/blog/2019/02/27/%E8%B6%85%E9%80%A3%E7%B5%90href-%E5%92%8Chref-javascript-void-0-%E5%B7%AE%E5%88%A5-href%E5%92%8Csrc%E4%B8%8D%E4%B8%80%E6%A8%A3%E7%9A%84%E5%9C%B0%E6%96%B9/)


Level 6: Follow the 🐇

第六題有點一隻半解，還需要再看看

```javascript
    function includeGadget(url) {
      var scriptEl = document.createElement('script');
 
      // This will totally prevent us from loading evil URLs!
      if (url.match(/^https?:\/\//)) {
        setInnerText(document.getElementById("log"),
          "Sorry, cannot load a URL containing \"http\".");
        return;
      }
 
      // Load this awesome gadget
      scriptEl.src = url;
 
      // Show log messages
      scriptEl.onload = function() { 
        setInnerText(document.getElementById("log"),  
          "Loaded gadget from " + url);
      }
      scriptEl.onerror = function() { 
        setInnerText(document.getElementById("log"),  
          "Couldn't load gadget from " + url);
      }
 
      document.head.appendChild(scriptEl);
    }
```
Hint 4：`google.com/jsapi?callback=foo`

[[偷米騎巴哥]帶你認識XSS攻擊手法, level 6](https://youtu.be/MMMkvHwqPRY?t=4872)