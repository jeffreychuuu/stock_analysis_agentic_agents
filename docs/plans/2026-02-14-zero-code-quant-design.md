# 2026-02-14 - 零代碼 (Zero-code) 投資組合與風險引擎設計文件

## 1. 核心理念
本專案已轉化為完全的 **「零代碼感 (Zero-code)」** 結構。專案倉庫中不再保留任何永久性的 `.py` 檔案。所有的量化運算（回測、相關性分析、風險模型）均由 AI 代理人透過動態執行（Dynamic Scripting）完成。

## 2. 系統架構
- **數據源**：使用 Yahoo Finance MCP (`yfmcp`) 獲取原始市場數據。
- **運算引擎 (動態)**：AI 代理人在需要時，利用 `Bash` 執行臨時 Python 指令進行統計運算。
- **邏輯定義**：所有的策略邏輯、風險門檻與配置原則，均寫在 `.claude/agents/` 的 Markdown 文件中。

## 3. 關鍵組件與職責

### 3.1 投資組合分析師 (Portfolio Analyst)
- **職責**：分析多資產間的相關性。
- **技術路徑**：獲取多個 Ticker 的 `history`，動態計算相關矩陣與風險平權比例。
- **輸出**：相關性矩陣表格與配置百分比。

### 3.2 策略回測與進階風險分析師 (Backtest & Risk Analyst)
- **職責**：驗證策略績效與預測未來風險。
- **量化指標**：
  - **真實回測**：包含手續費 (Commission) 與滑價 (Slippage)。
  - **VaR/CVaR (99%)**：衡量極端市場下的最大損失。
  - **蒙地卡羅模擬**：10,000 次路徑生成，預估獲利機率 (PoP)。

### 3.3 風險管理團隊 (Risk Managers)
- **職責**：基於 VaR 與蒙地卡羅數據進行跨立場辯論。
- **模式**：保守、中立、積極三方對抗，最終由用戶核准。

## 4. 工作流 (Workflow)
1. **Phase 1 (Research)**: 加入 `portfolio-analyst` 提供多資產視角。
2. **Phase 3 (Strategy & Quant)**: 交易員出策略後，立即執行「動態風險掃描」。
3. **Phase 5 (Decision)**: 決策報告整合所有量化風險指標。

## 5. 變更記錄
- 刪除 `lib/` 資料夾下的所有 Python 引擎。
- 更新 `.claude/skills/multi-agent-trading-analysis/SKILL.md` 以支持動態量化。
- 更新所有分析師 Agent 的 Markdown 說明，強調「零代碼」運作模式。
