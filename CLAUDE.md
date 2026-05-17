# 北疆十五日 · 絲路之約 — 專案說明

私人旅遊行程網站，供 10 人旅遊團使用。單一 `index.html`，部署在 GitHub Pages。

## 技術棧

- 純 HTML / CSS / JavaScript，無框架
- 地圖：Leaflet.js + 高德底圖 Tile（計畫升級至高德 JS API SDK）
- 字型：Google Fonts（Cormorant Garamond、Noto Serif TC/SC、Inter）
- 後端：Firebase Firestore（日記系統）
- 部署：GitHub Pages（靜態）

## 目前功能

- 15 天行程時間軸（含景點、飯店、時間表）
- 互動地圖（景點標記 + 虛線路線）
- 繁體 / 簡體語言切換
- RWD 手機版面
- **多人旅行日記**（Firebase Firestore）
  - 密碼牆：輸入名字 + 旅行密碼才能發文
  - 按天折疊：15 天各一區塊，點開看所有人當天的日記
  - 即時同步：onSnapshot，一人發文其他人馬上看到
  - 密碼存在 Firestore `config/settings.passcode`（純文字比對）
  - 登入狀態存 localStorage（`xinjiang_diary`）

## Firebase 設定位置

`index.html` 裡搜尋 `firebaseConfig`，把 6 個欄位填入：
```js
const firebaseConfig = {
  apiKey, authDomain, projectId,
  storageBucket, messagingSenderId, appId
};
```

Firestore 集合結構：
- `entries/{id}` — 每篇日記：`day`, `author`, `text`, `weather`, `createdAt`
- `config/settings` — 密碼文件：`passcode`（string，手動在 Console 新增）

## 開發路線圖

### 下一步：高德 JS API SDK 升級
目前用非官方 Tile URL，升級官方 SDK 後：
- 解決與 Google Maps 的 500m 座標偏移問題
- 解鎖即時路線規劃、一鍵導航功能
- 接高德天氣 API

### 之後考慮
- 日記加照片上傳（Firebase Storage）
- 路況提醒連結（獨庫 / 阿禾 / 伊昭公路 6 月封路風險高）

### 不做的功能
- 團員即時定位分享（使用者不需要）

## 注意事項

- 新疆 6 月晝夜溫差大，天氣預報功能實用性高
- 獨庫公路、阿禾公路、伊昭公路 6 月可能封路，可考慮加「今日路況」連結
- 費用分攤區塊已移除（2026-05-17）
