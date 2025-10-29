# 設定檢查清單

## ✅ 已完成的工作

### Firebase 設定
- [x] Firebase 設定已包含在 `env.js` 中
- [x] 應用程式已整合 Firebase Authentication
- [x] 資料庫已設定

### 功能開發
- [x] 到期任務提醒功能
- [x] Email 催辦功能
- [x] 手動 LINE 催辦功能（複製訊息）
- [x] LINE 自動催辦功能
- [x] LINE 設定介面

## ⚠️ 需要您完成的設定

### 1. LINE Messaging API 金鑰

**檔案位置：`env.js`**

```javascript
const lineConfig = {
    channelAccessToken: "", // ⬅️ 請在這裡填入 Token
    // 注意：不需要 Channel Secret
};
```

**設定步驟：**
1. 前往 https://developers.line.biz/console/
2. 登入後選擇或建立 Channel
3. 到 Messaging API 頁面
4. 產生 Long-lived Channel Access Token
5. 複製 Token 並貼到 `env.js`

**檢查點：**
- [ ] 已前往 LINE Developers Console
- [ ] 已產生 Long-lived Token
- [ ] 已將 Token 填入 `env.js` 檔案
- [ ] Token 格式正確（不含引號）
- [ ] 確認不需要設定 Channel Secret（本應用程式不需要）

### 2. LINE 接收者 ID

**需要取得的 ID：**
- 群組 ID（如果要發送到群組）
- 或 User ID（如果要發送給個人）

**取得方式：**
1. 將 LINE Bot 加入目標群組或加為好友
2. 在群組中發送訊息
3. 透過 Webhook 或 Console 記錄查看 ID

**檢查點：**
- [ ] 已將 LINE Bot 加入群組/加為好友
- [ ] 已取得接收者 ID
- [ ] 已在應用程式中測試發送功能

### 3. 測試功能

**測試項目：**
- [ ] 手動複製 LINE 訊息功能
- [ ] Email 催辦功能
- [ ] LINE 測試發送功能
- [ ] 自動催辦定時功能（可先將時間設在幾分鐘後測試）

## 📝 已知限制

### 瀏覽器環境限制

1. **瀏覽器必須保持開啟**
   - 應用程式在瀏覽器端運行
   - 關閉瀏覽器會停止定時功能
   - 需保持標籤頁開啟

2. **CORS 限制**
   - 直接從瀏覽器調用 LINE API 可能遇到 CORS 問題
   - 目前實作主要用於測試和開發環境
   - 生產環境建議使用伺服器端方案

### 建議的生產環境方案

使用 **Firebase Cloud Functions + Cloud Scheduler**：
- 24/7 持續運行
- 不受瀏覽器狀態影響
- 避免 CORS 問題
- 更穩定可靠

## 🚀 快速開始

### 第一次使用

1. **設定 LINE Token**
   ```bash
   編輯 env.js 檔案
   填入 channelAccessToken（不需要 Channel Secret）
   ```

2. **開啟應用程式**
   
   電腦版：
   ```bash
   用瀏覽器開啟 index.html
   登入 Google 帳號
   ```
   
   手機版：
   ```bash
   用 Safari 或 Chrome 開啟網頁
   加入主畫面（可像 App 使用）
   登入 Google 帳號
   ```

3. **設定自動催辦**
   - 在到期任務視窗點擊「自動催辦設定」
   - 開啟功能並設定時間
   - 輸入接收者 ID
   - 測試發送後儲存

### 測試自動催辦

1. 建立測試任務，設定期限為今天或明天
2. 設定自動催辦時間為 1-2 分鐘後
3. 等待時間到達，檢查是否收到 LINE 訊息

## 📞 需要幫助？

如遇到問題，請檢查：
1. `env.js` 檔案是否正確設定
2. Console（F12）是否有錯誤訊息
3. LINE Bot 是否已加入群組/好友
4. Token 是否有效

詳細說明請參考 `SETUP_GUIDE.md`

