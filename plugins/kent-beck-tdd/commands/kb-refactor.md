---
description: 重構消除重複（Refactor 階段）。在綠燈的保護下，改善程式碼品質。
---

# TDD Refactor - 重構消除重複（Refactor）

在測試通過後，消除重複、改善設計。測試是你的安全網。

**【功能名】**：{{feature_name}}

## Kent Beck 的 Refactor 理念

> "重構的核心是消除重複。"
> "測試通過後，就是重構的時機。"

### 什麼是重複？

Kent Beck 定義的重複包括：

#### 1. 明顯的程式碼重複

```javascript
// ❌ 重複
function processOrderA(order) {
  if (!order) throw new Error('Invalid order');
  return order.total * 1.1;
}

function processOrderB(order) {
  if (!order) throw new Error('Invalid order');
  return order.total * 0.9;
}

// ✅ 消除重複
function validateOrder(order) {
  if (!order) throw new Error('Invalid order');
}

function processOrderA(order) {
  validateOrder(order);
  return order.total * 1.1;
}
```

#### 2. 測試和實作之間的重複

```javascript
// 測試
test('5 * 2 = 10', () => {
  expect(multiply(5, 2)).toBe(10);
});

// ❌ 實作中的重複（硬編碼）
function multiply(a, b) {
  return 10; // 和測試中的 10 重複！
}

// ✅ 消除重複（真正的實作）
function multiply(a, b) {
  return a * b;
}
```

#### 3. 概念上的重複

```javascript
// ❌ 概念重複（魔術數字）
function applyDiscount(price) {
  return price * 0.9;
}

function calculateTax(price) {
  return price * 0.1;
}

// ✅ 消除重複（提取常數）
const DISCOUNT_RATE = 0.9;
const TAX_RATE = 0.1;

function applyDiscount(price) {
  return price * DISCOUNT_RATE;
}

function calculateTax(price) {
  return price * TAX_RATE;
}
```

## 重構步驟

### 1. 確認測試通過

```bash
npm test
# 必須是綠燈！🟢
```

**如果是紅燈**：
- 不要重構
- 先讓測試通過

### 2. 識別重複或異味

問自己：
- 有重複的程式碼嗎？
- 有硬編碼的值嗎？
- 有不清楚的命名嗎？
- 有太長的函式嗎？
- 有不必要的複雜度嗎？

### 3. 小步重構

**一次只改一個地方**

```
改一小步 → 執行測試 → 通過
    ↓
改下一步 → 執行測試 → 通過
    ↓
...
```

### 4. 每次都執行測試

**頻率**：每改一個地方就測試一次

```bash
# 改善命名
npm test

# 提取函式
npm test

# 提取常數
npm test
```

### 5. 如果測試失敗，立即回退

```bash
# 紅燈！
git checkout .  # 回退
# 或手動 Undo
```

## 常見重構技巧

### 技巧 1：消除硬編碼

**Before**
```javascript
class Dollar {
  times(multiplier) {
    return new Dollar(10); // 硬編碼
  }
}
```

**After**
```javascript
class Dollar {
  times(multiplier) {
    return new Dollar(this.amount * multiplier);
  }
}
```

### 技巧 2：提取函式

**Before**
```javascript
function processOrder(order) {
  // 驗證
  if (!order.items || order.items.length === 0) {
    throw new Error('No items');
  }

  // 計算
  let total = 0;
  for (const item of order.items) {
    total += item.price * item.quantity;
  }

  return total;
}
```

**After**
```javascript
function processOrder(order) {
  validateOrder(order);
  return calculateTotal(order.items);
}

function validateOrder(order) {
  if (!order.items || order.items.length === 0) {
    throw new Error('No items');
  }
}

function calculateTotal(items) {
  let total = 0;
  for (const item of items) {
    total += item.price * item.quantity;
  }
  return total;
}
```

### 技巧 3：提取常數

**Before**
```javascript
function calculateShipping(weight) {
  if (weight > 1000) return 100;
  return 50;
}
```

**After**
```javascript
const HEAVY_WEIGHT_THRESHOLD = 1000;
const SHIPPING_HEAVY = 100;
const SHIPPING_STANDARD = 50;

function calculateShipping(weight) {
  if (weight > HEAVY_WEIGHT_THRESHOLD) {
    return SHIPPING_HEAVY;
  }
  return SHIPPING_STANDARD;
}
```

