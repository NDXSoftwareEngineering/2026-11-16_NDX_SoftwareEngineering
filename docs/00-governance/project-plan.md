# Kế hoạch dự án theo nguyên tắc SMART / SMART Project Plan

Dự án / Project: Thực hành Software Engineering qua ứng dụng RealWorld Conduit / Software Engineering practice through the RealWorld Conduit application.
Phiên bản / Version: 1.1. Ngày lập / Date: 2026-11-16. Cập nhật / Updated: 2026-11-18 (sửa số liệu E2E / E2E count corrected).
Trạng thái / Status: v1.0 **APPROVED** by the user on 2026-11-16 (evidence E-10 in `decision-log.md`, DEC-015). v1.1 changes only the E2E test count in §4 and in the Phase 4 measure; the user confirmed that correction on 2026-11-18 (evidence E-11). v1.1 is APPROVED. Correction record: `docs/01-requirements/phase1-status.md` §5, C-01.
Người lập / Author: Kiro. Người duyệt duy nhất / Sole approver: người dùng / the user.

Căn cứ / Basis: `.kiro/steering/` và / and `docs/00-governance/decision-log.md`. Kế hoạch này không thay đổi phạm vi, công nghệ hay quy trình đã duyệt / This plan does not change the approved scope, technology, or process.

Ngôn ngữ / Language: tiêu đề và nội dung chính bằng tiếng Việt, kèm thuật ngữ và mã định danh tiếng Anh. Quy tắc song ngữ cho tài liệu dự án: DEC-016. / Main text in Vietnamese with English terms and identifiers. Bilingual rule for project documents: DEC-016.

## 1. Nhãn thông tin / Information labels

| Nhãn / Label | Ý nghĩa / Meaning |
| --- | --- |
| FACT | Đã kiểm chứng, có nguồn / Verified, with source |
| USER-STATED | Người dùng cung cấp (evidence E-10) / Provided by the user |
| ESTIMATE | Ước lượng của Kiro, chưa kiểm chứng; hiệu chỉnh sau mỗi gate / Kiro's estimate, not validated; recalibrated after each gate |
| ASSUMPTION | Giả định chưa xác nhận / Unconfirmed assumption |

## 2. Thông số do người dùng cung cấp / User-provided parameters (USER-STATED, E-10)

| Thông số / Parameter | Giá trị / Value |
| --- | --- |
| Số giờ mỗi tuần / Hours per week | 4 |
| Ngày bắt đầu / Start date | 2026-11-18 (thứ Tư / Wednesday) |
| Thời gian người dùng duyệt một gate / User review time per gate | 4 ngày / 4 days |
| Ngôn ngữ tài liệu dự án / Project document language | Song ngữ / Bilingual (DEC-016) |

## 3. Mục tiêu tổng thể (SMART) / Overall objective

| Tiêu chí / Criterion | Nội dung / Content |
| --- | --- |
| **S – Cụ thể / Specific** | Hoàn thành 5 giai đoạn tuần tự (Yêu cầu, Thiết kế, Xây dựng, Kiểm thử, Triển khai) cho ứng dụng RealWorld Conduit trên stack đã thống nhất. Mỗi giai đoạn kết thúc bằng một Quality Gate (QG1 đến QG5) do người dùng ký chấp nhận. / Complete the five sequential phases for RealWorld Conduit on the agreed stack. Each ends with a Quality Gate (QG1 to QG5) accepted by the user. |
| **M – Đo được / Measurable** | (1) 5/5 Quality Gate có bản ghi gate ACCEPTED. (2) 100% test của bộ Hurl RealWorld và 100% test của bộ Playwright RealWorld đạt, có đầu ra thô. (3) 100% yêu cầu đã duyệt có trạng thái xác minh PASS, hoặc có xử lý FAIL/BLOCKED được người dùng chấp nhận. (4) 0 lỗi Critical/High còn mở tại QG4 và QG5. |
| **A – Khả thi / Achievable** | Quy mô nguồn đã đo vừa phải (mục 4). Stack đã có trên máy (Python 3.12, Node, PostgreSQL 17, Git). Với 4 giờ mỗi tuần, tổng 120 giờ ESTIMATE cần khoảng 30 tuần làm việc; thời gian chờ duyệt thêm khoảng 3 tuần. Khả thi nếu giữ đúng 4 giờ mỗi tuần; rủi ro chính là số giờ thực tế thấp hơn (R-01). |
| **R – Liên quan / Relevant** | Đúng mục tiêu học tập trong `project-overview.md` §2: truy vết yêu cầu, thiết kế có quyết định, kiểm thử có bằng chứng, triển khai lặp lại được. |
| **T – Có thời hạn / Time-bound** | ESTIMATE: bắt đầu 2026-11-18; QG5 dự kiến **2027-07-06** (231 ngày, khoảng 33 tuần). Lịch chi tiết ở mục 6. |

