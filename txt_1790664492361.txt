import pandas as pd
import streamlit as st

st.title("每月發票與憑證核對清單")

# 選擇月份
selected_month = st.selectbox("選擇結帳月份", ["2026-08", "2026-09", "2026-10"])

# 模擬初始資料（實際應用可以存入資料庫或 CSV）
if "data" not in st.session_state:
  st.session_state.data = pd.DataFrame({
      "店家名稱": ["辦公室文具行", "中華電信", "雲端伺服器商"],
      "統一編號": ["87654321", "12345678", "98765432"],
      "金額": [1200, 2500, 4500],
      "發票來源": ["紙本三聯式", "電子發票", "信箱 PDF"],
      "是否已收到": [True, False, False],
  })

# 使用可編輯的表格，支援核取方塊
edited_df = st.data_editor(
    st.session_state.data,
    num_rows="dynamic",  # 允許動態新增或刪除店家
    use_container_width=True,
)

# 計算統計數據
total_amount = edited_df["金額"].sum()
received_count = edited_df["是否已收到"].sum()
total_count = len(edited_df)

st.divider()
st.metric(label="本月總金額", value=f"${total_amount:,}")
st.text(
    f"收齊進度：已收到 {received_count} / 總共 {total_count} 家（"
    f"{(received_count/total_count*100) if total_count > 0 else 0:.1f}%）"
)
