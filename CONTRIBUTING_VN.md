# Hướng dẫn Đóng góp

Cảm ơn bạn đã quan tâm đến việc đóng góp cho AI-DLC. Cho dù là báo cáo lỗi, quy tắc mới, sửa đổi hay cải thiện tài liệu, chúng tôi trân trọng phản hồi và đóng góp từ cộng đồng.

Vui lòng đọc kỹ tài liệu này trước khi gửi bất kỳ issue hoặc pull request nào.

## Nguyên lý

Trước khi đóng góp, hãy làm quen với [các nguyên lý](README.md#tenets) của chúng tôi.

## Quy tắc Đóng góp

Các quy tắc AI-DLC nằm trong `aidlc-rules/aws-aidlc-rule-details/`. Khi đóng góp:

- **Có thể tái tạo**: Các thay đổi phải được tái tạo nhất quán thông qua test case hoặc một chuỗi các bước.
- **Nguồn sự thật duy nhất**: Không sao chép nội dung. Nếu hướng dẫn áp dụng cho nhiều giai đoạn, hãy đặt nó vào `common/` và tham chiếu đến nó.
- **Giữ tính trung lập**: Phương pháp cốt lõi không nên giả định các IDE, agent hoặc model cụ thể. Các tệp cụ thể cho công cụ được tạo từ nguồn.

### Cấu trúc Quy tắc

Các quy tắc được tổ chức theo giai đoạn:

- `common/` - Hướng dẫn chung cho tất cả các giai đoạn
- `inception/` - Các quy tắc về lập kế hoạch và kiến trúc
- `construction/` - Các quy tắc về thiết kế và triển khai
- `operations/` - Các quy tắc về triển khai và giám sát

### Kiểm thử Thay đổi

Kiểm thử các thay đổi quy tắc của bạn với ít nhất một nền tảng được hỗ trợ (Amazon Q Developer, Kiro hoặc các công cụ khác) trước khi gửi. Mô tả những gì bạn đã kiểm thử trong PR của mình.

## Báo cáo Lỗi/Yêu cầu Tính năng

Sử dụng GitHub issues để báo cáo lỗi hoặc đề xuất tính năng. Trước khi gửi, hãy kiểm tra các issue hiện có để tránh trùng lặp.

Bao gồm:

- Quy tắc hoặc giai đoạn nào bị ảnh hưởng
- Hành vi mong đợi so với thực tế
- Nền tảng/model bạn đã kiểm thử cùng

## Đóng góp qua Pull Requests

Trước khi gửi pull request:

1. Làm việc trên branch `main` mới nhất
2. Kiểm tra các PR đang mở và mới được merge gần đây
3. Mở issue trước cho các thay đổi lớn

Để gửi:

1. Fork kho lưu trữ (repository)
2. Thực hiện thay đổi của bạn (giữ chúng tập trung)
3. Sử dụng thông điệp commit rõ ràng theo [conventional commits](https://www.conventionalcommits.org/) (ví dụ: `feat:`, `fix:`, `docs:`)
4. Gửi PR và phản hồi các nhận xét

## Quy tắc Ứng xử

Dự án này đã áp dụng [Quy tắc Ứng xử Nguồn mở của Amazon](https://aws.github.io/code-of-conduct).

Để biết thêm thông tin, hãy xem [Câu hỏi thường gặp về Quy tắc Ứng xử](https://aws.github.io/code-of-conduct-faq) hoặc liên hệ opensource-codeofconduct@amazon.com nếu có thêm bất kỳ câu hỏi hoặc nhận xét nào.

## Thông báo Vấn đề Bảo mật

Nếu bạn phát hiện ra vấn đề bảo mật tiềm ẩn, hãy thông báo cho AWS/Amazon Security thông qua [trang báo cáo lỗ hổng](http://aws.amazon.com/security/vulnerability-reporting/). Vui lòng không tạo GitHub issue công khai.

## Cấp phép

Xem tệp [LICENSE](LICENSE) để biết về việc cấp phép của dự án chúng tôi. Chúng tôi sẽ yêu cầu bạn xác nhận việc cấp phép cho đóng góp của bạn.