## 4. Quy mô nguồn đã đo / Measured source size (FACT)

Đo bằng script trên submodule `realworld-main/` (commit `25588846`). Số liệu E2E được sửa ngày 2026-11-18: bản v1.0 ghi khoảng 170 test, nhưng đó là tổng của 133 test, 22 nhóm `describe` và 15 hook. Số Hurl (157 request) được xác nhận lại bằng script ngày 2026-11-18.

| Nguồn / Source | Số liệu / Figures |
| --- | --- |
| OpenAPI (`specs/api/openapi.yml`) | 12 đường dẫn / paths, 19 thao tác / operations |
| Hurl (`specs/api/hurl/`) | 13 file, khoảng / about 157 request |
| E2E (`specs/e2e/`) | 12 file `.spec.ts`, 133 khai báo `test()` tĩnh / static declarations, 22 nhóm `describe` (số test chạy thực tế cao hơn vì các test XSS lặp theo payload) |

## 5. Kế hoạch theo giai đoạn (SMART) / Plan by phase

Công thức lịch / Schedule rule (ESTIMATE): thời gian làm việc = số giờ ÷ 4 giờ mỗi tuần; sau mỗi giai đoạn có 4 ngày chờ người dùng duyệt gate; giai đoạn sau bắt đầu sau khi gate được chấp nhận.

### Giai đoạn 1: Kỹ nghệ yêu cầu / Requirements Engineering, QG1

| Tiêu chí | Nội dung |
| --- | --- |
| S | Lập SRS, use case, RTM v1, nhật ký vấn đề. Đọc đủ các nguồn RealWorld chưa đọc (13 file Hurl, 12 file E2E, toàn bộ OpenAPI, `SELECTORS.md`, các trang tài liệu còn lại). / Produce SRS, use cases, RTM v1, issue log; read all unread RealWorld sources. |
| M | 19/19 thao tác OpenAPI, 13/13 file Hurl, 12/12 file E2E ánh xạ tới ít nhất một yêu cầu. 100% yêu cầu có nguồn hoặc nhãn CON/PROP và có phương pháp xác minh. Không có từ mơ hồ thiếu tiêu chí đo. Mọi vấn đề mở có chủ sở hữu và cách xử lý. |
| A | ESTIMATE: 20 giờ = 35 ngày làm việc. |
| R | Điều kiện bắt buộc trước thiết kế (`engineering-process.md` §5). |
| T | ESTIMATE: 2026-11-18 đến 2026-12-22 (làm việc), duyệt QG1 trong 4 ngày, hạn quyết định **2026-12-26**. |
| Sản phẩm | `docs/01-requirements/`: SRS, use case, RTM v1, nhật ký vấn đề, bản ghi nguồn; bản ghi gate QG1. |

### Giai đoạn 2: Thiết kế / Software Design, QG2

