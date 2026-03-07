# Bài Thuyết trình: AI-DLC (AI-Driven Development Life Cycle)

---

## Slide 1: AI-DLC là gì?

**Tổng quan về AI-DLC**

- Phương pháp luận phát triển phần mềm thông minh.
- Sử dụng AI làm trợ lý xuyên suốt vòng đời phát triển dự án.
- Tự động điều chỉnh mức độ chi tiết và các giai đoạn cần thiết dựa trên độ phức tạp của yêu cầu.
- Thích ứng linh hoạt thay vì áp đặt một quy trình cứng nhắc.

---

## Slide 2: Vai trò của AI trong AI-DLC

**AI hoạt động như một Kiến trúc sư phần mềm:**

- Phân tích yêu cầu và tự động đặt câu hỏi làm rõ.
- Lên kế hoạch tối ưu dựa trên độ phức tạp và rủi ro.
- Bỏ qua các bước không cần thiết cho những thay đổi đơn giản.
- Ghi nhận tất cả quyết định, lý do vào audit trail.
- Hướng dẫn từng pha với checkpoint & cổng phê duyệt rõ ràng.

---

## Slide 3: Một số thuật ngữ quan trọng

- **Phase (Pha)**: Một trong 3 pha chính (INCEPTION, CONSTRUCTION, OPERATIONS).
- **Stage (Giai đoạn)**: Hoạt động cụ thể (VD: Requirements Analysis, Code Generation).
- **ALWAYS / CONDITIONAL**: Giai đoạn luôn thực thi / Chỉ chạy khi có điều kiện.
- **Unit of Work**: Nhóm logic các user story để phát triển song song.
- **Brownfield / Greenfield**: Dự án có codebase sẵn / Dự án làm mới từ đầu.
- **NFR**: Yêu cầu phi chức năng (Non-Functional Requirements).

---

## Slide 4: Nguyên tắc Cốt lõi (Phần 1)

- **1. Thích ứng (Adaptive)**: _"Workflow thích ứng với công việc, không phải ngược lại"_. Đánh giá dựa trên độ rõ ràng, thực trạng codebase, độ phức tạp và rủi ro.
- **2. Con người kiểm soát (Human in the Loop)**: Mọi quyết định quan trọng đều cần phê duyệt. AI đề xuất, con người phê duyệt (yêu cầu sửa / duyệt đi tiếp).

---

## Slide 5: Nguyên tắc Cốt lõi (Phần 2)

- **3. Không trùng lặp**: Source of truth chỉ nằm ở một nơi duy nhất. Các định dạng phát sinh được lấy từ nguồn gốc.
- **4. Tái tạo (Reproducible)**: Rule đủ chuẩn để các AI model khác nhau (Claude, GPT, v.v) đều tạo ra kết quả tương tự.
- **5. Bất khả tri (Agnostic)**: Hoạt động nhịp nhàng với mọi IDE, AI agent (Kiro, Cursor, Copilot...).

---

## Slide 6: Kiến trúc 3 Pha (Tổng quan)

- 🔵 **INCEPTION (Khởi tạo)**: Lập kế hoạch & Thiết kế ứng dụng. Xác định rõ **CÁI GÌ** cần xây và **TẠI SAO**.
- 🟢 **CONSTRUCTION (Xây dựng)**: Thiết kế chi tiết, Code & Kiểm thử. Xác định **LÀM THẾ NÀO** để xây và tiến hành xây.
- 🟡 **OPERATIONS (Vận hành)**: Triển khai & Giám sát hệ thống (Placeholder cho tương lai).

---

## Slide 7: Pha 1 - INCEPTION (Bước đầu)

Gồm 7 giai đoạn (3 bắt buộc, 4 theo điều kiện):

- **Workspace Detection (Luôn chạy)**: Phát hiện trạng thái dự án (Brownfield/Greenfield), tech stack, build system.
- **Reverse Engineering (Có điều kiện)**: Dành riêng cho dự án cũ (Brownfield). Khám phá codebase, architecture và component inventory.

---

## Slide 8: Pha INCEPTION - Phân tích Yêu cầu

- **Requirements Analysis (Luôn chạy - Độ sâu thích ứng)**:
  - Thu thập, xác nhận yêu cầu dự án.
  - Tùy biến phân tích theo 3 mức độ sâu: Minimal (đơn giản), Standard (bình thường), Comprehensive (phức tạp).
  - Tự động sinh ra các câu hỏi (Q&A) để làm rõ thiết kế.

---

## Slide 9: Pha INCEPTION - Đi sâu vào Tính năng

- **User Stories (Có điều kiện)**: Khi gặp tính năng người dùng cuối (user-facing) phức tạp.
  - Lập Story Plan, thu thập feedback, xuất ra `stories.md` và `personas.md`.
- **Workflow Planning (Luôn chạy)**:
  - Dựa vào yêu cầu, đưa ra quyết định các "giai đoạn" tiếp theo (Mermaid diagram minh hoạ).

