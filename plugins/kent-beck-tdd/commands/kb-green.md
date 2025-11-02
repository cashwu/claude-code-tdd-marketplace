---
description: 讓測試通過（Green 階段）。用最簡單、甚至是"錯誤"的方式快速讓測試變綠。
---

# TDD Green - 讓測試通過（Green）

用最簡單的方式讓測試通過。可以作弊、可以硬編碼、可以"錯誤"。

**【功能名】**：{{feature_name}}

## Kent Beck 的 Green 階段理念

> "快速讓測試通過，用任何手段。"
> "乾淨的程式碼是目標，但先讓它能動。"

### 三種讓測試通過的策略

Kent Beck 在書中提出的三種策略：

#### 策略 1：Fake It（假實作）

**最快速的方式：回傳常數**

```javascript
// 測試
test('5 * 2 = 10', () => {
  expect(multiply(5, 2)).toBe(10);
});

// 假實作：直接回傳 10！
function multiply(a, b) {
  return 10;
}
```

**為什麼這樣做？**
- 快速得到綠燈
- 心理上的安全感
- 用下一個測試來逼出真正的實作

#### 策略 2：Obvious Implementation（明顯實作）

**當實作很明顯時，直接寫出來**

```javascript
// 測試
test('加法運算', () => {
  expect(add(2, 3)).toBe(5);
});

// 明顯實作：加法很簡單
function add(a, b) {
  return a + b;
}
```

**何時使用？**
- 實作非常簡單
- 你很有信心
- 不需要三角測量

#### 策略 3：Triangulation（三角測量）

**用多個測試來推導正確實作**

```javascript
// 第 1 個測試
test('1 + 1 = 2', () => {
  expect(add(1, 1)).toBe(2);
});

// 假實作
function add(a, b) {
  return 2;
}

// 第 2 個測試（三角測量）
test('2 + 3 = 5', () => {
  expect(add(2, 3)).toBe(5);
});

// 被逼出真正的實作
function add(a, b) {
  return a + b;
}
```

**何時使用？**
- 不確定正確的實作方式
- 想從多個角度驗證
- 設計還不清楚

## 實作步驟

### 1. 選擇策略

根據你的信心程度：

```
信心很低 → Fake It（假實作）
信心中等 → Triangulation（三角測量）
信心很高 → Obvious Implementation（明顯實作）
```

### 2. 寫最少的程式碼

**重點：最少**

```javascript
// 測試需要一個類別
test('Dollar 乘法', () => {
  const five = new Dollar(5);
  const product = five.times(2);
  expect(product.amount).toBe(10);
});

// 最少的實作
class Dollar {
  constructor(amount) {
    this.amount = amount;
  }

  times(multiplier) {
    return new Dollar(10); // 假實作！
  }
}
```

### 3. 執行測試

```bash
npm test
```

**看到綠燈！** 🟢

### 4. 更新 journey.md

```markdown
#### 🟢 Green - 讓測試通過

策略：{Fake It / Obvious / Triangulation}

實作說明：
{簡述你做了什麼}

程式碼位置：{檔案路徑}

測試結果：✅ 通過

#### 🤔 反思
- 這個實作明顯是假的/硬編碼的嗎？
- 需要下一個測試來逼出真正的實作嗎？
- 有沒有重複的程式碼？

#### 📝 下一步
執行 /kb-refactor 重構
```

## Fake It 的威力

### 範例：Money 的演進

**第 1 輪：完全假實作**

```javascript
test('5 * 2 = 10', () => {
  const five = new Dollar(5);
  expect(five.times(2).amount).toBe(10);
});

class Dollar {
  constructor(amount) {
    this.amount = amount;
  }
  times(multiplier) {
    return new Dollar(10); // 硬編碼！
  }
}
```

**第 2 輪：三角測量逼出真實作**

```javascript
test('5 * 3 = 15', () => {
  const five = new Dollar(5);
  expect(five.times(3).amount).toBe(15);
});

class Dollar {
  times(multiplier) {
    return new Dollar(this.amount * multiplier); // 真實作！
  }
}
```

## 常見的"作弊"技巧

### 技巧 1：回傳常數