| Tiêu chí | Nội dung |
| --- | --- |
| S | Lập SDD: kiến trúc, dữ liệu, API, giao diện, bảo mật. Ghi quyết định cho phiên bản công cụ, thư viện (JWT, Markdown, router), cổng (port), mục tiêu triển khai. Lập RTM v2 và phác thảo chiến lược kiểm thử. |
| M | 100% yêu cầu đã duyệt ánh xạ tới ít nhất một phần tử thiết kế. 0 phần tử thiết kế không có yêu cầu hoặc quyết định đã duyệt. 100% lựa chọn công nghệ và phiên bản có bản ghi quyết định đã duyệt. Thiết kế bảo mật phủ mọi yêu cầu bảo mật. |
| A | ESTIMATE: 20 giờ = 35 ngày. Kiến trúc đơn giản theo nguyên tắc RealWorld. |
| R | Điều kiện bắt buộc trước khi viết mã (`engineering-process.md` §6). |
| T | ESTIMATE: 2026-12-27 đến 2027-01-30, hạn quyết định **2027-02-03**. |
| Sản phẩm | `docs/02-design/`: SDD, bản ghi quyết định, RTM v2, phác thảo chiến lược kiểm thử; bản ghi gate QG2. |
| Điều kiện cần | QG1 ACCEPTED. |

### Giai đoạn 3: Xây dựng / Software Construction, QG3

| Tiêu chí | Nội dung |
| --- | --- |
| S | Cài đặt backend, frontend, cơ sở dữ liệu theo thiết kế đã duyệt. Unit test cùng lúc với mã. Phân tích tĩnh. RTM v3. |
| M | Build và chạy được từ bản checkout sạch theo README, có đầu ra lệnh. Mọi unit test đạt. 0 lỗi phân tích tĩnh chưa xử lý. 0 bí mật trong repository. 100% yêu cầu có mã cài đặt, hoặc hoãn được người dùng duyệt. |
| A | ESTIMATE: 45 giờ = 79 ngày. Giai đoạn lớn nhất và rủi ro nhất (R-04). |
| R | Hiện thực hóa thiết kế đã duyệt. |
| T | ESTIMATE: 2027-02-04 đến 2027-04-23, hạn quyết định **2027-04-27**. |
| Sản phẩm | Mã nguồn trong Git, unit test, README (C-07), báo cáo phân tích tĩnh, RTM v3; bản ghi gate QG3. |
| Điều kiện cần | QG2 ACCEPTED. Cài Hurl (DEC-004) khi bắt đầu. |

### Giai đoạn 4: Kiểm thử / Software Testing, QG4

| Tiêu chí | Nội dung |
| --- | --- |
| S | Kế hoạch kiểm thử; chạy unit, tích hợp, API (Hurl), E2E (Playwright), kiểm tra bảo mật; xử lý lỗi và hồi quy; RTM v4; báo cáo tổng kết. |
| M | Bộ Hurl RealWorld đạt toàn bộ (khoảng 157 request; số chính xác lấy từ đầu ra Hurl). Bộ Playwright RealWorld đạt toàn bộ (133 khai báo test tĩnh; số chạy thực tế lấy từ báo cáo Playwright). 100% yêu cầu có trạng thái xác minh PASS hoặc xử lý được duyệt. 0 lỗi Critical/High còn mở. Quét thư viện không còn High/Critical chưa xử lý. |
| A | ESTIMATE: 25 giờ = 44 ngày. Hai bộ test đã có sẵn. |
| R | Xác minh sản phẩm theo yêu cầu (`quality-assurance.md` QO-1 đến QO-6). |
| T | ESTIMATE: 2027-04-28 đến 2027-06-10, hạn quyết định **2027-06-14**. |
| Sản phẩm | `docs/04-testing/`: kế hoạch, ca kiểm thử, báo cáo thực thi có đầu ra thô, nhật ký lỗi, RTM v4, báo cáo tổng kết; bản ghi gate QG4. |
| Điều kiện cần | QG3 ACCEPTED. |

### Giai đoạn 5: Triển khai / Deployment, QG5

