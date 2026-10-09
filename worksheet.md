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

**North Star:** **Time-to-first-value (TTFV)** — hiện tại: **Chưa đo được (đang ở giai đoạn chạy thử nghiệm pilot)** — mục tiêu: **< 30 ngày**

| # | Tầng (L/O/G) | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | L | **Time-to-first-value (TTFV)** ⭐ | Số ngày từ lúc ký hợp đồng (pilot hoặc chính thức) đến lần đầu tiên chuyên viên của khách hàng thẩm định thành công ca gian lận thật trên buồng lái và ban hành quyết định (hoặc giải oan đúng). **Không đếm:** ngày cài đặt hạ tầng xong, ngày test dữ liệu giả lập (synthetic data), hoặc các ca test nội bộ của vendor. | `Ngày quyết định ca thực tế đầu tiên - Ngày ký hợp đồng` (ngày dương lịch) | Mỗi khách hàng · Customer Success Lead | POC → paid · NRR (gia hạn năm đầu) |
| 2 | L | **Pipeline coverage** | Tổng ACV của các cơ hội bán hàng B2B đã qua vòng đánh giá ban đầu (Qualified Opportunities: đã xác nhận nhu cầu & có ngân sách) chia cho chỉ tiêu doanh thu mới của quý. **Không đếm:** lead thô chỉ mới đăng ký thông tin, các liên hệ chưa qua cuộc họp thẩm định kỹ thuật (discovery call). | `Tổng ACV các Qualified Deals trong quý ÷ Chỉ tiêu doanh thu mới quý` | Hàng tuần · Head of Sales | Doanh thu mới ký kết · Sales cycle |
| 3 | L | **% deal chết ở khâu security/procurement** | Tỷ lệ cơ hội bán hàng bị khách hàng dừng hoặc từ chối tại khâu thẩm định an toàn thông tin (InfoSec), pháp lý dữ liệu viễn thông hoặc quy trình mua sắm trên tổng số deal đóng trong kỳ. **Không đếm:** các deal dừng do không khớp tính năng (feature mismatch) hoặc không có ngân sách từ đầu. | `Số deal dừng ở khâu Security & Procurement ÷ Tổng số deal đóng (Won + Lost) trong kỳ × 100%` | Hàng tháng · Sales Operations | Win rate · Sales cycle |
| 4 | O | **Usage depth trong tài khoản (Seat Active Rate)** | Tỷ lệ phần trăm số tài khoản chuyên viên (analyst seats) đã mua thực sự đăng nhập và ra quyết định thẩm định ít nhất 5 ca gian lận trong tuần. **Không đếm:** các tài khoản chỉ đăng nhập để xem mà không thao tác (read-only lurking), tài khoản admin kỹ thuật hoặc tài khoản demo nội bộ. | `Số analyst thẩm định ≥5 ca trong tuần ÷ Tổng số seats đã cấp phép × 100%` | Hàng tuần · Data Engineer (telemetry log) | NRR (gia hạn) · POC → paid |
| 5 | O | **Chi phí AI Inference & Compute ÷ Doanh thu** | Tổng chi phí token LLM (LangGraph dossier synthesis + Policy RAG) cộng chi phí cụm tính toán không gian (OSRM routing engine + Qdrant vector database) trong tháng chia cho tổng doanh thu định kỳ từ khách hàng trong tháng đó. **Không đếm:** chi phí lưu trữ database chung (Postgres), chi phí môi trường staging/dev và kiểm thử nội bộ. | `Tổng chi phí hoá đơn AI Model API & Spatial Routing trong tháng ÷ Tổng Doanh thu trong tháng × 100%` | Hàng tháng · Tech Lead & DevOps FinOps | Gross Margin (Biên lợi nhuận gộp) |
| 6 | O | **Chi phí triển khai Onboarding ÷ ACV** | Tổng chi phí nhân sự kỹ thuật (số giờ công kỹ sư giải pháp × đơn giá nội bộ 350.000 ₫/giờ) và tài nguyên cloud phát sinh để tích hợp dữ liệu viễn thông khách hàng cho đến khi go-live chia cho ACV của khách hàng đó. **Không đếm:** chi phí bảo trì sau go-live, chi phí sửa lỗi sản phẩm cốt lõi dùng chung (core product bug fixes). | `(Số giờ công kỹ sư triển khai × 350.000 ₫ + Chi phí cloud setup) ÷ ACV × 100%` | Mỗi khách hàng · Delivery Manager | Gross Margin · Tính khả thi của mô hình SaaS |
| 7 | G | **Gross Margin (Biên lợi nhuận gộp)** | Tỷ lệ phần trăm doanh thu còn lại sau khi trừ toàn bộ giá vốn hàng bán trực tiếp (COGS: gồm chi phí API token AI, hạ tầng cloud phục vụ khách hàng trực tiếp, và hỗ trợ kỹ thuật L1/L2 trực tiếp). **Không đếm:** chi phí R&D phát triển tính năng mới, lương đội ngũ sales/marketing và chi phí quản lý chung (G&A). | `(Doanh thu thuần - COGS) ÷ Doanh thu thuần × 100%` | Hàng quý · Kế toán trưởng | Bảng điểm kết quả tài chính cuối cùng |

**Đèn chi phí AI là đèn số:** **5** (*Chi phí AI Inference & Compute ÷ Doanh thu*)

## Trạm 3 — Ngưỡng

*(Sẽ điền ở Checkpoint 3)*

## Trạm 4 — 5 luật quyết định

*(Sẽ điền ở Checkpoint 4)*
