# Kế hoạch xây dựng hệ thống

## 1. Mục đích tài liệu

`BUSINESS_LOGIC.md` tiếp tục là nguồn thông tin chính thức về nghiệp vụ. Tài liệu này đóng vai trò kế hoạch triển khai và sẽ được điều chỉnh khi các quy tắc nghiệp vụ được xác nhận thêm.

## 2. Mức độ hiểu hiện tại

- Hiểu khoảng **90% nội dung được mô tả trong tài liệu**.
- Hiểu khoảng **70% nghiệp vụ thực tế cuối cùng**.
- Source code hiện mới chủ yếu có nền tảng Laravel, Inertia, authentication và dashboard ban đầu; các module logistics chưa được triển khai đáng kể.

Phần nghiệp vụ chưa thể khẳng định hoàn toàn chủ yếu là các nội dung đang được đánh dấu **Đề xuất** hoặc **Cần làm rõ**, không phải do tài liệu khó hiểu.

## 3. Tổng quan hệ thống

Hệ thống được định hướng là một **Logistics Operations & CRM Management Platform**, kết hợp các chức năng:

- Quản trị tổ chức, tài khoản và phân quyền.
- CRM và quản lý khách hàng.
- Pricing và phê duyệt giá.
- Báo giá và booking.
- Quản lý shipment, pickup và vận hành.
- Customer Service.
- Hóa đơn, thanh toán và công nợ.
- Thông báo đa kênh.
- AI trích xuất dữ liệu chứng từ.
- Dashboard và báo cáo quản trị.

Luồng nghiệp vụ trung tâm:

```text
Khách hàng
→ Sales tiếp nhận nhu cầu
→ Pricing thu thập và chuẩn hóa giá hãng
→ Ban Lãnh Đạo duyệt giá gốc
→ Pricing công bố giá
→ Sales Manager thiết lập phạm vi deal
→ Sales lập và gửi báo giá
→ Khách hàng chấp nhận và tạo booking
→ Operations/Pickup vận hành shipment
→ Customer Service theo dõi và hỗ trợ
→ Accounting xuất hóa đơn, ghi nhận thanh toán và quản lý công nợ
→ Ban Lãnh Đạo theo dõi và phê duyệt ngoại lệ
```

## 4. Những nguyên tắc đã nắm rõ

### 4.1. Phân quyền

Quyền truy cập không chỉ dựa trên tên role mà phải xét đồng thời:

```text
Quyền thực hiện hành động
+ Phạm vi dữ liệu được truy cập
+ Trạng thái hiện tại của đối tượng
+ Hạn mức hoặc yêu cầu phê duyệt
```

Một người dùng có thể đảm nhiệm nhiều vai trò. Phạm vi dữ liệu có thể giới hạn theo cá nhân, team, chi nhánh hoặc toàn công ty.

### 4.2. Sales và quyền sở hữu khách hàng

- Sales Manager quản lý team, khách hàng của team, việc bàn giao và phạm vi deal giá.
- Sales Representative chỉ thao tác trên khách hàng được giao, trừ khi có quyền ngoại lệ.
- Việc phân công và chuyển giao khách hàng phải lưu lịch sử theo thời gian.
- Lịch sử được dùng để truy vết trách nhiệm, doanh số, hoa hồng và giải quyết tranh chấp.

### 4.3. Pricing và phê duyệt giá

- Pricing thu thập, chuẩn hóa và so sánh giá hãng.
- Ban Lãnh Đạo chốt giá gốc chính thức.
- Pricing công bố giá đã được duyệt đến các phòng ban phù hợp.
- Giá cần có phiên bản và thời gian hiệu lực.
- Mọi lần gửi duyệt, từ chối, phê duyệt và công bố đều phải có audit log.

### 4.4. Báo giá

- Báo giá được tạo từ nhu cầu và dữ liệu giá phù hợp.
- Báo giá đã phát hành phải lưu snapshot dữ liệu giá tại thời điểm phát hành.
- Thay đổi bảng giá gốc không được làm thay đổi báo giá lịch sử.
- Báo giá ngoài giới hạn cho phép phải bị chặn hoặc chuyển sang quy trình phê duyệt, tùy quy tắc được chốt.

### 4.5. Booking và vận hành

- Booking được bàn giao từ Sales sang Operations.
- Operations kiểm tra tính đầy đủ trước khi tiếp nhận.
- Operations phân công nhân viên vận hành và Pickup Staff.
- Shipment cần có milestone, lịch sử trạng thái, người phụ trách, sự cố và bằng chứng giao nhận.
- Pickup chỉ được xem thông tin cần thiết cho nhiệm vụ được giao.

### 4.6. Customer Service

- Customer Service độc lập về chức năng với Sales.
- Customer Sale không đồng nghĩa với Sales Representative.
- Customer Manager quản lý team Customer Service nhưng không mặc nhiên có quyền Pricing, Sales, Operations hoặc Accounting.
- Toàn bộ trao đổi, escalation, bàn giao và quyết định hỗ trợ phải được lưu lịch sử.

