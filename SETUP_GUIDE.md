# 完整設定指南

## ⚠️ 重要：需要自行設定的項目

本應用程式有兩個功能需要使用實際的 API 金鑰和憑證：

### 1. Firebase（已完成設定）✅

您的 `env.js` 已包含 Firebase 設定，**無需修改**。

### 2. LINE Messaging API（需要設定）❌

#### 必須設定的項目：

**檔案：`env.js`**

```javascript
const lineConfig = {
    channelAccessToken: "", // ⚠️ 請在這裡填入您的 LINE Channel Access Token
    // ⚠️ 注意：不需要 Channel Secret（僅在需要接收 Webhook 時才需要）
};
```

**重要說明：**
- ✅ **只需要 Channel Access Token**：用於發送訊息
- ❌ **不需要 Channel Secret**：僅在需要接收 LINE 訊息時才需要（本應用程式不需要）

## LINE API 設定步驟

### 步驟 1：申請 LINE 官方帳號與 Messaging API

1. 前往 [LINE Developers Console](https://developers.line.biz/console/)
2. 使用您的 LINE 帳號登入
3. 建立或選擇一個 Provider
4. 建立一個新的 Channel，選擇 **Messaging API**
5. 完成基本資訊填寫

### 步驟 2：取得 Channel Access Token

1. 進入您的 Channel
2. 點擊 **Messaging API** 標籤頁
3. 捲到下方找到 **Channel access token (long-lived)** 區塊
4. 點擊 **Issue** 按鈕
5. **重要**：選擇 **Long-lived**（長期有效）
6. 複製生成的 Token
7. 貼到 `env.js` 檔案中：

```javascript
const lineConfig = {
    channelAccessToken: "您剛才複製的 Token", // 例如：abcdefghijklmnopqrstuvwxyz...
};
```

### 步驟 3：啟用 Webhook（選擇性）

1. 在同一頁面找到 **Webhook URL** 區塊
2. 如果您有自己的伺服器，可以設定 Webhook URL
3. 本應用程式目前不需要 Webhook，可以跳過

### 步驟 4：取得接收者 ID（Line ID 或群組 ID）

這是關鍵步驟，有兩種方式：

#### 方式 A：使用 LINE Bot 測試（推薦）

1. 在 Channel 的 **Messaging API** 頁面，啟用 **Allow bot to join group chats**
2. 取得 LINE Bot 的 QR Code 或連結
3. 將 Bot 加入目標群組或加為好友
4. 在群組中發送訊息
5. 查看瀏覽器 Console（F12），應該會顯示群組 ID 或 User ID

#### 方式 B：使用後端服務（進階）

如果您有伺服器，可以建立 Webhook 接收訊息來取得 ID：

```javascript
// 後端範例（使用 Express.js）
app.post('/webhook', (req, res) => {
    const events = req.body.events;
    events.forEach(event => {
        if (event.type === 'message') {
            console.log('Group ID:', event.source.groupId);
            console.log('User ID:', event.source.userId);
            console.log('Room ID:', event.source.roomId);
        }
    });
    res.sendStatus(200);
});
```

### 步驟 5：在應用程式中設定

1. 開啟應用程式
2. 當有到期任務時，會彈出到期任務提醒視窗
3. 點擊「**自動催辦設定**」按鈕
4. 在設定視窗中：
   - 開啟「啟用自動催辦」開關
   - 設定催辦時間（例如：09:00）
   - 選擇接收者類型（群組/個人）
   - 貼上從步驟 4 取得的 ID
5. 點擊「**測試發送**」確認設定正確
6. 如果測試成功，點擊「**儲存設定**」

## 支援裝置

### ✅ 手機操作

本應用程式完全支援手機瀏覽器操作：

1. **用 Safari（iPhone）或 Chrome（Android）開啟**
2. **加入主畫面**（可像 App 一樣使用）：
   - iPhone：分享按鈕 > 加入主畫面
   - Android：選單 > 加入主畫面
3. **所有功能皆可用**：
   - 新增/編輯任務
   - 設定催辦時間
   - 手動發送 LINE 訊息
   - 自動催辦設定

### ✅ 響應式設計

- 手機版：觸控友善的按鈕設計
- 平板版：適中的排版
- 電腦版：完整的桌面體驗

## 功能說明

### 自動催辦功能

設定完成後，系統會：
- 每天在指定時間自動檢查到期任務
- 自動發送催辦訊息到 LINE
- 記錄發送歷史

**限制說明**：
- ⚠️ **瀏覽器必須保持開啟**：此功能在瀏覽器端運行
- 瀏覽器標籤頁不能關閉
- 建議不要使用瀏覽器的節能模式

### 手動催辦功能

即使未啟用自動催辦，您仍可以：
- 點擊個別任務的 LINE 按鈕，複製訊息並手動發送
- 使用「一鍵複製所有 LINE 提醒」功能批量複製

### Email 催辦功能

點擊任務的 Email 按鈕，系統會：
- 自動開啟您的郵件軟體
- 填入催辦內容
- 您需要手動輸入負責人的 Email 地址

## 疑難排解

### Token 錯誤

**問題**：收到「LINE Channel Access Token 未設定」或「LINE 發送失敗」

**解決方法**：
1. 確認 `env.js` 中的 Token 已正確填寫
2. 確認 Token 格式正確（不包含引號）
3. 前往 LINE Developers Console 確認 Token 未過期
4. 重新生成 Long-lived Token

### 收不到訊息

**問題**：測試發送成功，但收不到訊息

**解決方法**：
1. 確認 LINE Bot 已加入目標群組或已是好友
2. 確認接收者 ID 正確
3. 檢查是否有防呆設定（某些群組限制 Bot 發言）

### 定時不執行

**問題**：設定好自動催辦，但到了時間沒有發送

**解決方法**：
1. 確認瀏覽器保持開啟
2. 確認標籤頁未關閉
3. 檢查 Console 是否有錯誤訊息
4. 確認時間設定正確

### 跨域（CORS）問題

**問題**：Console 顯示 CORS 錯誤

**解決方法**：
LINE Messaging API 不支援瀏覽器直接調用。目前實作可先用於測試。生產環境請改以伺服器端實作：
- 使用 Firebase Cloud Functions
- 或建立自己的後端服務

## 進階建議

### 生產環境建議

為確保 24/7 自動運行，建議：

1. 使用 **Firebase Cloud Functions**
2. 設定 **Cloud Scheduler** 定時觸發
3. 在伺服器端執行檢查和發送邏輯

這樣可以避免：
- 瀏覽器必須保持開啟的限制
- CORS 問題
- 定時不穩定的問題

## 測試清單

設定完成後，請依序測試：

- [ ] 填入 LINE Channel Access Token
- [ ] 取得接收者 ID
- [ ] 測試發送功能
- [ ] 設定自動催辦
- [ ] 建立測試任務並設定期限
- [ ] 確認定時功能正常
- [ ] 檢查訊息格式是否正確

## 技術支援

如有問題，請參考：
- [LINE Messaging API 官方文件](https://developers.line.biz/zh-hant/docs/messaging-api/)
- [LINE Developers Console](https://developers.line.biz/console/)

