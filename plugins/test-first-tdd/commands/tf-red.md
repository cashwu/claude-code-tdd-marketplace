---
description: 執行TDD的Red階段。建立會失敗的測試，明確定義應實作的功能。
---

# TDD Red階段（建立失敗的測試）

執行TDD的Red階段，建立會失敗的測試。

**【功能名】**：{{feature_name}}

## 事前準備

請確認以下文件已存在：
```
docs/tdd/{feature_name}/requirements.md
docs/tdd/{feature_name}/testcases.md
```

## Red 階段目標

建立會失敗的測試，明確定義功能的行為。

**重要原則：**
- 測試必須是會失敗的（因為功能尚未實作）
- 測試要清楚定義預期的行為
- 測試要容易理解和維護

## 測試程式碼要求

### 1. 測試檔案結構

```javascript
// 測試檔案：__tests__/{feature_name}.test.js

describe('{feature_name}', () => {
  // 【測試群組說明】：此群組測試的功能範圍

  test('測試案例名稱', () => {
    // 【測試目的】：說明此測試要驗證什麼
    // 【測試內容】：具體測試的處理
    // 【預期行為】：正常情況下的結果

    // Given - 準備測試資料
    // 【測試資料準備】：說明為何準備這些資料
    const input = testData;

    // When - 執行測試
    // 【執行功能】：呼叫要測試的功能
    const result = functionToTest(input);

    // Then - 驗證結果
    // 【驗證結果】：確認符合預期
    expect(result).toBe(expectedValue);
  });
});
```

### 2. 繁體中文註解規範

每個測試必須包含：

**測試開始的註解：**
```javascript
// 【測試目的】：此測試要確認什麼功能
// 【測試內容】：具體要測試的處理
// 【預期行為】：正常情況下應該得到的結果
```

**Given（準備）階段：**
```javascript
// 【測試資料準備】：說明準備這些資料的原因
// 【初始條件】：測試執行前的狀態
```

**When（執行）階段：**
```javascript
// 【執行功能】：說明呼叫哪個功能
// 【處理內容】：這個功能做什麼處理
```

**Then（驗證）階段：**
```javascript
// 【驗證結果】：要確認什麼結果
// 【預期值】：為什麼這是正確的結果
```

**每個 expect 的註解：**
```javascript
expect(result.value).toBe(10); // 【確認】：數值正確為 10
expect(result.status).toBe('success'); // 【確認】：狀態為成功
```

## 實作步驟

1. **選擇測試案例**：
   - 從 testcases.md 選擇要實作的測試
   - 建議從最簡單的正常系統測試開始

2. **建立測試檔案**：
   - 在適當位置建立測試檔案
   - 設定測試框架和相關 import

3. **撰寫測試程式碼**：
   - 按照 Given-When-Then 結構
   - 加入完整的繁體中文註解
   - 確保測試會失敗（功能尚未實作）

4. **執行測試**：
   - 確認測試會失敗
   - 失敗訊息要清楚易懂

5. **記錄到 memo**：
   - 更新 docs/tdd/{feature_name}/memo.md
   - 記錄 Red 階段的進度

## memo.md 格式

```markdown
# {feature_name} TDD 開發記錄

## Red 階段（建立失敗的測試）

### 日期
{當前日期時間}

### 已建立的測試
- 測試案例 1：{測試名稱}
- 測試案例 2：{測試名稱}
...

### 測試檔案位置
{測試檔案路徑}

### 預期的失敗
{說明為什麼這些測試會失敗}

### 下一步
進入 Green 階段，實作功能讓測試通過。
```

## 測試執行命令

根據使用的測試框架：

**Jest/Vitest：**
```bash
npm test
# 或
npm test -- {test_file_name}
```

**其他框架：**
請根據專案設定執行測試命令。

## 品質檢查

完成後請確認：
- ✅ 測試會失敗（因為功能未實作）
- ✅ 失敗訊息清楚易懂
- ✅ 測試有完整的繁體中文註解
- ✅ 測試結構清晰（Given-When-Then）
- ✅ 已記錄到 memo.md

## 下一步

Red 階段完成後，執行：
```
/tf-green
```
開始實作功能讓測試通過（Green 階段）。
