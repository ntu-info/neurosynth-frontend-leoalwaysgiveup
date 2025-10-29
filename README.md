[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/yOwut1-r)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=21272696&assignment_repo_type=AssignmentRepo)

# 🧠 Neurosynth Frontend

一個基於 AJAX 的神經科學術語查詢與研究搜尋系統前端介面。

## 📋 專案簡介

本專案為台大心理系程式設計作業 #7，實作了一個連接 Neurosynth 後端 API 的互動式網頁應用程式。使用 Tailwind CSS 打造美觀的使用者介面，並透過 AJAX 技術實現即時搜尋功能。

## ✨ 主要功能

### 1. 所有術語查詢 (`/terms`)
- 顯示所有可用的神經科學術語
- 以卡片形式呈現，支援點擊快速跳轉
- 響應式網格布局，自動適應不同螢幕尺寸

### 2. 術語關聯查詢 (`/terms/<term>`) ⚡ AJAX
- **即時查詢**：輸入時自動發送請求，無需按 Enter
- 使用 500ms debounce 機制優化效能
- 顯示與指定術語相關的所有資訊
- 支援多種資料格式自動解析

### 3. 研究邏輯搜尋 (`/query/<query>/studies`) ⚡ AJAX
- **即時搜尋**：輸入時自動更新結果
- 支援邏輯運算符：
  - `and` - 同時包含兩個術語
  - `or` - 包含任一術語
  - `not` - 排除特定術語
- 完整顯示研究資訊（標題、作者、期刊、年份）

## 🎨 技術特色

- ✅ **Tailwind CSS**：現代化的 UI 設計
- ✅ **AJAX 即時查詢**：使用者體驗優化
- ✅ **自動容錯切換**：支援多個後端伺服器自動切換
- ✅ **響應式設計**：完美支援桌面、平板、手機
- ✅ **錯誤處理**：友善的錯誤提示與重試機制
- ✅ **載入動畫**：流暢的視覺回饋

## 🚀 使用方法

### 線上版本
直接訪問 GitHub Pages 部署版本（待部署）

### 本地運行

1. **下載專案**
```bash
git clone https://github.com/[your-username]/neurosynth-frontend.git
cd neurosynth-frontend
```

2. **啟動本地伺服器**
```bash
python3 -m http.server 8000
```

3. **在瀏覽器中開啟**
```
http://localhost:8000
```

> ⚠️ **重要**：必須使用本地伺服器運行，直接開啟 HTML 檔案會遇到 CORS 限制。

## 🔧 技術架構

### 前端技術
- **HTML5** - 結構標記
- **Tailwind CSS** - 樣式框架
- **Vanilla JavaScript** - 互動邏輯
- **Fetch API** - AJAX 請求

### 後端 API
- **主伺服器**：`https://mil.psy.ntu.edu.tw:5000`
- **備用伺服器**：`https://hpc.psy.ntu.edu.tw:5000`

### API 端點
```
GET /terms                          # 取得所有術語
GET /terms/<term>                   # 查詢特定術語
GET /query/<query_string>/studies   # 搜尋研究
```

## 📸 功能展示

### 術語查詢範例
```
輸入：amygdala
結果：emotional, emotion, fear, faces, responses...
```

### 研究搜尋範例
```
查詢：amygdala not emotion
結果：385 項研究
```

## 🎯 核心實作重點

### AJAX 即時查詢
```javascript
// 使用 debounce 避免過度請求
document.getElementById('term-input').addEventListener('input', (e) => {
  clearTimeout(termLookupTimeout);
  termLookupTimeout = setTimeout(() => {
    fetchTermLookup(e.target.value);
  }, 500);
});
```

### 自動容錯機制
```javascript
// 支援多伺服器自動切換
async function fetchWithFallback(endpoint) {
  for (let server of API_SERVERS) {
    try {
      const resp = await fetch(`${server}${endpoint}`);
      if (resp.ok) return resp;
    } catch (err) {
      console.log(`Server ${server} failed, trying next...`);
    }
  }
  throw new Error('所有伺服器均無法連接');
}
```

## 📦 專案結構

```
neurosynth-frontend/
├── index.html              # 主要網頁檔案
├── README.md              # 專案說明文件
├── LICENSE                # 授權文件
└── info_07_tutorial/      # 教學檔案（老師提供）
```

## 🐛 常見問題

### Q: 為什麼顯示 "Failed to fetch"？
**A:** 請確保使用本地伺服器運行，而不是直接開啟 HTML 檔案。

### Q: 為什麼沒有顯示結果？
**A:** 檢查網路連線，或等待伺服器恢復。系統會自動嘗試切換到備用伺服器。

### Q: AJAX 功能如何測試？
**A:** 在「術語查詢」或「研究搜尋」分頁中輸入文字，無需按 Enter，系統會自動查詢並顯示結果。

## 👨‍💻 開發者

- **課程**：台灣大學心理學系 - 程式設計
- **作業**：Homework #7 - Neurosynth Frontend
- **截止日期**：2025/10/31 09:00

## 📝 授權

本專案遵循 MIT License。

## 🙏 致謝

- 感謝 Tren 老師提供後端 API 與教學資源
- Tailwind CSS 框架
- Neurosynth 專案團隊

---

**💡 提示**：本專案特別強調 AJAX 功能的實作，術語查詢與研究搜尋均採用即時查詢機制，提供流暢的使用者體驗。
