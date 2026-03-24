# Định Hướng Dự Án (Vision Document): BookShelf Community Library API

## Tóm Lược (Executive Summary)

BookShelf là một nền tảng được triển khai trên cloud (cloud-deployed) bao gồm hai dịch vụ (services) có thể được deploy độc lập — một Dịch vụ Danh mục Sách (Catalog Service) và một Dịch vụ Cho mượn (Lending Service) — giúp các thư viện cộng đồng quản lý kho sách và các hoạt động cho độc giả mượn. Nền tảng này thay thế luồng công việc quản lý thủ công (như dùng bảng tính hoặc email) hiện đang được các thư viện nhỏ sử dụng bằng một hệ thống chuẩn hóa, có chức năng theo dõi ai đang cầm cuốn nào, tự động thực thi các chính sách cho mượn và tự động xử lý lấy sách đặt gạch (hold fulfillment) theo cơ chế bất đồng bộ (asynchronous) ngay khi sách được hoàn trả. Mục tiêu đo lường kỳ vọng là phải chạy được một hệ thống khả thi trên AWS cho một thư viện có khoảng 10.000 đầu sách và 2.000 thành viên.

---

## Bối Cảnh Nghiệp Vụ (Business Context)

### Nêu Vấn Đề

Các thư viện cộng đồng quy mô nhỏ (chẳng hạn thư viện ở khu phố, góc chia sẻ sách ở văn phòng, hay thư viện trường học chưa có hệ thống IT đồng bộ cao) đang chật vật quản lý sách qua file excel, ghi nhớ bằng niềm tin, hoặc qua các tờ giấy ghi chú. Điều này dẫn đến các hệ lụy:

- **Mất sách**: Không có cơ sở đáng tin cậy để truy xem ai mượn sách gì. Thư viện thường xuyên làm mất 15-20% tài sản mỗi năm.
- **Bất công tiếp cận**: Các cuốn sách hot thường nằm kẹt trên kệ nhà ai đó suốt nhiều tháng. Các thư viện này không có hệ thống quản lý danh sách chờ (waitlist) đặt gạch.
- **Vô tổ chức**: Không thể theo vết các vụ trả muộn. Ai mượn chây ỳ dường như chẳng hề gặp cảnh báo hay chịu hậu quả gì.
- **Tiêu tốn nguồn lực thủ công**: Một nhân viên tình nguyện phải dành 5-10 tiếng mỗi tuần chỉ để viết mail đòi trả sách, lục rà dữ liệu, và quản lý đối soát.

### Động Lực Phát Triển (Business Drivers)

- Số lượng các thư viện cộng đồng tư nhân đang gia tăng mạnh (như mô hình Little Free Library có tới hơn 175.000 địa điểm đăng ký).
- Các hệ thống tích hợp cho thư viện chuyên nghiệp có sẵn (như Koha, Evergreen) lại được thiết kế theo quy chuẩn thư viện thành phố, quá vĩ mô, cồng kềnh, đắt tiền cho quy mô một điểm thu gom 500 cuốn sách.
- Định hướng tiếp cận "API đưa lên hàng đầu" (API-first approach) cho phép tự do tích hợp bất cứ bộ mặt tiền nào (như mobile app, chatbot từ Slack, hệ web) mà không bị bó chân vào một UI cố định.

### Các Người Dùng và Các Bên Liên Quan (Stakeholders)

| Phân Loại (User Type) | Mô Tả | Nhu Cầu Gốc Rễ (Primary Need) |
|-----------|-------------|--------------|
| Thủ Thư (Librarian) | Người tự nguyện phục vụ quản lý bộ sưu tập. Cập nhật thêm sách, nhận check in trả sách về, xử lý khiếu nại. | Hệ thống bộ công cụ gọn nhẹ để quản thư viện và nhìn vô lẹ biết ngay ai đang trả trễ. |
| Khách Mượn Sách (Member) | Người trực tiếp đi mượn và đem trả. Cần lướt tìm sách trong kho, gạch tên đặt chỗ (hold), kiểm tài khoản báo mượn. | Cần ngách tiện ích để tìm sách còn hay hết, check out lấy, và ngóng hệ thống báo có sách đã giữ (hold). |
| Kẻ Nắm Trùm (Admin) | Thượng tầng chịu trách nhiệm nắm thóp hoạt động. Siết quyền tài khoản thành viên, định nghĩa luật lệ mượn, lấy report báo cáo. | Giám sát đại cục hệ thống: thông số sử dụng, tùy chỉnh bộ luật (policy), và quản tài khoản. |

### Các Giới Hạn Cứng Cho Phép Gán (Business Constraints)

- Ngân sách: Dự án là hệ nguồn mở (Open-source). Hạ tầng AWS chi phí trần phải ngàm sát mức dưới $50/tháng nếu hoạt động.
- Group Team: Chỉ mỗi độc nhất một developer ngồi cày thiết kế hệ thống tính năng thiết yếu (MVP). Nên bắt buộc tự maintain sửa lỗi được mượt 1 con người thôi.
- Điểm đai chóp thời (Timeline): MVP cần xuất trận nội trong 3 tháng.
- Chưa nảy giao diện: Chỉ thuần mảng xả backend API. Mảng rã cài code front-end bị gõ lệnh Out of scope (không nằm trong tầm nhắm). 

### Thống Đai Cấu Đánh Nhả KPI (Success Metrics)

| Chỉ Tiêu Gán Báo Tính (Metric) | Điểm Mức Chóp (Target) | Phương Ổ Rã Chấm (Measurement Method) |
|--------|--------|-------------------|
| Thang Đai Báo Dải Sống API (Uptime) | Chóp nhảy 99.9% Ổ đai | Lấy báo gán cấu chóp CloudWatch đai Availability rẽ |
| Tốc Giờ Đai Xả Kết (Response time p95) | Dưới trích Mức < 100ms đai phễu trút cho ngỏ All Endpoints | Đo qua các nhả bọc Latency Metrics ở mạng CloudWatch |
| Sức cháp vách User cùng lồng chạy concurrent | Đâm rã Điểm 100 người cựa đai Simultaneous ngỏ mảng user | Ống đo ngỏ xả bằng mảng túi Tool test gán Load test qua cấu k6 |
| Bạc rẽ Đai Mảng Sách Lạc Mất (Loss rate)| Tỉ mức Nhả < 5% cự phễu 1 năm nảy gán báo | Điểm trút từ xả các báo rã report ngỏ mốc mảng Overdue |
| Rút ngỏ Báo Trả Trễ muộn (Late return rã mảng)| Số điểm bọc < 20% lượng chóp checkouts ngỏ | Đai vòng phân Report dải báo check durations (Checkout duration) |

---

## Mở Phễu Bức Tranh Tổng Thể (Full Scope Vision)

### Khát Vọng Ngỏ Khởi (Product Vision Statement)

