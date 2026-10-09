# OPERATING DASHBOARD — FleetWatch AI (P-063)

**Loại mô hình:** B2B SaaS Enterprise · **Cập nhật:** 2026-10-09 (21:38 GMT+7) · Phạm Thành Đạt – 2A202602721  
**NORTH STAR:** **Time-to-first-value (TTFV)** — hiện tại: **Đang đo (pilot)** — mục tiêu: **< 30 ngày**

### Đèn báo sớm (Leading — nhìn hằng ngày/tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **Time-to-first-value (TTFV)** ⭐ | Đang đo | 🟢 < 30 ngày · 🟡 30–60 ngày · 🔴 > 60 ngày | [TB] | POC → paid · NRR (gia hạn năm đầu) |
| **Pipeline coverage** | 4,2× | 🟢 ≥ 4× · 🟡 3× – 4× · 🔴 < 3× | [MH] | Doanh thu mới ký kết · Sales cycle |
| **% deal chết ở security/procurement** | 12% | 🟢 < 10% · 🟡 10% – 20% · 🔴 > 20% | [TB] | Tỷ lệ thắng thầu (Win rate) · Sales cycle |

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **Usage depth trong tài khoản** | 65% | 🟢 ≥ 60% · 🟡 30% – 60% · 🔴 < 30% | [TB] | NRR (tái ký) · POC → paid |
| **Chi phí AI Inference & Compute ÷ Doanh thu** | 3,8% | 🟢 < 5% · 🟡 5% – 10% · 🔴 > 10% | [MH] | Gross Margin (Biên lợi nhuận gộp) |
| **Chi phí triển khai Onboarding ÷ ACV** | 14% | 🟢 < 15% · 🟡 15% – 20% · 🔴 > 20% | [MH] | Gross Margin · Tính scale SaaS |

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| **Gross Margin (Biên lợi nhuận gộp)** | 66,2% | 🟢 ≥ 65% · 🟡 55% – 65% · 🔴 < 55% | [BM] ICONIQ 07/2026 (kiểm tra 27/08/2026) |

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ **NẾU** TTFV > 60 ngày **TRÊN** 3 khách pilot gần nhất **THÌ** đóng băng nhận pilot mới, cắt phạm vi xuống đúng 1 use case (DIC/`ERR-02`) trên 1 đội cho đến khi TTFV < 30 ngày **KHÔNG THÌ** không tuyển thêm sales.
2. ⏹ **NẾU** % deal chết ở Security/Procurement > 20% **TRONG** 1 quý **VÀ** deal quý ≥ 5 **THÌ** dừng chào hàng mới 2 tuần, tập trung 100% kỹ sư hoàn thiện Evidence Pack v1 **KHÔNG THÌ** không giảm giá để cứu deal.
3. ⏹ **NẾU** Chi phí triển khai Onboarding > 20% ACV **TRÊN** 2 khách liên tiếp **THÌ** dừng ký hợp đồng yêu cầu custom connector, chuẩn hoá 100% qua REST API/Webhook **KHÔNG THÌ** không nhận khách mới với giá cũ.
4. **NẾU** Chi phí AI Inference & Compute > 10% Doanh thu **TRONG** 2 tuần liên tiếp **THÌ** kích hoạt cache ngữ cảnh RAG, chuyển mô hình dossier brief sang Flash/Haiku và áp rate limit 100 queries/analyst/ngày **KHÔNG THÌ** không tự ý tăng giá giữa kỳ.
5. **NẾU** Usage depth < 30% **TRONG** 3 tuần **VÀ** tài khoản go-live > 30 ngày **THÌ** cử trực tiếp 1 Solution Engineer và 1 Product Specialist đến văn phòng khách hàng đào tạo lại 3 ngày **KHÔNG THÌ** không up-sell thêm seat hay module mới.

### Cổng gác 90 ngày

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| **30** | Pilot khởi động & Evidence Pack | ≥2 pilot onboarded & Evidence Pack v1 100% | 2 Biên bản Pilot ký + File `EVIDENCE_PACK_V1.pdf` | **FIX:** Ở lại 30 ngày, tập trung tích hợp cho đúng 1 đối tác |
| **60** | TTFV khách hàng pilot | TTFV < 30 ngày trên ≥2 khách pilot | Log hệ thống `audit_decision_logs.jsonl` ghi 5 ca thực xử lý | **PIVOT:** Thu hẹp thành API micro-service phát hiện đứng yên |
| **90** | POC → Paid & Mở rộng | ≥1 hợp đồng trả phí (ACV ≥ 720tr ₫) & 1 LOI mở rộng | Hợp đồng thương mại ký số đóng dấu (`contract_signed_p01.pdf`) | **KILL:** Dừng dự án, hoàn trả dữ liệu, bảo toàn 9 tháng runway |

**KILL CRITERIA:** Nếu đến ngày **07/01/2027** (mốc 90 ngày), tỷ lệ POC → Paid bằng 0 (không ký được hợp đồng thương mại nào có ACV ≥ 720.000.000 ₫), chúng tôi sẽ **dừng toàn bộ dự án FleetWatch AI**, hoàn trả dữ liệu cho khách hàng và bảo toàn 9 tháng runway tiền mặt còn lại.

**CHƯA ĐO ĐƯỢC:**
- **NRR (Net Revenue Retention):** Cần dữ liệu tái ký sau chu kỳ 12 tháng của khách hàng đầu tiên; dự kiến có số vào tháng 10/2027.
- **Chi phí triển khai Onboarding ÷ ACV quy mô lớn:** Cần timesheet tự động theo mã dự án và hạch toán cloud AWS tenant; dự kiến có số ngày 15/12/2026.
- **CAC Payback thực tế:** Cần báo cáo tài chính chuẩn hoá chi phí S&M quý; dự kiến có số ngày 31/01/2027.
