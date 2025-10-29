# LINE 自動催辦系統

這是一個基於網頁的自動化任務催辦系統，能夠透過 LINE 發送提醒訊息。

## 🌟 主要功能

- ✨ 自動催辦：在指定時間自動發送 LINE 提醒
- 📱 跨平台支援：支援電腦和手機使用
- 📧 多管道提醒：支援 LINE 和 Email 催辦
- 🔄 手動/自動模式：支援手動和自動催辦
- 🔐 Google 帳號整合：使用 Firebase 驗證

## 🚀 快速開始

### 1. 環境設定

1. 下載專案檔案
2. 開啟 `env.js` 檔案
3. 設定 LINE Channel Access Token（參考下方設定指南）

### 2. LINE API 設定

1. 前往 [LINE Developers Console](https://developers.line.biz/console/)
2. 建立或選擇一個 Channel
3. 取得 Channel Access Token
4. 將 Token 填入 `env.js`

### 3. 啟動應用程式

**電腦版**：
- 使用瀏覽器開啟 `index.html`
- 登入 Google 帳號

**手機版**：
- 用 Safari（iPhone）或 Chrome（Android）開啟
- 加入主畫面（選用）
- 登入 Google 帳號

## 📚 使用指南

詳細的使用說明請參考：
- [完整設定指南](SETUP_GUIDE.md)
- [LINE 整合指南](LINE_INTEGRATION_README.md)
- [手機使用說明](MOBILE_USAGE.md)
- [設定檢查清單](CHECKLIST.md)

## ⚠️ 重要注意事項

1. **瀏覽器需保持開啟**
   - 自動催辦功能需要瀏覽器保持運作
   - 建議使用桌機版本以確保穩定性

2. **手機使用限制**
   - 手機背景運行可能不穩定
   - 建議使用手動催辦或保持應用程式在前台

3. **替代方案**
   - 使用桌機版本
   - 考慮使用伺服器方案
   - 手動催辦功能永遠可用

## 🔧 進階功能

- Firebase 整合
- Email 催辦功能
- 批量催辦功能
- 自定義提醒時間
- 群組/個人 LINE 訊息發送

## 💡 建議使用方式

1. **基本使用**
   - 設定 LINE Token
   - 配置接收者 ID
   - 設定自動催辦時間

2. **進階使用**
   - 使用伺服器部署
   - 設定 Webhook（選用）
   - 整合 Cloud Functions

## 🆘 技術支援

如有問題，請參考：
- [LINE Messaging API 文件](https://developers.line.biz/zh-hant/docs/messaging-api/)
- [LINE Developers Console](https://developers.line.biz/console/)
- [Firebase 文件](https://firebase.google.com/docs)

## 📝 授權

本專案使用 MIT 授權條款。