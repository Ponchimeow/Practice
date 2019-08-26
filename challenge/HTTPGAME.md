## Lidemy HTTP Challenge

[ Lidemy HTTP Challenge ](https://lidemy-http-challenge.herokuapp.com/start)

以下為個人在破關上流程的程式碼，使用 process.argv[2] 接執行的 function，當想不出頭緒的時候，想一下 hint 或是再看一次 API 並搭配 Chrome 開發者工具，一步步的聽故事解下去 😸 

```javascript
const process = require('process');

const request = require('request');

const BASE_URL = 'https://lidemy-http-challenge.herokuapp.com';

eval(`${process.argv[2]}()`);

// Start
function start() {
  request.get(
    `${BASE_URL}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv1 {GOGOGO}
function lv1() {
  request.get(
    `${BASE_URL}/lv1?token={GOGOGO}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv1 我是誰
function lv1Name() {
  request.get(
    `${BASE_URL}/lv1?token={GOGOGO}&name='Ponchimeow'`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv2 {HellOWOrld}
function lv2() {
  request.get(
    `${BASE_URL}/lv2?token={HellOWOrld}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv2 尋書
function lv2ID() {
  for (let id = 54; id <= 58; id += 1) {
    request.get(
      `${BASE_URL}/lv2?token={HellOWOrld}&id=${id}`,
      (error, response, body) => {
        if (!error && response.statusCode === 200) {
          console.log(`請問是${id}這本嗎?`);
          console.log(body);
        }
      },
    );
  }
}

// lv3 {5566NO1}
function lv3() {
  request.get(
    `${BASE_URL}/lv3?token={5566NO1}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 趕鴨子上..喔不是，是書籍上架
function lv3AddBook() {
  request.post(
    `${BASE_URL}/api/books`, {
      form: {
        name: '《大腦喜歡這樣學》',
        ISBN: '9789863594475',
      },
    },
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 新書味，敘 id
function lv3TellID() {
  request.get(
    `${BASE_URL}/lv3?token={5566NO1}&id=1989`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv4 {LEarnHOWtoLeArn}
function lv4() {
  request.get(
    `${BASE_URL}/lv4?token={LEarnHOWtoLeArn}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 淡忘，重尋
function lv4FindBook() {
  request.get(
    `${BASE_URL}/api/books`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        const bookList = JSON.parse(body);
        bookList.forEach((item) => {
          if (item.name.match(/.*世界.*/) && item.author === '村上春樹') {
            console.log(`${item.id} ${item.name}`);
          }
        });
      }
    },
  );
}

// 尋獲記憶
function lv4TellID() {
  request.get(
    `${BASE_URL}/lv4?token={LEarnHOWtoLeArn}&id=79`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv5  {HarukiMurakami}
function lv5() {
  request.get(
    `${BASE_URL}/lv5?token={HarukiMurakami}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 抹去誤會
function lv5Del() {
  request.delete(
    `${BASE_URL}/api/books/23`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv6 {CHICKENCUTLET}
function lv6() {
  request.get(
    `${BASE_URL}/lv6?token={CHICKENCUTLET}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 芝麻開門!
function lv6Login() {
  const account = process.argv[3];
  const pw = process.argv[4];
  const encode = Buffer.from(`${account}:${pw}`, 'binary').toString('base64');
  request.get(
    `${BASE_URL}/api/v2/me`, {
      headers: {
        Authorization: `Basic ${encode}`,
      },
    },
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 是這個嗎?
function lv6Isthisit() {
  request.get(
    `${BASE_URL}/lv6?token={CHICKENCUTLET}&email=lib@lidemy.com`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}


// lv7  {SECurityIsImPORTant}
function lv7() {
  request.get(
    `${BASE_URL}/lv7?token={SECurityIsImPORTant}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 編修書目
function lv7DelMiss() {
  const account = process.argv[3];
  const pw = process.argv[4];
  const encode = Buffer.from(`${account}:${pw}`, 'binary').toString('base64');
  request.delete(
    `${BASE_URL}/api/v2/books/89`, {
      headers: {
        Authorization: `Basic ${encode}`,
      },
    },
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv8  {HsifnAerok}
function lv8() {
  request.get(
    `${BASE_URL}/lv8?token={HsifnAerok}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 年邁小失手，讓我來處理!
// 尋誤
function lv8FindErr() {
  const account = process.argv[3];
  const pw = process.argv[4];
  const encode = Buffer.from(`${account}:${pw}`, 'binary').toString('base64');
  request.get(
    `${BASE_URL}/api/v2/books`, {
      headers: {
        Authorization: `Basic ${encode}`,
      },
    },
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        const bookList = JSON.parse(body);
        bookList.forEach((item) => {
          if (item.name.match(/.*我.*/) && item.author.length === 4 && item.ISBN.match(/7$/)) {
            console.log(`${item.id}  ${item.name}  ${item.author}  ${item.ISBN}`);
          }
        });
      }
    },
  );
}

// 一筆揮下，終成過往
function lv8Update() {
  const account = process.argv[3];
  const pw = process.argv[4];
  const encode = Buffer.from(`${account}:${pw}`, 'binary').toString('base64');
  request.patch(
    `${BASE_URL}/api/v2/books/72`, {
      headers: {
        Authorization: `Basic ${encode}`,
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      form: {
        ISBN: '9981835423',
      },
    },
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv9 {NeuN}
function lv9() {
  request.get(
    `${BASE_URL}/lv9?token={NeuN}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 偽裝!(蛇叔驚嘆號)
function lv9Spy() {
  const account = process.argv[3];
  const pw = process.argv[4];
  const encode = Buffer.from(`${account}:${pw}`, 'binary').toString('base64');
  request.get(
    `${BASE_URL}/api/v2/sys_info`, {
      headers: {
        Authorization: `Basic ${encode}`,
        'X-Library-Number': '20',
        'User-Agent': 'Mozilla/5.0; MSIE 6.0;',
      },
    },
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      } else console.error(error);
    },
  );
}

// これですか？
function lv9Isthisit() {
  request.get(
    `${BASE_URL}/lv9?token={NeuN}&version=1A4938Jl7`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv10 {duZDsG3tvoA} 收我收我 爸脫Q^Q
function lv10() {
  request.get(
    `${BASE_URL}/lv10?token={duZDsG3tvoA}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 終極對決!
function lv10GuessNum() {
  const num = process.argv[3];
  request.get(
    `${BASE_URL}/lv10?token={duZDsG3tvoA}&num=${num}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv11 歇息，繼續前進! {IhateCORS}
function lv11() {
  request.get(
    `${BASE_URL}/lv11?token={IhateCORS}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// Hello World~
function lv11Hello() {
  request.get(
    `${BASE_URL}/api/v3/hello`, {
      headers: {
        Origin: 'lidemy.com',
      },
    },
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv12 {r3d1r3c7}
function lv12() {
  request.get(
    `${BASE_URL}/lv12?token={r3d1r3c7}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 秘密取貨
// 使用 chrome 開發者工具找到
function lv12GetToken() {
  request.get(
    `${BASE_URL}/api/v3/deliver_token`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv13 {qspyz}   
function lv13() {
  request.get(
    `${BASE_URL}/lv13?token={qspyz}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 傳送失敗? 協尋嚮導找尋歷史看板
// 使用瀏覽器代理 proxy，code 尚未
// function lv13Log() {
//   request.get(
//     {
//       url: 'http://lidemy-http-challenge.herokuapp.com/api/v3/logs',
//       host: '222.127.15.61',
//       port: '3128',
//       connection: 'keep-alive',
//     }, (error, response, body) => {
//       if (!error && response.statusCode === 200) {
//         console.log(body);
//       }
//     },
//   );
// }

// lv14 {SEOisHard}
function lv14() {
  request.get(
    `${BASE_URL}/lv14?token={SEOisHard}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// 兵器譜之首為何為之首(?)
// 有點誤打誤撞解出來XD
function lv14Seo() {
  request.get(
    `${BASE_URL}/api/v3/index`, {
      headers: {
        'User-Agent': 'Googlebot',
      },
    },
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

// lv15 嶺峰之巔(?)並不是，這只是剛踏出的一小步 {ILOVELIdemy!!!}
function lv15() {
  request.get(
    `${BASE_URL}/lv15?token={ILOVELIdemy!!!}`,
    (error, response, body) => {
      if (!error && response.statusCode === 200) {
        console.log(body);
      }
    },
  );
}

```