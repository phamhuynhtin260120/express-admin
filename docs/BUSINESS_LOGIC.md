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

### 3.2. Phân cấp tài khoản Sales

**Trạng thái: Đã xác nhận ở mức nghiệp vụ ban đầu**

Nhóm Sales được phân thành ít nhất hai vai trò:

#### Sales Manager

Sales Manager có trách nhiệm quản lý đội Sales và được phép:

- Tiếp nhận giá gốc do Ban Lãnh Đạo gửi xuống.
- Xem danh sách khách hàng và thông tin khách hàng của từng Sales thuộc phạm vi mình quản lý.
- Xem được Sales nào đang phụ trách từng khách hàng.
- Bàn giao khách hàng từ Sales này sang Sales khác.
- Quản lý giới hạn phần trăm tối thiểu và tối đa theo từng dịch vụ để Sales Representative có thể thương lượng giá với khách hàng trong phạm vi cho phép.

Việc bàn giao khách hàng phải lưu lịch sử, bao gồm Sales cũ, Sales mới, thời điểm hiệu lực, người thực hiện và lý do bàn giao.

#### Sales Representative (Saler)

Sales Representative chỉ được xem và thao tác trên các khách hàng mà mình đang phụ trách, trừ khi được cấp quyền ngoại lệ.

Sales Representative được sử dụng các cơ chế chăm sóc khách hàng do công ty cho phép, bao gồm:

- Gửi thông tin khuyến mãi.
- Gửi chương trình giảm giá.
- Gửi chương trình chăm sóc hoặc nội dung truyền thông khác.
- Liên hệ qua Zalo, email, Facebook, số điện thoại hoặc các kênh khác được công ty tích hợp và cho phép.

Hoạt động chăm sóc khách hàng phải đi qua kênh của công ty, tuân theo quyền được cấp và được lưu lịch sử để phục vụ theo dõi, kiểm tra và tránh gửi trùng.

### 3.3. Flow phân quyền cho nhóm Sales

**Trạng thái: Đề xuất dựa trên yêu cầu đã xác nhận**

```text
Admin tạo hoặc kích hoạt account
→ Gắn account vào bộ phận Sales
→ Chọn Sales Manager hoặc Sales Representative
→ Gắn vào team và người quản lý trực tiếp
→ Cấp phạm vi dữ liệu
→ Cấp quyền sử dụng các kênh chăm sóc khách hàng
→ Kích hoạt account
```

Phạm vi dữ liệu mặc định:

- **Sales Manager:** Khách hàng và hoạt động của các Sales trong team mình quản lý.
- **Sales Representative:** Chỉ khách hàng đang được phân công cho chính mình.

### 3.4. Quy tắc giá cần làm rõ

**Trạng thái: Cần làm rõ**

- Phần trăm tối thiểu và tối đa là tỷ lệ markup trên giá gốc, biên lợi nhuận, tỷ lệ giảm giá hay một công thức khác.
- Giới hạn được cấu hình theo dịch vụ, tuyến vận chuyển, hãng, khách hàng hay đồng thời nhiều điều kiện.
- Sales Representative được xem giá gốc hay chỉ được xem khoảng giá bán cho phép.
- Khi giá nằm ngoài khoảng cho phép, hệ thống chặn hoàn toàn hay tạo yêu cầu Sales Manager phê duyệt.
- Sales Manager có được tự thay đổi giới hạn hay giới hạn phải do Ban Lãnh Đạo phê duyệt.
- Giá gốc và giới hạn deal có thời gian hiệu lực hay không.

### 3.5. Khung phân tích các nhóm account còn lại

**Trạng thái: Đề xuất để tiếp tục thảo luận**

Mỗi nhóm account được phân tích theo năm yếu tố:

1. Phạm vi dữ liệu được phép xem.
2. Hành động được phép thực hiện.
3. Giới hạn nghiệp vụ.
4. Hành động cần phê duyệt hoặc không được tự phê duyệt.
5. Dữ liệu bắt buộc lưu audit log.

#### Ban Lãnh Đạo

**Trạng thái: Đã xác nhận đối với flow phê duyệt giá; các quyền khác vẫn là đề xuất**

Phạm vi:

