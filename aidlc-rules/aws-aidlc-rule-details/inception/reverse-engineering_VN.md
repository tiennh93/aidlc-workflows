# Kỹ thuật Đảo ngược (Reverse Engineering)

**Mục đích**: Phân tích codebase hiện có và tạo các artifact thiết kế toàn diện

**Thực thi khi**: Dự án brownfield được phát hiện (tìm thấy mã hiện có trong workspace)

**Bỏ qua khi**: Dự án greenfield (không có mã hiện có)

**Hành vi chạy lại**: Luôn chạy lại khi phát hiện dự án brownfield, ngay cả khi các artifact tồn tại. Điều này đảm bảo các artifact phản ánh trạng thái mã hiện tại.

## Bước 1: Khám phá Đa Gói

### 1.1 Quét Workspace

- Tất cả các gói (không chỉ những gói được đề cập)
- Mối quan hệ gói qua các tệp cấu hình
- Các loại gói: Ứng dụng, CDK/Cơ sở hạ tầng, Mô hình, Clients, Tests

### 1.2 Hiểu Ngữ cảnh Kinh doanh

- Hoạt động kinh doanh cốt lõi mà hệ thống đang triển khai tổng thể
- Tổng quan kinh doanh của mọi gói
- Danh sách các Giao dịch Kinh doanh được triển khai trong hệ thống

### 1.3 Khám phá Cơ sở hạ tầng

- Các gói CDK (package.json với các phụ thuộc CDK)
- Terraform (các tệp .tf)
- CloudFormation (mẫu .yaml/.json)
- Các tập lệnh triển khai

### 1.4 Khám phá Hệ thống Xây dựng

- Hệ thống xây dựng: Brazil, Maven, Gradle, npm
- Các tệp cấu hình cho khai báo hệ thống xây dựng
- Các phụ thuộc xây dựng giữa các gói

### 1.5 Khám phá Kiến trúc Dịch vụ

- Lambda functions (handlers, triggers)
- Dịch vụ Container (cấu hình Docker/ECS)
- Định nghĩa API (mô hình Smithy, thông số kỹ thuật OpenAPI)
- Kho dữ liệu (DynamoDB, S3, v.v.)

### 1.6 Phân tích Chất lượng Mã

- Ngôn ngữ lập trình và frameworks
- Chỉ số bao phủ kiểm thử
- Cấu hình Linting
- Quy trình CI/CD

## Bước 1: Tạo Tài liệu Tổng quan Kinh doanh

Tạo `aidlc-docs/inception/reverse-engineering/business-overview.md`:

```markdown
# Business Overview

## Business Context Diagram

[Mermaid diagram showing the Business Context]

## Business Description

- **Business Description**: [Overall Business description of what the system does]
- **Business Transactions**: [List of Business Transactions that the system implements and their descriptions]
- **Business Dictionary**: [Business dictionary terms that the system follows and their meaning]

## Component Level Business Descriptions

### [Package/Component Name]

- **Purpose**: [What it does from the business perspective]
- **Responsibilities**: [Key responsibilities]
```

## Bước 2: Tạo Tài liệu Kiến trúc

Tạo `aidlc-docs/inception/reverse-engineering/architecture.md`:

```markdown
# System Architecture

## System Overview

[High-level description of the system]

## Architecture Diagram

[Mermaid diagram showing all packages, services, data stores, relationships]

## Component Descriptions

### [Package/Component Name]

- **Purpose**: [What it does]
- **Responsibilities**: [Key responsibilities]
- **Dependencies**: [What it depends on]
- **Type**: [Application/Infrastructure/Model/Client/Test]

## Data Flow

[Mermaid sequence diagram of key workflows]

## Integration Points

- **External APIs**: [List with purposes]
- **Databases**: [List with purposes]
- **Third-party Services**: [List with purposes]

## Infrastructure Components

- **CDK Stacks**: [List with purposes]
- **Deployment Model**: [Description]
- **Networking**: [VPC, subnets, security groups]
```

## Bước 3: Tạo Tài liệu Cấu trúc Mã

Tạo `aidlc-docs/inception/reverse-engineering/code-structure.md`:

```markdown
# Code Structure

## Build System

- **Type**: [Maven/Gradle/npm/Brazil]
- **Configuration**: [Key build files and settings]

## Key Classes/Modules

[Mermaid class diagram or module hierarchy]

### Existing Files Inventory

[List all source files with their purposes - these are candidates for modification in brownfield projects]

**Example format**:

- `[path/to/file]` - [Purpose/responsibility]

## Design Patterns

### [Pattern Name]

- **Location**: [Where used]
- **Purpose**: [Why used]
- **Implementation**: [How implemented]

## Critical Dependencies

### [Dependency Name]

- **Version**: [Version number]
- **Usage**: [How and where used]
- **Purpose**: [Why needed]
```

## Bước 4: Tạo Tài liệu API

Tạo `aidlc-docs/inception/reverse-engineering/api-documentation.md`:

