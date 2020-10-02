# Custom Hook

- Hook을 직접 정의해서 만들 수 있다.

  - 단순히 함수만 호출하면 되기 때문에 재사용성이 좋음.

- Custom Hook의 이름은 use로 시작하는 편이 좋다.
  - 코드 가독성이 좋아지고 리액트 개발 도구의 도움을 받을 수 있음.

```javascript
import React, { useState, useEffect } from "react";

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return "Loading...";
  }
  return isOnline ? "Online" : "Offline";
}
```

위의 코드를 Custom Hook 으로 만든다면 아래와 같이 만들 수 있다.

```javascript
import { useState, useEffect } from "react";

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

```javascript
function FriendStatus(props) {
  // Custom Hook을 호출
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return "Loading...";
  }
  return isOnline ? "Online" : "Offline";
}
```