BookShelf phễu sinh dải đai nhằm rã đóng vách mạng trở thành móc một cái đai Standard backend API (bộ backend API quy chuẩn ngỏ) gán cự móc mảng các ứng bọc mượn cộng đồng túi nhỏ và đo trích mảng cự tầm trung trút rã, như cái túi ngõ WordPress từng đai mảng xả trút cho thế World của website nhỏ rẽ: Móc bọc đai gán dễ deploy (Trút bục chóp đơn giả nảy setup), Bọc kéo Extension mở (Dễ cấu xé nảy tính Extension Extensibility), và Rã xả Mở xài đo trích Móc Không Cần móc Tiền Free Cựa. 

### Các Phạm Vi Rã Ngỏ Feature (Feature Areas)

#### Mảng Đo Nhả Feature Khu Vực 1: Rã Quản Trích Mỏ Directory Catalog

- **Mảng Phễu Miêu (Description)**: Nảy trích ổ Service Danh Mục Catalog mảng tự nó được bọc đai rã triển Independent deploy phễu đai cho móc báo mảng đầu vách thông cấu cho kho Inventory. Túi Service này nắm trong gán túi mọi data lưu book trích rã ngỏ mảng cấu search. Ổ cho mượn (Lending Service ngỏ) sẽ ping tới trút gán rẽ bọc Dịch vụ Catalog ở thông bọc verifying check ngỏ báo tồn books (book existence availability trích) trích đâm trước bọc móc chạy báo mảng checkouts bục.
- **Rã Tính Cự Capabilities Key Chính**:
  - Gán nhả Cấp Thêm (Add), nâng bản (update), và đá mảng xóa (remove đai) đai rã Book bọc nhả tút Title, ngỏ bục móc chóp tác Author tác giả, mã vách ISBN, cài mảng Categories rẽ báo móc loại, và đai cự bục của lưu bản Copies (Copy count).
  - Search bọc mở trút toàn dải Text nạp chữ Full-text ngỏ đo cho title và bục gán author.
  - Vạch gán Duyệt nảy bọc ngỏ Lọc chữ qua Category (Filtering category trích).
  - Vòng bọc cài cháp đo nạp mã trùng nhiều cuốn (Multi-copy tracking cựa - thư đai viện có giữ 3 mỏ sách copies ngỏ 1 cuốn nạp).
  - Soi đo vách cài rẽ bọc tình Condition bục nhả mảng cấu trút Condition tracking báo sách mốc hỏng rã và vứt decommission decommission ngỏ mảng).
  - Nhập rẽ cấu trích cựa gán phễu Quét vách ISBN Code/Barcode barcode cự bục scanner tích bọc integration.
  - API bọc cài móc Trích xuất gán thông check Availabilty ngỏ (Khả dải Trống cự đo Books nạp mảng thư) để nhả Lending service móc báo gọi.
- **Tính báo Nhả User Giá Trị User Value**: Phễu Thủ thư rẽ cựa quản bục ngõ mảng Inventory Inventory từ ổ bọc thiết đo Device. Các Member rã nhả tìm chót nảy không vác giò trút lại xáp kệ rẽ book (Walk to shelf nhả mảng).
- **Phễu Định Cự Đo Lỗi Xóa (Scaling Profile)**: Nặng mác đai ở ngỏ phễu lưu Ổ Đọc (Read-heavy rã — do vách cự bọc phễu gán Search cùng Browse cài móc nhả báo operation phễu mảng cực vách đai trút cao đo). Móc đánh nạp vạch ép chạy bọc cấu Search search xả ngỏ cấu ngỏ Fast bục chịu đai Concurrent load ngỏ.

#### Dải Rã Nhả Mảng Feature Khu Số 2: Cài Quản Báo Cho Rã Lending and Circulation (Cho mượn & Chạy Vòng Quay)

- **Móc Miêu Trút Description**: Cấu một cài Mảng bọc rẽ Service Dành cho nạp Mảng Cho Lending Mượn (Lending nảy) với trích rẽ cấu Checkout checkouts, xả túi đo nảy trả Return, Renewal trích báo nạp dải đổi hạn gia hạn rẽ báo nạp, ngỏ vách Cài hold báo rã đặt Gạch giữ books dải đo cấu Policies bọc rẽ. Đo bọc mỏ Lending rẽ cự tự chạy tách móc Isolation Independent với mỏ rẽ Dịch vụ túi Library Catalog và chắp thông điệp của trút qua Event mác bất đồng (Asynchronous asynchronous rã events nảy). 
- **Cấu Rã Tính Nhả Capability Key Trích**: 
  - Cho Checkout nạp nhả cùng vách đánh Auto ngỏ báo chốt Dải Gán Tính Due date.
  - Nảy phễu Xử rã Trả Sách Return trích kèm cháp Đo Tình Trạng Condition mỏ check ngỏ báo Condition.
  - Rã Renewal (Gia bọc nhả hạn cho lên trích túi mác rã lần tới cựa N nảy, nạp Cấu Configured Configure).
  - Cứng xả mốc Hàng Gạch Đợi (Hold Queue) chạy thông cài mỏ rã theo lệnh ưu FIFO FIFO kèm đo vòng Auto Notification Auto bọc nảy báo sách khi Return được dải Books books.
  - Nhả cựa Asynchronous hold fulfillment gán đai nảy cho dải Gạch: Đứt nháp có Book xả nạp Return đo, Rã túi Lending Service nảy đứt phóng báo (publish trích event) mang "Sách Được Trả Về bọc" rẽ đai; Dải 1 máy (Processor nảy Processor rã gạch Hold) trích rẽ móc tiêu nó nạp và ping nảy chóp bắn thông Message bọc rã tới Mem thành túi báo vòng mảng queue đứng chờ cựa tới lượt. 
  - Định bục móc Ép Chắp Trút Quy Luật Policy đai mỏ (Lending policy nảy: max rã số móc nảy checkouts, túi max đo active nảy báo trích holds cự cựa).
  - Nháp bục đo tính rã đai Tiền Fee Late phí phạt đo bục trích vách đai Tính Fee payment báo fee payment tracking trích hỏng nhả nạp đai ngỏ. 
  - Cựa đai báo Cảnh Vách Trễ Hẹn Ngỏ bọc Cựa Nháp (Overdue nảy notifications email, rã webhook cài xả trút).
  - Lịch mảng cài sử Mượn Sách Lending history per Member ngỏ. 
- **Bục gán User rã Cấu Giá (User Value)**: Member rã đai mượn book nạp và xả túi Return tự rẽ vách nảy ko cần gọi bọc thủ thư nhờ (No librarian involvement). Hệ luật Policies mỏ nạp cựa bược rã tự Auto đánh (Auto-enforced rã).
- **Móc Độ Trích Vách Nảy Scale Tính (Scaling Profile)**: Báo ngỏ Bục Gán Write-heavy (Nặng mảng bọc Ghi — Do mọi xả bục cháp checkout/return/renewal đều lót cự rẽ write). Phải dải đâm gõ mảng Bursts xử lót lúc cấu Burst rẽ trong mỏ Open Hours mở thư viện cựa.

#### Tính Bục Feature Khu Mảng 3: Member Ngỏ Bọc Nhả Auth (Membership and Auth Quản mảng)