```markdown
# API Documentation

## REST APIs

### [Endpoint Name]

- **Method**: [GET/POST/PUT/DELETE]
- **Path**: [/api/path]
- **Purpose**: [What it does]
- **Request**: [Request format]
- **Response**: [Response format]

## Internal APIs

### [Interface/Class Name]

- **Methods**: [List with signatures]
- **Parameters**: [Parameter descriptions]
- **Return Types**: [Return type descriptions]

## Data Models

### [Model Name]

- **Fields**: [Field descriptions]
- **Relationships**: [Related models]
- **Validation**: [Validation rules]
```

## Bước 5: Tạo Kho Thành phần

Tạo `aidlc-docs/inception/reverse-engineering/component-inventory.md`:

```markdown
# Component Inventory

## Application Packages

- [Package name] - [Purpose]

## Infrastructure Packages

- [Package name] - [CDK/Terraform] - [Purpose]

## Shared Packages

- [Package name] - [Models/Utilities/Clients] - [Purpose]

## Test Packages

- [Package name] - [Integration/Load/Unit] - [Purpose]

## Total Count

- **Total Packages**: [Number]
- **Application**: [Number]
- **Infrastructure**: [Number]
- **Shared**: [Number]
- **Test**: [Number]
```

## Bước 6: Tạo Tài liệu Ngăn xếp Công nghệ

Tạo `aidlc-docs/inception/reverse-engineering/technology-stack.md`:

```markdown
# Technology Stack

## Programming Languages

- [Language] - [Version] - [Usage]

## Frameworks

- [Framework] - [Version] - [Purpose]

## Infrastructure

- [Service] - [Purpose]

## Build Tools

- [Tool] - [Version] - [Purpose]

## Testing Tools

- [Tool] - [Version] - [Purpose]
```

## Bước 7: Tạo Tài liệu Phụ thuộc

Tạo `aidlc-docs/inception/reverse-engineering/dependencies.md`:

```markdown
# Dependencies

## Internal Dependencies

[Mermaid diagram showing package dependencies]

### [Package A] depends on [Package B]

- **Type**: [Compile/Runtime/Test]
- **Reason**: [Why dependency exists]

## External Dependencies

### [Dependency Name]

- **Version**: [Version]
- **Purpose**: [Why used]
- **License**: [License type]
```

## Bước 8: Tạo Đánh giá Chất lượng Mã

Tạo `aidlc-docs/inception/reverse-engineering/code-quality-assessment.md`:

```markdown
# Code Quality Assessment

## Test Coverage

- **Overall**: [Percentage or Good/Fair/Poor/None]
- **Unit Tests**: [Status]
- **Integration Tests**: [Status]

## Code Quality Indicators

- **Linting**: [Configured/Not configured]
- **Code Style**: [Consistent/Inconsistent]
- **Documentation**: [Good/Fair/Poor]

## Technical Debt

- [Issue description and location]

## Patterns and Anti-patterns

- **Good Patterns**: [List]
- **Anti-patterns**: [List with locations]
```

## Bước 9: Tạo Tệp Dấu thời gian

Tạo `aidlc-docs/inception/reverse-engineering/reverse-engineering-timestamp.md`:

```markdown
# Reverse Engineering Metadata

**Analysis Date**: [ISO timestamp]
**Analyzer**: AI-DLC
**Workspace**: [Workspace path]
**Total Files Analyzed**: [Number]

## Artifacts Generated

- [x] architecture.md
- [x] code-structure.md
- [x] api-documentation.md
- [x] component-inventory.md
- [x] technology-stack.md
- [x] dependencies.md
- [x] code-quality-assessment.md
```

## Bước 10: Cập nhật Theo dõi Trạng thái

Cập nhật `aidlc-docs/aidlc-state.md`:

```markdown
## Reverse Engineering Status

- [x] Reverse Engineering - Completed on [timestamp]
- **Artifacts Location**: aidlc-docs/inception/reverse-engineering/
```

## Bước 11: Trình bày Thông điệp Hoàn thành cho Người dùng

```markdown
# 🔍 Reverse Engineering Complete

[AI-generated summary of key findings from analysis in the form of bullet points]

> **📋 <u>**REVIEW REQUIRED:**</u>**  
> Please examine the reverse engineering artifacts at: `aidlc-docs/inception/reverse-engineering/`

> **🚀 <u>**WHAT'S NEXT?**</u>**
>
> **You may:**
>
> 🔧 **Request Changes** - Ask for modifications to the reverse engineering analysis if required
> ✅ **Approve & Continue** - Approve analysis and proceed to **Requirements Analysis**
```

## Bước 12: Chờ Người dùng Phê duyệt

- **BẮT BUỘC**: Không tiếp tục cho đến khi người dùng phê duyệt rõ ràng
- **BẮT BUỘC**: Ghi nhật ký phản hồi của người dùng trong audit.md với đầu vào thô đầy đủ
