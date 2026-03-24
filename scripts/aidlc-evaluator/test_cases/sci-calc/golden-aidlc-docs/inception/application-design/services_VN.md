# Các Dịch Vụ (Services)

## Lớp Dịch Vụ (Service Layer)

Ứng dụng áp dụng mô hình kiến trúc **lớp dịch vụ mỏng** (thin service layer) trong đó tệp `app.py` đóng vai trò người điều phối (orchestrator):

### `app.py` — Trình Điều Phối Ứng Dụng
**Mô hình (Pattern)**: FastAPI application factory
**Trách nhiệm (Responsibilities)**:
1. **Đăng Ký Bộ Định Tuyến (Router Registration)**: Liên kết tất cả các mô-đun định tuyến (route modules) cùng với tiền tố URL của mỗi mô-đun.
2. **Đăng Ký Bộ Xử Lý Lỗi (Error Handler Registration)**: Ghi đè (override) bộ xử lý cấu trúc mã 422 của Pydantic, gắn thêm các bộ thu lỗi bao quát tổng thể (catch-all exception handler).
3. **Kiểm Tra Tình Trạng Health Check**: Cung cấp trực tiếp kết nối endpoint trả về `GET /health`.
4. **Phần Mềm Trung Đoạn (Middleware)**: Hoàn toàn không ứng dụng theo chuẩn MVP (không có xác thực auth, không có giới hạn tỷ lệ tốc độ rate-limiting).

### Ủy Quyền Định Tuyến Tới Lớp Engine (Route-to-Engine Delegation)
Tất cả các bộ xử lý định tuyến (route handler) đều duy trì một mẫu nhất quán (consistent pattern):
1. Đón nhận gói mô hình yêu cầu (request model) Pydantic đã được xác thực thành công.
2. Gọi kích hoạt tới hàm có tính năng tương ứng trong `math_engine`.
3. Bọc gói các hệ quả trả về gửi vào vỏ phong bì dạng `SuccessResponse`.
4. Khi chạm phải lỗi ngoại lệ (exception), hệ thống sẽ bắt lấy và gửi về vỏ bao bì chứa lỗi gói dạng `ErrorResponse`.

Ứng dụng quyết định **tuyệt đối không tạo cấu trúc một lớp dịch vụ riêng biệt (no separate service class)** — các nhánh mã định tuyến (routes) chỉ gọi trực tiếp vào hệ hàm hoạt động trong tầng máy cơ engine. Phương án thiết kế này vô cùng phù hợp theo đúng với phạm vi quy cách dự án: Tính toán rỗng hoàn toàn trọn vẹn vô trạng thái không cần ghi nhớ giữ liệu lịch sử lâu (stateless computation with no persistence), cũng không theo dõi quy mô hành vi trên phiên tương tác kết dùng (no user sessions), và tinh giảm lược bỏ mọi logic nghiệp vụ cồng kềnh bắt chéo lặp lờ (no cross-cutting business logic).

### Luồng Trình Xử Lý Ngoại Lệ Các Sự Cố Lỗi (Error Handling Flow)
1. Các định dạng sai hụt vi phạm trong mảng lọc Pydantic (Pydantic validation errors) → rẽ vào vòng hàm chuyên trị custom 422 handler → phản hồi bằng gói chứa đáp án `ErrorResponse(code="INVALID_INPUT")`
2. Lỗi gây rạn nứt vì thực hiện phép toán bị chia cho không (`DivisionByZeroError`) → lưới chắn báo gửi qua route handler → xuất mã tín báo trả kẹp `ErrorResponse(code="DIVISION_BY_ZERO", status=400)`
3. Cảnh báo lỗi vi phạm cấu mảng vùng giới hạn (`DomainError`) → lọt vô phễu xử lí vòng trạm route handler → ném hộp tin sự cố `ErrorResponse(code="DOMAIN_ERROR", status=400)`
4. Đoản phát xốc cảnh báo xả van tràn ngạch bộ chứa (`OverflowError`) → chạy đi qua cấu tuyến điểm route handler → trả ra thông điệp báo vượt hãm `ErrorResponse(code="OVERFLOW", status=400)`
5. Điểm gõ ngõ hẻm đường cùng không dò ra phương danh (Unknown endpoint) → gặp rào bật báo cáo từ cờ vạch mác FastAPI báo mã số chuẩn 404 → tung gói thả cho bộ lưới thủ công custom handler chặn và hốt lại → trả đáp trả phong thư thư gác đáp `ErrorResponse(code="NOT_FOUND", status=404)`
6. Gặp sự cố lạ lẫm đột biến không nằm trên đồ án cấu báo định dạng ngạch chẩn lường được (Unexpected exception) → tóm thả vô lưới phễu của bộ lọc thu lưới quét vét tổng kết toàn hẻm ngõ (catch-all handler) → ấn báo dội vết tích log đánh lưu mã gờ tại cấu khung rạch điểm mức ERROR → bắn gói thông điệp hồi âm trạm cuối đáp phát túi phong `ErrorResponse(code="INTERNAL_ERROR", status=500)`
