# Chân Dung Người Dùng

## Chân Dung 1: Thủ Thư (Quản Lý Thư Viện Tình Nguyện)

| Thuộc Tính | Chi Tiết |
|-----------|--------|
| **Tên** | Pat (Thủ Thư) |
| **Vai Trò** | Thủ Thư (Librarian) |
| **Mô Tả** | Tình nguyện viên quản lý bộ sưu tập vật lý tại thư viện cộng đồng. Thêm sách mới, xử lý trả sách, giải quyết tranh chấp mượn sách, và theo dõi các mục quá hạn. |
| **Mục Tiêu** | Quản lý danh mục hiệu quả, biết được ai đang có sách quá hạn chỉ với một cái nhìn, xử lý trả sách nhanh chóng, duy trì kho sách chính xác. |
| **Sự Thất Vọng** | Việc theo dõi thủ công bằng bảng tính, phải gửi email đòi sách quá hạn, mất sách mà không có ai chịu trách nhiệm. |
| **Sự Thoải Mái Về Kỹ Thuật** | Trung bình — thoải mái sử dụng giao diện web và API nhưng không phải là lập trình viên. |
| **Mẫu Sử Dụng** | Sử dụng hệ thống trong giờ mở cửa của thư viện (buổi tối/cuối tuần). Mức độ sử dụng tăng vọt khi xử lý trả sách hoặc sách quyên góp. |
| **Các Endpoint Chính** | CRUD sách, tìm kiếm sách, xử lý trả sách, báo cáo quá hạn, quản lý đặt trước (hold), xử lý thanh toán phí. |

## Chân Dung 2: Thành Viên (Người Mượn Sách)

| Thuộc Tính | Chi Tiết |
|-----------|--------|
| **Tên** | Alex (Thành Viên) |
| **Vai Trò** | Thành Viên (Member) |
| **Mô Tả** | Thành viên cộng đồng mượn và trả sách. Duyệt danh mục, đặt trước các sách phổ biến, và quản lý tài khoản của chính họ. |
| **Mục Tiêu** | Tìm các sách có sẵn một cách dễ dàng, mượn sách nhanh chóng, biết khi nào các sách đặt trước đã có, theo dõi ngày đến hạn. |
| **Sự Thất Vọng** | Không biết sách nào đang có sẵn, cơ hội tiếp cận sách phổ biến không công bằng, hay quên ngày đến hạn trả. |
| **Sự Thoải Mái Về Kỹ Thuật** | Đa dạng — API sẽ được một ứng dụng frontend tiêu thụ thay cho họ. |
| **Mẫu Sử Dụng** | Không thường xuyên — duyệt danh mục hàng tuần, mượn 1-3 cuốn sách mỗi tháng, trả sách trong thời hạn mượn. |
| **Các Endpoint Chính** | Tìm kiếm, mượn sách, trả sách, gia hạn, đặt/hủy đặt trước, các sách đang mượn, xem phí, quản lý hồ sơ. |

## Chân Dung 3: Quản Trị Viên (Quản Lý Vận Hành Thư Viện)

| Thuộc Tính | Chi Tiết |
|-----------|--------|
| **Tên** | Sam (Quản Trị Viên) |
| **Vai Trò** | Quản Trị Viên (Admin) |
| **Mô Tả** | Người chịu trách nhiệm về hoạt động tổng thể của thư viện. Quản lý tài khoản thành viên, cấu hình chính sách mượn sách, xem các báo cáo vận hành. |
| **Mục Tiêu** | Giám sát toàn bộ hoạt động của thư viện, quản lý các vấn đề của thành viên, xem xét các báo cáo sử dụng, đảm bảo việc thực thi chính sách công bằng. |
| **Sự Thất Trọng** | Không có khả năng nắm bắt mức độ sử dụng thư viện, không có cách quản lý các thành viên có vấn đề, thiếu các số liệu hoạt động. |
| **Sự Thoải Mái Về Kỹ Thuật** | Cao — thoải mái với bảng điều khiển quản trị, các công cụ API, và phân tích dữ liệu. |
| **Mẫu Sử Dụng** | Giám sát hàng ngày. Đôi khi thực hiện các hành động quản lý thành viên. Xem xét báo cáo hàng tuần. |
| **Các Endpoint Chính** | Tất cả các endpoint (quyền truy cập đầy đủ), tóm tắt bộ sưu tập, báo cáo quá hạn, quản lý thành viên, thanh toán phí, vô hiệu hóa tài khoản. |