```javascript
function getWelcomeMessage() {
  return "Hello, World!"; // 先硬編碼
}
```

### 技巧 2：複製測試資料

```javascript
// 測試
expect(processData(input)).toEqual({
  status: 'success',
  count: 5
});

// 實作：直接回傳期待值
function processData(input) {
  return { status: 'success', count: 5 };
}
```

### 技巧 3：最簡單的 if

```javascript
// 測試 1
test('even number', () => {
  expect(classify(2)).toBe('even');
});

// 假實作
function classify(n) {
  if (n === 2) return 'even';
}

// 測試 2 會逼出真實作
test('another even number', () => {
  expect(classify(4)).toBe('even');
});

function classify(n) {
  return n % 2 === 0 ? 'even' : 'odd';
}
```

## 實作的禁忌

Kent Beck 說明在 Green 階段應該避免的：

### ❌ 不要過度設計

```javascript
// ❌ 不要這樣（太複雜）
class Dollar {
  constructor(amount, currency = 'USD') {
    this.amount = amount;
    this.currency = currency;
  }

  times(multiplier) {
    // 處理多幣別、匯率轉換...
  }

  // 一堆還不需要的方法
  add() {}
  subtract() {}
  convert() {}
}

// ✅ 要這樣（夠用就好）
class Dollar {
  constructor(amount) {
    this.amount = amount;
  }

  times(multiplier) {
    return new Dollar(this.amount * multiplier);
  }
}
```

### ❌ 不要一次實作多個測試

```javascript
// 目前只有一個測試需要 times()
// 不要順便實作 add(), subtract()
// 等有測試需要時再加
```

### ❌ 不要重構

```javascript
// ❌ Green 階段不要重構
// 先讓測試通過
// 重構留給下一步

// ✅ 先這樣
function calculate(x) {
  return x * 2;
}

// 不要在這裡就改成
const MULTIPLIER = 2;
function calculate(x) {
  return x * MULTIPLIER;
}
// 重構留給 /kb-refactor
```

## 心態：接受"醜陋"的程式碼

Kent Beck 強調的心態轉變：

```
階段 1：讓它能動 (Make it work) ← 現在在這裡
    ↓
階段 2：讓它正確 (Make it right)
    ↓
階段 3：讓它快速 (Make it fast)
```

**Green 階段只追求"能動"**
- 硬編碼？沒關係
- 重複？沒關係
- 醜陋？沒關係

**下一步會改善**

## 範例完整流程

### 測試（失敗）

```javascript
test('購物車初始總價為 0', () => {
  const cart = new ShoppingCart();
  expect(cart.getTotal()).toBe(0);
});
```

### 實作策略選擇

**信心評估**：這個很簡單，用 Obvious Implementation

### 實作程式碼

```javascript
// src/shopping-cart.js

class ShoppingCart {
  getTotal() {
    return 0; // 最簡單的實作
  }
}

module.exports = ShoppingCart;
```

### 執行測試

```bash
npm test
# ✅ 通過！
```

### 記錄到 journey.md

```markdown
#### 🟢 Green - 讓測試通過

策略：Obvious Implementation

實作說明：
建立 ShoppingCart 類別，getTotal() 回傳 0。
雖然是硬編碼，但足以通過目前的測試。

程式碼位置：src/shopping-cart.js

測試結果：✅ 通過

#### 🤔 反思
- 實作很簡單，是硬編碼
- 下一個測試：加入商品後總價應該改變
- 目前沒有明顯重複

#### 📝 下一步
執行 /kb-refactor 檢查是否需要重構
```

## 速度的重要性

Kent Beck：**Green 階段要快**

**目標**：
- 幾秒鐘到幾分鐘
- 不要花太久時間
- 快速得到綠燈的心理回饋

**如果卡住**：
- 寫更小的測試
- 用更假的實作
- 回退重來

## 下一步

測試通過了？執行：
```
/kb-refactor
```

檢查是否有重複，進行重構！

## 記住 Kent Beck 的話

> "讓測試通過的技巧是暫時降低對程式碼品質的標準。"
> "骯髒的程式碼是通往乾淨程式碼的墊腳石。"
> "當你看到綠燈時，那是重構的信號。"