### 技巧 4：改善命名

**Before**
```javascript
function calc(x) {
  return x * 0.9;
}
```

**After**
```javascript
function applyDiscount(price) {
  const DISCOUNT_RATE = 0.9;
  return price * DISCOUNT_RATE;
}
```

### 技巧 5：簡化條件

**Before**
```javascript
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
```

**After**
```javascript
function canPurchase(user, product) {
  return user.age >= 18
    && user.balance >= product.price
    && product.stock > 0;
}
```

## 何時停止重構？

Kent Beck 的判斷標準：

### 停止的信號

✅ **可以停止了**：
- 沒有明顯的重複
- 程式碼清楚易懂
- 命名恰當
- 函式簡短

✅ **也可以停止**：
- 想不到明顯的改善
- 繼續重構的收益不大
- 想寫下一個測試了

### 不要過度重構

```javascript
// ✅ 夠好了
function calculateTotal(items) {
  let total = 0;
  for (const item of items) {
    total += item.price * item.quantity;
  }
  return total;
}

// ❌ 過度了（除非真的需要）
class TotalCalculator {
  constructor(strategy) {
    this.strategy = strategy;
  }

  calculate(items) {
    return this.strategy.compute(items);
  }
}

class StandardCalculationStrategy {
  compute(items) {
    return items.reduce((sum, item) =>
      sum + this.calculateItemTotal(item), 0);
  }

  calculateItemTotal(item) {
    return new Money(item.price)
      .multiply(item.quantity)
      .getAmount();
  }
}
// ... 太複雜了！
```

## 重構的節奏

Kent Beck 建議的節奏：

```
快速寫測試（幾分鐘）
    ↓
快速讓測試通過（幾分鐘）
    ↓
快速重構（幾分鐘）← 現在在這裡
    ↓
回到寫測試
```

**每個階段都要快**
- 不要花太久在重構
- 幾分鐘就好
- 保持節奏

## 更新 journey.md

```markdown
#### 🔵 Refactor - 重構

重構內容：
1. {改善項目 1}
   - 重構前：{簡述}
   - 重構後：{簡述}
   - 原因：{為什麼}

2. {改善項目 2}
   ...

測試狀態：✅ 持續通過

#### 📝 下一步
執行 /kb-review 回顧下一步
```

## 範例：Money 的重構

### 重構前

```javascript
class Dollar {
  constructor(amount) {
    this.amount = amount;
  }

  times(multiplier) {
    return new Dollar(this.amount * multiplier);
  }
}

class Franc {
  constructor(amount) {
    this.amount = amount;
  }

  times(multiplier) {
    return new Franc(this.amount * multiplier);
  }
}
```

**發現重複**：Dollar 和 Franc 幾乎一樣！

### 重構步驟

**Step 1：提取父類別**
```javascript
class Money {
  constructor(amount) {
    this.amount = amount;
  }
}

class Dollar extends Money {
  times(multiplier) {
    return new Dollar(this.amount * multiplier);
  }
}

class Franc extends Money {
  times(multiplier) {
    return new Franc(this.amount * multiplier);
  }
}
```

測試：✅ 通過

**Step 2：將 times 提升到父類別**
```javascript
class Money {
  constructor(amount) {
    this.amount = amount;
  }

  times(multiplier) {
    return new Money(this.amount * multiplier);
  }
}

class Dollar extends Money {}
class Franc extends Money {}
```

測試：✅ 通過

**消除了重複！**

## 重構的勇氣來自哪裡？

Kent Beck：**測試給你重構的勇氣**

```
沒有測試：
😰 不敢改程式碼
😰 怕改壞東西
😰 程式碼越來越爛

有測試：
😎 隨時可以重構
😎 測試會告訴你有沒有改壞
😎 程式碼越來越好
```

## 下一步

重構完成？執行：
```
/kb-review
```

回顧這一輪，計劃下一步！

## 記住 Kent Beck 的話

> "重構的時機是在測試通過之後。"
> "重複是設計的敵人。"
> "小步重構，頻繁測試，永遠保持綠燈。"
> "測試讓你有勇氣重構，重構讓程式碼保持清潔。"