- Xem dashboard, doanh thu, chi phí, lợi nhuận, công nợ và hiệu suất toàn công ty hoặc theo chi nhánh.
- Xem giá gốc, giá bán và biên lợi nhuận.
- Tiếp nhận dữ liệu giá hãng đã được Pricing thu thập và chuẩn hóa.
- Xem xét, điều chỉnh nếu cần và chốt giá gốc chính thức của công ty.
- Trả kết quả phê duyệt cho Pricing để Pricing công bố đến các phòng ban liên quan.
- Thiết lập chính sách biên lợi nhuận, mức giảm giá và hạn mức phê duyệt.
- Phê duyệt các trường hợp ngoại lệ có ảnh hưởng lớn đến lợi nhuận hoặc rủi ro tài chính.
- Không nhất thiết trực tiếp chỉnh sửa dữ liệu vận hành hằng ngày.

Các nội dung cần làm rõ:

- Ban Lãnh Đạo là một role chung hay gồm Giám đốc, Phó giám đốc và quản lý chi nhánh với phạm vi khác nhau.
- Những ngưỡng nào bắt buộc chuyển lên Ban Lãnh Đạo phê duyệt.
- Thành viên nào của Ban Lãnh Đạo được chốt giá và có cần nhiều cấp duyệt hay không.

#### Pricing

**Trạng thái: Đã xác nhận đối với flow thu thập và phân phối giá; các quyền khác vẫn là đề xuất**

Phạm vi:

- Tiếp nhận yêu cầu kiểm tra giá từ Sales.
- Thu thập giá từ các hãng.
- Quản lý giá đầu vào theo hãng, tuyến, dịch vụ và thời gian hiệu lực.
- Quản lý phụ phí, loại tiền tệ và điều kiện áp dụng.
- Chuẩn hóa và so sánh phương án từ nhiều hãng.
- Lập bộ giá đề xuất và gửi Ban Lãnh Đạo xem xét.
- Nhận giá gốc đã được Ban Lãnh Đạo chốt.
- Công bố và gửi giá đã duyệt đến các phòng ban liên quan theo đúng phạm vi được phép.
- Xem thông tin nhu cầu của khách hàng nhưng không mặc định được xem toàn bộ dữ liệu CRM nhạy cảm.

Giới hạn đề xuất:

- Không tự ý thay đổi giá của báo giá đã phát hành.
- Không được công bố giá đang chờ Ban Lãnh Đạo phê duyệt.
- Không được tự thay đổi giá gốc đã được Ban Lãnh Đạo chốt; nếu cần thay đổi phải tạo phiên bản mới và gửi duyệt lại.
- Không trực tiếp bàn giao khách hàng.
- Mọi thay đổi giá gốc phải lưu phiên bản, thời gian hiệu lực và người thực hiện.

Flow giá đã xác nhận:

```text
Pricing thu thập giá từ hãng
→ Pricing chuẩn hóa, so sánh và lập bộ giá đề xuất
→ Gửi Ban Lãnh Đạo
→ Ban Lãnh Đạo xem xét và chốt giá gốc
→ Trả giá đã duyệt cho Pricing
→ Pricing công bố đến các phòng ban liên quan
→ Các phòng ban sử dụng giá theo quyền và thời gian hiệu lực
```

Trạng thái giá đề xuất:

- **Draft:** Pricing đang tổng hợp.
- **Pending Approval:** Đã gửi Ban Lãnh Đạo, chưa được phép sử dụng.
- **Rejected:** Ban Lãnh Đạo từ chối và yêu cầu điều chỉnh.
- **Approved:** Ban Lãnh Đạo đã chốt nhưng Pricing chưa công bố.
- **Published:** Pricing đã công bố và các phòng ban được phép sử dụng.
- **Expired:** Giá đã hết thời gian hiệu lực.
- **Superseded:** Giá đã được thay thế bởi một phiên bản mới.

Mỗi lần gửi duyệt, từ chối, chốt hoặc công bố phải lưu người thực hiện, thời gian, phiên bản giá, ghi chú và phòng ban nhận giá.

#### Operations và Pickup

**Trạng thái: Đề xuất để thảo luận**

Nhóm vận hành có thể được phân thành ba cấp account:

##### Operations Manager

- Xem toàn bộ booking và shipment trong team, chi nhánh hoặc phạm vi được giao.
- Tiếp nhận booking đã được Sales bàn giao.
- Kiểm tra booking đã đủ dữ liệu để vận hành hay chưa.
- Phân công Operations Staff và Pickup Staff phụ trách.
- Điều phối lại người phụ trách khi phát sinh quá tải, nghỉ phép hoặc sự cố.
- Theo dõi tiến độ, shipment trễ hạn và các trường hợp ngoại lệ.
- Phê duyệt yêu cầu hủy, thay đổi lịch hoặc thay đổi thông tin vận hành quan trọng trong hạn mức được cấp.
- Chuyển sự cố nghiêm trọng lên Ban Lãnh Đạo hoặc bộ phận có thẩm quyền.

