---
title: Base64 编码与解码工具
author: YiranrumengQAQ
date: 2026-04-28
---

这是一个直接运行在你博客内核中的小工具，可以帮你快速处理 Base64 数据。

<div style="background: var(--glass-bg-card); border: 1px solid var(--glass-border); padding: 20px; border-radius: var(--radius-md); margin-top: 20px;">
    <h3 style="margin-top:0;">输入内容</h3>
    <textarea id="base64Input" style="width: 100%; height: 100px; border-radius: var(--radius-sm); border: 1px solid var(--glass-border); padding: 10px; font-family: var(--font-mono); margin-bottom: 10px;" placeholder="输入文字或 Base64 字符串..."></textarea>
    
    <div style="display: flex; gap: 10px; margin-bottom: 20px;">
        <button onclick="handleBase64('encode')" style="padding: 10px 20px; border-radius: 20px; border: none; background: var(--accent); color: white; cursor: pointer; flex: 1;">编码 (Encode)</button>
        <button onclick="handleBase64('decode')" style="padding: 10px 20px; border-radius: 20px; border: none; background: #34a853; color: white; cursor: pointer; flex: 1;">解码 (Decode)</button>
    </div>

    <h3 style="margin-top:0;">输出结果</h3>
    <textarea id="base64Output" readonly style="width: 100%; height: 100px; border-radius: var(--radius-sm); border: 1px solid var(--glass-border); padding: 10px; font-family: var(--font-mono); background: rgba(0,0,0,0.05);" placeholder="结果将显示在这里..."></textarea>
</div>

<script>
function handleBase64(type) {
    const input = document.getElementById('base64Input').value;
    const outputField = document.getElementById('base64Output');
    
    try {
        if (type === 'encode') {
            // 使用 btoa 进行编码，结合 encodeURIComponent 处理中文
            outputField.value = btoa(encodeURIComponent(input).replace(/%([0-9A-F]{2})/g, function(match, p1) {
                return String.fromCharCode('0x' + p1);
            }));
        } else {
            // 使用 atob 进行解码，结合 decodeURIComponent 处理中文
            const decoded = atob(input);
            outputField.value = decodeURIComponent(Array.prototype.map.call(decoded, function(c) {
                return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));
        }
    } catch (e) {
        outputField.value = "错误: 请确保输入的是有效的字符串或 Base64 格式";
    }
}
</script>

---
### 💡 技术说明
* **原理**：利用浏览器原生 JavaScript 的 `btoa()` 和 `atob()` 函数。
* **中文支持**：内置了 `URI` 转义逻辑，解决了原生 Base64 不直接支持中文的问题。
* **UI 适配**：样式自动继承了你博客的**玻璃拟态变量**（如 `var(--glass-bg-card)`）。
