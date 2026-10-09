# Worksheet — FleetWatch AI (P-063)

- **Họ tên:** Phạm Thành Đạt
- **MSSV:** 2A202602721
- **Ngày làm:** 2026-10-09
- **Thời gian thực hiện (Timeline):**
  - **Ca trưa (11:30 – 12:30):** Khởi tạo (11:32) → Trạm 1 (CP1: 11:51) → Trạm 2 (CP2: 12:24)
  - **Ca tối (21:00 – 21:40):** Trạm 3 (CP3: 21:11) → Trạm 4 (CP4: 21:26) → Trạm 5 (CP5: 21:38)
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

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn [BM]/[MH]/[TB] | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | **Time-to-first-value (TTFV)** ⭐ | < 30 ngày | 30–60 ngày | > 60 ngày | [TB] | B2B Enterprise AI cần thời gian kết nối API viễn thông, nhưng nếu quá 60 ngày khách hàng không thấy giá trị đo được thì rủi ro churn năm đầu > 80% (theo HANDBOOK §3.2, kiểm tra 27/08/2026). |
| 2 | **Pipeline coverage** | ≥ 4× | 3× – 4× | < 3× | [MH] | Với tỷ lệ thắng thầu (win rate) B2B thực tế là 25%, pipeline cần tối thiểu 4× chỉ tiêu doanh thu mới (`1 ÷ 25% = 4×`); nếu < 3× chắc chắn không đủ deal bù rớt phễu để đạt chỉ tiêu quý. |
| 3 | **% deal chết ở khâu security/procurement** | < 10% | 10% – 20% | > 20% | [TB] | Dữ liệu viễn thông vị trí xe là tài sản bảo mật cao; nếu >20% deal dừng ở InfoSec thì nút thắt nằm ở hồ sơ tuân thủ an toàn chứ không phải sản phẩm hay giá (theo HANDBOOK §3.2, kiểm tra 27/08/2026). |
| 4 | **Usage depth trong tài khoản (Seat Active Rate)** | ≥ 60% | 30% – 60% | < 30% | [TB] | Khi <30% số ghế analyst được mua hoạt động hàng tuần sau 60 ngày go-live, đây là tín hiệu báo động sớm khách hàng sẽ cắt giảm seat hoặc không gia hạn (theo HANDBOOK §3.2, kiểm tra 27/08/2026). |
| 5 | **Chi phí AI Inference & Compute ÷ Doanh thu** | < 5% | 5% – 10% | > 10% | [MH] | Để giữ Gross Margin mục tiêu ≥ 65%, tổng ngân sách hạ tầng trực tiếp tối đa 15% ARPU, trong đó trần tối đa cho AI token & OSRM routing là 10% ARPU (6.000.000 ₫/tháng); vượt mức này sẽ ăn mòn biên lợi nhuận gộp. |
| 6 | **Chi phí triển khai Onboarding ÷ ACV** | < 15% | 15% – 20% | > 20% | [MH] | Chi phí kỹ thuật onboarding vượt 20% ACV (> 144.000.000 ₫ tương đương ~410 giờ công) biến công ty thành dịch vụ gia công tùy biến (custom agency) không thể scale. |
| 7 | **Gross Margin (Biên lợi nhuận gộp)** | ≥ 65% | 55% – 65% | < 55% | [BM] | ICONIQ *State of AI 2026* (07/2026, kiểm tra 27/08/2026): Trung vị GM AI-native đạt 53% (2026E) so với 45% (2025); mục tiêu 65% đảm bảo vị thế vượt trội ngành và nếu rơi xuống < 55% là chi phí token/OSRM đã mất kiểm soát. |

### Phụ lục [MH] — phép tính (≥2)

**[MH] 1 — Chi phí triển khai Onboarding ÷ ACV**

