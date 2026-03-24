# Ứng Dụng Quản Lý Công Việc (Todo List) — Tầm Nhìn Sản Phẩm

## Tổng Quan

Một ứng dụng quản lý công việc (todo list) full-stack đơn giản cho phép người dùng tạo, đọc, cập nhật và xóa các tác vụ. Ứng dụng cung cấp một giao diện web rõ ràng để quản lý các công việc hàng ngày với tính năng lọc và theo dõi việc hoàn thành.

## Các Tính Năng Cốt Lõi

### Quản Lý Tác Vụ
- Tạo các tác vụ mới với tiêu đề và mô tả tùy chọn
- Đánh dấu các tác vụ là đã hoàn thành hoặc chưa hoàn thành (chuyển đổi - toggle)
- Chỉnh sửa tiêu đề và mô tả của các tác vụ hiện có
- Xóa vĩnh viễn các tác vụ
- Xem tất cả các tác vụ trong một danh sách có thể cuộn được

### Bộ Lọc
- Lọc các tác vụ theo trạng thái: Tất Cả (All), Đang Hoạt Động (Active - chưa hoàn thành), Đã Hoàn Thành (Completed)
- Hiển thị số lượng các tác vụ đang hoạt động còn lại

### Lưu Trữ Dữ Liệu
- Các tác vụ được lưu trữ lại qua các lần làm mới trang thông qua API REST
- Máy chủ lưu trữ các tác vụ trong bộ nhớ (không yêu cầu cơ sở dữ liệu cho MVP)

## Giao Diện Người Dùng (UI)

Giao diện người dùng là một ứng dụng một trang (SPA) bao gồm:
- Một tiêu đề (header) hiển thị tên ứng dụng
- Một trường nhập liệu ở trên cùng để thêm các tác vụ mới
- Một danh sách các tác vụ ở bên dưới, mỗi tác vụ bao gồm:
  - Một hộp kiểm (checkbox) để chuyển đổi trạng thái hoàn thành
  - Tiêu đề tác vụ (có gạch ngang khi đã hoàn thành)
  - Một nút chỉnh sửa
  - Một nút xóa
- Một thanh lọc (filter bar) ở dưới cùng với các tab: Tất Cả / Đang Hoạt Động / Đã Hoàn Thành
- Một bộ đếm (counter) hiển thị "Còn lại X mục"

## Yêu Cầu Phi Chức Năng

- Ứng dụng cần tải trong thời gian dưới 2 giây
- Giao diện người dùng phải phản hồi nhanh (responsive) và hoạt động tốt trên giao diện điện thoại (mobile viewports)
- Tất cả các thao tác CRUD đều phải hoàn tất trong vòng chưa đầy 500ms
- API phải trả về các mã trạng thái HTTP và thông báo lỗi phù hợp

## Nằm Ngoài Phạm Vi (Đối Với MVP)

- Xác thực người dùng / hỗ trợ nhiều người dùng
- Ngày hết hạn hoặc mức độ ưu tiên của tác vụ
- Kéo thả để sắp xếp lại
- Lưu trữ bằng cơ sở dữ liệu (lưu trữ trong bộ nhớ là có thể chấp nhận)
- CI pipeline / Triển khai (Deployment)
