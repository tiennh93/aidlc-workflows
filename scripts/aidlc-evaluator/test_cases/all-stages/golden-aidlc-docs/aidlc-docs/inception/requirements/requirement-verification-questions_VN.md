# Câu Hỏi Thẩm Định Yêu Cầu (Requirements Verification Questions) — ĐÃ CÓ ĐÁP ÁN

Xin vui lòng điền đáp án cho các câu hỏi sau để làm rõ những yêu cầu nghiệp vụ cho API Thư viện Cộng đồng BookShelf. Điền ký tự (A,B,C,D,..) vao sau thẻ `[Answer]:`.

---

## Câu Hỏi 1
Tài liệu Tầm nhìn (vision) có liệt kê câu hỏi mở về chế tài Thu phí phạt trễ hạn. Khi một user (member) đang bị nợ phí phạt mà vẫn cố nhấn mượn sách mới (checkout), hệ thống nên chặn luôn lệnh mượn đó hay cứ cho mượn nhưng kèm theo cảnh báo?

A) Chặn mượn sách (Block checkout) — user nào dính bất kỳ khoản nợ phí phạt nào đều không được mượn tiếp cho tới khi nộp phạt xong.
B) Chỉ chặn mượn sách khi tiền nợ phạt vượt quá một Hạn mức quy định tùy chỉnh (Ví dụ: $10.00).
C) Cho phép mượn sách nhưng đính kèm cảnh báo vào API response để báo rằng họ đang nợ phạt.
D) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: B

## Câu Hỏi 2
Khi một cuốn sách được trả về và đang có một hàng đợi (hold) đăng ký mượn cuốn đó từ trước, liệu có nên set một khoảng "Thời gian ân hạn" (grace period) trước khi cuốn sách đó được chuyển cho người tiếp theo trong hàng đợi không?

A) Không có thời gian ân hạn (No grace period) — ngay lúc trả sách xong thì lập tức Cấp phát (fulfill) sách cho người tiếp theo trong queue.
B) Có thời gian ân hạn ngắn (Ví dụ: 24h) để người vừa trả sách có cơ hội mượn lại nó trước khi sách chuyển qua tay người khác.
C) Cấu hình tùy chỉnh thời gian ân hạn, do Admin của thư viện set.
D) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: A

## Câu Hỏi 3
Khi một Admin khóa (deactivate) tài khoản của một Member, các sách người đó đang mượn và các sách đang xếp hàng mượn (holds) sẽ bị xử lý thế nào?

A) Hủy bỏ ngay lập tức toàn bộ holds; giữ nguyên các sách đang mượn (để hệ thống còn lấy cớ thu sách về).
B) Hủy bỏ toàn bộ holds và dán nhãn buộc trả sách Ngay Lập Tức cho mọi checkouts đang có.
C) Giữ nguyên vẹn mọi thứ — Lệnh khóa (deactivation) chỉ có tác dụng chặn user tạo ra lệnh Mượn (checkout), Hàng đợi (hold) hoặc Gia hạn (renew) mới. 
D) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: C

## Câu Hỏi 4
Tiền phạt trễ hạn (late fees) nên được cộng dồn thả ga vô thời hạn, hay sẽ bị Chặn (cap) lại ở một mức trần cụ thể?

A) Chạm trần ở một mức USD cố định cho mỗi cuốn bị mượn trễ (Ví dụ: Phạt max $10.00 như có ghi ở bảng vision).
B) Chạm trần dựa bằng đúng mức giá trị mua lại cuốn sách (Cái này đòi hỏi phải bổ sung trường thông tin `replacement_value` vào bảng data books).
C) Chạm trần ở một mức tùy chỉnh do Admin set ở màn hình config.
D) Không có mức trần chặn — Phạt lũy tiến vĩnh viễn cho tới khi đem sách đi trả thì thôi.
E) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: A

## Câu Hỏi 5
Đối với bản MVP thiết kế kiến trúc 2 cụm service, cụm Catalog Service và Lending Service sẽ được triển khai (deploy) theo phương án nào?

A) Hai cụm ứng dụng FastAPI rời nhau, mỗi con tống vô một hàm AWS Lambda đứng sau API Gateway riêng.
B) Hai cụm ứng dụng FastAPI rời nhau, nhưng deploy dưới dạng ECS Fargate container nằm sau API Gateway.
C) Nhường khâu quyết định lựa chọn hạ tầng triển khai này cho giai đoạn Nháp Hạ Tầng (Infrastructure Design), lúc đó sẽ chốt dựa trên phân tích số liệu NFR.
D) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: C

