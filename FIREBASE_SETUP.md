# Firebase 一次性設定指南（約 5 分鐘）

即時同步需要一個自己的 Firebase 專案。步驟如下：

## 1. 建立 Firebase 專案

1. 打開 https://console.firebase.google.com/
2. 用你的 Google 帳號登入
3. 點「建立專案 / Add project」
4. 專案名稱輸入：`listening-levels`（或你喜歡的名字）
5. 關閉 Google Analytics（教學用不需要），點「建立專案」

## 2. 啟用 Realtime Database

1. 左側選單找到「Realtime Database」（不是 Firestore）
2. 點「建立資料庫 / Create database」
3. 位置選：`asia-southeast1`（新加坡，最接近台灣）
4. 安全規則選「**以測試模式啟動 / Start in test mode**」→ 建立

## 3. 取得 Web 設定（config）

1. 回到專案首頁，點上方的齒輪 ⚙ → 「專案設定」
2. 往下滑到「**你的應用程式 / Your apps**」
3. 點 `</>` 圖示（Web 應用程式）
4. 應用程式名稱：`listening-levels-web` → 註冊應用程式
5. 會出現一段 JavaScript 設定，長這樣：

```js
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXX",
  authDomain: "listening-levels.firebaseapp.com",
  databaseURL: "https://listening-levels-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "listening-levels",
  storageBucket: "listening-levels.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890"
};
```

**複製整個 `firebaseConfig` 物件內容。**

## 4. 貼進 index.html

1. 打開 `index.html`
2. 找到 `const firebaseConfig = {` 這一行（約第 220 行）
3. 把整個物件**整段替換**成你剛複製的
4. 存檔

## 5. 調整資料庫規則（重要：30 天後測試模式會失效）

1. Firebase Console → Realtime Database → 規則分頁
2. 替換成：

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

3. 發布

**說明**：這樣任何知道「房間密碼」的人都可讀寫那個房間，不知道密碼的人看不到資料。給教室用的安全等級剛好，不會太嚴也不會太鬆。

## 6. 部署到網路（讓平板能開）

三選一：

### A. GitHub Pages（免費、最穩）
1. 把這個資料夾 push 到 GitHub（public repo）
2. Repo Settings → Pages → Source 選 `main` branch → Save
3. 得到網址 `https://你的帳號.github.io/listening-levels-practice/`

### B. Netlify（免費、最快）
1. 到 https://app.netlify.com/drop
2. 直接把整個資料夾拖上去
3. 立刻拿到網址

### C. 直接在老師筆電測試
用瀏覽器打開 `index.html` 也可以，但平板連不到。

## 使用流程

1. 老師開網址 → 輸入房間密碼（例：`habit5-0414`）+ 自己座號（例：`99999`）進入
2. 學生平板開網址 → 輸入**同一個房間密碼** + 自己的五位數座號
3. 每個人都可以看到所有煩惱、按抽籤
4. 課後老師按「清空整個房間」把資料刪除

## 常見問題

- **想換班級/場次**：換一個房間密碼就好，資料彼此隔離
- **多班同時用**：各班用不同房間密碼即可
- **要保留資料**：按「匯出煩惱池」存成 txt