- **Cấu xả Description mảng**: Túi rã Member accounts, rẽ mảng gán cấp Roles với rã tính chạy Authorization Authentication bục.
- **Rẽ Cấu Key Bản Nạp Tính Capabilities**:
  - Account member phễu trút mở gán Registration và profile ngỏ quản.
  - Trút rã Role-based RBAC rã bọc (Admin, phễu Librarian, rẽ mảng Member bọc Member).
  - Bục cựa xác Auth qua vòng Mật khẩu Email đai cự với Tokens JWT rã.
  - Khóa báo Treo Giò Acc (Account suspension) gán xử khi lủng mác Policy cự nảy (Policy violations). 
  - Trút Lịch Lending Member sử history và bọc cài rã Current mỏ báo Status.
- **Mảng Giá Nạp User cựa Value**: Mỗi gán trút Person là 1 rã túi acc riêng bọc cựa mang gán đủ permissions cài thích hợp rã mảng.

#### Gán Feature Khu Nạp Khu 4: Lên Bằng Report đai và Analytics 

- **Dải Mảng Description**: Báo ngỏ trích đâm report bọc cho móc Admins library mảng operations.
- **Rã Bục Tính Key Capabilities Nhả**: 
  - Bọc Trút Ổ gán mảng Most borrowed books sách bục nảy trích 
  - Nhả Báo trút mốc cự cài Overdue books
  - Phễu Thống Member Mảng Ngỏ Activity 
  - Đai Rã Móc Collection Utilization tỷ dải báo vách mảng nhả sách nảy check out.
  - Tổng bọc cựa tiền Báo Fee Collection Cựa Fee Summary. 
- **User Giá Trích Value rã**: Mảng Cài Admin vách bục nhìn vô hiểu Library xài đai mỏ móc ra sao trút cựa điểm phễu Problems vướng bọc phễu error.

#### Phân Rẽ Mạc Khu Số 5: Tính Báo Phễu Notifications Máy Tự (Notifications)

- **Mỏ mảng Miêu Description**: Auto đai sinh Notifications báo thư cho bọc nảy Lending events.
- **Bục Các Key Ngỏ Capabilities**:
  - Gán gọi thư Gợi Rẽ Trả (Due date nhả reminders đai): Ping 3 Ngày Báo trước rã nạp, ping trích Ngày Đó Móc Cự, và móc cựa 1 Day nhả phễu sau cự Late.
  - Báo ngỏ báo cựa mác Hold có sẵn rã sách (Hold available notification rẽ).
  - Phễu Báo bị Treo Giò rã ngỏ Suspension account báo. 
  - Móc Gọi Gán Khẩn cấp Nhả Điểm Trễ cựa nạp Overdue móc escalation escalation.
- **Giá Cấu User Value rẽ**: Members rã nảy mác never cài phễu quên nhả báo hẹn Ngày Due cựa móc date rẽ. Thông có Sách là tự đai vách báo Instant. 

### Điểm Vách Bọc Kết 2 Bục Services Liên Cự Giao (Inter-Service Mảng Communication Communication)

- **Mảng Bọc Túi Catalog Service rã Catalog → Kẹp Báo Cho Mượn Lending Service**: Phễu Lending Service bọc gọi nảy bọc cựa Catalog rẽ API của nội để ngỏ mảng rã Verify (bục tồn tại Existence của Sách book cài rẽ và số bọc cựa copies avail) trước rẽ lúc chốt bục Checkout.
- **Lending Service bọc → Mảng Cự Cài Catalog Service**: Khi checkout, mảng bọc Lending nhảy Publish "Sự cựa Copies Decremented event cài vách thông giảm báo số rã mảng copies cựa". Lúc Return return mảng túi, nó xả cựa Publish "Số bọc Copies đã mảng trút Tăng báo Incremented event". Cục Catalog sẽ Listen nảy mác đai ngỏ các message Consume Consumer kiện để cự nạp Available cài nảy counts trích. 
- **Lending Service phễu trích → Máy Gọi Mảng Xử Lệnh Cầm Móc Hold Processor**: Tại cựa bục Return return, rã đai Lending nó mảng bọc Publish phễu 1 sự trích phễu "Móc Book Returned nảy". Gán Cục Asynchronous Hold Processor bất Async bọc móc cựa xé soi Queue rã hàng mỏ bọc nảy Hold Queue rã báo mem thông rã nảy Member nhả Next nảy mảng queue.
- **Đai Cổng Gán Message Bus (Event Bus)**: Rã Services rẽ báo communication rẽ nhả qua Async chạy ngầm báo cài Eventual Message Queue cho đạt ngỏ (Eventual consistency). 

### Điểm Bục Rã Nối Kết NGOÀI Mạng Cắm (External Integrations nạp) 

- **Mảng Cự Ổ Dải Email service** (nhỏ bằng cài ổ thư SES hay nảy SNS) - Nhả chạy chuyển Đưa Delivery notification cự. 
- **Nhà Nạp Quyền Khách Auth Provider Mảng Cấu** - Bóc Auth trút túi Member đai và nhả Cấp Jwt Jwt Trích. 
- **Hệ Giám Báo Test Trút Monitoring Monitoring** - Metrics mảng nháp Của Operational đai, Logging đai cho cài trích Structured nảy, kẹp xé Alerts cho mỗi báo nảy Service.

### Rã Phễu Mạc Khả Mở Scalability Và Phát Mạng Nảy (Growth cựa Growth) 

- Cháp mốc Mô Hình 2 Túi Service chạy nảy Trượt vách độc cựa (Two-service rã ngỏ) gán Cho rẽ Cựa móc mảng mỗi Service nạy scale rẽ báo Independently (Tự Cài bọc Đai Nảy) : Dải đai Catalog Service thì Móc Scale bọc chạy Read throughput xả ngỏ mốc, ngỏ trút Lending Service thì cài chóp rẽ scale vách cho báo gán Write throughput cựa nảy.
- Rã chạy vách mỏ Khởi lúc đầu Single-library deployment đai bọc cựa cho mác Đơn báo rẽ Thư Viện. Sau mỏ rẽ qua túi Multi-tenant phễu rã (1 Mạng túi API nạp chia trút nhiều cựa túi Libraries dùng chung nảy rã) ngỏ vào Giai Phase cựa báo cự 2 mảng rẽ. 
- Nháp gán cự Trụ được bọc Support mức 100 ngàn bọc 100,000 cuộn rã Books nảy chóp báo trích cùng chứa nạp báo cự 20,000 Members móc nạp per mỏ đai mác chóp Tenant trong lúc mỏ Phase trích Phase 2 đai.

### Chuỗi Roadmap Cấu Trút Bằng (Long-Term Dải Roadmap)

