# Business Logic

## 1. Mục đích tài liệu

Tài liệu này là nguồn thông tin tập trung cho toàn bộ nghiệp vụ của dự án. Mọi nội dung nghiệp vụ được thống nhất trong quá trình thảo luận sẽ được cập nhật tại đây.

Quy ước trạng thái:

- **Đã xác nhận:** Yêu cầu hoặc quy tắc đã được chủ dự án xác nhận.
- **Đề xuất:** Phương án đang được xem xét, chưa phải yêu cầu cuối cùng.
- **Cần làm rõ:** Nội dung cần tiếp tục thảo luận trước khi triển khai.

## 2. Tổng quan dự án

**Trạng thái: Đã xác nhận**

Dự án là dashboard quản trị tổng thể cho một công ty logistics. Hệ thống hỗ trợ các bộ phận phối hợp trong toàn bộ quá trình từ tìm kiếm và chăm sóc khách hàng, kiểm tra giá, báo giá, booking, vận hành đơn hàng đến xuất hóa đơn và quản lý công nợ.

Tên gọi tiếng Anh phù hợp cho phạm vi sản phẩm: **Logistics Operations & CRM Management Platform**.

## 3. Nhóm người dùng

**Trạng thái: Đã xác nhận ở mức tổng quan**

- Admin.
- Sales.
- Pickup.
- Kế toán (Accounting).
- Chăm sóc khách hàng (Customer Service).
- Các vai trò khác có thể được bổ sung khi nghiệp vụ được làm rõ.

### 3.1. Phạm vi trách nhiệm đề xuất

**Trạng thái: Đề xuất**

- **Admin:** Quản lý cấu hình hệ thống, tài khoản, vai trò, quyền hạn và dữ liệu toàn công ty.
- **Sales:** Quản lý khách hàng được giao, tiếp nhận nhu cầu, yêu cầu kiểm tra giá, lập báo giá, tạo booking và theo dõi doanh số.
- **Pricing:** Quản lý bảng giá, phụ phí, so sánh hãng và phản hồi yêu cầu kiểm tra giá.
- **Pickup/Operations:** Tiếp nhận booking, tổ chức pickup, vận hành lô hàng và cập nhật trạng thái.
- **Accounting:** Quản lý hóa đơn, thanh toán, công nợ và hạn mức tín dụng.
- **Customer Service:** Theo dõi hành trình, cập nhật cho khách hàng và xử lý sự cố.
- **Manager:** Phê duyệt các trường hợp đặc biệt như giảm giá, margin thấp, chuyển giao khách hàng, hủy đơn hoặc vượt hạn mức tín dụng.

Một người dùng có thể đảm nhiệm nhiều vai trò. Quyền truy cập nên được kiểm soát theo hành động và phạm vi dữ liệu, không chỉ dựa trên tên vai trò.

## 4. Các phân hệ chính

**Trạng thái: Đã xác nhận ở mức tổng quan**

- Dashboard quản lý tổng thể.
- CRM và quản lý khách hàng.
- Phân công khách hàng cho Sales.
- Kiểm tra giá và quản lý bảng giá.
- Báo giá.
- Booking.
- Quản lý đơn hàng/lô hàng.
- Pickup và vận hành.
- Kế toán và công nợ.
- Email cảnh báo và thông báo.
- Báo cáo quản trị.

## 5. CRM và quản lý khách hàng

### 5.1. Phân loại khách hàng

**Trạng thái: Đã xác nhận ở mức nhu cầu; mô hình chi tiết là đề xuất**

Hệ thống phải quản lý được nhiều nhóm khách hàng, bao gồm nhưng không giới hạn:

- Khách hàng tiềm năng.
- Khách hàng đang phục vụ.
- Khách hàng có thể phục vụ.
- Các loại khách hàng khác phát sinh trong quá trình vận hành.

Không nên gom toàn bộ đặc điểm vào một trường loại khách hàng duy nhất. Mô hình đề xuất gồm:

- **Lifecycle status:** Lead, Prospect, Qualified, Active Customer, Inactive, Lost hoặc Blacklisted.
- **Service capability:** Có thể phục vụ, có thể phục vụ với điều kiện hoặc không thể phục vụ.
- **Classification:** VIP, thông thường hoặc rủi ro.
- **Financial status:** Bình thường, sắp quá hạn hoặc quá hạn.
- **Tags/needs:** Đường biển, đường hàng không, nội địa, xuất khẩu, nhập khẩu và các nhu cầu khác.

