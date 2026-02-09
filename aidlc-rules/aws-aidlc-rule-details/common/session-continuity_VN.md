# Mẫu Tiếp tục Phiên làm việc

## Nhắc nhở Chào mừng Trở lại

Khi một người dùng quay lại để tiếp tục công việc trên một dự án AI-DLC hiện có, hãy trình bày lời nhắc này:

```markdown
**Chào mừng trở lại! Tôi có thể thấy bạn có một dự án AI-DLC đang tiến hành.**

Dựa trên aidlc-state.md của bạn, đây là trạng thái hiện tại của bạn:

- **Dự án**: [tên-dự-án]
- **Giai đoạn Hiện tại**: [INCEPTION/CONSTRUCTION/OPERATIONS]
- **Giai đoạn (Stage) Hiện tại**: [Tên Giai đoạn]
- **Hoàn thành Lần cuối**: [Bước hoàn thành cuối cùng]
- **Bước Tiếp theo**: [Bước tiếp theo để làm việc]

**Bạn muốn làm gì hôm nay?**

A) Tiếp tục nơi bạn đã dừng lại ([Mô tả bước tiếp theo])
B) Xem xét giai đoạn trước ([Hiển thị các giai đoạn có sẵn])

[Answer]:
```

## BẮT BUỘC: Hướng dẫn Tiếp tục Phiên làm việc

1. **Luôn đọc aidlc-state.md trước** khi phát hiện dự án hiện có
2. **Phân tích trạng thái hiện tại** từ tệp quy trình làm việc để điền vào lời nhắc
3. **BẮT BUỘC: Tải Artifact Giai đoạn Trước** - Trước khi tiếp tục bất kỳ giai đoạn nào, tự động đọc tất cả các artifact liên quan từ các giai đoạn trước:
   - **Kỹ thuật Đảo ngược**: Đọc architecture.md, code-structure.md, api-documentation.md
   - **Phân tích Yêu cầu**: Đọc requirements.md, requirement-verification-questions.md
   - **User Stories**: Đọc stories.md, personas.md, story-generation-plan.md
   - **Thiết kế Ứng dụng**: Đọc các artifact application-design (components.md, component-methods.md, services.md)
   - **Thiết kế (Đơn vị)**: Đọc unit-of-work.md, unit-of-work-dependency.md, unit-of-work-story-map.md
   - **Thiết kế Theo Đơn vị**: Đọc functional-design.md, nfr-requirements.md, nfr-design.md, infrastructure-design.md
   - **Các Giai đoạn Mã**: Đọc tất cả các tệp mã, kế hoạch, VÀ tất cả các artifact trước đó
4. **Tải Ngữ cảnh Thông minh theo Giai đoạn**:
   - **Các Giai đoạn Đầu (Phát hiện Workspace, Kỹ thuật Đảo ngược)**: Tải phân tích workspace
   - **Yêu cầu/Stories**: Tải kỹ thuật đảo ngược + artifact yêu cầu
   - **Các Giai đoạn Thiết kế**: Tải yêu cầu + stories + kiến trúc + artifact thiết kế
   - **Các Giai đoạn Mã**: Tải TẤT CẢ artifact + tệp mã hiện có
5. **Thích ứng tùy chọn** dựa trên lựa chọn kiến trúc và giai đoạn hiện tại
6. **Hiển thị các bước tiếp theo cụ thể** thay vì mô tả chung chung
7. **Ghi lại lời nhắc tiếp tục** trong audit.md với dấu thời gian
8. **Tóm tắt Ngữ cảnh**: Sau khi tải các artifact, cung cấp tóm tắt ngắn gọn về những gì đã được tải để người dùng biết
9. **Đặt câu hỏi**: LUÔN đặt câu hỏi làm rõ hoặc phản hồi người dùng bằng cách đặt chúng trong các tệp .md. KHÔNG KHÔNG đặt các câu hỏi trắc nghiệm trực tiếp trong phiên trò chuyện.

## Xử lý Lỗi

Nếu artifact bị thiếu hoặc bị hỏng trong quá trình tiếp tục phiên, xem [error-handling.md](error-handling.md) để được hướng dẫn về quy trình khôi phục.