### 4.7. Accounting và AI chứng từ

- Accounting quản lý hóa đơn, thanh toán từng phần, số dư, công nợ và hạn mức tín dụng.
- Chứng từ đã phát hành không được xóa cứng.
- AI chỉ thực hiện nhận diện và đề xuất dữ liệu trích xuất.
- Người có quyền phải kiểm tra, chỉnh sửa và xác nhận trước khi dữ liệu được ghi nhận vào nghiệp vụ kế toán.

### 4.8. Audit và thông báo

- Các thay đổi quan trọng phải có audit log.
- Dữ liệu đã phát sinh giao dịch không được xóa cứng.
- Hoạt động gửi thông báo phải lưu nội dung, người gửi, người nhận, kênh, thời gian và kết quả.
- Hệ thống thông báo cần chống gửi trùng và hỗ trợ gửi lại khi thất bại.

## 5. Các vấn đề cần chốt trước khi triển khai sâu

Năm vấn đề ảnh hưởng trực tiếp đến kiến trúc và database:

1. Loại hình logistics được hỗ trợ trong MVP.
2. Quan hệ chính xác giữa Rate Inquiry, Quotation, Booking, Order và Shipment.
3. Công thức giá, markup, margin, discount và giới hạn deal.
4. State machine của giá, báo giá, booking và shipment.
5. Ma trận quyền theo role, team, chi nhánh, phạm vi dữ liệu và cấp phê duyệt.

Các nội dung quan trọng khác cần tiếp tục làm rõ:

- Cấu trúc bảng giá, phụ phí, tỷ giá và điều kiện hiệu lực.
- Trường dữ liệu bắt buộc trước khi bàn giao booking.
- Quyền sửa, hủy hoặc quay lui trạng thái của từng vai trò.
- SLA, escalation và hạn mức bồi thường của Customer Service.
- Ranh giới chăm sóc khách hàng giữa Sales và Customer Service.
- Quy trình xuất hóa đơn, đối soát, ghi nhận thanh toán và xử lý nợ quá hạn.
- Quy tắc liên kết chứng từ với khách hàng, booking, shipment và hóa đơn.

## 6. Chiến lược triển khai

Hệ thống nên được xây theo hướng **domain-first** và triển khai theo một lát cắt nghiệp vụ xuyên suốt. Không nên xây các màn hình rời rạc trước rồi mới ghép nghiệp vụ sau.

Thứ tự tổng quát:

```text
Account và phân quyền
→ CRM và phân công khách hàng
→ Rate Inquiry
→ Pricing và duyệt giá
→ Quotation
→ Booking
→ Operations và Shipment
→ Customer Service
→ Invoice và công nợ
→ Dashboard và báo cáo
```

## 7. Kế hoạch theo giai đoạn

### Giai đoạn 0 — Chốt nghiệp vụ nền tảng

Đầu ra dự kiến:

- Bộ thuật ngữ chuẩn.
- Sơ đồ luồng nghiệp vụ.
- State transition matrix.
- Permission matrix.
- ERD phiên bản đầu.

### Giai đoạn 1 — Nền tảng tổ chức và phân quyền

- User và account lifecycle.
- Branch, department và team.
- Quan hệ quản lý trực tiếp.
- Role và permission.
- Data scope theo cá nhân, team, chi nhánh và toàn công ty.
- Account activation, locking và deactivation.
- Audit log.
- File attachment.
- Activity history.
- Notification foundation.

### Giai đoạn 2 — CRM và Sales Ownership

- CRUD khách hàng và người liên hệ.
- Lifecycle, classification, service capability và financial status.
- Tags và nhu cầu dịch vụ.
- Phân công Sales chính và Sales hỗ trợ.
- Lịch sử chuyển giao khách hàng.
- Customer interaction.
- Kiểm soát dữ liệu khách hàng trùng.
- Phạm vi truy cập theo cá nhân và team.

### Giai đoạn 3 — Pricing

- Carrier, service, route, currency và surcharge.
- Rate inquiry từ Sales.
- Thu thập và so sánh nhiều phương án giá.
- Price version và thời gian hiệu lực.
- Quy trình trình duyệt, từ chối, phê duyệt và công bố.
- Cấu hình phạm vi deal của Sales.
- Audit toàn bộ thay đổi giá.

Giá gốc, phụ phí, phiên bản, điều kiện áp dụng và quy trình duyệt nên được mô hình hóa riêng, không dồn vào một bảng giá duy nhất.

### Giai đoạn 4 — Quotation

- Tạo báo giá từ rate inquiry.
- Tính giá bán và margin.
- Kiểm tra giới hạn deal.
- Quy trình duyệt ngoại lệ.
- Snapshot dữ liệu giá.
- Version báo giá.
- Gửi báo giá cho khách hàng.
- Theo dõi trạng thái accepted, rejected và expired.
- Chuyển báo giá được chấp nhận thành booking.

### Giai đoạn 5 — Booking và Operations

