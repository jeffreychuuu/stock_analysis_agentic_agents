---
name: multi-agent-trading-analysis
description: 執行包含全面人類審核 (Human-in-the-Loop) 的多代理人股票分析流程。每個階段均需用戶核准內容後方可進入下一階段。
---

# 多代理人交易分析 (全面核准工作流)

## 概述

本技能引導一個專業等級的股票分析流程。核心特點在於「用戶主導的內容篩選與流程控制」。每個階段的代理人輸出都必須經過用戶審核，用戶決定哪些觀點被採納、哪些被剔除，或是否需要重新進行研究/辯論。

## 嚴格命名協議 (Strict Naming Protocol)

**重要：** 為了確保 `TeamCreate` 與 `Task` 工具的穩定性，必須嚴格遵守以下命名規範：

- **團隊名稱**：必須與定義完全一致，僅限小寫字母與連字號（e.g., `debate-team`）。
- **代理人名稱**：必須使用其定義的文件名（不含擴展名），且與團隊定義中的名稱完全匹配（e.g., `bullish-analyst`）。
- **大小寫敏感**：所有工具調用中的名稱均為大小寫敏感。

## 執行流程

### 團隊組成 (Runtime Team Mapping)

| 團隊名稱 (Team Name) | 負責人 (Team Lead)   | 團隊成員 (Members)                                                                                   |
| :------------------- | :------------------- | :--------------------------------------------------------------------------------------------------- |
| `debate-team`        | `debate-moderator`   | `bullish-analyst`, `bearish-analyst`, `debate-moderator`                                             |
| `risk-manager-team`  | `chief-risk-officer` | `aggressive-risk-manager`, `neutral-risk-manager`, `conservative-risk-manager`, `chief-risk-officer` |

### 第一階段：專家研究與用戶確認

1. **數據獲取**：必須使用 Yahoo Finance MCP 工具並行獲取財務、新聞及價格歷史數據。
2. **專家報告**：調用 `fundamental-analyst`, `technical-analyst`, 及 `sentiment-analyst` 生成報告。
3. **用戶審核 (關鍵互動點)**：呈現三份報告。
   - **用戶操作**：調用 `AskUserQuestion` 詢問用戶，決定是否接受報告、指定哪些部分不合理需剔除、或要求針對特定項目重跑。
   - **輸出**：僅保留「用戶確認核准」的內容進入下一階段。

### 第二階段：牛熊立場辯論

本階段必須使用 `TeamCreate` 工具建立 **debate-team** 並進行協作，完成後必須使用 `TeamDelete` 刪除 **debate-team**。

1. **建立團隊**：使用 `TeamCreate` 建立 `debate-team`，指定 `debate-moderator` 為 Team Lead。
2. **初始化辯論**：將「用戶確認核准」的專家報告透過 `TaskCreate` 與 `TaskUpdate` 分配給 `bullish-analyst` 與 `bearish-analyst`。
3. **執行辯論**：Team Lead (`debate-moderator`) 必須使用 `SendMessage` 引導兩位分析師進行攻防，分析師僅能基於已核准的內容進行發言。
4. **用戶辯論審核 (關鍵互動點)**：展示由 `debate-moderator` 整理的衝突地圖報告。
5. **強制核准程序**：**必須** 調用 `AskUserQuestion` 詢問用戶：「是否核准此辯論結果進入下一階段？」。
   - **用戶操作**：決定是否接受辯論結果、篩選需要保留的攻防點、或要求重新進行更深層次的辯論。
   - **輸出**：產生「用戶確認核准」的辯論總結。未獲核准前嚴禁啟動第三階段。
6. **清理團隊**：辯論結束並獲得核准後，**必須**使用 `TeamDelete` 刪除 `debate-team`。

### 第三階段：交易策略自動制定

1. **策略生成**：調用 `trader` 代理人。
2. **輸入限制**：`trader` 僅能接收第一階段核准的報告內容及第二階段核准的辯論總結。
3. **策略審核 (關鍵互動點)**：展示交易策略建議。
4. **強制核准程序**：**必須** 調用 `AskUserQuestion` 詢問用戶：「是否採納此交易策略？」。
   - **用戶操作**：審核入場/目標/停損點位，決定是否採納、修改建議、或要求 `trader` 根據新指示重新構思。

### 第四階段：風險管理團隊辯論

本階段必須使用 `TeamCreate` 工具建立 **risk-manager-team** 並進行協作，完成後必須使用 `TeamDelete` 刪除 **risk-manager-team**。

1. **建立團隊**：使用 `TeamCreate` 建立 `risk-manager-team`，指定 `chief-risk-officer` 為 Team Lead。
2. **團隊介入**：調用 `aggressive-risk-manager`, `neutral-risk-manager`, `conservative-risk-manager` 加入團隊。
3. **輸入限制**：風險團隊僅能基於「用戶確認核准」的所有前序內容進行評估。
4. **分輪辯論與審核 (關鍵互動點)**：
   - 每一輪辯論由 `chief-risk-officer` 主持並透過 `SendMessage` 協調各方，必須向用戶展示各方的核心觀點及風險評級。
   - **強制核准程序**：每一輪結束後，**必須** 調用 `AskUserQuestion` 詢問用戶：「是否核准此輪風險評估內容？」。
   - **用戶操作**：調用 `AskUserQuestion` 詢問用戶，針對每一輪內容決定是否接受、剔除不合理觀點、或要求繼續下一輪/重跑本輪。
   - **輸出**：產生最終「用戶確認核准」的風險攻防紀錄。
5. **清理團隊**：風險評估結束並獲得核准後，**必須**使用 `TeamDelete` 刪除 `risk-manager-team`。

### 第五階段：自動決策與最終報告

1. **最終決策**：調用 `final-manager`。
2. **輸入限制**：`final-manager` 整合所有前序階段「用戶確認核准」的內容。
3. **任務**：自動做出 EXECUTION 或 REJECT 決策，並生成一份嚴格基於核准內容的專業投資報告。

## 核心準則

- **內容溯源**：嚴禁代理人引入未經用戶核准或專家報告中未提及的外部資訊。
- **強制停頓**：在所有「關鍵互動點」必須停止，等待用戶明確指示「接受、修改或重跑」後方可繼續。
- **一致性**：所有階段的輸出必須與前序階段用戶核准的內容保持嚴格一致。
- **語言**：所有輸出必須使用繁體中文。