| Giai bục Mỏ (Phase) | Móc Đâm Rã Trọng Point (Focus) | Lịch Nạp Mảng Khung bọc Timeframe |
|-------|-------|-----------|
| MVP | Làm rẽ Catalog mảng CRUD rã, Lending kẹp với policy rã Enforcement ngỏ, Nhả báo Auth roles member, báo Reports cơ bản mộc đai, hàng đợi mỏ báo Hold Queue gạch rã, và rã trút mác Phí Trễ Late Fees | Gói nháp trong Tháng mỏ 1 tới đai báo Tháng 3 |
| Phase số mỏ 2 | Rã đai phễu Notifications (qua mảng email cựa bục), nạp tính cự bọc Multi-tenant mảng, Mở rã cự API quét Code vạch đâm Mỏ (Barcode scanning trích API), cùng cháp đai Nâng Analytics bọc vạch  | Mác túi từ Tháng rã 4 tới bục trích 8 |
| Phase đâm túi bục 3 | Recommendation Engine đo bọc Hệ máy (Gợi báo nhắc ý sách đai mỏ thích cự), rẽ mỏ Mượn Liên thư Inter-library rã Mỏ nạp (Loan cựa báo loan nạp đai), Tính đai cự báo Móc Phone Mobile push trích mảng Notifications báo notifications | Trích từ nháp Tháng 9 đo đai móc Tới cựa 14 |

---

## Cấu Dải Định Mỏ Mảng Cắt Chọn Tính Này Đai MVP (MVP Scope)

### Mảng Đích MVP Nhắm Điểm Tút (MVP Objective)

Phải nhả xả trích được 2 Đôi Túi bục Service tách biệt độc cựa báo (Catalog Service cựa và bục mỏ đai Lending Service) ngỏ thư viện cộng đai đồng đồng bọc mỏ library rã nạp deploy cài ngỏ xả ngỏ dùng chạy bục Quản Catalog rã đai và Mảng Lending mượn sách mỏ cựa, để qua đó lấp đai loại cái mảng nảy test Manual tracking gán cựa tracking tracking đâm đo thủ cài vách công đi bằng nảy hệ Automation trích rã policy đo rã enforcement nỏ cựa tự gán và giải nhả rã báo Móc xử hold bọc Fulfillment chắp rẽ Async. 

### Chỉ Số Đai Đo Điểm Thành Mảng Cho Tút MVP Mác Bục (MVP Success Criteria)

- [ ] Vạch bọc trích nảy All MVP endpoints móc cựa chớp rã Implemented và trích Tested đo móc
- [ ] Báo rã mảng Mức Phủ 90%+ móc nạp Line coverage qua cái Unit tests bục nảy 
- [ ] Xả all đai nảy mỏ Contract Tests bọc qua bài test gán vách đánh trích OpenAPI specification 
- [ ] Nháp luật móc cấu ngỏ Lending policy rules (giới móc đai limit checkout nảy báo, nảy cựa túi hold limits, báo nảy vách Late Fees rã trích mỏ cựa, báo trút rẽ Renewals móc) bọc áp trích rã ngỏ correctly.
- [ ] Mảng phân Quyền ngỏ rã báo cựa Role-based Access bọng chạy work luồng cho móc báo All Three 3 nhóm Roles. 
- [ ] Gõ máy nạp đo API rã Response báo về dứt trong bọc Time 100ms móc cự đai p95 (Nhả nháp dải lúc bục dưới tải cài Gán Load test).
- [ ] Cắm báo Deploy gán đai trích AWS mác cùng nạp bọc Code Infrastructure-As-Code IAC nạp trích .

### Tính Năng rã Thuộc Diện Bọc In-Scope Cho Nhả Lệnh Gán MVP (Features In Scope) 

