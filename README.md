# useful-utils

some utils which might be useful in developing FrontEnd projects
在开发前端项目中也许有用的工具函数

## 1. getBase64FromUrl

Sometimes pictures need to be converted to base64

有时我们需要把图片转化为 base64 在页面中显示

```javascript
function getBase64FromUrl(imageUrl) {
  return new Promise((resolve, reject) => {
    if (imageUrl === "" || imageUrl === null) {
      reject(new Error("url is not valid"));
      // reject(new Error("无效的图片地址"))
    }
    const xhr = new XMLHttpRequest();
    xhr.onload = () => {
      const reader = new FileReader();
      reader.onloadend = () => {
        resolve(reader.result);
      };
      reader.readAsDataURL(xhr.response);
      reader.onerror = () => {
        reject(new Error("picture can't be converted to base64"));
        // reject(new Error("无法转换图片为base64格式"))
      };
    };
    xhr.open("GET", imageUrl);
    xhr.responseType = "blob";
    xhr.onerror = () => {
      reject(new Error("url is not valid"));
      // reject(new Error("无效的图片的地址"))
    };
    xhr.send();
  });
}
```

用法 / Usage:

```javascript
// replace placeholder with url of picture
// 把placeholder替换为图片的地址
var url = "placeholder";
getBase64FromUrl(url)
  .then((response) => {
    // do something with result(response)
    // 可以对response进行操作
  })
  .catch((error) => {
    console.log(error);
  });
```

## 2. addEventListener with throttle function

```javascript
import React from "react";
import _ from "lodash";

class App extends React.Component {
  constructor(props) {
    super(props);

    this.throttledResize = _.throttle(this.resize, 200);
    this.state = {};
  }

  componentDidMount() {
    window.addEventListener("resize", this.throttledResize);
  }

  componentWillUnmount() {
    window.removeEventListener("resize", this.throttledResize);
  }

  resize = () => {
    // do resize effect
  };
}
```

## 3. getCookie

from: https://javascript.info/cookie#getcookie-name

> returns the cookie with the given `name`:

```js
// returns the cookie with the given name,
// or undefined if not found
function getCookie(name) {
  let matches = document.cookie.match(
    new RegExp(
      "(?:^|; )" +
        name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, "\\$1") +
        "=([^;]*)"
    )
  );
  return matches ? decodeURIComponent(matches[1]) : undefined;
}
```

## 4. setCookie

from: https://javascript.info/cookie#setcookie-name-value-options

> Sets the cookie’s `name` to the given `value` with `path=/` by default (can be modified to add other defaults):

```js
function setCookie(name, value, options = {}) {
  options = {
    path: "/",
    // add other defaults here if necessary
    ...options,
  };

  if (options.expires instanceof Date) {
    options.expires = options.expires.toUTCString();
  }

  let updatedCookie =
    encodeURIComponent(name) + "=" + encodeURIComponent(value);

  for (let optionKey in options) {
    updatedCookie += "; " + optionKey;
    let optionValue = options[optionKey];
    if (optionValue !== true) {
      updatedCookie += "=" + optionValue;
    }
  }

  document.cookie = updatedCookie;
}

// Example of use:
setCookie("user", "John", { secure: true, "max-age": 3600 });
```

## 5. delete a cookie

from: https://javascript.info/cookie#deletecookie-name

> To delete a cookie, we can call it with a negative expiration date:

```js
function deleteCookie(name) {
  setCookie(name, "", {
    "max-age": -1,
  });
}
```
