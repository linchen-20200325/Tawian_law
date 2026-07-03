# 專案戰情室 (Project State)

## 📌 當前狀態
- **專案**: 台灣遺產稅・特留分・保險傳承規劃系統
- **環境**: Streamlit Community Cloud + GitHub
- **進度**: v1.0 初始版本上線。四階段確定性演算法完成，通過純函式單元測試 + Streamlit AppTest headless。

## 🧱 資料架構（SSOT）
- 無持久化、無外部 I/O。所有 2026 稅法/民法數字集中於 `app.py` 頂端「法定常數區塊」
  （`FUNERAL_DEDUCTION`、`BASIC_EXEMPTION`、`SPOUSE_DEDUCTION`、`CHILD_DEDUCTION`、
  `TAX_BRACKET_1/2`、`PROGRESSIVE_DIFF_15/20`、`ANNUAL_GIFT_EXEMPTION`、
  `HIGH_ASSET_ALERT`、`HIGH_AGE_ALERT`）→ 稅法參數的**單一真實來源**，修法只改此區塊。
- 資料流：輸入 → 純函式 → UI 單向流；時間/空間複雜度 O(1)。

## 🧠 記憶點 (Memory Checkpoint) — 2026-07-03
- **里程碑**: 從 `my-english-learn` 遷入獨立 `tawian_law` repo，建立 v1.0 初始提交。
- **核心函式**: `calc_net_estate` / `calc_estate_tax` / `calc_reserved_portion` / `calc_insurance_plan`。
- **驗證**: 9 項純函式單元測試 + AppTest headless 全綠（級距交界稅額連續、除零、負值、超額防禦）。
- **下一步接手點**: ①第二/三順位繼承人（父母、兄弟姊妹、祖父母）②農地/公設地扣除額
  ③配偶剩餘財產差額分配請求權。修改前務必先讀 `app.py` 法定常數區塊（SSOT）。

## 🛠️ 檔案結構
- `app.py`: Streamlit 主程式＋四階段演算法（純函式與 UI 解耦）。
- `requirements.txt`: 相依套件（streamlit）。
- `.streamlit/config.toml`: 主題設定。
- `README.md`: 專案說明＋部署指引。
- `STATE.md`: 專案熱資料與進度（本檔）。

## 🐞 待辦與已知 Bug
- [ ] 擴充第二/三順位繼承人邏輯（目前僅計算第一順位：子女＋配偶）。
- [ ] 農地作農業使用扣除額、公共設施保留地扣除額。
- [ ] 配偶剩餘財產差額分配請求權（婚後財產差額 ÷ 2 先自遺產扣除）。
