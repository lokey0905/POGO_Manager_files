# ShortCuts `isIntent` 使用說明

這份文件說明 `files.json` 中如何設定 `isIntent`。

## 基本欄位

每個項目都要有：

```json
{
  "title": "顯示名稱",
  "url": "目標網址或 Intent",
  "isIntent": true
}
```

- `title`: 按鈕顯示文字。
- `url`: 目標內容。
- `isIntent`: 
  - `false` -> 當一般下載/網頁連結處理。
  - `true` -> 先嘗試用 Android Intent 開啟。

## `url` 支援格式（`isIntent=true`）

### 1) App 元件格式（最穩定）

格式：`packageName/className`

```json
{
  "title": "系統設定",
  "url": "com.android.settings/.Settings",
  "isIntent": true
}
```

### 2) 標準 Intent URI

格式範例：

```json
{
  "title": "Chrome App設定",
  "url": "intent://settings/#Intent;action=android.settings.APPLICATION_DETAILS_SETTINGS;data=package:com.android.chrome;end",
  "isIntent": true
}
```

### 3) 舊版 Hash Intent（已支援）

如果你原本用的是 `intent:#Intent;...;end`，現在也可解析：

```json
{
  "title": "Chrome App設定",
  "url": "intent:#Intent;action=android.settings.APPLICATION_DETAILS_SETTINGS;data=package:com.android.chrome;end",
  "isIntent": true
}
```

## 建議寫法

- 優先用「App 元件格式」或「標準 Intent URI」。
- 若要提供失敗備援，可在 Intent 放入 `S.browser_fallback_url=`。

範例：

```json
{
  "title": "範例（含備援）",
  "url": "intent://open/#Intent;action=android.intent.action.VIEW;S.browser_fallback_url=https%3A%2F%2Fwww.google.com;end",
  "isIntent": true
}
```

## 常見問題

- 顯示「無法啟動應用程式」：
  - 目標 App 不存在，或該 Intent 在裝置上沒有可處理的 Activity。
  - 請先確認 `packageName` 與 `className` 正確。
- `isIntent=true` 但仍開網頁：
  - Intent 無法啟動時，程式會嘗試 fallback（例如 `browser_fallback_url` 或原本 http/https）。


