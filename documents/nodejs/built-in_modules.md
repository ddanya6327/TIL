# Built-in Modules

## OS

- const os = require('os');
  - os.platform()
  - os.uptime()
  - process와 달리 os의 정보를 가져옴.
- https://nodejs.org/api/os.html

## path

- const path = require('path');
  - path.sep (경로 구분 window(\\), linum(/))
  - path.delimiter (window(;), Linux(:))
  - path.dirname(\_\_filename);
  - path.extname(\_\_filename);
  - path.parse(\_\_filename);
  - path.format();
  - path.normalize();
  - path.isAbsolute();
  - path.join();
  - path.resolve();
- https://nodejs.org/api/path.html

## url

- const url = require('url');
- https://nodejs.org/api/url.html

## Query string

- https://nodejs.org/api/querystring.html

## Crypto

- 데이터 암호화 (단방향)
  - https://nodejs.org/api/crypto.html

```javascript
const crypto = require("crypto");

crypto.createHash("sha512").update("password").digest("base64");
```

- 복호화가 불가능하다.
