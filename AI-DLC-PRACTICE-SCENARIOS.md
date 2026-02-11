# Kịch bản thực hành AI-DLC — Từ cơ bản đến nâng cao

Tài liệu này xây dựng **15 kịch bản thực hành** được thiết kế có hệ thống để giúp bạn thành thạo AI-DLC qua từng cấp độ. Mỗi kịch bản ghi rõ:

- Prompt khởi động
- Các giai đoạn dự kiến sẽ kích hoạt
- Điểm học tập trọng tâm
- Bài tập mở rộng

---

## Mục lục

- [Cấp 1: Làm quen (Greenfield đơn giản)](#cấp-1-làm-quen-greenfield-đơn-giản)
- [Cấp 2: Trung bình (Greenfield nhiều tính năng)](#cấp-2-trung-bình-greenfield-nhiều-tính-năng)
- [Cấp 3: Nâng cao (Greenfield phức tạp, multi-unit)](#cấp-3-nâng-cao-greenfield-phức-tạp-multi-unit)
- [Cấp 4: Brownfield (Codebase có sẵn)](#cấp-4-brownfield-codebase-có-sẵn)
- [Cấp 5: Tình huống đặc biệt](#cấp-5-tình-huống-đặc-biệt)
- [Ma trận tổng hợp kịch bản](#ma-trận-tổng-hợp-kịch-bản)
- [Checklist đánh giá kỹ năng](#checklist-đánh-giá-kỹ-năng)

---

## Cấp 1: Làm quen (Greenfield đơn giản)

> **Mục tiêu**: Hiểu flow cơ bản, trải nghiệm các giai đoạn ALWAYS, làm quen cách trả lời câu hỏi trong file `.md`

### Kịch bản 1.1 — Hello World CLI Tool

**Prompt**:

```
Using AI-DLC, create a simple CLI tool in Python that converts temperature between Celsius and Fahrenheit
```

**Dự kiến các giai đoạn kích hoạt**:

| Pha          | Giai đoạn             | Trạng thái | Lý do                                    |
| ------------ | --------------------- | ---------- | ---------------------------------------- |
| INCEPTION    | Workspace Detection   | ✅ ALWAYS  | Phát hiện greenfield                     |
| INCEPTION    | Reverse Engineering   | ⏭️ SKIP    | Greenfield — không có code               |
| INCEPTION    | Requirements Analysis | ✅ ALWAYS  | Depth: **Minimal** — yêu cầu rất rõ ràng |
| INCEPTION    | User Stories          | ⏭️ SKIP    | Không có user-facing UI phức tạp         |
| INCEPTION    | Workflow Planning     | ✅ ALWAYS  | Plan đơn giản                            |
| INCEPTION    | Application Design    | ⏭️ SKIP    | Single component đơn giản                |
| INCEPTION    | Units Generation      | ⏭️ SKIP    | Chỉ 1 unit                               |
| CONSTRUCTION | Functional Design     | ⏭️ SKIP    | Logic đơn giản                           |
| CONSTRUCTION | NFR Requirements      | ⏭️ SKIP    | Không có NFR                             |
| CONSTRUCTION | NFR Design            | ⏭️ SKIP    | Không có NFR                             |
| CONSTRUCTION | Infrastructure Design | ⏭️ SKIP    | CLI tool, không cần infrastructure       |
| CONSTRUCTION | Code Generation       | ✅ ALWAYS  | Sinh code                                |
| CONSTRUCTION | Build and Test        | ✅ ALWAYS  | Test + build instructions                |

**Điểm học tập**:

- [x] Workflow cơ bản: Welcome Message → Workspace Detection → Requirements → Workflow Planning → Code → Build
- [x] Cách `aidlc-state.md` được tạo và cập nhật (xem [workspace-detection.md](aidlc-rules/aws-aidlc-rule-details/inception/workspace-detection.md))
- [x] Cách phê duyệt từng giai đoạn (2 options: Request Changes / Approve & Continue)
- [x] Hiểu **Minimal depth** trong Requirements Analysis (xem [depth-levels.md](aidlc-rules/aws-aidlc-rule-details/common/depth-levels.md))

**Bài tập mở rộng**:

1. Sau khi hoàn thành, đóng chat và mở lại — quan sát **Session Continuity** (xem [session-continuity.md](aidlc-rules/aws-aidlc-rule-details/common/session-continuity.md))
2. Kiểm tra file `aidlc-docs/audit.md` — tất cả tương tác đã được ghi log chưa?

---

### Kịch bản 1.2 — Static Website Generator

**Prompt**:

```
Using AI-DLC, build a simple static site generator that converts Markdown files to HTML pages
```

**Dự kiến các giai đoạn kích hoạt**:

| Pha          | Giai đoạn             | Trạng thái                      |
| ------------ | --------------------- | ------------------------------- |
| INCEPTION    | Workspace Detection   | ✅ ALWAYS                       |
| INCEPTION    | Requirements Analysis | ✅ ALWAYS (Depth: **Standard**) |
| INCEPTION    | User Stories          | ⏭️ SKIP                         |
| INCEPTION    | Workflow Planning     | ✅ ALWAYS                       |
| CONSTRUCTION | Code Generation       | ✅ ALWAYS                       |
| CONSTRUCTION | Build and Test        | ✅ ALWAYS                       |

**Điểm học tập**:

- [x] Sự khác biệt giữa **Minimal** và **Standard** depth
- [x] Requirements Analysis tạo `requirement-verification-questions.md` — thực hành trả lời câu hỏi bằng `[Answer]:` tag
- [x] Format câu hỏi multiple-choice (xem [question-format-guide.md](aidlc-rules/aws-aidlc-rule-details/common/question-format-guide.md))

**Bài tập mở rộng**:

1. Cố ý trả lời mơ hồ (VD: "B hoặc C") → quan sát AI tạo follow-up questions
2. Thử chọn option "Other" và mô tả tùy chỉnh

---

### Kịch bản 1.3 — REST API đơn giản

**Prompt**:

```
Using AI-DLC, create a simple REST API in Node.js with Express that manages a to-do list with CRUD operations using in-memory storage
```

**Dự kiến**: Tương tự kịch bản 1.1 nhưng Requirements Analysis ở mức **Standard** vì có nhiều endpoints.

**Điểm học tập**:

- [x] Lần đầu thấy NFR Requirements có thể được kích hoạt (tùy AI đánh giá)
- [x] Code Generation plan có nhiều bước hơn: Business Logic → API Layer → Testing
- [x] Cấu trúc greenfield single unit: `src/`, `tests/` (xem [code-generation.md](aidlc-rules/aws-aidlc-rule-details/construction/code-generation.md))

---

## Cấp 2: Trung bình (Greenfield nhiều tính năng)

> **Mục tiêu**: Kích hoạt User Stories, Application Design, hiểu cơ chế thích ứng sâu hơn

### Kịch bản 2.1 — E-Commerce Backend

**Prompt**:

```
Using AI-DLC, build a backend for an e-commerce platform that supports product catalog management, shopping cart, user authentication, and order processing
```

**Dự kiến các giai đoạn kích hoạt**:

| Pha          | Giai đoạn             | Trạng thái                           | Lý do                                                |
| ------------ | --------------------- | ------------------------------------ | ---------------------------------------------------- |
| INCEPTION    | Workspace Detection   | ✅ ALWAYS                            | Greenfield                                           |
| INCEPTION    | Requirements Analysis | ✅ ALWAYS (Depth: **Comprehensive**) | Nhiều tính năng, phức tạp                            |
| INCEPTION    | User Stories          | ✅ CONDITIONAL                       | Nhiều persona (buyer, seller, admin), user-facing    |
| INCEPTION    | Workflow Planning     | ✅ ALWAYS                            | Cần plan chi tiết                                    |
| INCEPTION    | Application Design    | ✅ CONDITIONAL                       | Nhiều components (Product, Cart, Auth, Order)        |
| INCEPTION    | Units Generation      | ✅ CONDITIONAL                       | Có thể chia thành nhiều units                        |
| CONSTRUCTION | Functional Design     | ✅ CONDITIONAL                       | Business logic phức tạp (pricing, inventory, orders) |
| CONSTRUCTION | NFR Requirements      | ✅ CONDITIONAL                       | Security (auth), Performance (catalog)               |
| CONSTRUCTION | NFR Design            | ✅ CONDITIONAL                       | Design patterns cho NFRs                             |
| CONSTRUCTION | Infrastructure Design | ✅ CONDITIONAL                       | Database, caching, API gateway                       |
| CONSTRUCTION | Code Generation       | ✅ ALWAYS (per-unit)                 | Sinh code cho mỗi unit                               |
| CONSTRUCTION | Build and Test        | ✅ ALWAYS                            | Integration tests giữa units                         |

**Điểm học tập**:

- [x] **Comprehensive depth** — nhiều câu hỏi, yêu cầu chi tiết (xem [requirements-analysis.md](aidlc-rules/aws-aidlc-rule-details/inception/requirements-analysis.md))
- [x] **User Stories** kích hoạt: Thấy Part 1 (Planning) và Part 2 (Generation) (xem [user-stories.md](aidlc-rules/aws-aidlc-rule-details/inception/user-stories.md))
- [x] **Application Design** tạo `components.md`, `services.md`, `component-dependency.md` (xem [application-design.md](aidlc-rules/aws-aidlc-rule-details/inception/application-design.md))
- [x] **Units Generation** chia hệ thống thành units (xem [units-generation.md](aidlc-rules/aws-aidlc-rule-details/inception/units-generation.md))
- [x] **Vòng lặp per-unit** trong Construction — mỗi unit hoàn thành đầy đủ trước khi chuyển unit tiếp

**Bài tập mở rộng**:

1. Trong Workflow Planning, yêu cầu **bỏ qua** NFR Design → quan sát cảnh báo tác động (xem [workflow-changes.md](aidlc-rules/aws-aidlc-rule-details/common/workflow-changes.md))
2. Sau User Stories, yêu cầu **thêm** Application Design nếu AI bỏ qua → quan sát thêm giai đoạn

---

### Kịch bản 2.2 — Blog Platform với CMS

**Prompt**:

```
Using AI-DLC, create a blog platform with a content management system. It should support multiple authors, categories, comments, and a rich text editor. Include both a public-facing blog and an admin dashboard.
```

**Dự kiến**: Tương tự kịch bản 2.1, nhưng tập trung vào:

**Điểm học tập mới**:

- [x] Phân biệt **Component** vs **Service** vs **Module** (xem [terminology.md](aidlc-rules/aws-aidlc-rule-details/common/terminology.md))
- [x] **Personas** rõ ràng hơn: Author, Reader, Admin → map vào stories
- [x] Intelligent Assessment của User Stories (xem [user-stories.md](aidlc-rules/aws-aidlc-rule-details/inception/user-stories.md) — phần "Intelligent Assessment Guidelines")

---

### Kịch bản 2.3 — Real-time Chat Application

**Prompt**:

```
Using AI-DLC, build a real-time chat application with WebSocket support, user presence indicators, message history, file sharing, and end-to-end encryption
```

**Điểm học tập mới**:

- [x] **NFR Requirements** nặng: latency, concurrent connections, encryption (xem [nfr-requirements.md](aidlc-rules/aws-aidlc-rule-details/construction/nfr-requirements.md))
- [x] **NFR Design** patterns: connection pooling, message queuing, encryption patterns (xem [nfr-design.md](aidlc-rules/aws-aidlc-rule-details/construction/nfr-design.md))
- [x] **Infrastructure Design** mapping: WebSocket server, message broker, storage (xem [infrastructure-design.md](aidlc-rules/aws-aidlc-rule-details/construction/infrastructure-design.md))

---

## Cấp 3: Nâng cao (Greenfield phức tạp, multi-unit)

> **Mục tiêu**: Thành thạo multi-unit workflow, microservices decomposition, toàn bộ giai đoạn Construction

### Kịch bản 3.1 — Microservices E-Learning Platform

**Prompt**:

```
Using AI-DLC, design and build a microservices-based e-learning platform on AWS. Requirements:
- User service: registration, authentication, profiles with OAuth2
- Course service: course creation, curriculum management, media uploads
- Enrollment service: enrollment workflows, progress tracking, certificates
- Payment service: subscription plans, payment processing with Stripe
- Notification service: email, push notifications, in-app alerts
- API Gateway for routing and rate limiting
Each service should be independently deployable with its own database.
```

**Dự kiến**: **TẤT CẢ giai đoạn** kích hoạt ở **Comprehensive depth**.

**Điểm học tập**:

- [x] **Units Generation** full power: 5+ units of work, dependency matrix (xem [units-generation.md](aidlc-rules/aws-aidlc-rule-details/inception/units-generation.md))
- [x] Cấu trúc greenfield multi-unit (microservices): `{unit-name}/src/`, `{unit-name}/tests/`
- [x] **Per-unit loop** thực tế: Functional Design → NFR Req → NFR Design → Infra Design → Code Gen cho MỖI service
- [x] **Build and Test** toàn diện: unit tests + integration tests + contract tests giữa services
- [x] `unit-of-work-dependency.md` và `unit-of-work-story-map.md` mapping chi tiết

**Bài tập mở rộng**:

1. Trong quá trình Code Generation cho unit thứ 2, yêu cầu **restart** unit thứ 1 → quan sát cascade impact (xem [workflow-changes.md](aidlc-rules/aws-aidlc-rule-details/common/workflow-changes.md))
2. Kiểm tra `audit.md` sau khi hoàn thành — xác nhận toàn bộ interactions được log

---

### Kịch bản 3.2 — Multi-tenant SaaS Platform

**Prompt**:

```
Using AI-DLC, build a multi-tenant SaaS project management platform similar to Jira/Asana. Requirements:
- Tenant isolation with separate schemas per tenant
- Role-based access control (Admin, Manager, Developer, Viewer)
- Project boards with Kanban and Scrum views
- Sprint planning, backlog management, story points
- Time tracking and reporting dashboards
- REST API + GraphQL API
- Webhook integrations for Slack, GitHub, GitLab
Deploy on AWS with CDK infrastructure as code.
```

**Điểm học tập mới**:

- [x] Complexity cao nhất → Comprehensive depth ở MỌI giai đoạn
- [x] **Application Design** phức tạp: nhiều components, services, business rules
- [x] **Infrastructure Design** chi tiết: CDK stacks, multi-tenant architecture, database per tenant
- [x] Thấy rõ sự khác biệt giữa **Functional Design** (technology-agnostic) và **NFR Design** (patterns cụ thể)

---

## Cấp 4: Brownfield (Codebase có sẵn)

> **Mục tiêu**: Kích hoạt Reverse Engineering, hiểu cách AI-DLC xử lý codebase hiện có, brownfield file modification rules

### Kịch bản 4.1 — Bug Fix trên dự án có sẵn

**Chuẩn bị**: Tạo một dự án Node.js đơn giản trước (VD: Express API với vài routes).

**Prompt**:

```
Using AI-DLC, fix the bug where the GET /users endpoint returns 500 error when the database connection times out. Add proper error handling and retry logic.
```

**Dự kiến**:

| Pha          | Giai đoạn             | Trạng thái                     | Lý do                               |
| ------------ | --------------------- | ------------------------------ | ----------------------------------- |
| INCEPTION    | Workspace Detection   | ✅ ALWAYS                      | Phát hiện **brownfield**            |
| INCEPTION    | Reverse Engineering   | ✅ CONDITIONAL                 | Codebase hiện có, chưa có artifacts |
| INCEPTION    | Requirements Analysis | ✅ ALWAYS (Depth: **Minimal**) | Bug fix rõ ràng                     |
| INCEPTION    | User Stories          | ⏭️ SKIP                        | Bug fix đơn giản                    |
| INCEPTION    | Workflow Planning     | ✅ ALWAYS                      | Plan đơn giản                       |
| CONSTRUCTION | Code Generation       | ✅ ALWAYS                      | Sửa file hiện có                    |
| CONSTRUCTION | Build and Test        | ✅ ALWAYS                      | Verify fix                          |

**Điểm học tập**:

- [x] **Reverse Engineering** lần đầu kích hoạt (xem [reverse-engineering.md](aidlc-rules/aws-aidlc-rule-details/inception/reverse-engineering.md)):
  - Tạo `business-overview.md`, `architecture.md`, `code-structure.md`, `api-documentation.md`, `component-inventory.md`, `technology-stack.md`
- [x] **Brownfield Code Generation** rules: sửa file tại chỗ, KHÔNG tạo bản copy (xem [code-generation.md](aidlc-rules/aws-aidlc-rule-details/construction/code-generation.md) — phần "Brownfield File Modification Rules")
- [x] Minimal depth — quy trình nhanh gọn cho bug fix

---

### Kịch bản 4.2 — Thêm tính năng vào dự án có sẵn

**Chuẩn bị**: Dùng lại dự án từ kịch bản 4.1 (đã có reverse engineering artifacts).

**Prompt**:

```
Using AI-DLC, add a notification system to the existing application. Users should receive email notifications when their orders are placed, shipped, and delivered. Include an admin panel to manage notification templates.
```

**Dự kiến**:

| Pha          | Giai đoạn             | Trạng thái                      | Lý do                               |
| ------------ | --------------------- | ------------------------------- | ----------------------------------- |
| INCEPTION    | Workspace Detection   | ✅ ALWAYS                       | Resume from `aidlc-state.md`        |
| INCEPTION    | Reverse Engineering   | ⏭️ CONDITIONAL                  | Artifacts đã tồn tại → có thể rerun |
| INCEPTION    | Requirements Analysis | ✅ ALWAYS (Depth: **Standard**) | Tính năng mới rõ ràng               |
| INCEPTION    | User Stories          | ✅ CONDITIONAL                  | User-facing feature, 2 personas     |
| INCEPTION    | Workflow Planning     | ✅ ALWAYS                       |                                     |
| INCEPTION    | Application Design    | ✅ CONDITIONAL                  | Component mới (Notification)        |
| CONSTRUCTION | Functional Design     | ✅ CONDITIONAL                  | Template engine, notification logic |
| CONSTRUCTION | NFR Requirements      | ✅ CONDITIONAL                  | Email delivery reliability          |
| CONSTRUCTION | Code Generation       | ✅ ALWAYS                       | Mix: sửa file cũ + tạo file mới     |
| CONSTRUCTION | Build and Test        | ✅ ALWAYS                       | Integration tests                   |

**Điểm học tập**:

- [x] **Session Continuity** thực tế: AI load `aidlc-state.md` và artifacts trước đó (xem [session-continuity.md](aidlc-rules/aws-aidlc-rule-details/common/session-continuity.md))
- [x] Brownfield Code Generation: **vừa sửa** file hiện có **vừa tạo** file mới
- [x] Workflow Planning phân tích **Cross-Package Impact** (xem [workflow-planning.md](aidlc-rules/aws-aidlc-rule-details/inception/workflow-planning.md) — phần "2.2 Change Impact Assessment")

---

### Kịch bản 4.3 — Migration kiến trúc (Lambda → Container)

**Chuẩn bị**: Dự án có AWS Lambda functions và CDK infrastructure.

**Prompt**:

```
Using AI-DLC, migrate the existing Lambda-based order processing service to run as a containerized service on AWS ECS Fargate. The service currently has 5 Lambda functions handling order creation, payment processing, inventory updates, notification dispatch, and order status queries. Maintain the same API contracts.
```

**Dự kiến**: **TẤT CẢ giai đoạn** kích hoạt, **Comprehensive depth**.

**Điểm học tập**:

- [x] **Reverse Engineering** phân tích kiến trúc Lambda hiện tại một cách chi tiết
- [x] **Workflow Planning** phát hiện **Architectural Transformation** (xem [workflow-planning.md](aidlc-rules/aws-aidlc-rule-details/inception/workflow-planning.md) — phần "2.1 Transformation Scope Detection")
- [x] **Infrastructure Design** map từ Lambda → ECS Fargate (xem [infrastructure-design.md](aidlc-rules/aws-aidlc-rule-details/construction/infrastructure-design.md))
- [x] **Multi-Module Coordination**: CDK stacks, networking, API Gateway updates (xem [workflow-planning.md](aidlc-rules/aws-aidlc-rule-details/inception/workflow-planning.md) — phần "Step 5")
- [x] **Component Relationship Mapping**: Primary Component → Infrastructure → Shared → Dependent

---

## Cấp 5: Tình huống đặc biệt

> **Mục tiêu**: Thành thạo xử lý edge cases, workflow changes, error recovery

### Kịch bản 5.1 — Thay đổi giữa workflow

**Prompt ban đầu**:

```
Using AI-DLC, build a task management API with user authentication
```

**Bài tập**: Sau khi hoàn thành Requirements Analysis, thực hiện từng hành động:

| Bước | Hành động                                                                    | Quan sát                                                                                                                                               |
| ---- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1    | Approve Requirements, để workflow tiếp tục                                   | User Stories có thể bị SKIP                                                                                                                            |
| 2    | Yêu cầu: "I want to add User Stories"                                        | AI thêm giai đoạn đã bỏ qua (xem [workflow-changes.md](aidlc-rules/aws-aidlc-rule-details/common/workflow-changes.md) — phần "Adding a Skipped Phase") |
| 3    | Approve User Stories, approve Workflow Planning                              | Construction bắt đầu                                                                                                                                   |
| 4    | Khi đang ở Code Generation, yêu cầu: "Skip NFR Design"                       | AI cảnh báo impact, yêu cầu confirm                                                                                                                    |
| 5    | Sau khi code 1 unit xong, yêu cầu: "Restart Functional Design for this unit" | AI archive artifacts cũ, chạy lại                                                                                                                      |

**Điểm học tập**:

- [x] 4 loại workflow changes: Thêm, Bỏ, Restart hiện tại, Restart trước
- [x] Cascade impact khi restart giai đoạn trước
- [x] Tất cả thay đổi được log trong `audit.md`

---

### Kịch bản 5.2 — Câu trả lời mơ hồ & Overconfidence Prevention

**Prompt**:

```
Using AI-DLC, build a data analytics dashboard
```

**Bài tập**: Khi AI tạo câu hỏi, cố ý trả lời mơ hồ:

| Câu hỏi AI               | Câu trả lời mơ hồ                     | Kỳ vọng                                   |
| ------------------------ | ------------------------------------- | ----------------------------------------- |
| "Loại database?"         | "Depends on the data"                 | AI tạo follow-up hỏi cụ thể hơn           |
| "Số lượng users?"        | "Not sure, maybe a few hundred"       | AI hỏi lại với thresholds cụ thể          |
| "Authentication method?" | "Mix of A and B"                      | AI hỏi: "Khi nào dùng A, khi nào dùng B?" |
| "Deployment?"            | "Somewhere between cloud and on-prem" | AI hỏi hybrid strategy cụ thể             |

**Điểm học tập**:

- [x] **Overconfidence Prevention** hoạt động (xem [overconfidence-prevention.md](aidlc-rules/aws-aidlc-rule-details/common/overconfidence-prevention.md))
- [x] AI KHÔNG tiến hành khi còn mơ hồ
- [x] Follow-up questions được tạo tự động
- [x] Contradiction detection khi trả lời mâu thuẫn (xem [question-format-guide.md](aidlc-rules/aws-aidlc-rule-details/common/question-format-guide.md) — phần "Contradiction and Ambiguity Detection")

---

### Kịch bản 5.3 — Session Continuity & Error Recovery

**Prompt**:

```
Using AI-DLC, build a payment processing microservice with Stripe integration
```

**Bài tập theo timeline**:

| Ngày   | Hành động                                              | Quan sát                                                                                                   |
| ------ | ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| Ngày 1 | Bắt đầu, hoàn thành Inception phase                    | `aidlc-state.md` ghi nhận tiến trình                                                                       |
| Ngày 2 | Mở lại chat → AI hiển thị "Welcome back" prompt        | Session Continuity load artifacts                                                                          |
| Ngày 2 | Chọn "Continue where you left off"                     | AI resume đúng vị trí                                                                                      |
| Ngày 3 | Xóa 1 file artifact (VD: `requirements.md`) rồi resume | Error Handling: AI phát hiện missing file                                                                  |
| Ngày 3 | Quan sát phản ứng                                      | AI đề xuất recovery (xem [error-handling.md](aidlc-rules/aws-aidlc-rule-details/common/error-handling.md)) |

**Điểm học tập**:

- [x] **Session Continuity** templates và smart context loading (xem [session-continuity.md](aidlc-rules/aws-aidlc-rule-details/common/session-continuity.md))
- [x] **Error Handling** 4 mức severity (xem [error-handling.md](aidlc-rules/aws-aidlc-rule-details/common/error-handling.md))
- [x] Recovery procedures khác nhau cho mỗi loại lỗi

---

## Ma trận tổng hợp kịch bản

Bảng dưới đây giúp bạn nhanh chóng chọn kịch bản phù hợp với kỹ năng muốn luyện:

| #   | Kịch bản             | Loại       | Depth         | Giai đoạn kích hoạt | Kỹ năng chính                           |
| --- | -------------------- | ---------- | ------------- | ------------------- | --------------------------------------- |
| 1.1 | CLI Tool             | Greenfield | Minimal       | 5/13                | Flow cơ bản, approval gates             |
| 1.2 | Static Site Gen      | Greenfield | Standard      | 5/13                | `[Answer]:` tag, question format        |
| 1.3 | REST API             | Greenfield | Standard      | 6/13                | Code Generation plan, project structure |
| 2.1 | E-Commerce           | Greenfield | Comprehensive | 12/13               | User Stories, Application Design, Units |
| 2.2 | Blog + CMS           | Greenfield | Comprehensive | 11/13               | Personas, Intelligent Assessment        |
| 2.3 | Chat App             | Greenfield | Comprehensive | 12/13               | NFR nặng, Infrastructure Design         |
| 3.1 | E-Learning μServices | Greenfield | Comprehensive | 13/13               | Multi-unit, per-unit loop               |
| 3.2 | SaaS Platform        | Greenfield | Comprehensive | 13/13               | Max complexity, CDK                     |
| 4.1 | Bug Fix              | Brownfield | Minimal       | 6/13                | Reverse Engineering, brownfield rules   |
| 4.2 | Add Feature          | Brownfield | Standard      | 10/13               | Session Continuity, mixed code gen      |
| 4.3 | Lambda→ECS           | Brownfield | Comprehensive | 13/13               | Architecture migration, multi-module    |
| 5.1 | Workflow Changes     | Greenfield | Standard      | Varies              | Mid-workflow changes                    |
| 5.2 | Mơ hồ                | Greenfield | Standard      | Varies              | Overconfidence Prevention               |
| 5.3 | Session Recovery     | Greenfield | Standard      | Varies              | Session Continuity, Error Handling      |

### Lộ trình học đề xuất

```
Tuần 1:  Kịch bản 1.1 → 1.2 → 1.3     (Làm quen flow cơ bản)
Tuần 2:  Kịch bản 2.1 → 2.2            (Kích hoạt nhiều giai đoạn hơn)
Tuần 3:  Kịch bản 2.3 → 3.1            (NFR + Multi-unit)
Tuần 4:  Kịch bản 4.1 → 4.2            (Brownfield basics)
Tuần 5:  Kịch bản 3.2 → 4.3            (Max complexity)
Tuần 6:  Kịch bản 5.1 → 5.2 → 5.3     (Edge cases & mastery)
```

---

## Checklist đánh giá kỹ năng

Sau khi hoàn thành tất cả kịch bản, tự đánh giá:

### Kiến thức nền tảng

- [ ] Phân biệt Phase vs Stage (xem [terminology.md](aidlc-rules/aws-aidlc-rule-details/common/terminology.md))
- [ ] Hiểu 3 pha: INCEPTION, CONSTRUCTION, OPERATIONS (xem [process-overview.md](aidlc-rules/aws-aidlc-rule-details/common/process-overview.md))
- [ ] Biết giai đoạn nào ALWAYS vs CONDITIONAL (xem [core-workflow.md](aidlc-rules/aws-aidlc-rules/core-workflow.md))
- [ ] Phân biệt Greenfield vs Brownfield

### Pha INCEPTION

- [ ] Hiểu Workspace Detection flow (xem [workspace-detection.md](aidlc-rules/aws-aidlc-rule-details/inception/workspace-detection.md))
- [ ] Biết khi nào Reverse Engineering kích hoạt (xem [reverse-engineering.md](aidlc-rules/aws-aidlc-rule-details/inception/reverse-engineering.md))
- [ ] Hiểu 3 mức depth của Requirements Analysis (xem [requirements-analysis.md](aidlc-rules/aws-aidlc-rule-details/inception/requirements-analysis.md))
- [ ] Biết Intelligent Assessment cho User Stories (xem [user-stories.md](aidlc-rules/aws-aidlc-rule-details/inception/user-stories.md))
- [ ] Hiểu Application Design tạo những artifacts gì (xem [application-design.md](aidlc-rules/aws-aidlc-rule-details/inception/application-design.md))
- [ ] Biết Units Generation chia hệ thống thế nào (xem [units-generation.md](aidlc-rules/aws-aidlc-rule-details/inception/units-generation.md))
- [ ] Hiểu Workflow Planning phân tích impact thế nào (xem [workflow-planning.md](aidlc-rules/aws-aidlc-rule-details/inception/workflow-planning.md))

### Pha CONSTRUCTION

- [ ] Hiểu per-unit loop (mỗi unit hoàn thành đầy đủ trước)
- [ ] Biết Functional Design là technology-agnostic (xem [functional-design.md](aidlc-rules/aws-aidlc-rule-details/construction/functional-design.md))
- [ ] Hiểu NFR pipeline: Requirements → Design → Infrastructure (xem [nfr-requirements.md](aidlc-rules/aws-aidlc-rule-details/construction/nfr-requirements.md), [nfr-design.md](aidlc-rules/aws-aidlc-rule-details/construction/nfr-design.md), [infrastructure-design.md](aidlc-rules/aws-aidlc-rule-details/construction/infrastructure-design.md))
- [ ] Biết Code Generation có Part 1 (Planning) + Part 2 (Generation) (xem [code-generation.md](aidlc-rules/aws-aidlc-rule-details/construction/code-generation.md))
- [ ] Hiểu Brownfield file modification rules (không tạo bản copy)
- [ ] Biết Build and Test tạo những loại test nào (xem [build-and-test.md](aidlc-rules/aws-aidlc-rule-details/construction/build-and-test.md))

### Cơ chế hỗ trợ

- [ ] Thành thạo format `[Answer]:` tag (xem [question-format-guide.md](aidlc-rules/aws-aidlc-rule-details/common/question-format-guide.md))
- [ ] Hiểu Adaptive Depth: Stage Selection vs Detail Level (xem [depth-levels.md](aidlc-rules/aws-aidlc-rule-details/common/depth-levels.md))
- [ ] Biết 4 loại workflow changes (xem [workflow-changes.md](aidlc-rules/aws-aidlc-rule-details/common/workflow-changes.md))
- [ ] Hiểu Session Continuity (xem [session-continuity.md](aidlc-rules/aws-aidlc-rule-details/common/session-continuity.md))
- [ ] Nắm Error Handling severity levels (xem [error-handling.md](aidlc-rules/aws-aidlc-rule-details/common/error-handling.md))
- [ ] Hiểu Content Validation rules (xem [content-validation.md](aidlc-rules/aws-aidlc-rule-details/common/content-validation.md))

### Kỹ năng thực hành

- [ ] Biết vị trí tất cả artifacts trong `aidlc-docs/`
- [ ] Kiểm tra `audit.md` sau mỗi workflow
- [ ] Biết cách resume session bị gián đoạn
- [ ] Có thể chủ động yêu cầu thêm/bỏ giai đoạn

---

> **Tham chiếu**: Xem [AI-DLC-GUIDE.md](AI-DLC-GUIDE.md) để hiểu chi tiết từng giai đoạn và file rule tương ứng.