| Feature rã | Miêu báo ngỏ Description bọc | Cấp Tút Phân Mỏ Định Chọn Priority cựa | Lý Giải mỏ Phục Đo Nảy Trích MVP mảng Bọc (Rationale)|
|---------|-------------|----------|------------------------|
| Gắn cấu đai Book CRUD trích Book | Gán Thêm (Add), nâng mảng Update, ngỏ Get (Get), xem Dải List (List), xóa đai cựa (Delete) báo các bọc Books với mảng ngỏ Data title rã móc, author đai, đai ISBN mã mảng, cài dải Categories bọc, và copy rã counts | Buộc Phải Có Mảng (Must Have) | Cột lõi mảng Core báo Catalog bọc functionality rã. Chẳng trút đai mượn báo sách nào mốc ngỏ nếu tút mỏng bọc Catalog nảy mảng. |
| Mốc Trích Book Search đai | Vạch Search books đai tìm bục qua substring text Title rã ngỏ móc hoặc bọc Author cựa, Filter cài Category ngỏ và số đo đai báo Availability | Buộc Xả Mác (Must Have) | Members móc ngỏ cần báo mảng tìm sách. Móc Librarian còn đâm gán rã search để rẽ nạp tút lấy đầu móc Titles cựa Titles |
| Rã Đo Tính Trống Khả Mảng Availabilty | Ghi bục Track available_copies đo mác cùng thông gán total_copies rã. Rút trừ khi xả checkout ngỏ decrements và đai tăng lại rã increments return | Có nhả mảng Mới Xong (Must Have) | Tránh xả test lộn cho nảy bục vay cựa móc nhả 1 đai mác đai cuốn rã sách copy vật rẽ nảy bọc physical đai cựa |
| Khách Member Khởi Nạp Lập Register bọc Account | Nảy Bọc Setup Lập mảng Account Register ngỏ thông đai tên (name), ngỏ bọc nảy email, gán mật đai báo vách mật khẩu phễu password. Cự tự gán đai Gán Tên Vai cựa Auto-assigned nạp role cựa Mảng Member. | Hàng Ngỏ Cựa Buộc (Must Have) | Tút đo náp phải đai có hòm cự mảng tài khoản cựa báo để bọc đai gán Users mượn trích rã báo Books đai. |
| Túi Mem Thông Cựa Member profile đai báo | Lấy nảy vách báo Get Profile và cựa sửa update. Bộ đai Admin Admin trích rã móc tút Get bất bọc cứ Acc rẽ mem báo profile nào | Ngỏ Trích (Must Have) | Đai móc quản Acc nạp cơ bản trích bọc gán acc management rã . |
| Quyền Role-based Access ngỏ bọc  | Hệ Mảng Role Có 3 Báo Cựa: Bộ Admin (Mở cựa móc bọc Full Quyền All Access), Ổ cài Librarian mỏ (Báo ngỏ trích Catalog và báo Quản Lending trích management mảng), và hệ rẽ Member (Bục cài gán tự phục vách Self-service mượn ngỏ borrowing). | Móc Rẽ Mốc (Must Have) | Túi đo bảo An rã Security đai cựa móc Required đai báo phân nảy Permission rã |
| Xác Cửa Auth JWT bục | Login đăng Nhập mỏ bọc nảy thả gán file JWT bục. Cự All đâm mọi API trích khóa đều đo bọc nhả cần 1 JWT xả bọc hòm Valid đai. Cái rẽ nảy đứt hết mọc Expiry khi chạy cài ngỏ 1 Rã cựa 24h đo tiếng. | Bắt Cài Có(Must Have) | Ngỏ bọc cài Mác Không Lưu Trạng Thái Auth xả mảng Stateless xả API gán. Đâm dải cài Cựa Trích để chạy đai Rã rẽ móc ép luật cựa |
| Phễu Xuất Checkout ngỏ mượn rã | Khách Member mảng Cự nhận mượn nạp Checkout cuốn rã. Máy Mảng Hệ rẽ System ngỏ Check kiểm: Tồn Bọc cuốn nháp Sách đó Không rẽ, Báo Số Bản Rã Khả cựa Copies phễu đâm Còn trống đo rã, và móc gán mảng dải hạn của rã Mem kia vách có bọc dưới mức dải 5 Cuốn cho Limit Checkout cựa. Máy lưu ngỏ nạp mốc Hẹn Ngày Trả date 14 dải mạc Days cài ngỏ tính báo từ lúc mượn mảng Checkout. | Dải Buộc Chóp Nạp (Must) | Máy móc cự bục Core của cho mượn rẽ đo.  |
| Ổ cựa Báo Hồi sách Return mảng | Hệ mem thành viên rã hoặc trút bọc thủ thư móc rẽ hoàn báo 1 nhả cuốn vách đai book rã. Cấu rẽ Hệ Máy Nhả Tính bọc System calculates tiền bọc cựa Late Phí Fee trút nếu bục nảy trả bị bọc quá trệ mỏ đo hạn ($0.25 bọc trên 1 dải mác ngỏ mảng day, trút đóng cựa đai mức chặn bọc móc Cap móc $10.00). Update ngỏ báo bản copies đai khả nạp mảng rã available copies rẽ. | Rã nảy Mác bục (Must Have) | Hệ Lệnh lõi Core Lending operation |
| Nhả Đi Gia Gia hạn Renewal mỏ  | Mem thành nảy móc viên Renewal gia báo nháp 1 bục cựa đơn đang mượn rã còn nảy dải nhả. Nảy gia thêm mỏ cựa Extend được bọc chóp 14 dải nảy mác date đo Days. Chỉ bục được nhả renewal Max cựa đai 2 rã cuộn mảng per trích Checkout. Cấp báo cắm ngỏ Không cho đai Renew nếu ngỏ sách đó có mảng người đặt gạch rã đo Hold active holds phễu cựa trích.  | Thường Có Cài Mảng Phải (Must) | Độ rẽ chóp Thường nảy gặp rẽ của rã Mem vách mác báo.  Góp bọc móc Giảm phễu trút nảy dải quá báo late overdue rẽ. |
| Mỏ Báo Đai Hiện Chóp Báo Đang Đứt Sách (Active Checkouts) | Rã Móc Danh sách các cuốn đang chóp mượn cựa nạp nhả mạc active cài cựa member rã mác. Vách chóp kẹp sách Book chóp Rẽ Thông Infor, cài đai Ngày Nhả Date cựa checkout mỏ date, hạn móc và ngày ngỏ Renewal count rã nạp mác. | Báo Nhả Nạp Mức Có (Must) | Ai cựa mượn cũng muốn ngỏ biết mình cầm rã cấu móc đai rẽ quyển bọc list nào vách trích . |
| Khóa Rã Cựa Nảy Nhả Gạch Giữ (Hold bục placement) | Gán Mem túi đặt Holder gạch ngỏ nạp 1 mỏ sách Unavailable đai nhả hết mốc sách bục trích ngỏ. Máy System ngỏ rã báo Verify Check: cựa Có Sách mảng đó báo Books Không đâm rẽ, Rẽ sách Copy nạp cựa 0 Có Empty trống trích đo, và mảng Cự Ngỏ Dưới nạp móc Cấp Max Hold nhả báo Limit cựa (Bọc Gán Max là 3 ngỏ đo đai mảng). Mem Chưa bọc nảy đai mảng đai trích đã Hold cuốn bọc cựa Rẽ sách trút đo này rồi vách. Hàng bục đâm cấu Hold Queue là Rã FIFO rã báo mỏ nạp cự đo.  | Mỏ Có Nhả Xả Bục (Must Have) | Đảm đai mỏ Công bục Fairness bằng nhả mỏ đo chóp sách hot bục. | 
| Tháo cựa gạch ngó Hold bọc Cancellation chóp | Member cự Hủy vòng trút bỏ nảy Own hold cục bục mảng cựa đai Hold của cựa Mình rã.  Librarian bọc hoặc chóp bọc Admin vách chớp Gắn Quyền ngỏ cựa Cancel thảy đai Any móc hold phễu rã đâm trích bục nạp ngỏ. | Phải dải trích đo (Must Have) | Người ta hay mỏ Rã báo đổi chóp Minds móc ngỏ mảng bục phễu cựa Minds nảy.  Và thủ thư sẽ vạch gán Manage Mảng Hàng Nhả Queue đo.  |
| Hàng Đợi Hold Vạch Mạng Rã Status nhả đai ngỏ Queue | Vạch Gọi rã vị trí mảng nháp Position ngỏ Đai của Trong hàng đợi Gạch Hold Queue cựa cho một Trút Sách Rã book mác cụ trút specific. | Mỏ Đai Bọc ngỏ Móc Gán (Must Have) | Mấy Mem móc mỏ Bục Thích ngỏ Ngóng đo bục Đo Tới Lượt Chừng nảy Vách nào đai bọc Móc cựa báo. |
| Dải Phí Nạp Thơ Fees Fee tracking phễu cựa Tracking | Ổ Dõi Theo Nhả Vạch Các khoản Phí nạp Outstanding Fee cựa Nhả Chưa dải đai đóng rã Nợ ngỏ Fee báo của ngỏ Member đai.  Ổ Fees này Cựa Sinh ra từ nảy Auto Automatically do Mảng Late ngỏ cựa Return bục ngỏ Phạt báo. | Phễu Đi Có Bọc (Must Have) | Ngỏ Thúc Cháp cựa vách dải Kỉ luật Cựa Phạt ngỏ Nhả Muộn cựa Trút đai mảng. | 
| Đai bục trả gán mảng Thanh Phí nạp Gán nhả Fee Payment mảng Payment | Ổ bọc Lệnh ghi nhả phễu Nháp Payment nạp Fee (Gán cho dải Partial Đai hay Trút cựa Full đo trả nạp đai ngỏ đứt vách). Kho Admin cựa phễu ngỏ rã bọc dải và bọc trích Librarian nạp có bọc móc quyền Rã Process phễu Nháp payments. | Túi xé Đo (Must Have) | Có Bục Ợ đai Nợ Phí ngỏ Thì Có ngỏ Cơ rẽ rã thanh cháp Cựa Toát mảng Xóa dải Nháp Nợ ngỏ rã phễu. |
| Báo Ngỏ Bảng Overdue Nháp (Phễu Cựa Late) Report mỏ | Quẳng Report danh bục Móc rã All cuốn rã phễu đai chưa trả Ngỏ báo Hạn (Overdue) đai móc checkouts kèm nhả rã báo Member báo info ngỏ trút vách thông đai phễu days overdue nảy phễu cựa rã ngỏ mạc đâm số bọc muộn đo. Mỏ Librarian rã nảy kèm Admin ngỏ vách phễu rã access gán. | Rã Nãy Gán Phải (Must) | Rã Mỏ Cựa Thủ Bục Thư nảy xả cần phễu ngỏ Lệnh Follow đai để báo up chóp Móc bục đo Cự Móc sách muộn cựa. |
| Mỏ Phễu Gộp Mảng Report Đai Collection mỏ Gán Tổng Nháp (Summary) | Đo đai Total All books, ngỏ rã All members mỏ, bao cuốn sách Checked Out mỏ, mảng Sách nảy Còn Nhả Rã đai đo đai, tổng Fee chóp nảy vách nợ móc chép ngỏ trích. Cho mảng xả Admin bọc nảy access dải nạp. | Có Thiết Cựa Bục (Must)| Bục móc Admins xả đai nhả phễu Bọc Rã Report đai Dash bục Report nảy để đai view cựa mảng bọc library status đai cựa.  |
| Health Ổ Thiết Health Nhả Mảng check test | Điểm Cửa GET nảy `/health` đai cấu trả dải mác rã Status đai vách với Version version xả. | Phải cựa Nhả bục (Must Have)| Móc đai đo vạch ngỏ dải Monitoring cựa nảy máy rẽ. | 

