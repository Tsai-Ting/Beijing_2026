# 北疆十五日 · 絲路之約 — 專案說明

私人旅遊行程網站，供 10 人旅遊團使用。單一 `index.html`，部署在 GitHub Pages。

## 技術棧

- 純 HTML / CSS / JavaScript，無框架
- 地圖：Leaflet.js + 高德底圖 Tile
- 字型：Google Fonts（Cormorant Garamond、Noto Serif TC/SC、Inter）
- 日記後端：Firebase Firestore
- 照片儲存：Cloudinary
- 部署：GitHub Pages

## 目前功能

- 15 天行程時間軸（含景點、飯店、時間表）
- 互動地圖（景點標記 + 虛線路線）
- 繁體 / 簡體語言切換
- RWD 手機版面
- 多人旅行日記（Firebase + Cloudinary）

---

## 帳號與設定

### GitHub
- Repo：https://github.com/Tsai-Ting/Beijing_2026
- 網站網址：https://tsai-ting.github.io/Beijing_2026/

### Firebase
- 專案：`xinjiang-2026`
- Console：https://console.firebase.google.com
- Firestore 集合：
  - `entries/{id}` — 日記：`day`, `author`, `text`, `weather`, `photoURL`, `createdAt`
  - `config/settings` — 密碼：`passcode`（手動在 Console 設定，不要改 code）

### Cloudinary
- Cloud name：`dy3jyj9kv`
- Upload Preset：`xinjiang-2026`（Unsigned）
- Console：https://cloudinary.com

---

## 日記系統運作方式

1. 訪客點「加入旅行團」→ 輸入名字 + 旅行密碼
2. 密碼與 Firestore `config/settings.passcode` 比對
3. 通過後可點「✏ 寫日記」，選天數、寫內容、選天氣、上傳照片（選填）
4. 照片上傳到 Cloudinary，URL 存進 Firestore
5. 即時同步（onSnapshot），所有人馬上看到新日記
6. 登入狀態存 localStorage（`xinjiang_diary`），重整不用重新登入

---

## 更新網站流程

改完 `index.html` 後：

```bash
git add index.html
git commit -m "說明改了什麼"
git push
```

等 1-2 分鐘，瀏覽器按 `Cmd + Shift + R` 強制重新整理。

---

## Firestore 安全規則

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /entries/{id} {
      allow read: if true;
      allow create: if request.resource.data.keys().hasAll(['day','author','text','weather','createdAt']);
      allow update, delete: if false;
    }
    match /config/settings {
      allow read: if true;
      allow write: if false;
    }
  }
}
```

## Cloudinary Storage 規則

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /diary-photos/{allPaths=**} {
      allow read: if true;
      allow write: if request.resource.size < 15 * 1024 * 1024
                   && request.resource.contentType.matches('image/.*');
    }
  }
}
```

---

## 開發路線圖

### 下一步
- 高德 JS API SDK 升級（API 待接入）
  - 解決 500m 座標偏移問題
  - 即時路線規劃、一鍵導航
  - 高德天氣 API

### 之後考慮
- 路況提醒連結（獨庫 / 阿禾 / 伊昭公路 6 月封路風險高）

### 不做
- 團員即時定位分享（使用者不需要）
- 費用分攤（已移除，2026-05-17）