##### Operations Staff / Coordinator

- Chỉ xem booking và shipment được giao cho mình, trừ khi được cấp phạm vi theo team.
- Xác nhận tiếp nhận công việc.
- Kiểm tra chứng từ và thông tin vận hành.
- Làm việc với hãng, kho, cảng, sân bay hoặc đối tác vận chuyển.
- Lập kế hoạch pickup và giao việc cho Pickup Staff theo cơ chế được phép.
- Cập nhật mốc hành trình, trạng thái vận chuyển và thời gian dự kiến.
- Ghi nhận sự cố, mức độ ảnh hưởng và phương án xử lý.
- Tải lên chứng từ hoặc hình ảnh liên quan đến shipment.
- Phối hợp với Sales và Customer Service khi thông tin thay đổi.

##### Pickup Staff

- Chỉ xem các nhiệm vụ pickup hoặc giao hàng được giao cho mình.
- Xem địa chỉ, người liên hệ, số điện thoại, thời gian hẹn và hướng dẫn lấy/giao hàng cần thiết.
- Xác nhận nhận việc, bắt đầu, đến điểm lấy hàng, lấy hàng thành công, giao hàng và hoàn tất.
- Ghi nhận lý do thất bại hoặc chậm trễ.
- Tải lên ảnh hàng hóa, biên bản, chữ ký, bill hoặc bằng chứng giao nhận.
- Không được xem dữ liệu khách hàng ngoài phạm vi cần thiết cho nhiệm vụ.

Flow vận hành đề xuất:

```text
Sales xác nhận booking
→ Bàn giao booking cho Operations
→ Operations Manager kiểm tra tính đầy đủ
→ Trả lại Sales bổ sung hoặc chấp nhận booking
→ Phân công Operations Staff
→ Operations Staff lập kế hoạch và phân công Pickup Staff
→ Pickup Staff thực hiện và cập nhật bằng chứng
→ Operations Staff tiếp tục theo dõi shipment
→ Customer Service nhận trạng thái để thông báo khách hàng
→ Operations Manager xác nhận hoàn tất hoặc xử lý ngoại lệ
```

Trạng thái tiếp nhận công việc đề xuất:

- **Pending Handover:** Sales chưa bàn giao đầy đủ.
- **Pending Review:** Operations đang kiểm tra dữ liệu.
- **Need More Information:** Thiếu thông tin, trả lại Sales bổ sung.
- **Accepted:** Operations đã tiếp nhận.
- **Assigned:** Đã phân công nhân sự vận hành.
- **In Progress:** Đang thực hiện.
- **Exception:** Có sự cố cần xử lý.
- **Operationally Completed:** Đã hoàn tất phần vận hành.
- **Cancelled:** Đã hủy theo đúng thẩm quyền.

Giới hạn và kiểm soát đề xuất:

- Operations và Pickup không được xem giá gốc, margin hoặc công nợ nếu không cần cho nhiệm vụ.
- Không được sửa giá bán, công nợ hoặc thông tin tài chính đã xác nhận.
- Pickup Staff không được tự đổi booking, shipment hoặc khách hàng liên quan đến nhiệm vụ.
- Không được tự hủy booking hoặc shipment nếu chưa có phê duyệt theo quy trình.
- Mọi thay đổi người phụ trách, trạng thái, lịch trình và thông tin quan trọng phải lưu audit log.
- Hình ảnh và bằng chứng giao nhận phải lưu người tải lên, thời gian, vị trí nếu được phép và shipment liên quan.

Các nội dung cần làm rõ:

- Pickup Staff là nhân viên nội bộ, tài xế, đối tác bên ngoài hay có thể gồm cả ba loại.
- Operations Manager quản lý theo team, chi nhánh, loại dịch vụ hay tuyến vận chuyển.
- Operations Staff được tự chọn Pickup Staff hay chỉ Operations Manager/Dispatcher được phân công.
- Trạng thái shipment nào do hệ thống tự cập nhật, trạng thái nào do nhân viên cập nhật và trạng thái nào cần quản lý xác nhận.
- Có cần lưu vị trí GPS và theo dõi thời gian thực của Pickup Staff hay không.
- Trường dữ liệu tối thiểu nào Sales phải hoàn thành trước khi được phép bàn giao booking.

