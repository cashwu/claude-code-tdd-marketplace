# Test-First TDD Plugin

結構化的測試優先開發方法，強調完整的規劃、文檔和驗證流程。

## 理念

這個 plugin 實現了有計劃、有文檔的 TDD 方法：

- **從需求開始**：先整理清楚的需求文件，明確功能目標
- **完整規劃**：預先規劃所有測試案例，包含正常、異常和邊界情況
- **詳細文檔**：每個階段都有完整的繁體中文註解和文檔記錄
- **驗證完整性**：確保所有規劃的測試都已實作並通過
- **可追溯性**：從需求到測試到實作，全程可追溯

## 核心原則

### 1. 計劃驅動
先規劃後執行，確保開發的完整性和系統性。

### 2. 文檔完整
- **requirements.md**：詳細的需求規格
- **testcases.md**：完整的測試案例清單
- **memo.md**：開發過程記錄
- **verification-report.md**：完整性驗證報告

### 3. 繁體中文註解
所有測試和實作程式碼都必須包含完整的繁體中文註解，提高可讀性和維護性。

### 4. 品質保證
通過完整性驗證確保：
- 所有測試案例都已實作
- 所有測試都通過
- 所有需求都有對應的測試

## 命令

### `/tf-requirements` - 需求定義
整理功能需求，建立結構化的需求文件。

**輸出**：
- `docs/tdd/{feature_name}/requirements.md`

**內容**：
- 功能概要和目標
- 輸入輸出規格
- 限制條件
- 使用範例

### `/tf-testcases` - 測試案例規劃
根據需求規劃所有測試案例。

**輸出**：
- `docs/tdd/{feature_name}/testcases.md`

**分類**：
- 正常系統測試（基本功能）
- 異常系統測試（錯誤處理）
- 邊界值測試（極端情況）

### `/tf-red` - 建立測試（Red）
實作會失敗的測試，明確定義功能行為。

**特點**：
- Given-When-Then 結構
- 完整的繁體中文註解
- 更新 memo.md 記錄進度

### `/tf-green` - 實作功能（Green）
用最簡單的方式讓測試通過。

**原則**：
- 先求能動，再求完美
- 可以使用硬編碼或假實作
- 標註待改善項目

### `/tf-refactor` - 重構（Refactor）
在測試保護下改善程式碼品質。

**重點**：
- 消除重複（DRY 原則）
- 提取魔術數字為常數
- 改善命名和可讀性
- 小步改善，頻繁測試

### `/tf-verify` - 完整性驗證
驗證所有測試案例都已實作並通過。

**檢查**：
- 測試執行結果
- 測試案例實作率
- 需求覆蓋率
- 生成驗證報告

## 工作流程

```
/tf-requirements   ← 定義需求
    ↓
/tf-testcases      ← 規劃測試案例
    ↓
/tf-red            ← 建立測試 (Red)
    ↓
/tf-green          ← 實作功能 (Green)
    ↓
/tf-refactor       ← 重構 (Refactor)
    ↓
/tf-verify         ← 驗證完整性
    ↓
完成或返回補充
```

## 適用場景

### 適合使用 Test-First TDD 的情況

✅ **團隊協作**
- 需要清楚的文檔供團隊成員參考
- 多人協作開發
- 知識傳承

✅ **需求明確**
- 需求規格清楚
- 可以預先規劃測試案例
- 功能範圍明確

✅ **品質要求高**
- 需要完整的測試覆蓋
- 需要可追溯性
- 需要驗證完整性

✅ **合規需求**
- 需要文檔化開發過程
- 需要證明測試覆蓋率
- 需要稽核追蹤

### 不太適合的情況

❌ **探索性開發**：需求還不明確，需要通過開發來探索

❌ **快速原型**：只是驗證想法，不需要完整文檔

❌ **個人快速開發**：不需要正式文檔，Kent Beck TDD 可能更適合

## 生成的文檔

在 `docs/tdd/{feature_name}/` 目錄下：