### Đoạn Ngăn Sức Ngoài Giới (Cấm Ngỏ Trích File Báo Cựa bọc Out Of Scope MVP)

| Tên Bọc Feature Tính | Nguyên Do ngỏ Dời Vách cự (Reason For Deferral)| Lên Dải Ngỏ Trích (Phase) |
|---------|-------------------|--------------|
| Nhả Thông Thiết Mạng Email | Ép bọc đai gính cài Dependency thư SES/SNS rã nhả cùng bục Complex đai nảy hệ cựa Async. Báo Manual check báo ngỏ report nháp đủ rồi cựa cho MVP đai.  | Báo Mỏ Phase 2 |
| Đa Khách Trích Mạng Hệ Cựa Ổ Trút Multi-tenant | Làm Nhả cho 1 ổ Library là bục ok Test mảng xài MVP rồi cựa trích. Làm đa ngỏ xả đai phức Cựa trút Data Isolation đai xả gán nhả cựa.  | Cho Phase đo 2 |
| Móc Cài Đai Mã Báo Quét Mã Vạch Barcode/ISBN | Phải bục móc Gọi Giao Gọi ngoài đai rã gọi báo External Cựa quét ISBN rã API đai nảy vách móc. Manual đánh đai Tay ok đai MVP bục rồi nảy | Xả Trút Gán Phase 2 |
| Rà Nảy Đo Súc Cháp Analytics ngỏ xả Bục cao (Advanced) | Cái ngỏ Report Basic kia rã bọc cựa đủ xài rồi đai ngỏ xả đai mảng. | Lên Mảng Rã Phase 2 |
| Cấu Dải Recommendation nháp móp Hệ Engine | Muốn nhả đo đai vách Data đo của bục lịch Lending bọc vạch Rã history nháp ngỏ bục báo. Rẽ ngỏ bọc Library ngỏ Cắm data chưa đủ cựa. | Nhả Tút Phase 3|
| Mỏ Liên Mượn Phễu Inter-library cựa nảy ngỏ (Bục rẽ Loan cho) | Ép cựa phải gán Đa Máy Mạng Multi-tenant đai mới xả nhả làm dải được phễu cựa trích.  | Thảy cựa Phase 3 |
| Khóa Account suspension trích rã | Gán Manual process chóp Admin đủ dùng cựa MVP đai rẽ nhả. Chạy Automate cái này nhả mỏ đo Rã bọc Phức luật rẽ mảng Policy ngỏ | Ở Mảng Đai Phase 2 | 
| Quản Móp Cấu Reset ngỏ vách Password bục | Ngoài nhả MVP cựa. Kêu Mỏ Admin bọc tút đai tạo Acc Móc mới mảng New cựa đai rã bọc Acc đâm  | Đo Túi Gán Phase 2 |
| Cháp Nảy Danh Cấu Trang Đo Pagination pagination | Cái Mảng GET list nảy móc All trả 1 cục trút data ở MVP cựa xả ok luôn bục. Với nạp mảng thư viện ngỏ Nhỏ cựa < 10,000 cuốn bục rã OK đai nạp mác.  | Xả 2 Phase 2 |

### Hành Trình Thiết Diễn Của User đai MVP (User Journeys)

#### Khuyến Hành 1: Móc Thủ Thư Gõ Add bục Sách đai Manage cựa Nhả Catalog 

1. Móc Thư đai Librarian bật Auth rẽ login qua ngỏ Cựa file Email/Pass bục, nhả túi trích lấy đai file bọc Token JWT cựa.
2. Library nảy trút Thư Librarian tút gọi Mở Add thêm 5 mảng Sách cuốn ngỏ New Books chóp nảy bọc rã báo rẽ `POST /api/v1/books` bục nảy tút với ngỏ Titles, gán mác Authors, bục ngỏ ISBN, danh mỏ loại categories trích dải, đai báo số copies báo count rã bục cựa mảng copy.
3. Ổ Librarian chóp rẽ nảy Gõ test Search cuốn bọc Book đó test nảy vách bục bằng cái Title đai coi ngỏ rã thêm trích tút Mở rã Code đúng ko (verified gán nảy mảng cựa correctly ngỏ rã). 
4. Mỏ rã Cựa Báo Librarian đai Update báo Ngỏ Lượng cựa Số mỏ copies nạp trích count rã khi phễu bọc có 1 ai đai cuốn mảng Donate nạp bục rã báo tới.
5. Cửa Thủ đai Librarian coi bọc báo Dải Báo Report Collection nạp chướng summary để báo cựa Nhìn bục xem Total ngỏ rầm Inventory nảy. 

**Về Đích Bọc Outcome**: Mảng Bọc Library đai mục catalog cựa Up to Date nhả mỏ (bục Mới Tính) cài và Search rã Lên vách Được cự. 

#### Mảng Lệnh Chọn Chóp User 2: Nhả Mượn Mem Member nạp Borrows, và đai Renew nảy đai, rồi gán Return cựa cuốn mỏ rã sách bục 

1. Mảng Member mọc bọc ngỏ nạp Trích Register qua `/api/v1/members/register`, sau Móc bục vào Logon chóp Login phễu cựa. 
2. Mem Ổ nảy Gõ Test đai Search chữ "Python", nhả phễu Thấy 3 cuốn.
3. Cự Mem bấm lệnh bục Checkout mỏ cuốn "Fluent Python" với `POST /api/v1/checkouts`. 
4. Hết gán 10 ngày, cái Member đai gia hạn vách Renewal mảng ngỏ nạp chóp checkout chóp cự nhả 14 đai ngỏ days vách nảy báo cựa mảng bục ngỏ rã thảy days.
5. Của cựa Member đem đi đo phễu Tr Return sách mác cự On time (Theo đúng rã nhả hạn mảng Date móc mảng cựa ngỏ). Không tính Cựa vách trút cựa Fee bục phễu nhả charged nảy. 

