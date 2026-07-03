# 專案戰情室 (Project State)

## 📌 當前狀態
- **專案**: 台灣遺產稅・特留分・保險傳承規劃系統
- **環境**: Streamlit Community Cloud + GitHub
- **進度**: v1.1 上線。四階段確定性演算法完成，通過純函式單元測試 + Streamlit AppTest headless。

## 🆕 v1.1 更新（2026-07-03）：完整繼承順位
- 民法繼承由「僅第一順位」擴充為**完整四順位＋配偶並存（§1144）**：
  - 無子女 → 配偶＋父母（配偶 ½）；再無父母 → 配偶＋兄弟姊妹（配偶 ½）；再無 → 配偶＋祖父母（配偶 ⅔）。
- 特留分分數（§1223）：卑親屬/父母/配偶＝應繼分 ½；兄弟姊妹/祖父母＝應繼分 ⅓。
- 遺產稅新增**父母（直系尊親屬）扣除額 138 萬/人**（上限 2 人）。
- 重組家庭：新增「前段婚姻子女數」欄位＋專屬診斷（前婚子女與現任子女法律平等，須靠策略Ｂ補償）。
- `calc_reserved_portion` → 改寫為 `calc_inheritance`；驗證：11 項純函式測試＋7 情境 AppTest 全綠。

## 🧱 資料架構（SSOT）
- 無持久化、無外部 I/O。所有 2026 稅法/民法數字集中於 `app.py` 頂端「法定常數區塊」
  （`FUNERAL_DEDUCTION`、`BASIC_EXEMPTION`、`SPOUSE_DEDUCTION`、`CHILD_DEDUCTION`、
  `TAX_BRACKET_1/2`、`PROGRESSIVE_DIFF_15/20`、`ANNUAL_GIFT_EXEMPTION`、
  `HIGH_ASSET_ALERT`、`HIGH_AGE_ALERT`）→ 稅法參數的**單一真實來源**，修法只改此區塊。
- 資料流：輸入 → 純函式 → UI 單向流；時間/空間複雜度 O(1)。

## 🧠 記憶點 (Memory Checkpoint) — 2026-07-03（v1.1）
- **里程碑**: v1.0 遷入獨立 `Tawian_law` repo → v1.1 補完整四順位繼承＋配偶並存＋父母扣除額。
- **核心函式**: `calc_net_estate` / `calc_estate_tax(含 num_parents)` / `calc_inheritance` / `calc_insurance_plan`。
- **驗證**: 11 項純函式單元測試 + 7 情境 AppTest headless 全綠（四順位、配偶並存比例、特留分分數、除零、歸國庫）。
- **下一步接手點**: ①農地/公設地扣除額 ②配偶剩餘財產差額分配請求權 ③代位繼承。
  修改前務必先讀 `app.py` 法定常數區塊（SSOT）。

## 🛠️ 檔案結構
- `app.py`: Streamlit 主程式＋四階段演算法（純函式與 UI 解耦）。
- `requirements.txt`: 相依套件（streamlit）。
- `.streamlit/config.toml`: 主題設定。
- `README.md`: 專案說明＋部署指引。
- `STATE.md`: 專案熱資料與進度（本檔）。

## 🐞 待辦與已知 Bug
- [x] 擴充第二/三/四順位繼承人邏輯＋配偶並存比例（v1.1 完成）。
- [ ] 農地作農業使用扣除額、公共設施保留地扣除額。
- [ ] 配偶剩餘財產差額分配請求權（婚後財產差額 ÷ 2 先自遺產扣除）。
- [ ] 代位繼承（子女先於被繼承人死亡，由孫輩代位）。