## Câu Hỏi 6
Tính năng quản lý và Authentication xác thực user Member sẽ nằm ở đâu trong kiến trúc 2 service này?

A) Code nằm ở cụm Lending Service — vì member vốn dĩ dính líu chặt chẽ tới luồng nghiệp vụ mượn sách.
B) Code dời qua một lớp Layer chuyên biệt xài chung (Ví dụ: thông qua middleware) để cả 2 services cùng dùng ké, còn database lưu Member sẽ nằm bên Lending Service.
C) Trải code ra cho cả 2 services — mỗi service tự túc validate JWT độc lập, nhưng Lending Service sẽ toàn quyền nắm giữ Database lưu thành viên.
D) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: C

## Câu Hỏi 7
Đối với tính năng "Cấp phát sách nối tiếp bất đồng bộ" (asynchronous hold fulfillment) ở bản MVP, mức độ code thực tiễn sẽ nằm ở cỡ nào?

A) Code full bộ event bất đồng bộ qua message AWS (SQS/SNS/EventBridge) để xào nấu event thực tế giữa các services.
B) Viết event bất đồng bộ chạy nội bộ in-process ngay trong ruột cái Lending Service (Giả lập event bus thông qua từ khóa `async` của Python).
C) Viết hàm code đồng bộ (synchronous) đơn giản móc thẳng vô khâu Trả sách (return) — Cập nhật cập rập trạng thái Hold ngay lúc xử lý Return. Còn vụ chẻ code chạy Event Async xịn để dành cho Phase 2.
D) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: C

## Câu Hỏi 8
2 cụm service sẽ giao tiếp thế nào để làm thủ tục "Hỏi xem sách có tồn tại/trống không" trong lúc chạy hàm Checkout mượn sách?

A) Call lệnh HTTP request trực tiếp từ Lending Service đâm qua API nội bộ (internal API) của khối Catalog Service.
B) Xóa ranh giới, call chéo qua database View (vi phạm rào cách ly data — không khuyến cáo dùng trò này).
C) Cache lại data từ sách vô Lending Service, khi nào Catalog có biến thì Catalog chọt event báo Lending đồng bộ (sync) theo.
D) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: A

## Câu Hỏi 9
Đối với MVP, project có nên kèm theo phần Code Cấu Hình Hạ Tầng AWS CDK (Infrastructure-as-code) để deploy thẳng luôn không, hay chỉ cần focus viết code app (application code) và nhắm mắt pass qua khâu deploy?

A) Kèm cả tảng code hạ tầng CDK nhét vô 2 services luôn (Code dựng Lambda/Fargate, API Gateway, DynamoDB/RDS, SQS, v.v).
B) Kèm CDK vô nhưng chỉ viết code dạng khung xương Stubs/Templates để khoe kiến trúc chứ khoan làm tới mức production-ready cứng cáp gánh đạn. 
C) Chỉ focus đập Code Ứng dụng app thôi — Gói đính kèm ba cái Document hướng dẫn deploy là đủ, MVP tuyệt đối không nhét CDK Code vào.
D) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: C

## Câu Hỏi 10
Cụm Database nào sẽ được phân bổ cho hai service?

A) Xài DynamoDB cho cả 2 khối (đáp ứng đúng luồng access theo kiểu key-value bốc là ăn ngay, lại dễ Scale dạng serverless không máy chủ).
B) Xài DynamoDB cắm cho Catalog Service, Còn Lending Service ghim xài RDS PostgreSQL (vì luồng mượn trả dính nhiều đến truy vấn quan hệ relational queries quá).
C) Xài RDS PostgreSQL thầu cả hai service (xài đồ rành mạch theo truy vấn quan hệ, thân thuộc, xài theo chẩn SQL).
D) Cứ để cho vòng xét duyệt NFR Requirements và Infrastructure Design nhảy vào phân tích định đoạt dựa theo data tối ưu lúc đó.
E) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: D

## Câu Hỏi 11: Mở Rộng Bảo Mật (Security Extensions)
Các extension rule về chuẩn bảo mật (Từ SECURITY-01 đến SECURITY-15) có bị bắt ép áp giải vô dự án lần này chặn không?

A) Có — ép thực thi áp vô toàn khóa toàn bộ SECURITY rules như là một chốt chặn bắt buộc chặn blocking constraints (Rất khuyến nghị cho mô hình triển khai thực chiến cấp production-grade).
B) Không — Skip luôn khỏi check toàn bộ rules bảo mật đi (Chỉ thích hợp kiểu PoCs, code thử làm prototype, hoặc dự án chơi thí nghiệm).
C) Ý kiến khác (Vui lòng diễn giải phía sau thẻ [Answer]:)

[Answer]: A