| Tiêu chí | Nội dung |
| --- | --- |
| S | Đóng gói bản phát hành, cấu hình môi trường không đưa bí mật vào Git, triển khai theo hướng dẫn, smoke test, báo cáo cuối và bài học. Kế hoạch bảo trì sau dự án (DEC-014). |
| M | Triển khai lặp lại được theo hướng dẫn có đầu ra ghi lại. Smoke test đạt ở môi trường đích. Thẻ Git khớp bản đã kiểm thử. Báo cáo cuối liệt kê vấn đề tồn đọng. |
| A | ESTIMATE: 10 giờ = 18 ngày. Môi trường đích quyết định ở Giai đoạn 2. |
| R | Hoàn tất vòng đời từ yêu cầu đến vận hành. |
| T | ESTIMATE: 2027-06-15 đến 2027-07-02, hạn quyết định **2027-07-06**. |
| Sản phẩm | `docs/05-deployment/`: hướng dẫn triển khai, mô tả cấu hình, ghi chú phát hành, báo cáo smoke test, báo cáo cuối; bản ghi gate QG5. |
| Điều kiện cần | QG4 ACCEPTED; mục tiêu triển khai được duyệt. |

## 6. Lịch tổng hợp / Schedule summary (ESTIMATE)

| Giai đoạn | Bắt đầu | Kết thúc làm việc | Hạn quyết định gate (4 ngày) | Giờ |
| --- | --- | --- | --- | --- |
| 1. Yêu cầu | 2026-11-18 | 2026-12-22 | QG1: 2026-12-26 | 20 |
| 2. Thiết kế | 2026-12-27 | 2027-01-30 | QG2: 2027-02-03 | 20 |
| 3. Xây dựng | 2027-02-04 | 2027-04-23 | QG3: 2027-04-27 | 45 |
| 4. Kiểm thử | 2027-04-28 | 2027-06-10 | QG4: 2027-06-14 | 25 |
| 5. Triển khai | 2027-06-15 | 2027-07-02 | QG5: 2027-07-06 | 10 |
| **Tổng** | | | | **120 giờ, 231 ngày (khoảng 33 tuần)** |

Dữ liệu tính toán: script tính lịch ngày 2026-11-16 (làm tròn lên theo ngày). Một số hạn rơi vào cuối tuần; người dùng có thể dùng đủ 4 ngày.

Điều chỉnh: nếu số giờ thực tế thấp hơn 4 mỗi tuần, mọi mốc sau đó dời theo tỉ lệ. Lịch được tính lại sau mỗi gate bằng số giờ thực tế.

## 7. Theo dõi và điều chỉnh / Monitoring

| Hoạt động | Tần suất | Chỉ số |
| --- | --- | --- |
| Báo cáo tiến độ | Cuối mỗi tuần làm việc | Giờ thực tế so với 4 giờ, việc hoàn thành, vấn đề mới, quyết định chờ duyệt |
| Kiểm tra độ phủ truy vết | Trước mỗi gate | Độ phủ tiến, lùi và nguồn (`requirements-and-sources.md` §5) |
| Duyệt gate | Cuối mỗi giai đoạn | Tiêu chí chấp nhận, bằng chứng, vấn đề mang theo |
| Hiệu chỉnh ước lượng | Sau mỗi gate | Chênh lệch giờ thực tế so với ước lượng; cập nhật kế hoạch bằng `CR-NNN` nếu chênh trên 25% |
| Thay đổi phạm vi, kiến trúc, công nghệ | Khi phát sinh | `CR-NNN` và người dùng duyệt |

Với 4 giờ mỗi tuần, đơn vị công việc nên nhỏ: mỗi buổi 2 giờ hoặc mỗi tuần một mục tiêu cụ thể, có sản phẩm kiểm tra được.

## 8. Rủi ro chính / Key risks

