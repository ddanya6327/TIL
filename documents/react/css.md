# CSS

1. 일반적인 CSS

- component에서 직접 css를 import

box.css

```css
.big {
    width: 200px;
}

.small {
    width: 100px;
}

...
```

box.js

```javascript
import React from "react";
import "./Box.css"; // css를 import

export default function Box({ size }) {
  if (size === "big") {
    return <div className="box big">Big</div>;
  } else {
    return <div className="box small">Small</div>;
  }
}
```

=> 일반적인 css로 작성시 class 이름이 충돌 할 수 있음.
(ex: box.css에도 .big의 값을 주고 있고 box2.css에서도 .big에 값을 준다면 .big값이 중복되서 box2의 값이 적용됨)

2. css-module

- 이름.module.css 형태의 파일
- 클래스 이름 뒤에 고유의 해쉬 값을 붙이기 때문에 충돌 위험성이 없음

box.js

```javascript
import React from "react";
import Style from "./Box.module.css"; // css-module을 import

export default function Box({ size }) {
  if (size === "big") {
    return <div className={`${Style.box} ${Style.big}`}>Big</div>;
  } else {
    return <div className={`${Style.box} ${Style.small}`}>Small</div>;
  }
}
```

- classnames 라이브러리를 사용하면 더 쉽게 작성 가능

```javascript
...
import cn from 'classnames';

export default function Box({ size }) {
  const isBig = size === 'big';
  return (
      <button
        className={cn(Style.Button, {
            // key : css명, value: boolean
            // 값이 true인 css 명을 사용.
            [Style.big]: isBig,
            [Style.small]: !isBig,
        })}
      >
  )
}
```

3. Sass

- 별도의 문법(변수나 믹스인)을 이용해서 재사용성 높은 CSS를 작성 할 수 있음.

- node-sass 패키지 설치 필요.

- css파일 이름은 \*.scss 로 작성

Box.module.scss

```scss
@import "./shared.scss"; // 다른 css 를 import 할 수 있음

.big {
  width: 200px;
}
.box {
  // 변수를 사용 할 수 있다.
  background-color: $infoColor; // shared.scss 에 정의 된 변수
}
```

4. css-in-js

- JS 파일 안에서 css를 작성 할 수 있음.

styled-components 라이브러리를 사용해서 작성한 코드

```javascript
import React from "react";
import styled from "styled-components";

const BoxCommon = styled.div`
  height: 50px;
  background-color: #aaaaaa;
`;

const BoxBig = styled(BoxCommon)`
  width: 200px;
`;

const BoxSmall = styled(BoxCommon)`
  width: 100px;
`;

export default function Box({ size }) {
  if (size === "big") {
    return <BoxBig>Big</BoxBig>;
  } else {
    return <BixSmall>Small</BoxSmall>;
  }
}
```

- 동적인 처리를 하는 경우

```javascript
import React from "react";
import styled from "styled-components";

const BoxCommon = styled.button`
  width: ${(props) => (props.isBig ? 100 : 50)}px;
  height: 30px;
  background-color: yellow;
`;

export default function Box({ size }) {
  const isBig = size === "big";
  const label = isBig ? "Big" : "Small";
  return <BoxCommon isBig={isBig}>{label}</BoxCommon>;
}
```
