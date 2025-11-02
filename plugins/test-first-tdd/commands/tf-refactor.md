---
description: 執行TDD的Refactor階段。在測試通過後，改善程式碼品質。
---

# TDD Refactor階段（程式碼重構）

執行TDD的Refactor階段，改善程式碼品質。

**【功能名】**：{{feature_name}}

## 事前準備

請確認：
- 所有測試都通過
- Green 階段的實作已完成
- memo.md 已記錄待改善項目

## Refactor 階段目標

**在保持測試通過的前提下，改善程式碼品質。**

**重要原則：**
- 測試必須持續通過
- 不改變功能行為
- 小步改善，頻繁測試
- 改善後測試，測試失敗立即回復

## 改善的重點

### 1. 提高可讀性
- 改善變數和函式命名
- 增加或改善註解
- 簡化複雜的邏輯

### 2. 消除重複（DRY原則）
```javascript
// ❌ 重複的程式碼
function processA(data) {
  if (!data) throw new Error('無效資料');
  return data.value * 2;
}

function processB(data) {
  if (!data) throw new Error('無效資料');
  return data.value * 3;
}

// ✅ 提取共通邏輯
function validateData(data) {
  if (!data) throw new Error('無效資料');
}

function processA(data) {
  validateData(data);
  return data.value * 2;
}

function processB(data) {
  validateData(data);
  return data.value * 3;
}
```

### 3. 改善程式結構
- 單一職責原則（每個函式只做一件事）
- 提取魔術數字為常數
- 簡化條件判斷

### 4. 改善註解品質
```javascript
/**
 * 【功能說明】：計算折扣後的價格
 * 【改善說明】：提取折扣計算邏輯，提高可讀性
 * 【使用情境】：結帳時計算最終價格
 *
 * @param {number} price - 原始價格
 * @param {number} discount - 折扣百分比（0-100）
 * @returns {number} - 折扣後價格
 */
function calculateDiscountedPrice(price, discount) {
  // 【參數驗證】：確保價格和折扣都是有效數值
  if (price < 0 || discount < 0 || discount > 100) {
    throw new Error('無效的價格或折扣');
  }

  // 【折扣計算】：套用折扣百分比
  const discountAmount = price * (discount / 100);

  // 【最終價格】：原價減去折扣金額
  return price - discountAmount;
}
```

## 重構步驟

### 步驟 1：執行測試確認全部通過
```bash
npm test
```

### 步驟 2：識別改善點
根據 memo.md 中記錄的待改善項目：
- 重複的程式碼
- 不清楚的命名
- 複雜的邏輯
- 缺少的註解

### 步驟 3：小步改善
每次只改善一個地方：
1. 改善一個項目
2. 執行測試
3. 測試通過，繼續下一個
4. 測試失敗，立即回復

### 步驟 4：更新 memo.md
記錄重構內容

## 常見重構技巧

### 技巧 1：提取函式
```javascript
// Before
function processOrder(order) {
  // 驗證訂單
  if (!order.items || order.items.length === 0) {
    throw new Error('訂單沒有商品');
  }

  // 計算總價
  let total = 0;
  for (const item of order.items) {
    total += item.price * item.quantity;
  }

  return total;
}

// After
function processOrder(order) {
  validateOrder(order);
  return calculateTotal(order.items);
}

function validateOrder(order) {
  // 【訂單驗證】：確保訂單包含商品
  if (!order.items || order.items.length === 0) {
    throw new Error('訂單沒有商品');
  }
}

function calculateTotal(items) {
  // 【總價計算】：計算所有商品的總價
  let total = 0;
  for (const item of items) {
    total += item.price * item.quantity;
  }
  return total;
}
```

### 技巧 2：提取常數
```javascript
// Before
function calculateShipping(weight) {
  if (weight > 1000) {
    return 100;
  }
  return 50;
}

// After
// 【運費常數】：定義不同重量級別的運費
const SHIPPING_COST = {
  STANDARD: 50,
  HEAVY: 100,
};

// 【重量門檻】：超過此重量視為重貨
const HEAVY_WEIGHT_THRESHOLD = 1000;

function calculateShipping(weight) {
  // 【運費計算】：根據重量決定運費
  if (weight > HEAVY_WEIGHT_THRESHOLD) {
    return SHIPPING_COST.HEAVY;
  }
  return SHIPPING_COST.STANDARD;
}
```

### 技巧 3：簡化條件
```javascript
// Before
function canPurchase(user, product) {
  if (user.age >= 18) {
    if (user.balance >= product.price) {
      if (product.stock > 0) {
        return true;
      }
    }
  }
  return false;
}

// After
function canPurchase(user, product) {
  // 【購買條件檢查】：驗證使用者和商品狀態
  const isAdult = user.age >= 18;
  const hasEnoughBalance = user.balance >= product.price;
  const isInStock = product.stock > 0;

  return isAdult && hasEnoughBalance && isInStock;
}
```

## memo.md 更新格式

在 memo.md 中加入 Refactor 階段記錄：

```markdown
## Refactor 階段（程式碼重構）

### 日期
{當前日期時間}

### 改善內容
1. {改善項目 1}
   - 改善前：{描述}
   - 改善後：{描述}
   - 原因：{為什麼要這樣改善}

2. {改善項目 2}
   ...

### 測試結果
✅ 所有測試持續通過

### 程式碼品質評估
- 可讀性：{評估}
- 維護性：{評估}
- 效能：{評估}

### 下一步
進入完整性驗證階段。
```

## 品質檢查

完成後請確認：
- ✅ 所有測試都通過
- ✅ 程式碼更容易理解
- ✅ 減少了重複
- ✅ 註解更完整清楚
- ✅ 已記錄改善內容到 memo.md

## 注意事項

**禁止的行為：**
- ❌ 改變功能行為
- ❌ 新增功能
- ❌ 一次改太多
- ❌ 不執行測試就繼續改

**允許的行為：**
- ✅ 改善命名
- ✅ 提取函式
- ✅ 提取常數
- ✅ 增加註解
- ✅ 簡化邏輯

## 下一步

Refactor 階段完成後，執行：
```
/tf-verify
```
驗證開發完整性。
