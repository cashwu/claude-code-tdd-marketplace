# 安裝指南

這份文件說明如何安裝和使用 TDD Methodologies Marketplace 中的 plugins。

## 前置要求

- 已安裝 Claude Code
- 對 TDD（測試驅動開發）有基本了解

## 安裝步驟

### 步驟 1：加入 Marketplace

在 Claude Code 中執行以下命令，將 TDD Methodologies marketplace 加入到您的環境：

**方法 1：從 GitHub 安裝（推薦）**
```bash
/plugin marketplace add cashwu/claude-code-tdd-marketplace
```

**方法 2：從本地安裝**
```bash
/plugin marketplace add /Users/cash/Downloads/tdd/marketplace
```

**注意：** 如果使用本地安裝，請將路徑替換為您實際存放 marketplace 的絕對路徑。

### 步驟 2：瀏覽可用的 Plugins

```bash
/plugin
```

選擇「Browse Plugins」查看可用的 plugins。

您應該會看到：
- **kent-beck-tdd** - Kent Beck's organic TDD
- **test-first-tdd** - Structured Test-First TDD

### 步驟 3：安裝 Plugin

您可以選擇安裝其中一個或兩個 plugins：

**安裝 Kent Beck TDD：**
```bash
/plugin install kent-beck-tdd
```

**安裝 Test-First TDD：**
```bash
/plugin install test-first-tdd
```

**或者兩個都安裝（推薦）：**
```bash
/plugin install kent-beck-tdd
/plugin install test-first-tdd
```

### 步驟 4：驗證安裝

安裝後，您應該能夠使用以下命令：

**Kent Beck TDD 命令：**
- `/kb-start`
- `/kb-red`
- `/kb-green`
- `/kb-refactor`
- `/kb-review`

**Test-First TDD 命令：**
- `/tf-requirements`
- `/tf-testcases`
- `/tf-red`
- `/tf-green`
- `/tf-refactor`
- `/tf-verify`

在 Claude Code 中輸入 `/` 應該會看到這些命令出現在自動完成列表中。

## 快速開始

### 使用 Kent Beck TDD

```bash
/kb-start
```

然後按照提示：
1. 輸入功能想法
2. 想第一個最簡單的測試
3. 開始 Red-Green-Refactor 循環

### 使用 Test-First TDD

```bash
/tf-requirements
```

然後按照流程：
1. 整理需求規格
2. 規劃測試案例 (`/tf-testcases`)
3. 建立測試 (`/tf-red`)
4. 實作功能 (`/tf-green`)
5. 重構改善 (`/tf-refactor`)
6. 驗證完整性 (`/tf-verify`)

## 團隊設置

如果您想讓整個團隊使用這些 plugins，可以將 marketplace 設定加入專案的 `.claude/settings.json`：

**使用 GitHub Repo（推薦）：**
```json
{
  "marketplaces": [
    "cashwu/claude-code-tdd-marketplace"
  ],
  "plugins": [
    "kent-beck-tdd",
    "test-first-tdd"
  ]
}
```

**使用本地路徑：**
```json
{
  "marketplaces": [
    "/path/to/tdd/marketplace"
  ],
  "plugins": [
    "kent-beck-tdd",
    "test-first-tdd"
  ]
}
```

團隊成員在信任該專案後，plugins 會自動安裝。

## 卸載

如果需要卸載 plugins：

```bash
/plugin uninstall kent-beck-tdd
/plugin uninstall test-first-tdd
```

如果需要移除 marketplace：

```bash
/plugin marketplace remove tdd-methodologies
```

## 故障排除

### 問題：找不到 plugins

**解決方法：**
1. 確認 marketplace manifest 的路徑正確
2. 檢查 `manifests/tdd-plugins.json` 中的 plugin 路徑是否正確
3. 確認所有必要的文件都存在

### 問題：命令沒有出現

**解決方法：**
1. 重新啟動 Claude Code
2. 檢查 plugin 是否安裝成功 (`/plugin` 查看已安裝的 plugins)
3. 確認命令文件存在於 `commands/` 目錄中

### 問題：執行命令時出錯

**解決方法：**
1. 檢查命令文件的 frontmatter（`---` 之間的 description）是否正確
2. 確認命令文件的語法正確
3. 查看 Claude Code 的錯誤訊息

## 更新

如果 plugins 有更新：

1. 更新文件內容
2. 如果版本號有變更，更新 `plugin.json` 中的 version
3. 重新安裝 plugin：
   ```bash
   /plugin uninstall kent-beck-tdd
   /plugin install kent-beck-tdd
   ```

## 實際專案範例

想看實際使用這些方法開發的專案嗎？以下是使用這兩種方法開發的 Tennis Kata 範例：

### 🎾 Kent Beck TDD 範例
[ai-tdd-tennis](https://github.com/cashwu/ai-tdd-tennis) - 展示有機演進的 TDD 開發過程

### 🎾 Test-First TDD 範例
[ai-tdd-test-first-tennis](https://github.com/cashwu/ai-tdd-test-first-tennis) - 展示結構化的 TDD 開發流程

兩個專案都實作相同的 Tennis Kata，可以直接比較兩種方法的差異！

## 獲取幫助

- 查看 [marketplace README](./README.md) 了解兩種方法的比較和實際範例
- 查看各 plugin 的 README：
  - [Kent Beck TDD README](./plugins/kent-beck-tdd/README.md)
  - [Test-First TDD README](./plugins/test-first-tdd/README.md)
- 查看命令文件了解詳細用法
- 參考上述的實際專案範例了解如何應用

## 目錄結構參考

```
marketplace/
├── .claude-plugin/
│   └── marketplace.json  # Marketplace manifest (必要)
├── README.md              # Marketplace 總覽
├── INSTALL.md            # 本安裝指南
├── LICENSE               # MIT License
└── plugins/
    ├── kent-beck-tdd/
    │   ├── .claude-plugin/
    │   │   └── plugin.json
    │   ├── commands/      # 5 個命令
    │   └── README.md
    └── test-first-tdd/
        ├── .claude-plugin/
        │   └── plugin.json
        ├── commands/      # 6 個命令
        └── README.md
```

**重要文件說明：**
- `.claude-plugin/marketplace.json` - Marketplace 的主要配置文件（必要）
- `.claude-plugin/plugin.json` - 每個 plugin 的配置文件（必要）
- `commands/` - 包含各個命令的 markdown 文件

---

## 授權

本專案採用 MIT License 授權，您可以自由使用、修改和分發。

詳細授權條款請參見 [LICENSE](./LICENSE) 文件。

---

祝您 TDD 開發順利！🚀
