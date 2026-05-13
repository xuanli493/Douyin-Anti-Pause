# 🎬 Douyin Background Play Fix

修复抖音网页端在**后台或长时间无操作时自动暂停播放**的问题。  
支持直播和普通视频页面。

---

## ✨ 功能

- 🚫 阻止“长时间无操作自动暂停”
- ▶️ 保持直播 / 视频持续播放
- 🧠 伪造页面始终处于“可见状态”
- ⚡ 无需安装客户端，纯网页端生效

---

## 📦 安装方式

### 方式一：油猴（推荐）

1. 安装浏览器扩展：
   - Tampermonkey（油猴）

2. 点击安装脚本（GreasyFork）  
   👉 *https://greasyfork.org/zh-CN/scripts/576472-douyin-anti-pause*

---

### 方式二：手动安装（不推荐 这种方式无自动更新 可能会失效）

1. 新建一个 Userscript
2. 粘贴以下代码：

```javascript
// ==UserScript==
// @name         Douyin Anti Pause
// @namespace    http://tampermonkey.net/
// @version      1.1
// @description  阻止抖音网页端长时间无操作自动暂停
// @match        *://www.douyin.com/*
// @match        *://live.douyin.com/*
// @match        *://*.douyin.com/*
// @run-at       document-start
// @grant        unsafeWindow
// @license      MIT
// ==/UserScript==
 
(function() {
    'use strict';
 
    console.log("Douyin Anti Pause 已加载");
 
    const docProto = Document.prototype;
 
    try {
        Object.defineProperty(docProto, 'hidden', {
            configurable: true,
            get: () => false
        });
 
        Object.defineProperty(docProto, 'visibilityState', {
            configurable: true,
            get: () => 'visible'
        });
 
        document.hasFocus = () => true;
 
        console.log("visibility hook 成功");
    } catch (e) {
        console.error("visibility hook 失败", e);
    }
 
})();
````

---

## ⚙️ 实现原理（简要）

抖音网页端会通过以下方式判断用户是否“活跃”：

* `document.visibilityState`
* `document.hidden`
* `document.hasFocus()`

当页面处于后台时：
👉 触发定时器 → 自动暂停播放

本脚本通过：

* 伪造页面始终为“可见”
* 拦截 `video.pause()`

从而阻止该逻辑生效。

---

## ⚠️ 注意事项

* 虽然本脚本拦截网页了主动触发的 `video.pause()`
但经过测试 并不会影响手动点击暂停键的效果

---

## 🔄 可能的失效情况

如果抖音更新前端逻辑，可能出现失效，例如：

* 使用自定义播放器替代 `HTMLVideoElement`
* 增加额外检测逻辑

---


## 📄 License

MIT

---

## ⭐ 支持

如果这个脚本对你有帮助，可以点个 Star 👍

```
