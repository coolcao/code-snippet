
---
tag: [javascript, js, 剪切板, 粘贴板, 剪贴板]
---

```js
// return a promise
function copyToClipboard(textToCopy) {
    // navigator.clipboard需要https才能正常运行
    if (navigator.clipboard && window.isSecureContext) {
        return navigator.clipboard.writeText(textToCopy);
    } else {
        // 如果不是安全环境（非https），使用 document.execCommand('copy')
        let textArea = document.createElement("textarea");
        textArea.value = textToCopy;
        // make the textarea out of viewport
        textArea.style.position = "fixed";
        textArea.style.left = "-999999px";
        textArea.style.top = "-999999px";
        document.body.appendChild(textArea);
        textArea.focus();
        textArea.select();
        return new Promise((res, rej) => {
            // document.execCommand('copy')这个接口后面可能会被丢弃
            document.execCommand('copy') ? res() : rej();
            textArea.remove();
        });
    }
}

// 如何使用
copyToClipboard("I'm going to the clipboard !")
    .then(() => console.log('text copied !'))
    .catch(() => console.log('error'));
```