| ID | Rủi ro | Xác suất / Tác động (ESTIMATE) | Giảm thiểu |
| --- | --- | --- | --- |
| R-01 | Số giờ thực tế dưới 4 giờ mỗi tuần | Trung bình / Cao | Báo cáo hằng tuần; dời mốc theo tỉ lệ, không bỏ gate |
| R-02 | Bộ test RealWorld khó đạt 100% trên Windows (thiếu bash, CRLF) | Trung bình / Trung bình | Chạy Hurl từ PowerShell (DEC-004); thử bộ Hurl và E2E với skeleton nhỏ ở đầu Giai đoạn 3 |
| R-03 | Yêu cầu mơ hồ hoặc mâu thuẫn giữa các nguồn | Trung bình / Trung bình | Ghi thành ISS; người dùng quyết định (`requirements-and-sources.md` §7) |
| R-04 | Giai đoạn 3 vượt ước lượng | Cao / Cao | Chia phần nhỏ; hiệu chỉnh sau mỗi phần |
| R-05 | Phiên bản thư viện không tương thích | Thấp / Trung bình | Ghim phiên bản bằng quyết định ở Giai đoạn 2 |
| R-06 | NPR 7150.2D hết hạn 2027-03-08, trước QG5 (2027-07-06) | Cao / Thấp | Kiểm tra NODIS tìm bản thay thế trước 2027-03-01; ghi quyết định tiếp tục dùng 7150.2D hoặc chuyển bản mới (ISS-009) |
| R-07 | Bằng chứng phê duyệt chỉ nằm trong chat | Thấp / Trung bình | Sổ bằng chứng trong decision log, ghi nguyên văn |
| R-08 | Kỳ nghỉ Tết Nguyên đán 2027 và các kỳ nghỉ khác làm gián đoạn lịch | Trung bình / Thấp | Ngày chính xác chưa kiểm chứng; báo trước các tuần nghỉ để dời lịch |
| R-09 | Tài liệu song ngữ làm tăng công sức viết và duyệt | Trung bình / Trung bình | Ước lượng 120 giờ chưa tính phần song ngữ (A-10); hiệu chỉnh sau QG1 |

## 9. Giả định, điểm mở / Assumptions and open points

| ID | Nội dung | Trạng thái |
| --- | --- | --- |
| A-07 | 4 giờ mỗi tuần, bắt đầu 2026-11-18 | USER-STATED, confirmed (E-10) |
| A-08 | Ước lượng công sức theo giai đoạn (20, 20, 45, 25, 10 giờ) | ESTIMATE của Kiro, chưa kiểm chứng |
| A-09 | Người dùng duyệt mỗi gate trong 4 ngày | USER-STATED, confirmed (E-10) |
| A-10 | Ước lượng 120 giờ chưa tính công sức dịch và duyệt bản song ngữ | ASSUMPTION; đo ở Giai đoạn 1 |
| OPEN-01 | Chi tiết định dạng song ngữ (DEC-016): bản nào có hiệu lực khi hai bản khác nhau; dịch song song từng đoạn hay hai phần riêng; có dịch các tài liệu quản trị hiện có không | Chờ người dùng xác nhận |
| OPEN-02 | Phạm vi: kế hoạch này và các tài liệu quản trị hiện có giữ nguyên ngôn ngữ cho đến khi OPEN-01 được quyết định | Đề xuất của Kiro |

## 10. Điều kiện bắt đầu Giai đoạn 1 / Phase 1 entry

| # | Điều kiện | Trạng thái |
| --- | --- | --- |
| 1 | Quality Gate S1 chấp nhận | Đạt (E-09) |
| 2 | Kế hoạch này được duyệt | Đạt (E-10, DEC-015) |
| 3 | Người dùng ra lệnh bắt đầu Giai đoạn 1 | Đạt (người dùng ra lệnh ngày 2026-11-18; Giai đoạn 1 đang chạy) |

## 11. Bản ghi duyệt / Approval record

Ghi lại từ tin nhắn của người dùng, nguyên văn trong evidence E-10 (`decision-log.md` §1).

| Trường / Field | Giá trị / Value |
| --- | --- |
| Quyết định / Decision | APPROVED ("Tôi duyệt") |
| Số giờ mỗi tuần (A-07) | 4 |
| Ngày bắt đầu | 2026-11-18 |
| Thời gian duyệt mỗi gate (A-09) | 4 ngày |
| Ngôn ngữ tài liệu | Song ngữ (DEC-016) |
| Ngày | 2026-11-16 |