#### Customer Service

Phạm vi đề xuất:

- Xem thông tin liên hệ của khách hàng và các shipment được giao chăm sóc.
- Theo dõi hành trình và cập nhật trạng thái cho khách hàng.
- Gửi thông báo qua các kênh công ty cho phép.
- Tiếp nhận, phân loại và xử lý yêu cầu hoặc khiếu nại.
- Ghi nhận lịch sử trao đổi và mức độ hài lòng của khách hàng.
- Phối hợp với Sales và Operations khi có sự cố.

Giới hạn đề xuất:

- Không mặc định được xem giá gốc hoặc biên lợi nhuận.
- Không được cam kết giảm giá hoặc bồi thường ngoài hạn mức cho phép.
- Nội dung gửi hàng loạt hoặc nội dung nhạy cảm có thể cần phê duyệt.

#### Accounting

Phạm vi đề xuất:

- Xem booking, shipment và thông tin khách hàng cần thiết để lập hóa đơn và đối soát.
- Tạo hóa đơn, ghi nhận thanh toán và theo dõi công nợ.
- Quản lý hạn thanh toán và hạn mức tín dụng.
- Xác nhận dữ liệu được AI trích xuất từ bill hoặc hóa đơn trước khi ghi nhận kế toán.
- Gửi hoặc lên lịch cảnh báo công nợ.
- Xem doanh thu, chi phí và lợi nhuận theo phạm vi được giao.

Giới hạn và kiểm soát đề xuất:

- Không xóa chứng từ đã phát hành; việc điều chỉnh phải tạo lịch sử hoặc chứng từ thay thế.
- Người tạo khoản thanh toán không nên tự phê duyệt khoản thanh toán đó nếu công ty áp dụng nguyên tắc tách nhiệm vụ.
- Thay đổi công nợ, hạn mức tín dụng và trạng thái thanh toán phải có audit log.
- Các khoản xóa nợ, giảm nợ hoặc điều chỉnh lớn phải qua cấp có thẩm quyền phê duyệt.

#### Admin hệ thống

Phạm vi đề xuất:

- Tạo, kích hoạt, khóa và ngừng hoạt động account.
- Gán role, team, chi nhánh và phạm vi dữ liệu.
- Cấu hình quyền sử dụng các tính năng và kênh tích hợp.
- Xem audit log phục vụ quản trị và bảo mật.

Giới hạn đề xuất:

- Quyền quản trị kỹ thuật không mặc nhiên đồng nghĩa với quyền phê duyệt nghiệp vụ.
- Admin không nên tự sửa giá, công nợ hoặc kết quả kinh doanh chỉ vì có quyền quản trị hệ thống.
- Các thao tác giả lập người dùng hoặc truy cập dữ liệu nhạy cảm phải được ghi log đặc biệt.

### 3.6. Luồng phối hợp account

**Trạng thái: Đã xác nhận đối với flow giá; các bước sau báo giá vẫn là đề xuất**

```text
Pricing thu thập và chuẩn hóa giá hãng
→ Ban Lãnh Đạo chốt giá gốc
→ Pricing công bố giá đã duyệt
→ Sales Manager thiết lập phạm vi deal
→ Sales Representative tư vấn và báo giá khách hàng
→ Operations/Pickup thực hiện booking và vận chuyển
→ Customer Service theo dõi và giao tiếp với khách hàng
→ Accounting xuất hóa đơn, ghi nhận thanh toán và theo dõi công nợ
→ Ban Lãnh Đạo theo dõi báo cáo và phê duyệt ngoại lệ
```

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
- Tải lên và trích xuất dữ liệu từ bill, hóa đơn bằng AI.
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

### 8.3. Trích xuất bill và hóa đơn bằng AI

**Trạng thái: Đã xác nhận ở mức ý tưởng; nơi lưu và quy trình xử lý cần làm rõ**

Hệ thống cho phép người dùng tải lên hình ảnh bill hoặc hóa đơn. AI sẽ nhận diện và trích xuất nội dung văn bản cùng các trường dữ liệu có cấu trúc để phục vụ việc nhập liệu.

Dữ liệu có thể trích xuất bao gồm nhưng không giới hạn:

