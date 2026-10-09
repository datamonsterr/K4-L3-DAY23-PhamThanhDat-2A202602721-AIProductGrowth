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

### 1. Trả lời 3 câu hỏi theo thực tế hôm nay
1. **Ai trả tiền cho bạn?** Doanh nghiệp vận tải & nền tảng gọi xe công nghệ (B2B Enterprise như Green Smart Mobility - GSM / Xanh SM, Be Group, hoặc các đơn vị vận hành đội xe thương mại).
2. **Ai dùng sản phẩm?** Chính nhân sự của doanh nghiệp trả tiền — cụ thể là đội ngũ chuyên viên phòng chống gian lận (Fraud Operations Analysts), quản trị rủi ro và an toàn vận hành.
3. **Nếu có bên trung gian, bạn có chạm được người dùng cuối không?** **Không.** Chúng tôi hoàn toàn không chạm vào người dùng cuối. Ứng dụng là một buồng lái nội bộ (internal adjudication cockpit); hành khách và tài xế không nhìn thấy thương hiệu, không tương tác với hệ thống, và mọi luồng ứng dụng di động tài xế/khiếu nại đều được hoãn (*deferred*) khỏi phạm vi Phase 1.

**Câu chốt loại:** Chúng tôi là **B2B** vì tiền đến từ doanh nghiệp vận tải/gọi xe công nghệ (như Green Smart Mobility - GSM / Xanh SM), người dùng trực tiếp là đội ngũ chuyên viên phòng chống gian lận (Fraud Operations Analysts) của doanh nghiệp đó, và chúng tôi hoàn toàn không tương tác hay chạm trực tiếp vào hành khách/tài xế cuối (mọi giao diện tài xế/hành khách và khiếu nại đều nằm ngoài phạm vi Phase 1).

### 2. Bảng đèn §3 của B2B (HANDBOOK §3.2)

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| **Time-to-first-value (TTFV)** ⭐ | 🔧 | Đo được trong 2 tuần — Cần kết nối timestamp ký hợp đồng trên CRM với event log ghi nhận lần đầu tiên chuyên viên của khách hàng nhấn xác nhận chế tài một ca gian lận thật trên buồng lái. |
| **Pipeline coverage** | ✅ | Đo được hôm nay — Nằm trong CRM (HubSpot/Salesforce): Tổng ACV của các deal đạt chuẩn Qualified chia cho chỉ tiêu doanh thu mới của quý. |
| **% deal chết ở khâu security/procurement** | ✅ | Đo được hôm nay — Báo cáo Win/Loss Analysis trong CRM: Lọc theo trường Lost Reason = "Security/InfoSec Rejection" hoặc "Procurement Stall". |
| **POC → paid** | ✅ | Đo được hôm nay — Tỷ lệ hợp đồng pilot 30 ngày chuyển đổi thành hợp đồng thuê bao năm chính thức trong CRM. |
| **Sales cycle (tuần)** | ✅ | Đo được hôm nay — Dữ liệu CRM: Số ngày trung bình từ stage Qualified Opportunity đến Closed Won. |
| **Usage depth trong tài khoản** | ✅ | Đo được hôm nay — Telemetry logs của webapp: Số lượng chuyên viên thẩm định ≥5 ca/tuần chia cho tổng số ghế (seat licenses) đã mua. |
| **Chi phí triển khai ÷ ACV** | 🔧 | Đo được trong 2 tuần — Cần bảng chấm công (timesheet) ghi nhận số giờ công kỹ sư giải pháp hỗ trợ tích hợp cổng API viễn thông và chi phí môi trường sandbox cho từng khách hàng. |
| **Tập trung doanh thu** | ✅ | Đo được hôm nay — Sổ kế toán quản trị: Doanh thu của khách hàng lớn nhất chia cho tổng doanh thu định kỳ trong tháng. |
| **NRR (Net Revenue Retention)** | ❌ | Chưa đo được và chưa biết cách — Sản phẩm mới ở Phase 1 MVP, chưa có khách hàng nào đi hết chu kỳ hợp đồng 12 tháng để có dữ liệu mở rộng (expansion) hay churn năm thứ hai. |
| **Gross Margin** | ✅ | Đo được hôm nay — Báo cáo tài chính tháng: Tổng doanh thu trừ đi chi phí trực tiếp (hoá đơn model API, chi phí cụm OSRM, Qdrant và hosting AWS). |
| **CAC payback** | 🔧 | Đo được trong 2 tuần — Cần chuẩn hoá hạch toán phân bổ chi phí lương đội ngũ sales doanh nghiệp, chi phí marketing và chi phí hạ tầng pilot theo từng quý. |

## Trạm 2 — Thẻ đèn

*(Sẽ điền ở Checkpoint 2)*

## Trạm 3 — Ngưỡng

*(Sẽ điền ở Checkpoint 3)*

## Trạm 4 — 5 luật quyết định

*(Sẽ điền ở Checkpoint 4)*
