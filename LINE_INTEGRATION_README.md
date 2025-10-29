# LINE 自動催辦功能設定指南

## 概述

本功能整合了 LINE Messaging API，讓您可以每日定時自動發送到期任務催辦通知到 LINE 群組或個人。

## 功能特色

- ✅ 每日定時自動催辦
- ✅ 支援 LINE 群組與個人訊息
- ✅ 自動彙整到期任務
- ✅ 測試發送功能
- ✅ 可自訂催辦時間

## 設定步驟

### 1. 取得 LINE Channel Access Token

1. 前往 [LINE Developers Console](https://developers.line.biz/console/)
2. 選擇您的 Provider 和 Channel
3. 進入 **Messaging API** 頁籤
4. 在 **Channel access token** 區段點擊 **Issue** 建立新的 Token
5. 選擇 **Long-lived**（長期有效的 Token）
6. 複製生成的 Token

### 2. 設定 Token

開啟 `env.js` 檔案，將 Token 填入：

```javascript
const lineConfig = {
    channelAccessToken: "YOUR_CHANNEL_ACCESS_TOKEN_HERE",
};
```

### 3. 取得 LINE 接收者 ID

#### 方法一：群組 ID

1. 將您的 LINE Bot 加入目標群組
2. 請某人在群組中發送訊息
3. 查看瀏覽器的 Console 或網路請求，找到群組的 Group ID

#### 方法二：個人 User ID

1. 請使用者將您的 LINE Bot 加為好友
2. 在聊天中發送訊息
3. 查看 Console 找到 User ID

**注意**：標準的 LINE Messaging API 無法直接取得這些 ID，您需要在伺服器端建立 Webhook 接收來自 LINE 的訊息，從中提取 ID。

### 4. 在應用程式中設定

1. 當有到期任務時，點擊 **自動催辦設定** 按鈕
2. 開啟「啟用自動催辦」開關
3. 設定催辦時間（例如：09:00）
4. 選擇接收者類型（群組或個人）
5. 貼上剛才取得的 ID
6. 點擊「測試發送」確認設定正確
7. 點擊「儲存設定」

## 使用方式

### 自動催辦

設定完成後，系統將在每日指定時間自動檢查是否有到期任務：

- **已過期任務**：超過截止日期的任務
- **今日到期任務**：今日為截止日期的任務

如果發現到期任務，系統會自動發送催辦訊息到指定的 LINE 群組或個人。

### 訊息格式範例

```
🔔 【任務催辦通知】

⚠️ 已過期任務 (2 項)：
1. 完成季度報告
   負責人：張三
   截止日期：2024/01/15

2. 更新客戶資料
   負責人：李四
   截止日期：2024/01/18

📅 今日到期任務 (1 項)：
1. 提交週報
   負責人：王五

請儘速回報進度，謝謝！

---
工作任務追蹤平台
```

### 手動測試

在設定頁面中，點擊「測試發送」按鈕，會發送一則測試訊息，確認設定是否正確。

## 注意事項

### 瀏覽器需要保持開啟

⚠️ **重要**：由於此功能是在瀏覽器端運行，因此：

1. 瀏覽器視窗必須保持開啟狀態
2. 瀏覽器標籤頁需要保持在前台（如果可能的話）
3. 不建議使用瀏覽器的節能模式

### 建議使用伺服器端方案

對於生產環境，建議：

1. 使用雲端函數（如 Firebase Cloud Functions、Google Cloud Functions）
2. 設定 Cloud Scheduler 定時觸發
3. 在伺服器端執行檢查和發送邏輯

這樣可以確保：
- 24/7 不間斷運行
- 不受瀏覽器狀態影響
- 更穩定可靠

## 疑難排解

### Token 錯誤

如果收到「LINE API 錯誤」，請檢查：
- Token 是否正確
- Token 是否過期（Long-lived Token 理論上不會過期）
- Token 是否有發送訊息的權限

### 收不到訊息

請確認：
- LINE Bot 已加入群組或已成為好友
- 接收者 ID 是否正確
- 設定是否已儲存且已啟用自動催辦

### 定時不執行

檢查：
- 瀏覽器是否保持開啟
- 檢查 Console 是否有錯誤訊息
- 時間設定是否正確

## 進階說明

### 如何建立 Webhook 接收 ID

如需在 Web 環境中取得 LINE ID，可以：

1. 使用 LINE Webhook URL（需要在 LINE Developers Console 設定）
2. 建立簡易的後端服務接收 LINE 訊息
3. 從接收到的訊息中提取 Group ID 或 User ID

範例：

```javascript
// LINE 會 POST 訊息到您的 Webhook URL
app.post('/webhook', (req, res) => {
    const events = req.body.events;
    events.forEach(event => {
        if (event.type === 'message') {
            console.log('Group ID:', event.source.groupId);
            console.log('User ID:', event.source.userId);
        }
    });
    res.sendStatus(200);
});
```

### 使用 Cloud Functions 自動化

```javascript
// Cloud Functions 範例（Firebase）
const functions = require('firebase-functions');
const admin = require('firebase-admin');
const axios = require('axios');

admin.initializeApp();

exports.dailyLineReminder = functions.pubsub
    .schedule('0 9 * * *') // 每天 09:00 執行
    .onRun(async (context) => {
        // 取得到期任務
        // 格式化訊息
        // 發送 LINE 訊息
        return null;
    });
```

## 參考資源

- [LINE Messaging API 文件](https://developers.line.biz/zh-hant/docs/messaging-api/)
- [LINE Developers Console](https://developers.line.biz/console/)
- [Firebase Cloud Functions](https://firebase.google.com/docs/functions)

## 支援

如有任何問題，請聯絡開發團隊或查看官方文件。