- Số bill hoặc số hóa đơn.
- Ngày phát hành và hạn thanh toán.
- Bên phát hành và bên nhận.
- Mã số thuế.
- Loại tiền tệ.
- Tiền hàng, thuế, phụ phí và tổng tiền.
- Mã booking, shipment hoặc đơn hàng liên quan nếu nhận diện được.
- Nội dung văn bản gốc được AI nhận diện.
- Mức độ tin cậy của từng trường dữ liệu.

Luồng xử lý đề xuất:

```text
Người dùng tải hình ảnh
→ Hệ thống lưu file gốc
→ AI nhận diện và trích xuất dữ liệu
→ Người dùng kiểm tra, chỉnh sửa và xác nhận
→ Hệ thống lưu dữ liệu vào phân hệ phù hợp
```

AI không nên tự động ghi nhận công nợ hoặc cập nhật dữ liệu kế toán khi chưa có bước kiểm tra và xác nhận của người có quyền.

Các nội dung cần làm rõ:

- Dữ liệu sau khi xác nhận sẽ được lưu vào công nợ, hóa đơn, chứng từ hay một khu vực tài liệu dùng chung.
- Hệ thống hỗ trợ những loại chứng từ nào ngoài bill và hóa đơn.
- Định dạng file, dung lượng và số lượng file được phép tải lên.
- Có cần hỗ trợ tài liệu nhiều trang hoặc PDF hay không.
- Chứng từ được liên kết với khách hàng, booking, shipment hoặc đơn hàng theo quy tắc nào.
- Vai trò nào được tải lên, kiểm tra, xác nhận hoặc từ chối kết quả AI.
- Cách xử lý khi AI không đọc được, đọc thiếu hoặc nhận diện sai dữ liệu.

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
- Quyền truy cập phải xét cả hành động lẫn phạm vi dữ liệu: Sales Representative xem khách của mình, Sales Manager xem dữ liệu theo đội và Admin xem toàn công ty.
- Hoạt động bàn giao khách hàng và thay đổi giới hạn deal giá phải có audit log.
- Hoạt động chăm sóc khách hàng qua các kênh của công ty phải lưu lịch sử người gửi, người nhận, nội dung, kênh gửi, thời gian và kết quả gửi.

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
10. Nơi lưu, quy trình xác nhận và liên kết dữ liệu được AI trích xuất từ bill hoặc hóa đơn.
11. Ý nghĩa và công thức của phần trăm tối thiểu/tối đa trong cơ chế deal giá của Sales.
12. Phạm vi team của Sales Manager và quy trình phê duyệt khi Sales muốn báo giá ngoài giới hạn.
13. Phạm vi quyền chi tiết của Ban Lãnh Đạo, Pricing, Operations/Pickup, Customer Service, Accounting và Admin.
14. Các ngưỡng phê duyệt và nguyên tắc tách người tạo với người duyệt ở từng nghiệp vụ.
15. Danh sách phòng ban nhận từng loại giá và mức độ chi tiết mà mỗi phòng ban được phép xem.
16. Cơ cấu Operations Manager, Operations Staff và Pickup Staff; cơ chế phân công và dữ liệu tối thiểu để nhận booking.

## 12. Nhật ký cập nhật

| Ngày | Nội dung |
|---|---|
| 2026-07-10 | Bổ sung đề xuất phân quyền Operations Manager, Operations Staff và Pickup Staff cùng flow bàn giao booking và thực hiện vận hành. |
| 2026-07-10 | Xác nhận flow Pricing thu thập giá hãng, Ban Lãnh Đạo chốt giá gốc và Pricing công bố giá đã duyệt đến các phòng ban liên quan. |
| 2026-07-10 | Bổ sung khung đề xuất để phân tích quyền của Ban Lãnh Đạo, Pricing, Operations/Pickup, Customer Service, Accounting và Admin hệ thống. |
| 2026-07-10 | Bổ sung phân cấp Sales Manager và Sales Representative, phạm vi khách hàng, quyền bàn giao, giới hạn deal giá và các kênh chăm sóc khách hàng. |
| 2026-07-10 | Bổ sung tính năng tải hình ảnh bill hoặc hóa đơn, dùng AI trích xuất dữ liệu và chờ xác định nơi lưu sau bước kiểm tra. |
| 2026-07-10 | Khởi tạo tài liệu từ nội dung thảo luận ban đầu: tổng quan hệ thống, actors, CRM, phân công Sales, giá, booking, đơn hàng, công nợ và email cảnh báo. |