```text
Đầu vào (từ mô hình tài chính Day 24 của tôi):
- Giá trị hợp đồng năm (ACV): 720.000.000 ₫ / năm (ARPU: 60.000.000 ₫/tháng).
- Đơn giá giờ công nội bộ kỹ sư giải pháp (Solutions Engineer): 350.000 ₫ / giờ.
- Gross Margin mục tiêu: 65% → Tổng trần Giá vốn hàng bán (COGS) tối đa: 35% ACV = 252.000.000 ₫/năm.
- Trong 35% trần COGS: 15% dành cho hạ tầng cloud & AI inference định kỳ (108.000.000 ₫), 5% dành cho hỗ trợ vận hành L1/L2 (36.000.000 ₫).
- Ngân sách tối đa còn lại cho chi phí triển khai tích hợp onboarding khách hàng năm đầu là 15% ACV = 108.000.000 ₫ (~308 giờ công kỹ sư).

Phép tính:
- Mức lý tưởng (Xanh): Chi phí triển khai < 15% ACV (< 108.000.000 ₫ hay < 308 giờ công kỹ sư).
- Mức báo động (Vàng): Chi phí triển khai từ 15% đến 20% ACV (108.000.000 ₫ – 144.000.000 ₫, tương đương 308 – 411 giờ công).
- Mức nguy hiểm (Đỏ): Chi phí triển khai > 20% ACV (> 144.000.000 ₫ hay > 411 giờ công kỹ sư). Vượt mức này, biên lợi nhuận năm đầu rơi xuống dưới 50%, công ty bị kéo tụt thành công ty gia công dịch vụ.

Kết quả → 🟢 < 15% · 🟡 15% – 20% · 🔴 > 20%
```

**[MH] 2 — Chi phí AI Inference & Compute ÷ Doanh thu**

```text
Đầu vào (từ mô hình Value Metric & Cost/Job Day 25 của tôi):
- ARPU thuê bao doanh nghiệp: 60.000.000 ₫ / tháng.
- Gói tiêu chuẩn giám sát tối đa 200.000 cuốc xe/tháng, tỷ lệ sinh ca nghi vấn DIC/gian lận viễn thông ~1% = 2.000 ca/tháng.
- Chi phí biên AI/Job: 300 ₫ / ca thẩm định (LangGraph multi-agent dossier brief ~100 ₫ + OSRM & DAVI map-matching ~150 ₫ + Qdrant policy search ~50 ₫).
- Chi phí AI cơ sở định mức = 2.000 ca × 300 ₫ = 600.000 ₫ / tháng (~1% ARPU).

Phép tính:
- Để đạt mục tiêu Gross Margin ≥ 65%, tổng chi phí trực tiếp (COGS) hàng tháng tối đa = 35% × 60.000.000 ₫ = 21.000.000 ₫.
- Trong đó, hạ tầng server lưu trữ và mạng cơ bản chiếm 10% ARPU (6.000.000 ₫), chi phí vận hành hỗ trợ chiếm 15% ARPU (9.000.000 ₫).
- Ngân sách trần tối đa cho phép đối với biến phí AI Model Inference + Cụm tính toán OSRM / Qdrant là 10% ARPU = 6.000.000 ₫ / tháng (tương ứng gấp 10 lần mức tiêu thụ cơ sở, phòng ngừa analyst tra cứu policy liên tục hoặc cuốc xe tăng đột biến).
- Nếu chi phí AI vượt quá 10% ARPU (> 6.000.000 ₫/tháng/khách), chi phí inference đang bị rò rỉ do prompt loop, thiếu semantic cache hoặc bùng nổ dữ liệu bất thường.

Kết quả → 🟢 < 5% · 🟡 5% – 10% · 🔴 > 10%
```

**[MH] 3 — Pipeline Coverage (Hệ số bao phủ phễu bán hàng)**

```text
Đầu vào (từ kế hoạch tăng trưởng Sales-led GTM B2B):
- Chỉ tiêu doanh thu mới mỗi quý (New ARR Target): 1.440.000.000 ₫ (tương đương ký mới 2 khách hàng doanh nghiệp ACV 720.000.000 ₫/quý).
- Tỷ lệ thắng thầu (Win Rate) thực tế từ cơ hội đủ điều kiện (Qualified Opportunity) sang ký hợp đồng (Closed Won): 25%.

Phép tính:
- Theo công thức nghịch đảo win rate: Coverage cần thiết = 1 ÷ Win Rate = 1 ÷ 25% = 4,0×.
- Tổng giá trị pipeline qualified cần duy trì trong quý = 1.440.000.000 ₫ × 4,0 = 5.760.000.000 ₫ (tương đương 8 deal enterprise trong phễu).
- Ngưỡng an toàn (Xanh): Coverage ≥ 4,0× (≥ 5.760.000.000 ₫).
- Ngưỡng cảnh báo (Vàng): Coverage từ 3,0× đến 4,0× (4.320.000.000 ₫ – 5.760.000.000 ₫, tương đương 6 – 8 deal).
- Ngưỡng nguy hiểm (Đỏ): Coverage < 3,0× (< 4.320.000.000 ₫, ít hơn 6 deal). Khi phễu dưới 3×, xác suất trượt chỉ tiêu quý vượt quá 70%.

Kết quả → 🟢 ≥ 4× · 🟡 3× – 4× · 🔴 < 3×
```

