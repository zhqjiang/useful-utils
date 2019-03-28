# useful-utils
some utils which might be useful in developing FrontEnd projects
在开发前端项目中也许有用的工具函数

## 1. getBase64FromUrl

有时我们需要把图片转化为base64在页面中显示
Sometimes pictures need to be converted to base64

```javascript
function getBase64FromUrl (imageUrl) {
    return new Promise((resolve, reject) => {
        if (imageUrl === '' || imageUrl === null) {
            reject(new Error("url is not valid"))
            // reject(new Error("无效的图片地址"))
        }
        const xhr = new XMLHttpRequest()
        xhr.onload = () => {
            const reader = new FileReader()
            reader.onloadend = () => {
                resolve(reader.result)
            }
            reader.readAsDataURL(xhr.response)
            reader.onerror = () => {
                reject(new Error("picture can't be converted to base64"))
                // reject(new Error("无法转换图片为base64格式"))
            }
        }
        xhr.open('GET', '/api' + imageUrl)
        xhr.responseType = 'blob'
        xhr.onerror = () => {
            reject(new Error("url is not valid"))
            // reject(new Error("无效的图片的地址"))
        }
        xhr.send()
    })
}
```

用法 / Usage:
```javascript
// replace placeholder with url of picture
// 把placeholder替换为图片的地址
var url = "placeholder" 
getBase64FromUrl(url)
    .then(response => {
    // do something with result(response) 
    // 可以对response进行操作
    })
    .catch(error => {
        console.log(error)
    })
```