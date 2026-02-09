# Hướng dẫn chi tiết về AI-DLC (AI-Driven Development Life Cycle)

## Mục lục

1. [Tổng quan về AI-DLC](#1-tổng-quan-về-ai-dlc)
2. [Nguyên tắc cốt lõi](#2-nguyên-tắc-cốt-lõi)
3. [Kiến trúc 3 pha](#3-kiến-trúc-3-pha)
4. [Pha INCEPTION - Lập kế hoạch](#4-pha-inception---lập-kế-hoạch)
5. [Pha CONSTRUCTION - Xây dựng](#5-pha-construction---xây-dựng)
6. [Pha OPERATIONS - Vận hành](#6-pha-operations---vận-hành)
7. [Cơ chế thích ứng (Adaptive)](#7-cơ-chế-thích-ứng-adaptive)
8. [Các quy tắc hỗ trợ (Common Rules)](#8-các-quy-tắc-hỗ-trợ-common-rules)
9. [Cấu trúc thư mục đầu ra](#9-cấu-trúc-thư-mục-đầu-ra)
10. [Cách triển khai AI-DLC vào dự án](#10-cách-triển-khai-ai-dlc-vào-dự-án)
11. [Sơ đồ tham chiếu file nguồn](#11-sơ-đồ-tham-chiếu-file-nguồn)

---

## 1. Tổng quan về AI-DLC

**AI-DLC (AI-Driven Development Life Cycle)** là một phương pháp luận phát triển phần mềm thông minh, sử dụng AI làm trợ lý xuyên suốt vòng đời phát triển. Thay vì áp đặt một quy trình cứng nhắc, AI-DLC **tự động điều chỉnh** mức độ chi tiết và các giai đoạn cần thiết dựa trên độ phức tạp của yêu cầu.

> **Nguồn**: Đọc thêm tại [README.md](README.md) và bài blog chính thức trên [AWS DevOps Blog](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/).

### AI-DLC là gì?

Theo mô tả trong [aidlc-rules/aws-aidlc-rule-details/common/welcome-message.md](aidlc-rules/aws-aidlc-rule-details/common/welcome-message.md), AI-DLC hoạt động như một kiến trúc sư phần mềm giàu kinh nghiệm:

- **Phân tích yêu cầu** và đặt câu hỏi làm rõ khi cần
- **Lên kế hoạch tối ưu** dựa trên độ phức tạp và rủi ro
- **Bỏ qua bước không cần thiết** cho thay đổi đơn giản, nhưng cung cấp đầy đủ cho dự án phức tạp
- **Ghi nhận tất cả** quyết định và lý do vào audit trail
- **Hướng dẫn từng pha** với checkpoint và cổng phê duyệt rõ ràng

### Thuật ngữ quan trọng

> **Nguồn**: [aidlc-rules/aws-aidlc-rule-details/common/terminology.md](aidlc-rules/aws-aidlc-rule-details/common/terminology.md)

| Thuật ngữ             | Giải thích                                                                  |
| --------------------- | --------------------------------------------------------------------------- |
| **Phase (Pha)**       | Một trong ba pha chính: INCEPTION, CONSTRUCTION, OPERATIONS                 |
| **Stage (Giai đoạn)** | Hoạt động cụ thể trong mỗi pha (VD: Requirements Analysis, Code Generation) |
| **ALWAYS**            | Giai đoạn luôn thực thi                                                     |
| **CONDITIONAL**       | Giai đoạn chỉ thực thi khi đáp ứng điều kiện                                |
| **Unit of Work**      | Nhóm logic các user story để phát triển                                     |
| **Brownfield**        | Dự án có codebase sẵn                                                       |
| **Greenfield**        | Dự án bắt đầu từ đầu                                                        |
| **NFR**               | Non-Functional Requirements (Yêu cầu phi chức năng)                         |

---

## 2. Nguyên tắc cốt lõi

> **Nguồn**: [aidlc-rules/aws-aidlc-rules/core-workflow.md](aidlc-rules/aws-aidlc-rules/core-workflow.md) — phần "Adaptive Workflow Principle" và "Key Principles"

### 2.1. Thích ứng (Adaptive)

**"Workflow thích ứng với công việc, không phải ngược lại."**

AI model đánh giá thông minh dựa trên:

1. Ý định và mức độ rõ ràng của người dùng
2. Trạng thái codebase hiện tại
3. Độ phức tạp và phạm vi thay đổi
4. Đánh giá rủi ro và tác động

### 2.2. Con người kiểm soát (Human in the Loop)

Mọi quyết định quan trọng đều cần **phê duyệt rõ ràng** từ người dùng. AI đề xuất, con người phê duyệt. Mỗi giai đoạn kết thúc bằng 2 tùy chọn:

- 🔧 **Yêu cầu thay đổi** — chỉnh sửa kết quả
- ✅ **Phê duyệt & tiếp tục** — chuyển sang giai đoạn tiếp theo

### 2.3. Không trùng lặp

Source of truth chỉ nằm ở một nơi duy nhất. Nếu cần hỗ trợ định dạng khác, sẽ sinh tự động từ nguồn gốc.

### 2.4. Tái tạo (Reproducible)

Các rule đủ rõ ràng để các model AI khác nhau tạo ra kết quả tương tự.

### 2.5. Bất khả tri (Agnostic)

Hoạt động với bất kỳ IDE, agent, hoặc model AI nào.

---

## 3. Kiến trúc 3 pha

> **Nguồn**: [aidlc-rules/aws-aidlc-rule-details/common/process-overview.md](aidlc-rules/aws-aidlc-rule-details/common/process-overview.md)

```
                         User Request
                              │
                              ▼
        ╔═══════════════════════════════════════╗
        ║  🔵 INCEPTION PHASE                   ║
        ║  Lập kế hoạch & Thiết kế ứng dụng    ║
        ╠═══════════════════════════════════════╣
        ║ • Workspace Detection    (ALWAYS)     ║
        ║ • Reverse Engineering    (CONDITIONAL) ║
        ║ • Requirements Analysis  (ALWAYS)     ║
        ║ • User Stories           (CONDITIONAL) ║
        ║ • Workflow Planning      (ALWAYS)     ║
        ║ • Application Design     (CONDITIONAL) ║
        ║ • Units Generation       (CONDITIONAL) ║
        ╚═══════════════════════════════════════╝
                              │
                              ▼
        ╔═══════════════════════════════════════╗
        ║  🟢 CONSTRUCTION PHASE                ║
        ║  Thiết kế chi tiết, Code & Test       ║
        ╠═══════════════════════════════════════╣
        ║ • Vòng lặp per-unit:                  ║
        ║   - Functional Design    (CONDITIONAL) ║
        ║   - NFR Requirements     (CONDITIONAL) ║
        ║   - NFR Design           (CONDITIONAL) ║
        ║   - Infrastructure Design(CONDITIONAL) ║
        ║   - Code Generation      (ALWAYS)     ║
        ║ • Build and Test         (ALWAYS)     ║
        ╚═══════════════════════════════════════╝
                              │
                              ▼
        ╔═══════════════════════════════════════╗
        ║  🟡 OPERATIONS PHASE                  ║
        ║  Triển khai & Giám sát (tương lai)    ║
        ╠═══════════════════════════════════════╣
        ║ • Operations             (PLACEHOLDER) ║
        ╚═══════════════════════════════════════╝
                              │
                              ▼
                          Hoàn thành
```

**Tóm tắt**:

- **INCEPTION**: Xác định **CÁI GÌ** cần xây và **TẠI SAO**
- **CONSTRUCTION**: Xác định **LÀM THẾ NÀO** để xây + xây + kiểm thử
- **OPERATIONS**: **TRIỂN KHAI** và **VẬN HÀNH** (đang phát triển)

---

## 4. Pha INCEPTION - Lập kế hoạch

> **Nguồn chính**: [aidlc-rules/aws-aidlc-rules/core-workflow.md](aidlc-rules/aws-aidlc-rules/core-workflow.md) — phần "INCEPTION PHASE"

Pha Inception gồm **7 giai đoạn**, trong đó 3 giai đoạn luôn chạy và 4 giai đoạn có điều kiện.

### 4.1. Workspace Detection (LUÔN CHẠY)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/inception/workspace-detection.md](aidlc-rules/aws-aidlc-rule-details/inception/workspace-detection.md)

**Mục đích**: Phát hiện trạng thái workspace và xác định loại dự án.

**Quy trình**:

1. Kiểm tra file `aidlc-docs/aidlc-state.md` đã tồn tại chưa (nếu có → tiếp tục phiên cũ)
2. Quét workspace tìm source code, build files, cấu trúc dự án
3. Xác định **brownfield** (có code sẵn) hay **greenfield** (dự án mới)
4. Tạo file trạng thái ban đầu `aidlc-docs/aidlc-state.md`
5. Tự động chuyển sang giai đoạn tiếp theo (không cần phê duyệt)

**Kết quả**: Ghi nhận Project Type, Programming Languages, Build System, Project Structure.

### 4.2. Reverse Engineering (CÓ ĐIỀU KIỆN - Chỉ cho Brownfield)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/inception/reverse-engineering.md](aidlc-rules/aws-aidlc-rule-details/inception/reverse-engineering.md)

**Chạy khi**: Phát hiện codebase hiện có.  
**Bỏ qua khi**: Dự án greenfield hoặc đã có artifacts reverse engineering.

**Quy trình**:

1. Khám phá multi-package (ứng dụng, infrastructure, models, clients, tests)
2. Phân tích business context và transaction
3. Phát hiện infrastructure (CDK, Terraform, CloudFormation)
4. Phân tích code quality

**Artifacts đầu ra** (trong `aidlc-docs/inception/reverse-engineering/`):

- `business-overview.md` — Tổng quan nghiệp vụ
- `architecture.md` — Tài liệu kiến trúc hệ thống
- `code-structure.md` — Cấu trúc code và design patterns
- `api-documentation.md` — Tài liệu API
- `component-inventory.md` — Bảng kiểm kê components
- `technology-stack.md` — Stack công nghệ

### 4.3. Requirements Analysis (LUÔN CHẠY - Độ sâu thích ứng)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/inception/requirements-analysis.md](aidlc-rules/aws-aidlc-rule-details/inception/requirements-analysis.md)

**Mục đích**: Thu thập và xác nhận yêu cầu dự án.

**3 mức độ sâu**:
| Mức | Khi nào | Nội dung |
|-----|---------|----------|
| **Minimal** | Yêu cầu đơn giản, rõ ràng | Chỉ ghi nhận phân tích ý định |
| **Standard** | Phức tạp bình thường | Yêu cầu chức năng + phi chức năng |
| **Comprehensive** | Phức tạp, rủi ro cao | Yêu cầu chi tiết với truy vết |

**Quy trình gồm 9 bước**:

1. Load context reverse engineering (nếu brownfield)
2. Phân tích yêu cầu (Request Clarity, Type, Scope, Complexity)
3. Xác định mức độ sâu
4. Đánh giá yêu cầu hiện tại
5. Phân tích tính đầy đủ
6. Tạo câu hỏi làm rõ → file `requirement-verification-questions.md`
7. Tạo tài liệu yêu cầu → `requirements.md`
8. Cập nhật trạng thái
9. Trình bày và chờ phê duyệt

### 4.4. User Stories (CÓ ĐIỀU KIỆN)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/inception/user-stories.md](aidlc-rules/aws-aidlc-rule-details/inception/user-stories.md)

**Chạy khi**: Tính năng user-facing mới, nhiều persona, logic nghiệp vụ phức tạp.  
**Bỏ qua khi**: Refactoring nội bộ, bug fix đơn giản, thay đổi infrastructure.

**Gồm 2 phần**:

**Part 1 — Planning**:

1. Đánh giá nhu cầu user stories (Intelligent Assessment)
2. Tạo story plan với checklist
3. Tạo câu hỏi theo định dạng `[Answer]:` tag
4. Thu thập và phân tích câu trả lời
5. Hỏi follow-up nếu có mơ hồ

**Part 2 — Generation**:

1. Thực thi plan đã được phê duyệt
2. Tạo `stories.md` theo tiêu chí INVEST
3. Tạo `personas.md` với user archetypes
4. Map personas đến user stories

### 4.5. Workflow Planning (LUÔN CHẠY)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/inception/workflow-planning.md](aidlc-rules/aws-aidlc-rule-details/inception/workflow-planning.md)

**Mục đích**: Xác định giai đoạn nào cần chạy và tạo execution plan toàn diện.

**Quy trình**:

1. Load tất cả context trước đó (reverse engineering, requirements, user stories)
2. Phân tích phạm vi và tác động chi tiết
3. Xác định giai đoạn: User Stories, Application Design, Units Generation, NFR
4. Phân tích multi-module coordination (brownfield)
5. Tạo workflow visualization (Mermaid diagram)
6. Trình bày và chờ phê duyệt

**Đặc biệt với brownfield**: Phân tích Transformation Scope, Cross-Package Impact, Component Relationship Mapping.

### 4.6. Application Design (CÓ ĐIỀU KIỆN)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/inception/application-design.md](aidlc-rules/aws-aidlc-rule-details/inception/application-design.md)

**Chạy khi**: Cần components/services mới, cần thiết kế service layer, cần xác định component dependencies.

**Artifacts**:

- `components.md` — Định nghĩa components và trách nhiệm
- `component-methods.md` — Method signatures
- `services.md` — Định nghĩa services và orchestration
- `component-dependency.md` — Ma trận phụ thuộc

### 4.7. Units Generation (CÓ ĐIỀU KIỆN)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/inception/units-generation.md](aidlc-rules/aws-aidlc-rule-details/inception/units-generation.md)

**Chạy khi**: Hệ thống cần chia thành nhiều unit of work.  
**Bỏ qua khi**: Chỉ có một unit đơn giản.

**Cũng gồm 2 phần** (Planning + Generation):

- Planning: Tạo decomposition plan, câu hỏi, thu thập answers
- Generation: Thực thi plan → tạo `unit-of-work.md`, `unit-of-work-dependency.md`, `unit-of-work-story-map.md`

---

## 5. Pha CONSTRUCTION - Xây dựng

> **Nguồn chính**: [aidlc-rules/aws-aidlc-rules/core-workflow.md](aidlc-rules/aws-aidlc-rules/core-workflow.md) — phần "CONSTRUCTION PHASE"

Pha Construction hoạt động theo cơ chế **vòng lặp per-unit**: mỗi unit of work được thiết kế và code hoàn chỉnh trước khi chuyển sang unit tiếp theo.

### 5.1. Functional Design (CÓ ĐIỀU KIỆN, per-unit)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/construction/functional-design.md](aidlc-rules/aws-aidlc-rule-details/construction/functional-design.md)

**Chạy khi**: Data models mới, logic nghiệp vụ phức tạp, business rules cần thiết kế chi tiết.

**Tập trung vào**:

- Logic nghiệp vụ chi tiết và thuật toán
- Domain models với entities và relationships
- Business rules, validation logic, constraints
- **Technology-agnostic** (không quan tâm infrastructure)

**Artifacts** (trong `aidlc-docs/construction/{unit-name}/functional-design/`):

- `business-logic-model.md`
- `business-rules.md`
- `domain-entities.md`

### 5.2. NFR Requirements (CÓ ĐIỀU KIỆN, per-unit)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/construction/nfr-requirements.md](aidlc-rules/aws-aidlc-rule-details/construction/nfr-requirements.md)

**Chạy khi**: Có yêu cầu về performance, security, scalability, hoặc cần chọn tech stack.

**Đánh giá**:

- Scalability, Performance, Availability
- Security, Reliability, Maintainability
- Lựa chọn tech stack

**Artifacts**:

- `nfr-requirements.md`
- `tech-stack-decisions.md`

### 5.3. NFR Design (CÓ ĐIỀU KIỆN, per-unit)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/construction/nfr-design.md](aidlc-rules/aws-aidlc-rule-details/construction/nfr-design.md)

**Chạy khi**: NFR Requirements đã thực thi và cần áp dụng design patterns.

**Tập trung vào**:

- Resilience patterns (circuit breaker, retry, fallback)
- Scalability patterns (horizontal scaling, caching)
- Performance patterns (connection pooling, batching)
- Security patterns (authentication, encryption)

**Artifacts**:

- `nfr-design-patterns.md`
- `logical-components.md`

### 5.4. Infrastructure Design (CÓ ĐIỀU KIỆN, per-unit)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/construction/infrastructure-design.md](aidlc-rules/aws-aidlc-rule-details/construction/infrastructure-design.md)

**Chạy khi**: Cần mapping sang infrastructure services thực tế (AWS, Azure, GCP).

**Quy trình**: Map logical components → actual infrastructure choices (compute, storage, messaging, networking, monitoring).

**Artifacts**:

- `infrastructure-design.md`
- `deployment-architecture.md`
- `shared-infrastructure.md` (nếu có shared infrastructure)

### 5.5. Code Generation (LUÔN CHẠY, per-unit)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/construction/code-generation.md](aidlc-rules/aws-aidlc-rule-details/construction/code-generation.md)

**Gồm 2 phần**:

**Part 1 — Planning**:

- Phân tích unit context
- Tạo code generation plan chi tiết (với checkboxes)
- Xác định cấu trúc: Business Logic → Unit Testing → API Layer → Repository Layer → Database Migrations → Documentation → Deployment
- Chờ phê duyệt plan

**Part 2 — Generation**:

- Thực thi plan từng bước
- Ghi code vào **workspace root** (KHÔNG BAO GIỜ vào `aidlc-docs/`)
- **Brownfield**: Sửa file hiện có (không tạo bản copy)
- **Greenfield**: Tạo file mới
- Đánh dấu checkbox sau mỗi bước

**Quy tắc quan trọng về cấu trúc code**:
| Loại dự án | Pattern |
|-----------|---------|
| Brownfield | Dùng cấu trúc hiện có |
| Greenfield single unit | `src/`, `tests/`, `config/` |
| Greenfield multi-unit (microservices) | `{unit-name}/src/`, `{unit-name}/tests/` |
| Greenfield multi-unit (monolith) | `src/{unit-name}/`, `tests/{unit-name}/` |

### 5.6. Build and Test (LUÔN CHẠY)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/construction/build-and-test.md](aidlc-rules/aws-aidlc-rule-details/construction/build-and-test.md)

**Chạy sau khi tất cả units hoàn thành Code Generation.**

**Tạo hướng dẫn toàn diện**:

- `build-instructions.md` — Hướng dẫn build (prerequisites, steps, troubleshooting)
- `unit-test-instructions.md` — Chạy unit tests
- `integration-test-instructions.md` — Test tương tác giữa units
- `performance-test-instructions.md` — Load/stress testing (nếu áp dụng)
- `build-and-test-summary.md` — Tóm tắt tổng thể

---

## 6. Pha OPERATIONS - Vận hành

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/operations/operations.md](aidlc-rules/aws-aidlc-rule-details/operations/operations.md)

**Trạng thái hiện tại**: Pha này là **placeholder** cho tương lai.

**Phạm vi dự kiến**:

- Deployment planning và execution
- Monitoring và observability setup
- Incident response procedures
- Maintenance và support workflows
- Production readiness checklists

Hiện tại, các hoạt động build & test đã được xử lý trong pha CONSTRUCTION.

---

## 7. Cơ chế thích ứng (Adaptive)

> **Nguồn chi tiết**: [aidlc-rules/aws-aidlc-rule-details/common/depth-levels.md](aidlc-rules/aws-aidlc-rule-details/common/depth-levels.md)

### 7.1. Hai chiều thích ứng

**Stage Selection (Nhị phân)**: Workflow Planning quyết định CHẠY hoặc BỎ QUA mỗi giai đoạn.

**Detail Level (Liên tục)**: Khi giai đoạn chạy, **TẤT CẢ artifacts đều được tạo**, nhưng mức độ chi tiết thích ứng:

- **Vấn đề đơn giản** → Artifacts ngắn gọn, chỉ thông tin thiết yếu
- **Vấn đề phức tạp** → Artifacts toàn diện, chi tiết đầy đủ

### 7.2. Yếu tố ảnh hưởng mức chi tiết

1. **Request Clarity**: Yêu cầu rõ ràng đến đâu?
2. **Problem Complexity**: Không gian giải pháp phức tạp thế nào?
3. **Scope**: Một file, một component, hay toàn hệ thống?
4. **Risk Level**: Tác động của lỗi hoặc thiếu sót?
5. **Available Context**: Greenfield hay brownfield, tài liệu hiện có
6. **User Preferences**: Người dùng muốn ngắn gọn hay chi tiết?

### 7.3. Nguyên tắc vàng

> **"Tạo đúng mức chi tiết cần thiết cho vấn đề — không nhiều hơn, không ít hơn."**

---

## 8. Các quy tắc hỗ trợ (Common Rules)

### 8.1. Question Format Guide

> **Nguồn**: [aidlc-rules/aws-aidlc-rule-details/common/question-format-guide.md](aidlc-rules/aws-aidlc-rule-details/common/question-format-guide.md)

- **KHÔNG BAO GIỜ** hỏi câu hỏi trực tiếp trong chat
- Tất cả câu hỏi phải đặt trong file `.md` chuyên dụng
- Dùng format multiple-choice (A, B, C, D...) với `[Answer]:` tag
- **LUÔN** có option cuối "Other (please describe)"
- Phát hiện contradictions và ambiguities trong câu trả lời

### 8.2. Content Validation

> **Nguồn**: [aidlc-rules/aws-aidlc-rule-details/common/content-validation.md](aidlc-rules/aws-aidlc-rule-details/common/content-validation.md)

Trước khi tạo bất kỳ file nào:

- Validate Mermaid diagram syntax
- Validate ASCII art diagrams
- Escape ký tự đặc biệt
- Cung cấp text alternative cho nội dung trực quan
- Nếu validation thất bại → dùng fallback content

### 8.3. Session Continuity

> **Nguồn**: [aidlc-rules/aws-aidlc-rule-details/common/session-continuity.md](aidlc-rules/aws-aidlc-rule-details/common/session-continuity.md)

Khi người dùng quay lại phiên làm việc:

1. Đọc `aidlc-state.md` để xác định trạng thái
2. Load artifacts của các giai đoạn trước
3. Hiển thị prompt "Welcome back" với tiến trình hiện tại
4. Cho phép tiếp tục hoặc review giai đoạn trước

### 8.4. Error Handling

> **Nguồn**: [aidlc-rules/aws-aidlc-rule-details/common/error-handling.md](aidlc-rules/aws-aidlc-rule-details/common/error-handling.md)

4 mức severity: Critical, High, Medium, Low. Mỗi mức có quy trình xử lý riêng cho từng giai đoạn.

### 8.5. Overconfidence Prevention

> **Nguồn**: [aidlc-rules/aws-aidlc-rule-details/common/overconfidence-prevention.md](aidlc-rules/aws-aidlc-rule-details/common/overconfidence-prevention.md)

Nguyên tắc: **"Hỏi quá nhiều tốt hơn là giả định sai."**

- Mặc định: HỎI khi có bất kỳ sự mơ hồ nào
- Phân tích kỹ TẤT CẢ câu trả lời tìm ambiguities
- Tạo follow-up questions cho mọi response không rõ ràng
- KHÔNG tiến hành khi còn mơ hồ chưa giải quyết

### 8.6. Workflow Changes

> **Nguồn**: [aidlc-rules/aws-aidlc-rule-details/common/workflow-changes.md](aidlc-rules/aws-aidlc-rule-details/common/workflow-changes.md)

Hỗ trợ 4 loại thay đổi giữa workflow:

1. **Thêm giai đoạn đã bỏ qua** → Kiểm tra dependencies, cập nhật plan
2. **Bỏ qua giai đoạn đã lên kế hoạch** → Cảnh báo tác động, xác nhận
3. **Restart giai đoạn hiện tại** → Archive artifacts cũ, chạy lại
4. **Restart giai đoạn trước** → Cascade reset tất cả giai đoạn phụ thuộc

---

## 9. Cấu trúc thư mục đầu ra

> **Nguồn**: [aidlc-rules/aws-aidlc-rules/core-workflow.md](aidlc-rules/aws-aidlc-rules/core-workflow.md) — phần "Directory Structure"

```
<WORKSPACE-ROOT>/                       # ⚠️ CODE ỨNG DỤNG Ở ĐÂY
├── [cấu trúc dự án cụ thể]            # Tùy thuộc dự án
│
├── aidlc-docs/                         # 📄 CHỈ TÀI LIỆU
│   ├── inception/                      # 🔵 PHA INCEPTION
│   │   ├── plans/                      # Các plan files
│   │   ├── reverse-engineering/        # Artifacts reverse engineering (brownfield)
│   │   ├── requirements/               # Yêu cầu + câu hỏi
│   │   ├── user-stories/              # User stories + personas
│   │   └── application-design/         # Components, services, units
│   │
│   ├── construction/                   # 🟢 PHA CONSTRUCTION
│   │   ├── plans/                      # Các plan files
│   │   ├── {unit-name}/               # Artifacts per-unit
│   │   │   ├── functional-design/
│   │   │   ├── nfr-requirements/
│   │   │   ├── nfr-design/
│   │   │   ├── infrastructure-design/
│   │   │   └── code/                   # Chỉ markdown summaries
│   │   └── build-and-test/
│   │
│   ├── operations/                     # 🟡 PHA OPERATIONS (placeholder)
│   ├── aidlc-state.md                  # Tracking trạng thái workflow
│   └── audit.md                        # Audit trail đầy đủ
```

**QUY TẮC QUAN TRỌNG**:

- Code ứng dụng: **Workspace root** (KHÔNG BAO GIỜ trong `aidlc-docs/`)
- Tài liệu: **Chỉ trong `aidlc-docs/`**

---

## 10. Cách triển khai AI-DLC vào dự án

> **Nguồn**: [README.md](README.md) — phần "Quick Start"

### Bước 1: Tải rules

Tải release mới nhất từ [Releases page](../../releases/latest) và giải nén.

### Bước 2: Copy rules vào dự án

#### Với Kiro IDE/CLI

```bash
mkdir -p .kiro/steering
cp -R aidlc-rules/aws-aidlc-rules .kiro/steering/
cp -R aidlc-rules/aws-aidlc-rule-details .kiro/
```

Cấu trúc mong đợi:

```
<project-root>/
  .kiro/
    steering/
      aws-aidlc-rules/        # ← core-workflow.md ở đây
    aws-aidlc-rule-details/    # ← rule details ở đây
```

#### Với Amazon Q Developer IDE

```bash
mkdir -p .amazonq/rules
cp -R aidlc-rules/aws-aidlc-rules .amazonq/rules/
cp -R aidlc-rules/aws-aidlc-rule-details .amazonq/
```

Cấu trúc mong đợi:

```
<project-root>/
  .amazonq/
    rules/
      aws-aidlc-rules/
    aws-aidlc-rule-details/
```

#### Với agent khác

Đặt `aws-aidlc-rules/` vào nơi agent đọc project rules, và `aws-aidlc-rule-details/` ở cấp sibling.

### Bước 3: Bắt đầu sử dụng

Trong cửa sổ chat của AI agent, bắt đầu bằng cụm từ:

> **"Using AI-DLC, ..."** + mô tả ý định phát triển phần mềm của bạn.

Ví dụ:

- "Using AI-DLC, build a REST API for user management with JWT authentication"
- "Using AI-DLC, refactor the payment module to use microservices"
- "Using AI-DLC, add a notification system with email and SMS support"

### Bước 4: Tương tác với workflow

1. AI-DLC tự động kích hoạt và hiển thị **Welcome Message**
2. Trả lời **câu hỏi có cấu trúc** trong các file `.md` (không phải trong chat)
3. **Review kỹ** mọi plan mà AI tạo ra
4. **Phê duyệt từng giai đoạn** để duy trì quyền kiểm soát
5. Tất cả artifacts được lưu trong thư mục `aidlc-docs/`

---

## 11. Sơ đồ tham chiếu file nguồn

Bảng dưới đây liệt kê tất cả file rule và mục đích cụ thể của chúng:

### Core Rules

| File                                                                             | Mục đích                                                                         |
| -------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| [aws-aidlc-rules/core-workflow.md](aidlc-rules/aws-aidlc-rules/core-workflow.md) | **File chính** — định nghĩa toàn bộ workflow 3 pha, điều kiện chạy mỗi giai đoạn |

### Common (Quy tắc chung)

| File                                                                                                          | Mục đích                                     |
| ------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| [common/process-overview.md](aidlc-rules/aws-aidlc-rule-details/common/process-overview.md)                   | Tổng quan workflow với Mermaid diagram       |
| [common/terminology.md](aidlc-rules/aws-aidlc-rule-details/common/terminology.md)                             | Bảng thuật ngữ AI-DLC                        |
| [common/depth-levels.md](aidlc-rules/aws-aidlc-rule-details/common/depth-levels.md)                           | Giải thích cơ chế adaptive depth             |
| [common/welcome-message.md](aidlc-rules/aws-aidlc-rule-details/common/welcome-message.md)                     | Thông điệp chào mừng hiển thị khi bắt đầu    |
| [common/question-format-guide.md](aidlc-rules/aws-aidlc-rule-details/common/question-format-guide.md)         | Quy tắc format câu hỏi multiple-choice       |
| [common/session-continuity.md](aidlc-rules/aws-aidlc-rule-details/common/session-continuity.md)               | Hướng dẫn tiếp tục phiên làm việc            |
| [common/content-validation.md](aidlc-rules/aws-aidlc-rule-details/common/content-validation.md)               | Quy tắc validate nội dung trước khi tạo file |
| [common/error-handling.md](aidlc-rules/aws-aidlc-rule-details/common/error-handling.md)                       | Xử lý lỗi và phục hồi                        |
| [common/overconfidence-prevention.md](aidlc-rules/aws-aidlc-rule-details/common/overconfidence-prevention.md) | Ngăn AI giả định quá mức                     |
| [common/workflow-changes.md](aidlc-rules/aws-aidlc-rule-details/common/workflow-changes.md)                   | Xử lý thay đổi giữa workflow                 |
| [common/ascii-diagram-standards.md](aidlc-rules/aws-aidlc-rule-details/common/ascii-diagram-standards.md)     | Tiêu chuẩn vẽ ASCII diagram                  |

### Inception (Lập kế hoạch)

| File                                                                                                        | Mục đích                             |
| ----------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| [inception/workspace-detection.md](aidlc-rules/aws-aidlc-rule-details/inception/workspace-detection.md)     | Phát hiện trạng thái workspace       |
| [inception/reverse-engineering.md](aidlc-rules/aws-aidlc-rule-details/inception/reverse-engineering.md)     | Phân tích codebase hiện có           |
| [inception/requirements-analysis.md](aidlc-rules/aws-aidlc-rule-details/inception/requirements-analysis.md) | Thu thập và phân tích yêu cầu        |
| [inception/user-stories.md](aidlc-rules/aws-aidlc-rule-details/inception/user-stories.md)                   | Tạo user stories và personas         |
| [inception/workflow-planning.md](aidlc-rules/aws-aidlc-rule-details/inception/workflow-planning.md)         | Lên kế hoạch thực thi workflow       |
| [inception/application-design.md](aidlc-rules/aws-aidlc-rule-details/inception/application-design.md)       | Thiết kế component và service layer  |
| [inception/units-generation.md](aidlc-rules/aws-aidlc-rule-details/inception/units-generation.md)           | Phân rã hệ thống thành units of work |

### Construction (Xây dựng)

| File                                                                                                              | Mục đích                                       |
| ----------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| [construction/functional-design.md](aidlc-rules/aws-aidlc-rule-details/construction/functional-design.md)         | Thiết kế business logic chi tiết (per-unit)    |
| [construction/nfr-requirements.md](aidlc-rules/aws-aidlc-rule-details/construction/nfr-requirements.md)           | Xác định NFR và chọn tech stack (per-unit)     |
| [construction/nfr-design.md](aidlc-rules/aws-aidlc-rule-details/construction/nfr-design.md)                       | Áp dụng NFR patterns (per-unit)                |
| [construction/infrastructure-design.md](aidlc-rules/aws-aidlc-rule-details/construction/infrastructure-design.md) | Mapping sang infrastructure thực tế (per-unit) |
| [construction/code-generation.md](aidlc-rules/aws-aidlc-rule-details/construction/code-generation.md)             | Sinh code theo plan (per-unit)                 |
| [construction/build-and-test.md](aidlc-rules/aws-aidlc-rule-details/construction/build-and-test.md)               | Hướng dẫn build và test toàn diện              |

### Operations (Vận hành)

| File                                                                                    | Mục đích                                          |
| --------------------------------------------------------------------------------------- | ------------------------------------------------- |
| [operations/operations.md](aidlc-rules/aws-aidlc-rule-details/operations/operations.md) | Placeholder cho deployment & monitoring tương lai |

---

> **Lưu ý**: Mỗi file rule trên cũng có phiên bản tiếng Việt (`_VN.md`) trong cùng thư mục.