**Tút Lợi (Outcome)**: Phễu đai Member tự dải Mượn gán rã bọc cựa Vách Nhả Trả Return rã Đai cuốn móc Sách full ngỏ Vòng Đời bọc Lifecycle rã chóp nảy đo mác rẽ phễu đai . 

#### Dải Ngỏ Trình 3: Móc Bọc Member Xếp Cựa Hàng Chờ (Đặt Gạch đai Hold Cự 1 Rã Đai Cuốn Sách Hot)

1. Gán Member đi ngỏ Search nạp mảng Cựa Cuốn "Designing Data-Intensive Applications" rồi cựa đai Thấy báo mảng 0 Còn Cuốn (0 bục Copies đo available trích ngỏ).
2. Xả Member đi đai Bấm phễu Hold qua trích `/api/v1/holds` cựa rẽ ngỏ. 
3. Túi nạp đai đai Member mảng nạp Coi Ngỏ Cựa Hàng cháp Chờ rã queue trích Position: Tính đai báo rã báo cựa Vị Thứ 2 (2nd in line cựa mảng).
4. Có ngỏ Một mảng Khứa bục Member khác rã rẽ mảng Return sách bục. Rã Cái Hold nảy bục First in queue đai cháp cựa rẽ Đầu Hàng nạp fulfilled đai. 
5. Lúc cái Nhả Mem Mảng phễu cựa First in line đó nảy Trả Sách vách Return nhả, cự ngỏ cái Hold của đai phễu member Nhả mình test nảy xả móc đai Advance bục ngỏ xả Lên số 1 Position đai 1 mác nảy. 
6. Cái Cuốn nảy mảng Thư vách Book phễu nạp Available Trống trút báo ra Cho rã Mình Mem Member bục mảng móc cự Nhả Checkout rã.

**Quả Đầu Xả (Outcome)**: Cực mảng báo đai bọc Công Bằng đai Fairness nạp tự động Access cựa bục cho Hot ngỏ đai móc Popular trích bọc rã Books nảy. 

#### Mảng Điểm Cuối Journey 4: Trả Nảy Trễ rã Gắn đo Fee Phạt Tút Fee nạp

1. Nảy Đai Mem bọc Member checkout 1 đai Cuốn và quên cựa mảng rã ngỏ ngâm mảng nảy cho lố trích Không bọc Return đúng cài hẹn cựa bọc đai cựa báo bục mác date.
2. Thư bọc ngỏ Librarian mở coi mỏ Report quá mỏ Hạn trích Overdue nảy báo mảng gán Thấy vách có Member này cự rã nạp checkout bóc đai trễ mảng quá dải báo 5 days.
3. Cựa Member đai đem sách bọc Return. Hệ mốc cài máy System ngỏ Calculates Móc nạp vách bục Phạt phí chóp trích đo xả Late fee là $1.25 bục nạp ($0.25 bục x 5 mỏ days cựa rã ngỏ trích mảng) đai.
4. Member vào ngỏ ngó thắt cựa bọc Outstanding fee đai của ngỏ nảy mình đai cự báo mác rã. 
5. Cửa ngỏ Librarian bấm xử cựa đo bọc nháp báo pay phí móc mỏ nạp qua vách trích System bọc đai gộp Payment rã.

**Khóa Cự Báo Nhả (Outcome)**: Tiền cựa báo Late fee vách nhả ngỏ cự tính cài mảng báo tự động bọc Auto rã track đai khi Trả bục phễu Done cựa móc nhả Paid mảng chóp cự phễu đai. 

### Cọc Giới Ngỏ Đo và Cái Đầu Dựa Phễu Sự Trích (Constraints và Assumptions) MVP

- **Cắm Báo Assumptions**: Cứ phễu 1 Ngỏ Dịch (Service vách) nạp là tự rã Quản Own data store. Catalog Service và cựa cài ngỏ Lending Service là mác bọc tút tuyệt đai ngỏ Không Ngỏ Share database rẽ bục nảy dùng mác phễu chung cự nhả gán đo phễu. 
- **Assumption Gán**: 24 giờ đai trích ngỏ Expiry nhả đối cài với Token JWT bục khi cựa nảy Không bục phễu Refresh token ngỏ rã đo phễu cháp xả mạc là cựa chập lấy cái bọc cựa (Acceptable cho mỏ MVP bục đai).
- **Asssumption**: Cái hòm rẽ nảy Async bọc báo ngỏ trích Hold Fulfillment dùng cài Event message queue là bộ phễu cự Rã Required đứt cài báo trong vách trích MVP ngỏ nảy — Ngỏ Return book cựa là phải mảng móc báo xả kích đai nảy Triggers hold móc Notification báo xỏ cháp chặn liền gán Mảng mà Không bục nỏ ngỏ Blocking cựa gán bọc Return móc mảng rã nháp responses đo phễu đai cựa vách.
- **Rã Bọc Tút Ngỏ Nhận Hãm (Accepted Limitation)**: Cái vách nhả Không đai bọc Có Notifications chóp mỏ Email. Hold cựa Notification báo đứt nhả mốc rẽ Mảng Ý móc là nảy báo đi đai Trích Update chóp trạng rã thái đai móc Hold rẽ cựa record status rã đo, chứ chóp nảy đai Không ngỏ móc đi Bắn Send bục trút Email cài.
- **Móc Hãm Đai Accepted Cự Limitation**: Đo phễu Không rã phân báo vách trang đai Pagination. Các dải đai API list đều phễu nạp bọc vách Xả Trả Full chóp mảng rã kết bục trích Results cựa đai. 
- **Limitation Trích**: Chỉ Deploy đai nháp bục cựa 1 Single library đai mỏ (Trúc Cựa Không rẽ mảng đo Multitenant bọc cự ngỏ trích đo báo).

### Trịnh Giải Báo Nhỏ Chuẩn nảy Độ Done MVP Đai (Definition of Done)

- [ ] Vát Cải All MVP đo Ổ Endpoints rẽ cựa báo làm nhả đai Trích Pass bọc Contract ngỏ Tests
- [ ] Mảng Dải Test móc Unit chạy vách với cựa Nạp Line Bọc Độ phủ 90% cựa nảy +
- [ ] Chóp báo Ngỏ Trích Xác Thực mỏ các ngỏ túi Business rules rã vách (Checkout limit phễu nạp, rã báo nhả Hold limit cự đai, phễu nảy báo late fee cựa đo, nảy nhả renewal bọc ngỏ cự ngỏ bọc nảy)
- [ ] Mốc Quyền cự Access ngỏ Role Enforced trên chóp đo đai All mảng rã bục báo nảy Protected đai mốc rã ngỏ nạp endpoints
- [ ] Lượng Tải test đai xả vách Nhả đo 100 nảy ngỏ Users chóp mảng concurrent mỏ rã, báo đo phễu đai p95 nháp nảy < 100ms
- [ ] Nhả cựa bọc đai Deploy báo bục cả 2 túi Mảng Dịch Service vách rã lọt cái AWS mỏ bọc Infra cài bục cựa nảy trút IAC bọc náp cài code rã ngỏ mạc đai trích móc. 
- [ ] Sợi Dây Nối bục xả Inter-Service đai xả Trút nạp bọc Communication vách nháp chạy OK (Trích rẽ Event nhả dải Event cựa Driven bục nảy Hold fulfillment rã mạc). 
- [ ] Quy cài bọc Open API 3.x phễu báo Nháp cựa Specification gán Đấm Ngỏ ăn đai Khớp mảng báo Match đo Trích cựa với rã Code đai Impl cài rã mảng implementation đo bục.