## Trạm 4 — 5 luật quyết định

Đánh dấu ⏹ cho luật dừng (có 3 luật dừng, đạt yêu cầu ≥2).

1. ⏹ **NẾU** TTFV > 60 ngày, **TRÊN** 3 khách hàng pilot gần nhất, **THÌ** đóng băng việc tiếp nhận thêm khách hàng pilot mới, cắt phạm vi thử nghiệm xuống đúng 1 use case duy nhất (phát hiện tài xế đứng yên ép khách hủy - DIC/`ERR-02`) trên 1 đội vận hành cho đến khi TTFV của cả 3 khách đạt < 30 ngày, **KHÔNG THÌ** không được tuyển thêm nhân viên kinh doanh để bù số vì phễu đang tắc nghẽn ở khâu triển khai và giá trị chưa được chứng minh.
2. ⏹ **NẾU** tỷ lệ deal chết ở khâu Security/Procurement > 20%, **TRONG** 1 quý liên tiếp, **VÀ** tổng số deal tham gia đàm phán trong quý ≥ 5 deal, **THÌ** dừng toàn bộ hoạt động chào hàng mới trong 2 tuần, tập trung 100% kỹ sư hoàn thiện Evidence Pack v1 (gồm báo cáo kiểm thử an toàn dữ liệu, thiết kế phân vùng PostGIS/Redis cách ly, và báo cáo đối soát tỷ lệ phạt oan), **KHÔNG THÌ** không được giảm giá chào bán để cứu hợp đồng vì khách hàng từ chối do rủi ro an ninh dữ liệu viễn thông chứ không phải vì giá cao.
3. ⏹ **NẾU** Chi phí triển khai Onboarding > 20% ACV, **TRÊN** 2 khách hàng doanh nghiệp liên tiếp, **THÌ** dừng ký các hợp đồng có yêu cầu tích hợp dữ liệu viễn thông tùy biến riêng (custom data connector) và chuyển toàn bộ việc kết nối dữ liệu sang chuẩn Webhook/REST API đóng gói sẵn, **KHÔNG THÌ** không được tiếp tục nhận khách hàng doanh nghiệp mới với mức giá cũ để tránh tình trạng công ty biến thành xưởng gia công phần mềm chịu lỗ biên.
4. **NẾU** Chi phí AI Inference & Compute > 10% Doanh thu, **TRONG** 2 tuần liên tiếp, **THÌ** kích hoạt ngay cơ chế cache ngữ cảnh Policy RAG, chuyển mô hình trích xuất dossier brief từ mô hình lớn sang Gemini 2.5 Flash / Haiku và áp trần rate limit 100 queries/analyst/ngày, **KHÔNG THÌ** không được tự ý nâng giá gói đăng ký của khách hàng giữa kỳ hợp đồng để bù đắp chi phí lãng phí token.
5. **NẾU** Usage Depth trong tài khoản < 30%, **TRONG** 3 tuần liên tiếp, **VÀ** tài khoản khách hàng đã go-live > 30 ngày, **THÌ** cử trực tiếp 1 Solution Engineer và 1 Product Specialist vào làm việc tại văn phòng khách hàng trong 3 ngày để trực tiếp đào tạo lại quy trình thao tác buồng lái thẩm định và tháo gỡ điểm nghẽn trải nghiệm người dùng, **KHÔNG THÌ** không được chào bán thêm module tính năng mới hoặc cố gắng up-sell thêm số lượng seat vào tài khoản đó.