- Tạo booking từ quotation.
- Checklist dữ liệu bắt buộc.
- Bàn giao Sales sang Operations.
- Review, yêu cầu bổ sung hoặc tiếp nhận.
- Phân công Operations Staff và Pickup Staff.
- Quản lý shipment, package/container và milestone.
- Ghi nhận sự cố.
- Upload chứng từ và bằng chứng giao nhận.
- Audit trạng thái và người phụ trách.

Định hướng mô hình:

- **Booking:** Cam kết sử dụng dịch vụ.
- **Shipment:** Lô hàng thực tế được vận hành.
- **Pickup task:** Nhiệm vụ lấy hoặc giao hàng cụ thể.

Định nghĩa cuối cùng cần được xác nhận theo hoạt động thực tế của công ty.

### Giai đoạn 6 — Customer Service

- Ticket và customer interaction.
- Phân công Customer Sale.
- Priority và SLA.
- Theo dõi shipment theo phạm vi được cấp.
- Chuyển xử lý sang Sales, Operations hoặc Accounting.
- Escalation.
- Lịch sử trao đổi đa kênh.
- Giới hạn giảm phí hoặc bồi thường.
- Khảo sát mức độ hài lòng.

### Giai đoạn 7 — Accounting

- Invoice và invoice items.
- Khoản phải thu.
- Thanh toán từng phần.
- Phân bổ payment vào invoice.
- Hạn thanh toán và số dư.
- Credit limit.
- Công nợ quá hạn.
- Đối soát doanh thu, chi phí và lợi nhuận theo shipment.
- Quy trình điều chỉnh và phê duyệt.

Sau khi nghiệp vụ kế toán ổn định mới tích hợp AI chứng từ theo luồng:

```text
Upload file gốc
→ AI trích xuất dữ liệu
→ Người dùng kiểm tra
→ Người có quyền xác nhận
→ Ghi nhận vào phân hệ nghiệp vụ phù hợp
```

### Giai đoạn 8 — Notification và báo cáo

- Event-driven notification.
- Template theo kênh.
- Lịch gửi.
- Cơ chế chống gửi trùng.
- Retry khi gửi lỗi.
- Dashboard theo vai trò.
- Báo cáo doanh số, margin, vận hành, SLA và công nợ.

## 8. Quy trình triển khai một module

Mỗi module nên được thực hiện theo trình tự:

1. Chốt business rules.
2. Thiết kế state machine.
3. Thiết kế database.
4. Xây authorization policy.
5. Xây service/action xử lý nghiệp vụ.
6. Viết feature tests cho happy path, failure path và edge cases.
7. Xây controller/API.
8. Xây giao diện.
9. Kiểm thử xuyên vai trò và phạm vi dữ liệu.
10. Cập nhật `BUSINESS_LOGIC.md` khi có quyết định nghiệp vụ mới.

## 9. Phạm vi MVP đầu tiên

MVP đầu tiên tập trung vào luồng:

```text
Admin tạo account
→ Phân quyền Sales, Pricing và Ban Lãnh Đạo
→ Sales tạo khách hàng
→ Sales Manager phân công khách hàng
→ Sales tạo rate inquiry
→ Pricing nhập và trình giá
→ Ban Lãnh Đạo duyệt giá gốc
→ Pricing công bố giá
→ Sales tạo quotation
→ Khách hàng chấp nhận
→ Tạo booking
```

Lát cắt này đủ để kiểm chứng các phần kiến trúc quan trọng:

- Phân quyền theo hành động và phạm vi dữ liệu.
- Workflow phê duyệt.
- Versioning.
- Audit log.
- Snapshot dữ liệu giá.
- Sự liên kết giữa CRM, Pricing, Quotation và Booking.

Operations, Customer Service và Accounting sẽ được nối tiếp sau khi luồng MVP trên hoạt động ổn định.

## 10. Bước triển khai đầu tiên

Bước đầu tiên là chốt mô hình **Account – Organization – Permission**, sau đó thiết kế ERD nền tảng cho:

- User.
- Branch.
- Department.
- Team.
- Team membership.
- Reporting line.
- Role.
- Permission.
- User role.
- Data scope.
- Audit log.

Đây là nền móng được sử dụng bởi gần như toàn bộ module phía sau. Sau khi mô hình này được xác nhận, có thể bắt đầu migration, model, policy và feature test cho giai đoạn 1.

## 11. Nguyên tắc quản lý tài liệu

- `BUSINESS_LOGIC.md` là nguồn sự thật về nghiệp vụ.
- Tài liệu này là nguồn kế hoạch triển khai.
- Quyết định nghiệp vụ mới phải được cập nhật vào `BUSINESS_LOGIC.md` trước hoặc đồng thời với việc triển khai.
- Trạng thái đề xuất không được tự động xem là yêu cầu đã xác nhận.
- Mọi thay đổi kiến trúc lớn phải được đối chiếu với luồng nghiệp vụ xuyên phòng ban.

## 12. Nhật ký

| Ngày | Nội dung |
|---|---|
| 2026-07-10 | Khởi tạo kế hoạch triển khai từ nội dung trao đổi và `BUSINESS_LOGIC.md`. |
