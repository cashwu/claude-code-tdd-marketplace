---
description: 執行TDD的Green階段。實作功能讓失敗的測試通過。
---

# TDD Green階段（實作功能）

執行TDD的Green階段，實作最小功能讓測試通過。

**【功能名】**：{{feature_name}}

## 事前準備

請確認：
- Red 階段的測試已建立
- 測試執行後會失敗
- memo.md 已記錄 Red 階段進度

## Green 階段目標

**實作最小功能讓測試通過。**

**重要原則：**
- 只要能讓測試通過就好
- 程式碼簡潔優先，不用考慮完美
- 複雜的最佳化留到 Refactor 階段
- 先求能動，再求完美

## 實作原則

### 1. 最小實作優先
```javascript
// ❌ 不要一開始就寫複雜的實作
function calculate(a, b) {
  // 複雜的驗證和錯誤處理
  // 效能最佳化
  // 各種邊界條件處理
  ...
}

// ✅ 先寫最簡單能通過測試的程式碼
function calculate(a, b) {
  return a + b;
}
```

### 2. 階段性實作
- 先讓一個測試通過
- 再讓下一個測試通過
- 逐步增加功能

### 3. 容許暫時的解決方案
- 可以使用硬編碼
- 可以先忽略邊界條件
- 重構階段再改善

## 實作程式碼註解要求

### 函式層級註解
```javascript
/**
 * 【功能說明】：此函式的功能
 * 【實作方針】：為什麼這樣實作
 * 【對應測試】：為了通過哪個測試
 *
 * @param {type} paramName - 參數說明
 * @returns {type} - 回傳值說明
 */
function functionName(paramName) {
  // 實作內容
}
```

### 處理區塊註解
```javascript
function processData(input) {
  // 【輸入驗證】：檢查輸入是否有效
  if (!input) {
    throw new Error('輸入不可為空');
  }

  // 【主要處理】：核心功能實作
  const result = doSomething(input);

  // 【回傳結果】：回傳處理結果
  return result;
}
```

### 變數註解
```javascript
// 【初始化】：設定初始值
const initialValue = 0;

// 【計數器】：追蹤處理數量
let count = 0;
```

## 實作步驟

1. **執行測試確認失敗**：
   ```bash
   npm test
   ```

2. **實作最小功能**：
   - 只實作讓測試通過所需的程式碼
   - 加入繁體中文註解
   - 保持程式碼簡單

3. **再次執行測試**：
   - 確認測試通過
   - 如果失敗，修正後重試

4. **更新 memo.md**：
   - 記錄實作內容
   - 標註下一步改善項目

## memo.md 更新格式

在 memo.md 中加入 Green 階段記錄：

```markdown
## Green 階段（實作功能）

### 日期
{當前日期時間}

### 實作內容
{簡述實作了什麼功能}

### 實作檔案位置
{實作檔案路徑}

### 測試結果
- ✅ 測試 1：通過
- ✅ 測試 2：通過
...

### 待改善項目
{列出應該在 Refactor 階段改善的地方}

### 下一步
進入 Refactor 階段，改善程式碼品質。
```

## 常見的最小實作策略

### 策略 1：硬編碼
```javascript
// 先用固定值通過測試
function getGreeting() {
  return "Hello, World!";
}
```

### 策略 2：假實作
```javascript
// 先回傳符合格式的假資料
function fetchData() {
  return { status: 'success', data: [] };
}
```

### 策略 3：簡單邏輯
```javascript
// 用最簡單的邏輯實作
function isEven(num) {
  return num % 2 === 0;
}
```

## 品質檢查

完成後請確認：
- ✅ 所有測試都通過
- ✅ 實作程式碼有繁體中文註解
- ✅ 程式碼簡單易懂
- ✅ 已記錄到 memo.md
- ✅ 已標註待改善項目

## 下一步

Green 階段完成後，執行：
```
/tf-refactor
```
開始改善程式碼品質（Refactor 階段）。
