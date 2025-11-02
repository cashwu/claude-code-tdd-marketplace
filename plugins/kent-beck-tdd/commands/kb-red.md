---
description: 寫下一個測試（Red 階段）。一次只寫一個最小的測試，讓它失敗。
---

# TDD Red - 寫下一個測試（Red）

寫下一個小測試，看它失敗。

**【功能名】**：{{feature_name}}

## Kent Beck 的 Red 階段理念

> "寫一個小測試。讓它失敗。"

### 一次一個測試的原則

**為什麼一次只寫一個？**
- 保持專注
- 快速反饋
- 小步前進
- 容易回退

### 如何選擇下一個測試？

Kent Beck 的建議：

1. **從 To-Do List 選**（心理 To-Do，不是文件）
   - 腦中想到什麼測試，記下來
   - 選最簡單的開始

2. **從上一個測試的經驗**
   - 上一個測試通過了，下一步自然浮現
   - "如果這樣，那會怎樣？"

3. **三角測量**
   - 如果不確定實作，寫第二個類似的測試
   - 從多個角度逼近正確實作

## 寫測試的步驟

### 1. 更新 journey.md

```markdown
### 第 N 輪 - {日期時間}

#### 🤔 想法
{為什麼要寫這個測試？從上一輪學到什麼？}

#### 🔴 Red - 寫測試
測試名稱：{測試名稱}
```

### 2. 寫測試程式碼

**測試結構：Given-When-Then**

```javascript
test('簡短描述測試意圖', () => {
  // Given - 準備
  // 【準備】：{為什麼需要這些資料？}
  const input = testData;

  // When - 執行
  // 【執行】：{測試什麼行為？}
  const result = functionUnderTest(input);

  // Then - 驗證
  // 【驗證】：{為什麼期待這個結果？}
  expect(result).toBe(expected);
});
```

### 3. 執行測試，確認失敗

```bash
npm test
```

**必須看到失敗！**
- 如果沒失敗，測試可能有問題
- 失敗訊息要清楚

### 4. 記錄失敗訊息

在 journey.md 中：
```markdown
#### 失敗訊息
```
{實際的錯誤訊息}
```

#### 📝 下一步
執行 /kb-green 讓測試通過
```

## 測試大小的藝術

### 測試要多小？

Kent Beck 的答案：**取決於你的信心**

**信心低（不確定）**：
- 寫更小的測試
- 更頻繁的反饋

**信心高（很確定）**：
- 可以寫大一點的測試
- 跨越明顯的步驟

### 範例：小測試 vs 大測試

**小測試（信心低時）**：
```javascript
// 第 1 個測試：最基本的
test('5 元乘以 2 等於 10 元', () => {
  const five = new Dollar(5);
  const product = five.times(2);
  expect(product.amount).toBe(10);
});

// 第 2 個測試：檢查副作用
test('乘法不改變原物件', () => {
  const five = new Dollar(5);
  five.times(2);
  expect(five.amount).toBe(5); // 原值不變
});
```

**大測試（信心高時）**：
```javascript
// 直接測試完整行為
test('Dollar 乘法運算', () => {
  const five = new Dollar(5);
  const ten = five.times(2);
  expect(ten.amount).toBe(10);
  expect(five.amount).toBe(5); // 也檢查副作用
});
```

## 三角測量的時機

當你不確定實作時，用三角測量：

```javascript
// 第 1 個測試
test('1 + 1 = 2', () => {
  expect(add(1, 1)).toBe(2);
});

// 最簡單的假實作
function add(a, b) {
  return 2; // 硬編碼
}

// 第 2 個測試（三角測量）
test('2 + 3 = 5', () => {
  expect(add(2, 3)).toBe(5);
});

// 現在必須寫真正的實作
function add(a, b) {
  return a + b;
}
```

## To-Do List 技巧

Kent Beck 建議在測試程式碼中寫註解：

```javascript
describe('Dollar', () => {
  // TODO: 測試加法
  // TODO: 測試負數
  // TODO: 測試相等性

  test('乘法', () => {
    // 目前的測試
  });
});
```

或在 journey.md 中：

```markdown
## 心理 To-Do List
- [x] 基本乘法
- [ ] 乘法副作用
- [ ] 加法運算
- [ ] 負數處理
```

## 常見問題

### Q: 要不要測試所有邊界情況？

Kent Beck：**不用一開始就全測**
- 先從明顯的案例開始
- 邊界情況在需要時再加入
- 讓測試自然演進

### Q: 測試名稱要多詳細？

Kent Beck：**描述意圖，不是實作**
```javascript
// ✅ 好的測試名稱
test('購物車加入商品後，總價增加')

// ❌ 不好的測試名稱
test('testAddItem')
```

### Q: 要寫測試到什麼程度？

Kent Beck：**寫到你有信心為止**
- 如果還不確定，再寫一個測試
- 如果已經確定，可以跳到實作

## 範例：Money 第一個測試

```javascript
// __tests__/money.test.js

describe('Dollar', () => {
  test('乘法運算', () => {
    // 【準備】：建立 5 元的 Dollar 物件
    const five = new Dollar(5);

    // 【執行】：乘以 2
    const product = five.times(2);

    // 【驗證】：結果應該是 10 元
    expect(product.amount).toBe(10);
  });
});
```

執行測試：
```bash
npm test

# 結果：失敗！
# Error: Dollar is not defined
```

在 journey.md 記錄：
```markdown
### 第 1 輪

#### 🔴 Red - 寫測試
測試名稱：Dollar 乘法運算

#### 失敗訊息
```
Error: Dollar is not defined
```

#### 📝 下一步
執行 /kb-green 建立 Dollar 類別
```

## 下一步

測試寫好且失敗了？執行：
```
/kb-green
```

用最簡單的方式讓測試通過！

## 記住

> "測試不是目的，而是思考的工具。"
> "失敗的測試是進步的開始。"
> "一次一小步，但要持續前進。"