Luồng vòng đời khách hàng tham khảo:

```text
Lead → Prospect → Qualified → Active Customer → Inactive / Lost / Blacklisted
```

### 5.2. Phân công khách hàng cho Sales

**Trạng thái: Đã xác nhận**

Hệ thống phải xác định được khách hàng thuộc Sales nào tại từng thời điểm. Chỉ lưu Sales hiện tại trên khách hàng là không đủ; cần lưu lịch sử phân công.

Mỗi lần phân công hoặc chuyển giao dự kiến lưu:

- Khách hàng.
- Sales phụ trách.
- Thời gian bắt đầu.
- Thời gian kết thúc.
- Người thực hiện phân công.
- Lý do chuyển giao.
- Vai trò Sales chính hoặc Sales hỗ trợ.
- Ghi chú.
- Trạng thái xác nhận bàn giao.

Lịch sử này được dùng để truy vết trách nhiệm chăm sóc, tính doanh số, tính hoa hồng và xử lý tranh chấp.

## 6. Kiểm tra giá và quản lý giá

### 6.1. Nhu cầu đã xác nhận

**Trạng thái: Đã xác nhận**

- Cho phép kiểm tra giá theo nhu cầu cụ thể của khách hàng.
- Cho phép kiểm tra và so sánh giá theo từng hãng.
- Cho phép CRUD dữ liệu giá.

### 6.2. Các yếu tố cấu thành giá

**Trạng thái: Đề xuất**

Giá có thể phụ thuộc vào:

- Hãng vận chuyển.
- Loại dịch vụ: đường biển, đường hàng không, đường bộ hoặc loại khác.
- Điểm đi, điểm đến, cảng hoặc sân bay.
- Loại hàng.
- Trọng lượng và thể tích.
- Loại container hoặc kiện hàng.
- Incoterm.
- Ngày hiệu lực và ngày hết hạn.
- Phụ phí.
- Loại tiền tệ và tỷ giá.
- Giá mua, giá bán và margin.
- Điều kiện giá riêng theo khách hàng.

### 6.3. Luồng kiểm tra giá đề xuất

```text
Tiếp nhận nhu cầu khách hàng
→ Chuẩn hóa điều kiện tìm kiếm
→ Tìm bảng giá còn hiệu lực
→ So sánh nhiều hãng
→ Tính phụ phí và margin
→ Sales lựa chọn phương án
→ Lập và gửi báo giá
```

Báo giá nên lưu bản chụp dữ liệu giá tại thời điểm phát hành. Việc thay đổi bảng giá gốc sau đó không được làm thay đổi báo giá lịch sử.

## 7. Báo giá, booking và đơn hàng

### 7.1. Khái niệm đề xuất

**Trạng thái: Đề xuất, cần thống nhất thuật ngữ**

- **Rate Inquiry:** Yêu cầu kiểm tra giá.
- **Quotation/Quote:** Báo giá gửi cho khách hàng.
- **Booking:** Xác nhận đặt dịch vụ sau khi khách hàng chấp nhận.
- **Shipment/Order:** Lô hàng hoặc đơn hàng thực tế được vận hành.

### 7.2. Luồng nghiệp vụ tổng thể

**Trạng thái: Đề xuất**

```text
Yêu cầu kiểm tra giá
→ Lấy và so sánh giá hãng
→ Lập báo giá
→ Khách hàng chấp nhận
→ Tạo booking
→ Xác nhận với hãng
→ Pickup
→ Vận chuyển
→ Giao hàng
→ Xuất hóa đơn
→ Theo dõi công nợ
→ Hoàn tất
```

### 7.3. Các quy tắc cần làm rõ

**Trạng thái: Cần làm rõ**

- Ai được tạo, sửa hoặc hủy đơn ở từng trạng thái?
- Trạng thái nào được phép quay lui?
- Sau khi booking được xác nhận, những trường nào bị khóa?
- Hủy đơn có cần phê duyệt không?
- Thay đổi giá hoặc chi phí có bắt buộc lưu lịch sử không?
- Một booking có thể chứa nhiều shipment, container hoặc package không?
- Booking và shipment có phải hai đối tượng riêng trong mô hình thực tế của công ty không?

## 8. Kế toán và công nợ