---

## Ổ Bọc Rủi Xả Và Cựa Cái Bục Cháp Ổ Bọc Bộ Vách Depend (Risks and Dependencies nảy) 

### Rủi Cực Báo Key (Key Risks rã nảy đo)

| Các bục Risk | Nhỏ Độ Cao Trích Nảy Likelihood | Móc Sát Phễu Báo Độ Hưởng Impact | Móc Khắc Mảng Ngỏ Phục (Mitigation) |
|------|-----------|--------|------------|
| Code bọc Rules của Business dính phải các đai nạp cựa mạc vách Edge Cases (ngỏ cựa Ví ngỏ cự rã nhả: Chóp cái rẽ gán đai Sách đai đâm bị Trích mảng đo Xóa Khỏi bọc nảy cựa Decommissioned rã phễu ngỏ bọc khi đai Đang rẽ báo Nằm móc Trong gán bục Hold bọc đợi nảy đai nạp?) | Cụ Dễ dính cựa Medium | Nhóm cự ngỏ mảng Cỡ bọc Medium | Chọn cài Mảng Mở Bọc Rã Phễu đo Define các Edge chóp cases đai Phân bọc khâu bọc nạp Inception nảy cựa Requirements rã đai và mảng bọc Functional đai vách Functional cựa xả Design rã đo. Xài mảng xả rẽ bục Các Clarifying hỏi cựa vách. |
| Móc Ổ Túi Access bục nạp Patterns rã Query báo ở ngỏ AWS DynamoDB bọc đai gán chóp cựa bọc Dính vách trích đo rã mác Báo ngỏ Lệch cựa Nhịp so với cài dải đo mảng Relational mỏ Data của (Ví rẽ nảy: "All cháp xả trích bọc rã muộn rã đâm trút rẽ Checkout overdue nảy chóp báo Sort ngỏ Rã theo gán bục Ngày phễu đai nảy Days") | Rẽ Cao Cựa bọc Medium | Ngỏ Móp Nặng Báo Rất High | Ốp Rã Thiết bục Đo Mác nảy cựa Bọc Trích Dynamo nảy Table và móc cài Cấu đai GSI phễu Ngỏ mác Kỹ càng cựa đai Design đai Infra trích nạp. Ngỏ vạch xét bục mảng móc cự đai cựa design Single-Table thiết trích rã ngỏ trút. |
| Nảy Mác Cựa Đai Xác Thực Auth JWT ngỏ bục báo gây móc Phức gán đai mạc Complexity cựa trút rẽ cho tút 1 Developer Solo bục đai vách móc nảy đai mảng.  | Ít nảy cựa bọc Low | Nảy Medium cựa gán mạc đo | Tút Dùng mảng cựa cái Thư phễu Viện cài test đủ Library ngỏ mảng bục rã nảy túi mác chuẩn ngỏ xả Bục (python-jose đai nháp hoặc cự PyJWT nảy). Cho nhả nháp trích mảng cự Code auth rã ngỏ rã Xả tút Simple cài: Chớp Ngỏ No cựa rã token móc refresh rã đo, Trút Không cựa OAuth đai ngỏ vách nảy móc Flows. |
| Nảy bục mác Cựa dải Tiền bục móc Fees Phạt Late ngỏ nảy rã vách Edge Cases mảng ngõ trích đo (Tính theo Múi ngỏ Bục Time Timezone rã nảy, 1 Nửa phễu Ngỏ bục Partial Days đai vách rã ngỏ bục, gán Fee cap trút).  | Vượt đai cựa mỏ Medium | Nháp mức Medium bọc | Tạo trút Ngỏ cài Định nghĩa Đai ngọn Rõ cái Precise rules bọc trút trong Functional móc phễu báo Design cự. Móc Cài bục Time đai Phễu rã Mác ngỏ Mảng trích cự UTC đo cho All rã bọc cựa Timestamp. |

### Quản ngỏ Trút Dependencies Ngoại Mỏ Vi 

- **Mỏ Áp Kho AWS Account** - Bọc Test phải có Tài báo Account mảng đo bục móc Deploy rã. (Developer nảy mác phải trút quyền truy ngỏ Access rẽ). 
- **Vòng Mở Python 3.13** - Tút Mảng cài Môi trường rã ngỏ Runtime (Required móc). Cái này đã có đai cự ngỏ nảy Phễu trên ngỏ bục trút AWS Lambda móc nảy đai.

### Túi Mảng Câu Báo Đang Cựa Open Trích Mở Bọc Rã Ngỏ Questions

- [ ] Nếu móc mem ngỏ Có báo đai nhả Fee Trễ báo có nên Cắm ngỏ nảy bọc Phạt Fee Accruing móc Tới Vô đai cựa Hạn mảng bục rã max cự nhả days báo ngỏ trích đo Không, Hay móc Nên dừng mảng phễu cự Rẽ chặn Capped phễu cựa mác ở bằng Tiền bục Trị giá cuốn ngỏ Sách bục rã rẽ nảy phễu Replacement value cài nảy?
- [ ] Ổ Nếu có Mem đai Đang nợ cựa Fee mảng xả phễu Nhả trích Test móc vách ngỏ mảng cự đo Checkout sách cựa bục, Thì rã Móc System có ngỏ nên báo Block đo hay nảy cựa là đai Chỉ Nhả Warn đai Cựa ngỏ nạp mảng? 
- [ ] Gán hàng ngỏ hold Queue có nên bắn Notification bục mảng Next Mem đai Ngay rã Liền nhả đo khi nảy rã Gán Books bục Return móc ngỏ nảy không, Hay có gán báo Grace Period báo (Gian hạn cài nhả chờ rã ngỏ cựa mảng nảy delay rã mác) 1 tí cho chóp khứa Return rẽ đó mỏ báo Re-checkout mảng đai bọc lại Không cựa đâm?
- [ ] Nếu 1 mảng Admin nảy Báo Tắt ngỏ Trừ vách Khử Khứa Acc bục (Deactivates rã account rã phễu đai nảy), Thì cái bọc Đám Hold xếp mác đai báo ngỏ đai Active Checkouts và cựa Active Holds của khứa đó phễu System phải nhả giải rã quyết đai gì cự ngõ?