---

## Slide 10: Pha INCEPTION - Thiết kế Hệ thống

- **Application Design (Có điều kiện)**:
  - Tạo tài liệu thiết kế services, components, dependency matrix.
- **Units Generation (Có điều kiện)**:
  - Phân rã hệ thống thành nhiều mảnh ghép nhỏ (Units of Work) để phân chia và triển khai độc lập, song song.

---

## Slide 11: Pha 2 - CONSTRUCTION (Cách vận hành)

- Vận hành bằng cơ chế **Vòng lặp (Per-Unit Loop)**.
- Từng "Unit of Work" được thiết kế, code, và hoàn chỉnh độc lập xong xuôi rồi mới chuyển sang Unit tiếp theo.

---

## Slide 12: Pha CONSTRUCTION - Thiết kế (Per-unit)

- **Functional Design (Có điều kiện)**: Xây dựng Logic nghiệp vụ, Domain models, Business rules. (Bất khả tri về hệ thống mạng hay máy chủ).
- **NFR Requirements (Có điều kiện)**: Khảo sát hiệu suất (performance), bảo mật, chọn tech stack.
- **NFR Design (Có điều kiện)**: Vẽ ra các design pattern như Resilience, Scalability (Caching, Queue) hay Security.

---

## Slide 13: Pha CONSTRUCTION - Cơ sở Hạ tầng

- **Infrastructure Design (Có điều kiện)**:
  - Ánh xạ từ các Component logic thành kiến trúc trên Cloud (AWS, Azure, GCP).
  - Quy chi tiết về Compute, Database, Networking, Monitoring.

---

## Slide 14: Pha CONSTRUCTION - Sinh Code

- **Code Generation (Luôn chạy, per-unit)**:
  - _Lập Kế Hoạch_: Lên danh sách step-by-step (VD: Logic -> DB -> API -> UI -> Test).
  - _Sinh Mã_: Ghi mã vào môi trường codebase (workspace root). Với dự án cũ (Brownfield), sẽ sửa đổi tại chỗ thay vì tạo bản sao.

---

## Slide 15: Pha CONSTRUCTION - Kiểm thử & Build

- **Build and Test (Luôn chạy)**:
  - Bắt đầu chạy sau khi TẤT CẢ các Unit of Work đã xong bước Code Generation.
  - Tạo ra các tệp hướng dẫn toàn diện bao gồm: `build-instructions.md`, `unit-test.md`, `integration-test.md`...

---

## Slide 16: Pha 3 - OPERATIONS (Vận hành)

- **Tình trạng**: Đang là chức năng _Placeholder_ để mở rộng trong bản release kế tiếp.
- **Chức năng mong đợi**:
  - Hỗ trợ Deployment workflow.
  - Cấu hình Monitoring & Observability setup.
  - Xây dựng Production readiness checklists.
  - Phản ứng với Incident response (Xử lý sự cố hệ thống).

---

## Slide 17: Tính Thích ứng (Adaptive) của AI-DLC

Sự điều chỉnh nằm ở cả **Hai Chiều**:

1. **Stage Selection (Nhị phân)**: Workflow Quyết định chạy hay skip hẳn một bước, tránh tốn sức ở các dự án giản đơn.
2. **Detail Level (Liên tục)**: Khi bước đó chạy, nó sẽ cho ra các Report/Artifact với mức độ dài/ngắn khác biệt (Chỉ lấy điểm chính vs Lập báo cáo dài tập).

---

## Slide 18: Các yếu tố tạo nên AI-DLC Adaptive

AI tính toán sự phức tạp để gen kết quả dựa trên 6 yếu tố:

1. Độ trơn tru của Yêu cầu ban đầu (Request Clarity).
2. Độ phức tạp của Vấn đề (Problem Complexity).
3. Tầm ảnh hưởng kỹ thuật (Scope).
4. Mức Rủi ro (Risk Level).
5. Trạng thái Dự án (Greenfield hay Brownfield).
6. Sở thích người dùng (Muốn xem Tóm lược hay Xem kịch bản đồ sộ).

---

## Slide 19: "Nguyên Tắc Vàng" của Cơ chế Thích ứng

> **"Tạo đúng mức chi tiết cần thiết cho vấn đề — không nhiều hơn, không ít hơn."**

- Giúp kỹ sư không bị nhức đầu vì đống document rườm rà cho một task cỏn con.
- Vẫn có một bức tranh bao quát siêu sâu khi cần phát triển một Platform tính logic cực mạnh.

---

## Slide 20: Tóm tắt chung

- **AI-DLC** xóa mờ ranh giới của quy trình SDLC cổ điển và tính cá nhân hóa của AI.
- Đặt **con người làm trọng tâm quyết định**, AI đóng vai trò trợ lý đắc lực lo liệu kế hoạch và thực thi (execution).
- Hỗ trợ đồng thời 6 multi-agent platforms IDEs danh tiếng (thực thi ở mọi nơi).