### 8.1. Phạm vi đề xuất

**Trạng thái: Đã xác nhận nhu cầu quản lý công nợ; chi tiết là đề xuất**

- Hóa đơn.
- Khoản phải thu.
- Hạn thanh toán.
- Thanh toán từng phần.
- Số dư còn lại.
- Công nợ chưa đến hạn và quá hạn.
- Hạn mức tín dụng của khách hàng.
- Đối soát doanh thu, chi phí và lợi nhuận theo shipment.

### 8.2. Mốc cảnh báo tham khảo

**Trạng thái: Đề xuất**

- Trước hạn thanh toán 3 ngày.
- Đúng ngày thanh toán.
- Quá hạn 1 ngày.
- Quá hạn 7 ngày.
- Quá hạn 15 ngày.
- Quá hạn 30 ngày.
- Khi khách hàng vượt hạn mức tín dụng.

## 9. Email cảnh báo và thông báo

### 9.1. Nhu cầu đã xác nhận

**Trạng thái: Đã xác nhận**

Hệ thống cần gửi email cảnh báo hoặc thông báo đến khách hàng về:

- Công nợ.
- Nợ quá hạn.
- Giá cả.
- Trạng thái đơn hàng.
- Những nội dung khác liên quan đến khách hàng và hàng hóa.

### 9.2. Mô hình xử lý đề xuất

```text
Sự kiện → Điều kiện → Người nhận → Mẫu nội dung → Kênh gửi
```

Ví dụ:

- Đơn thay đổi trạng thái: thông báo khách hàng và Customer Service.
- Giá sắp hết hạn: thông báo Sales.
- Công nợ sắp đến hạn: thông báo khách hàng và Kế toán.
- Booking chưa được hãng xác nhận: cảnh báo Operations.
- Lô hàng bị trì hoãn: thông báo khách hàng, Sales và Customer Service.

Hệ thống nên lưu lịch sử gửi gồm người nhận, nội dung, thời gian, trạng thái gửi và lỗi phát sinh; đồng thời cần cơ chế chống gửi trùng và hỗ trợ gửi lại.

## 10. Nguyên tắc dữ liệu và kiểm soát

**Trạng thái: Đề xuất**

- Các thay đổi quan trọng phải có audit log.
- Giá, phụ phí và tỷ giá phải có thời gian hiệu lực.
- Báo giá và đơn hàng phải giữ dữ liệu lịch sử tại thời điểm phát sinh.
- Không xóa cứng dữ liệu đã phát sinh giao dịch.
- Phân công khách hàng phải có lịch sử theo thời gian.
- Trạng thái booking và shipment cần có luồng chuyển trạng thái rõ ràng.
- Thông báo phải chống gửi trùng và có khả năng gửi lại.
- Quyền truy cập phải xét cả hành động lẫn phạm vi dữ liệu: Sales xem khách của mình, Manager xem dữ liệu theo đội và Admin xem toàn công ty.

## 11. Nội dung ưu tiên thảo luận tiếp theo

**Trạng thái: Đề xuất**

Ưu tiên làm rõ luồng xuyên suốt từ khi khách hàng yêu cầu giá đến khi hoàn tất công nợ, vì đây là luồng kết nối CRM, Pricing, Sales, Booking, Operations, Customer Service và Accounting.

Các điểm cần chốt tiếp theo:

1. Loại hình logistics mà công ty cung cấp.
2. Phân biệt chính xác Booking, Order và Shipment trong hoạt động thực tế.
3. Vòng đời khách hàng và điều kiện chuyển trạng thái.
4. Quy tắc xác lập và chuyển giao quyền sở hữu khách hàng cho Sales.
5. Luồng trạng thái của báo giá, booking và shipment.
6. Cấu trúc bảng giá, phụ phí và quy tắc tính margin.
7. Quy trình xuất hóa đơn, ghi nhận thanh toán và xử lý nợ quá hạn.
8. Ma trận phân quyền và các bước cần phê duyệt.
9. Danh sách sự kiện cần gửi email hoặc thông báo.

## 12. Nhật ký cập nhật

| Ngày | Nội dung |
|---|---|
| 2026-07-10 | Khởi tạo tài liệu từ nội dung thảo luận ban đầu: tổng quan hệ thống, actors, CRM, phân công Sales, giá, booking, đơn hàng, công nợ và email cảnh báo. |
