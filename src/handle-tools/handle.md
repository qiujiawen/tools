### 1、获取URL中params值

```
function getUrlParam(url) {

    let obj = {};
    let reg = /([^?&=]*)=([^?&=]*)/g;
    url.replace(reg, function (src, $1, $2) {
        obj[$1] = $2;
    });
       return obj;
}

let params = getUrlParam('./buzz?b=353&c=10');
console.log(params.b); // 打印出 '353'

```