- **requirements.md**：功能需求規格
- **testcases.md**：測試案例清單
- **memo.md**：開發過程記錄
  - Red 階段進度
  - Green 階段實作
  - Refactor 階段改善
  - 完整性驗證結果
- **verification-report.md**：完整性驗證報告

## 註解規範

### 測試程式碼
```javascript
// 【測試目的】：說明此測試要驗證什麼
// 【測試內容】：具體測試的處理
// 【預期行為】：正常情況下的結果

// 【測試資料準備 Given】：準備測試所需的資料
const input = testData;

// 【執行測試 When】：呼叫要測試的功能
const result = functionToTest(input);

// 【驗證結果 Then】：確認結果符合預期
expect(result).toBe(expectedValue); // 【確認】：數值正確
```

### 實作程式碼
```javascript
/**
 * 【功能說明】：此函式的功能
 * 【實作方針】：為什麼這樣實作
 * 【對應測試】：為了通過哪個測試
 */
function functionName(param) {
  // 【輸入驗證】：檢查輸入是否有效
  // 【主要處理】：核心功能實作
  // 【回傳結果】：回傳處理結果
}
```

## 哲學

Test-First TDD 的核心是：

> "計劃周全，執行確實，驗證完整。"

這個方法強調：
- 📋 詳細規劃，減少返工
- 📝 完整文檔，便於維護
- ✅ 驗證完整性，確保品質
- 🔍 可追溯性，滿足合規
- 👥 團隊協作，知識共享

## 與 Kent Beck TDD 的比較

| 特性 | Test-First TDD | Kent Beck TDD |
|------|----------------|---------------|
| 起點 | 需求文件 | 第一個測試 |
| 規劃 | 預先規劃所有測試 | 逐步演進 |
| 文檔 | 詳細完整 | 簡單記錄 |
| 適合 | 團隊、明確需求 | 個人、探索開發 |
| 驗證 | 完整性驗證 | 主觀判斷 |

## 開始使用

```bash
/tf-requirements
```

輸入功能描述，開始結構化的 TDD 開發流程！

## 實際專案範例

想看看這個方法的實際應用嗎？

### 🎾 Tennis Kata 範例
**Repository**: [ai-tdd-test-first-tennis](https://github.com/cashwu/ai-tdd-test-first-tennis)

這是一個使用 Test-First TDD 方法開發的完整 Tennis Kata 實作，展示：

- ✅ **完整規劃**：詳細的 `requirements.md` 和 `testcases.md`
- ✅ **測試分類**：正常系統、異常系統、邊界值測試的完整規劃
- ✅ **中文註解**：所有測試和實作都有完整的繁體中文註解
- ✅ **開發記錄**：`memo.md` 記錄每個階段的進度和改善項目
- ✅ **完整性驗證**：包含驗證報告，確保所有功能都已實作

**適合學習**：
- 如何撰寫結構化的需求文件
- 如何預先規劃完整的測試案例
- 如何使用 Given-When-Then 結構
- 如何撰寫清楚的中文註解
- 如何驗證開發的完整性

**文檔結構**：
```
docs/tdd/tennis-scoring/
├── requirements.md         # 完整的需求規格
├── testcases.md           # 預先規劃的測試案例
├── memo.md                # 開發過程記錄
└── verification-report.md # 完整性驗證報告
```

這個專案是按照本 plugin 的方法論開發的實際範例，可以直接參考！

## 參考資料

- TDD 核心概念：Red-Green-Refactor 循環
- 測試分類：正常、異常、邊界值
- DRY 原則：Don't Repeat Yourself
- Given-When-Then 測試結構

## 致謝

本 plugin 的開發方法和文檔結構參考並修改自 [classmethod/tsumiki](https://github.com/classmethod/tsumiki) 專案。

特別感謝 classmethod 團隊提供的優秀 TDD 方法論和實踐經驗。

## 授權

MIT License - 詳見 [LICENSE](../../LICENSE) 文件

Copyright (c) 2025 cash
