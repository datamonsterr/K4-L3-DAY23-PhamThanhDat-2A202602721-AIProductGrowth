# Worksheet — FleetWatch AI (P-063)

- **Họ tên:** Phạm Thành Đạt
- **MSSV:** 2A202602721
- **Ngày làm:** 2026-10-09
- **Sản phẩm:** FleetWatch AI — Nền tảng phát hiện gian lận viễn thông & Buồng lái thẩm định gian lận cho doanh nghiệp vận tải/gọi xe công nghệ (Green Smart Mobility - GSM / Xanh SM)

---

### Số liệu tài chính đầu vào (Day 24 & Day 25)

*Ghi chú: Số liệu mô hình kinh doanh B2B SaaS phân khúc Enterprise/Mid-market Mobility Platform.*

- **ARPU:** **60.000.000 ₫/tháng** (~$2.400/tháng, tương ứng ACV = 720.000.000 ₫/năm cho mỗi khách hàng doanh nghiệp vận tải). Gói giám sát viễn thông đến 200.000 cuốc xe/tháng, 15 ghế analyst, trọn gói OSRM spatial reasoning + LangGraph dossier synthesis + Policy RAG.
- **Gross Margin (GM mục tiêu):** **65%** (sau chi phí inference LLM, GPU vector search Qdrant, hạ tầng PostGIS + OSRM cluster, và cloud storage snapshot).
- **CAC:** **180.000.000 ₫/khách hàng doanh nghiệp** (GTM sales-led B2B: chi phí nhân sự sales chuyên trách, giải pháp kỹ thuật presales, chi phí chạy pilot/PoC 30 ngày).
- **CAC Payback mục tiêu:** **4,6 tháng** (Mục tiêu trần cho phép theo chuẩn Bessemer B2B SMB/Mid-market: < 12 tháng. Phép tính: Lãi gộp/khách/tháng = 60.000.000 ₫ × 65% = 39.000.000 ₫/tháng. Payback = 180.000.000 ₫ ÷ 39.000.000 ₫ = 4,62 tháng).
- **Runway:** **14 tháng** (Tốc độ tiêu hao tiền mặt với đội ngũ 4 kỹ sư. Lưu ý: chu kỳ bán hàng B2B ~19 tuần ≈ 4,5 tháng, runway thực tế còn lại để chứng minh mô hình là: 14 − 4,5 = 9,5 tháng).
- **Value Metric:** **Số ca gian lận/bất thường được điều tra & ra quyết định (Adjudicated Fraud Cases)**.
- **Cost/Job:** **300 ₫ / ca** (chi phí inference LangGraph Multi-Agent brief ~100 ₫ + tính toán không gian OSRM & DAVI ~150 ₫ + Qdrant vector search ~50 ₫). Khi xử lý thủ công, mỗi ca tốn 12,5 phút công của Fraud Analyst (~45.000 ₫ lương). Giá trị mang lại gấp >100 lần chi phí AI.

---

## Trạm 1 — Loại mô hình

*(Sẽ điền ở Checkpoint 1)*

## Trạm 2 — Thẻ đèn

*(Sẽ điền ở Checkpoint 2)*

## Trạm 3 — Ngưỡng

*(Sẽ điền ở Checkpoint 3)*

## Trạm 4 — 5 luật quyết định

*(Sẽ điền ở Checkpoint 4)*
