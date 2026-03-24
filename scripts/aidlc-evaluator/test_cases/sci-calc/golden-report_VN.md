# Báo Cáo Đánh Giá AIDLC

> **Lần chạy:** `20260218T125810-b84d042dff254a72b4ffec926fe5ea99`
> **Tạo lúc:** 2026-02-18T13:45:16+00:00

## Phán Quyết

| Yếu tố đánh giá | Kết quả |
|-----------|--------|
| Unit Test | ✅ **192/192** passed |
| Contract Test | ✅ **88/88** passed |
| Chất Lượng Code | ❌ 18 phát hiện (5 lỗi) |
| Điểm Định Tính | 🟢 **0.89** |

## Tổng Quan Lần Chạy

| Thuộc Tính | Giá Trị |
|----------|-------|
| Trạng Thái | `Status.COMPLETED` |
| Mô hình Executor | `global.anthropic.claude-opus-4-6-v1` |
| Mô hình Simulator| `us.anthropic.claude-sonnet-4-5-20250929-v1:0` |
| Khu Vực | `us-west-2` |
| Thời Gian Thực | 24.1m |
| Bàn Giao (Handoffs)| 3 (executor → simulator → executor) |
| Bắt Đầu | 2026-02-18T12:58:13.159285+00:00 |
| Hoàn Thành | 2026-02-18T13:22:44.249897+00:00 |

## Token Sử Dụng

| Tác nhân | Đầu vào | Đầu ra | Tổng |
|-------|------:|-------:|------:|
| Executor | 5.7M | 77K | 5.7M |
| Simulator | 180K | 2K | 182K |
| **Tổng** | **9.7M** | **140K** | **9.8M** |

## Dòng Thời Gian Bàn Giao

| # | Tác nhân | Thời lượng |
|--:|-------|----------|
| 1 | executor | 16.3m |
| 2 | simulator | 1.1m |
| 3 | executor | 6.7m |

## Artifacts Đã Tạo

| Danh Mục | Số Lượng |
|----------|------:|
| File mã nguồn | 17 |
| File kiểm thử (Test) | 18 |
| File cấu hình | 4 |
| Tổng số file | 72 |
| Dòng code | 3,522 |
| Tài liệu AIDLC (inception) | 8 |
| Tài liệu AIDLC (construction) | 5 |
| Tổng tài liệu AIDLC | 15 |

## Unit Test

**✅ 192 passed** / Tổng số 192

**Độ bao phủ:** 91.3%

## Contract Test (Đặc Tả API)

**✅ 88/88** endpoint đã được xác thực

### Sức khỏe ✅ 1/1

| Kiểm Thử | Phương Thức | Đường Dẫn | Trạng Thái | Độ Trễ |
|------|--------|------|:------:|--------:|
| ✅ health check | GET | `/health` | 200 | 14ms |


### Số Học ✅ 15/15

| Kiểm Thử | Phương Thức | Đường Dẫn | Trạng Thái | Độ Trễ |
|------|--------|------|:------:|--------:|
| ✅ add positive integers | POST | `/api/v1/arithmetic/add` | 200 | 4ms |
| ✅ add negative numbers | POST | `/api/v1/arithmetic/add` | 200 | 2ms |
| ✅ add floats | POST | `/api/v1/arithmetic/add` | 200 | 2ms |
| ✅ add missing field → 422 | POST | `/api/v1/arithmetic/add` | 422 | 2ms |
| ✅ subtract | POST | `/api/v1/arithmetic/subtract` | 200 | 2ms |
| ✅ multiply | POST | `/api/v1/arithmetic/multiply` | 200 | 2ms |
| ✅ multiply by zero | POST | `/api/v1/arithmetic/multiply` | 200 | 2ms |
| ✅ divide | POST | `/api/v1/arithmetic/divide` | 200 | 3ms |
| ✅ divide by zero → error | POST | `/api/v1/arithmetic/divide` | 400 | 2ms |
| ✅ modulo | POST | `/api/v1/arithmetic/modulo` | 200 | 2ms |
| ✅ modulo by zero → error | POST | `/api/v1/arithmetic/modulo` | 400 | 2ms |
| ✅ abs negative | POST | `/api/v1/arithmetic/abs` | 200 | 2ms |
| ✅ abs positive | POST | `/api/v1/arithmetic/abs` | 200 | 1ms |
| ✅ negate positive | POST | `/api/v1/arithmetic/negate` | 200 | 1ms |
| ✅ negate negative | POST | `/api/v1/arithmetic/negate` | 200 | 2ms |


### Lũy Thừa ✅ 11/11

| Kiểm Thử | Phương Thức | Đường Dẫn | Trạng Thái | Độ Trễ |
|------|--------|------|:------:|--------:|
| ✅ 2^10 | POST | `/api/v1/powers/power` | 200 | 3ms |
| ✅ 5^0 | POST | `/api/v1/powers/power` | 200 | 1ms |
| ✅ sqrt(16) | POST | `/api/v1/powers/sqrt` | 200 | 1ms |
| ✅ sqrt(0) | POST | `/api/v1/powers/sqrt` | 200 | 1ms |
| ✅ sqrt(-1) → domain error | POST | `/api/v1/powers/sqrt` | 400 | 2ms |
| ✅ cbrt(27) | POST | `/api/v1/powers/cbrt` | 200 | 2ms |
| ✅ cbrt(-8) | POST | `/api/v1/powers/cbrt` | 200 | 2ms |
| ✅ square(5) | POST | `/api/v1/powers/square` | 200 | 2ms |
| ✅ square(-3) | POST | `/api/v1/powers/square` | 200 | 1ms |
| ✅ 4th root of 16 | POST | `/api/v1/powers/nth_root` | 200 | 2ms |
| ✅ nth_root negative even → domain error | POST | `/api/v1/powers/nth_root` | 400 | 1ms |


### Lượng Giác ✅ 20/20

| Kiểm Thử | Phương Thức | Đường Dẫn | Trạng Thái | Độ Trễ |
|------|--------|------|:------:|--------:|
| ✅ sin(0) | POST | `/api/v1/trigonometry/sin` | 200 | 4ms |
| ✅ sin(90 deg) | POST | `/api/v1/trigonometry/sin` | 200 | 2ms |
| ✅ cos(0) | POST | `/api/v1/trigonometry/cos` | 200 | 2ms |
| ✅ tan(0) | POST | `/api/v1/trigonometry/tan` | 200 | 2ms |
| ✅ asin(0) | POST | `/api/v1/trigonometry/asin` | 200 | 2ms |
| ✅ asin(1) | POST | `/api/v1/trigonometry/asin` | 200 | 1ms |
| ✅ asin(2) → domain error | POST | `/api/v1/trigonometry/asin` | 400 | 1ms |
| ✅ acos(1) | POST | `/api/v1/trigonometry/acos` | 200 | 2ms |
| ✅ acos(2) → domain error | POST | `/api/v1/trigonometry/acos` | 400 | 2ms |
| ✅ atan(0) | POST | `/api/v1/trigonometry/atan` | 200 | 2ms |
| ✅ atan2(0, 1) | POST | `/api/v1/trigonometry/atan2` | 200 | 2ms |
| ✅ atan2(1, 0) | POST | `/api/v1/trigonometry/atan2` | 200 | 1ms |
| ✅ sinh(0) | POST | `/api/v1/trigonometry/sinh` | 200 | 2ms |
| ✅ cosh(0) | POST | `/api/v1/trigonometry/cosh` | 200 | 2ms |
| ✅ tanh(0) | POST | `/api/v1/trigonometry/tanh` | 200 | 2ms |
| ✅ asinh(0) | POST | `/api/v1/trigonometry/asinh` | 200 | 2ms |
| ✅ acosh(1) | POST | `/api/v1/trigonometry/acosh` | 200 | 1ms |
| ✅ acosh(0.5) → domain error | POST | `/api/v1/trigonometry/acosh` | 400 | 1ms |
| ✅ atanh(0) | POST | `/api/v1/trigonometry/atanh` | 200 | 2ms |
| ✅ atanh(1) → domain error | POST | `/api/v1/trigonometry/atanh` | 400 | 1ms |


### Logarit ✅ 11/11

| Kiểm Thử | Phương Thức | Đường Dẫn | Trạng Thái | Độ Trễ |
|------|--------|------|:------:|--------:|
| ✅ ln(1) | POST | `/api/v1/logarithmic/ln` | 200 | 3ms |
| ✅ ln(e) | POST | `/api/v1/logarithmic/ln` | 200 | 2ms |
| ✅ ln(0) → domain error | POST | `/api/v1/logarithmic/ln` | 400 | 2ms |
| ✅ ln(-1) → domain error | POST | `/api/v1/logarithmic/ln` | 400 | 1ms |
| ✅ log10(100) | POST | `/api/v1/logarithmic/log10` | 200 | 1ms |
| ✅ log10(1) | POST | `/api/v1/logarithmic/log10` | 200 | 2ms |
| ✅ log2(8) | POST | `/api/v1/logarithmic/log2` | 200 | 2ms |
| ✅ log(8, base=2) | POST | `/api/v1/logarithmic/log` | 200 | 2ms |
| ✅ log base 1 → domain error | POST | `/api/v1/logarithmic/log` | 400 | 2ms |
| ✅ exp(0) | POST | `/api/v1/logarithmic/exp` | 200 | 2ms |
| ✅ exp(1) | POST | `/api/v1/logarithmic/exp` | 200 | 1ms |


### Thống Kê ✅ 12/12

| Kiểm Thử | Phương Thức | Đường Dẫn | Trạng Thái | Độ Trễ |
|------|--------|------|:------:|--------:|
| ✅ mean | POST | `/api/v1/statistics/mean` | 200 | 4ms |
| ✅ median odd count | POST | `/api/v1/statistics/median` | 200 | 2ms |
| ✅ median even count | POST | `/api/v1/statistics/median` | 200 | 2ms |
| ✅ mode | POST | `/api/v1/statistics/mode` | 200 | 2ms |
| ✅ stdev | POST | `/api/v1/statistics/stdev` | 200 | 2ms |
| ✅ variance | POST | `/api/v1/statistics/variance` | 200 | 2ms |
| ✅ pstdev | POST | `/api/v1/statistics/pstdev` | 200 | 2ms |
| ✅ pvariance | POST | `/api/v1/statistics/pvariance` | 200 | 2ms |
| ✅ min | POST | `/api/v1/statistics/min` | 200 | 2ms |
| ✅ max | POST | `/api/v1/statistics/max` | 200 | 2ms |
| ✅ sum | POST | `/api/v1/statistics/sum` | 200 | 1ms |
| ✅ count | POST | `/api/v1/statistics/count` | 200 | 1ms |


### Hằng Số ✅ 10/10

| Kiểm Thử | Phương Thức | Đường Dẫn | Trạng Thái | Độ Trễ |
|------|--------|------|:------:|--------:|
| ✅ get all constants | GET | `/api/v1/constants` | 200 | 3ms |
| ✅ get pi | GET | `/api/v1/constants/pi` | 200 | 2ms |
| ✅ get e | GET | `/api/v1/constants/e` | 200 | 1ms |
| ✅ get tau | GET | `/api/v1/constants/tau` | 200 | 2ms |
| ✅ get golden_ratio | GET | `/api/v1/constants/golden_ratio` | 200 | 3ms |
| ✅ get sqrt2 | GET | `/api/v1/constants/sqrt2` | 200 | 2ms |
| ✅ get ln2 | GET | `/api/v1/constants/ln2` | 200 | 2ms |
| ✅ get ln10 | GET | `/api/v1/constants/ln10` | 200 | 2ms |
| ✅ get inf | GET | `/api/v1/constants/inf` | 200 | 1ms |
| ✅ get nan | GET | `/api/v1/constants/nan` | 200 | 2ms |


### Chuyển Đổi ✅ 7/7

| Kiểm Thử | Phương Thức | Đường Dẫn | Trạng Thái | Độ Trễ |
|------|--------|------|:------:|--------:|
| ✅ 180 degrees to radians | POST | `/api/v1/conversions/angle` | 200 | 3ms |
| ✅ boiling point C to F | POST | `/api/v1/conversions/temperature` | 200 | 2ms |
| ✅ freezing point C to K | POST | `/api/v1/conversions/temperature` | 200 | 2ms |
| ✅ 1 meter to feet | POST | `/api/v1/conversions/length` | 200 | 2ms |
| ✅ 1 mile to kilometers | POST | `/api/v1/conversions/length` | 200 | 2ms |
| ✅ 1 kg to pounds | POST | `/api/v1/conversions/weight` | 200 | 1ms |
| ✅ 1 stone to kilograms | POST | `/api/v1/conversions/weight` | 200 | 1ms |


### Không Tồn Tại ✅ 1/1

| Kiểm Thử | Phương Thức | Đường Dẫn | Trạng Thái | Độ Trễ |
|------|--------|------|:------:|--------:|
| ✅ unknown endpoint → 404 | GET | `/api/v1/nonexistent` | 404 | 1ms |


## Chất Lượng Code

**❌ 18 phát hiện** (5 lỗi, 13 cảnh báo)

**Linter:** ruff 0.15.1

| File | Dòng | Mã | Thông Điệp | Mức Độ |
|------|-----:|------|---------|----------|
| `app.py` | 3 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `math_engine.py` | 7 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `math_engine.py` | 12 | `F401` | `typing.Any` được import nhưng không dùng | 🟡 warning |
| `arithmetic.py` | 65 | `E501` | Dòng quá dài (101 > 100) | 🔴 error |
| `arithmetic.py` | 78 | `E501` | Dòng quá dài (107 > 100) | 🔴 error |
| `logarithmic.py` | 3 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `logarithmic.py` | 72 | `E501` | Dòng quá dài (108 > 100) | 🔴 error |
| `powers.py` | 74 | `E501` | Dòng quá dài (103 > 100) | 🔴 error |
| `trigonometry.py` | 75 | `E501` | Dòng quá dài (109 > 100) | 🔴 error |
| `conftest.py` | 8 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `test_arithmetic.py` | 3 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `test_arithmetic.py` | 9 | `F401` | `sci_calc.engine.math_engine.MathOverflowError` được import nhưng không dùng | 🟡 warning |
| `test_constants.py` | 3 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `test_conversions.py` | 3 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `test_logarithmic.py` | 3 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `test_powers.py` | 3 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `test_statistics.py` | 3 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |
| `test_trigonometry.py` | 3 | `I001` | Khối import chưa được sắp xếp hoặc format | 🟡 warning |

*Trình quét bảo mật (bandit) không có sẵn.*

## Đánh Giá Định Tính (Tính Tương Đồng Ngữ Nghĩa)

**Tổng Điểm: 🟢 0.8910**

### Giai Đoạn Inception

| Tiêu Chí | Điểm Số |
|-----------|------:|
| Ý Định (Intent) | 0.90 |
| Thiết Kế (Design) | 0.89 |
| Độ Hoàn Thiện (Completeness)| 0.88 |
| **Tổng** | **0.89** |

| Tài Liệu | Ý Định | Thiết Kế | Hoàn Thiện | Tổng |
|----------|-------:|-------:|---------:|--------:|
| `component-dependency.md` | 1.00 | 0.95 | 0.90 | 0.96 |
| `component-methods.md` | 1.00 | 0.95 | 0.85 | 0.95 |
| `components.md` | 1.00 | 1.00 | 1.00 | 1.00 |
| `services.md` | 0.95 | 0.90 | 0.85 | 0.91 |
| `application-design-plan.md` | 1.00 | 1.00 | 1.00 | 1.00 |
| `execution-plan.md` | 1.00 | 0.95 | 0.95 | 0.97 |
| `requirement-verification-questions.md` | 0.30 | 0.40 | 0.50 | 0.38 |
| `requirements.md` | 0.95 | 0.95 | 0.95 | 0.95 |

<details><summary><code>component-dependency.md</code> — 0.96</summary>

Cả hai tài liệu đều nắm bắt được ý định giống nhau: lập tài liệu tổng quan dependency cho một service toán học sử dụng FastAPI với sự phân tách rõ ràng. Thiết kế tương đồng về mặt kiến trúc (routes, models, engine), với những dependency patterns như nhau. Các chênh lệch nhỏ là ứng viên (CANDIDATE) sử dụng file path (.py extensions) thay vì ký hiệu module, cung cấp lược đồ luồng dữ liệu thay cho luồng phụ thuộc tương quan. Tóm lại thì cả hai có độ tương đồng cực kỳ lớn.

</details>

<details><summary><code>component-methods.md</code> — 0.95</summary>

Ý định tương đồng hoàn toàn: cả hai tài liệu đều khai báo chung các phép toán học, cấu trúc request/response, và cấu trúc API. Những điểm chênh lệch phần lớn phụ thuộc ở việc đặt tên cho request model (VD: BinaryOperationRequest vs TwoOperandRequest). Tài liệu ứng viên (CANDIDATE) có bỏ bớt chi tiết bảng route path/method chung tuy nhiên chúng được mô tả cụ thể bên trong thông tin ngầm, dẫn đến độ tương đồng cuối vẫn rất cao.

</details>

<details><summary><code>components.md</code> — 1.00</summary>

Cả hai tài liệu mô tả kiến trúc components tương tự với cùng định dạng 4 lớp (app entry point, routes, models, engine). Tất cả hệ thống route models, engine layer tách biệt và phân bổ chuẩn chỉnh y chang nhau đều được thể hiện trọn vẹn, do vậy chúng đạt mức điểm tương quan ngữ nghĩa tối đa.

</details>

<details><summary><code>services.md</code> — 0.91</summary>

Cả hai tài liệu cùng mô tả chung cấu trúc mỏng với việc ủy quyền route-to-engine và đã loại bỏ lớp service. CANDIDATE có bao gồm phần 404 handler và đã bỏ lỡ chi tiết CORS middleware. Bù lại tài liệu này đã ghi chú thêm về health check nằm ngoài tài nguyên của REFERENCE, kéo điểm xuống đôi chút do thiếu thông tin tham chiếu chuẩn.

</details>

<details><summary><code>application-design-plan.md</code> — 1.00</summary>

Cả hai tài liệu đều nắm bắt được ý định tuyệt đối: kiến trúc ba layer (Routes, Models, Engine) dùng chung cho Scientific Calculator API cùng với FastAPI và Pydantic v2. Không có thêm các câu hỏi về technical design bởi tech-env hoàn chỉnh từ hai phía đều đề cập toàn bộ thông tin. Độ tương đồng cực kỳ lớn và chuẩn xác tuyệt đối trên nội dung tham chiếu.

</details>

<details><summary><code>execution-plan.md</code> — 0.97</summary>

Cả hai tài liệu đều có mục tiêu và ý định tương tự, ghi rõ cách triển khai strategy. Mọi thông tin và workflows đều được thể hiện rất đồng đều và logic. Khác biệt nhỏ ở sự súc tích trong bản ứng cử so với REFERENCE làm độ tương đồng có phần lệch nhau về mặt format nội dung trình bày.

</details>

<details><summary><code>requirement-verification-questions.md</code> — 0.38</summary>

Hai tài liệu cung cấp danh sách câu hỏi xác thực làm rõ về mặt ngữ nghĩa khá trái chiều trong mối băn khoăn về kỹ thuật. Trong khi REFERENCE tập trung vào xử lý phẩy động, giới hạn mảng, cấu hình CORS, xử lý định dạng NaN ... Thì CANDIDATE tập trung vào format trả về mode, xử lý chống tràn, đơn vị không định dạng và xác thực điểm phủ coverage.

</details>

<details><summary><code>requirements.md</code> — 0.95</summary>

Cả hai nắm bắt gần như toàn bộ thông tin xác suất. Một số khác biệt nhỏ nhặt đến từ nhóm chức năng về cấu hình CORS hay thiết lập xử lý NaN, phân định FR có sự khác nhau cơ bản đôi chút trong chuẩn đánh số đánh format yêu cầu. Nhìn chung bao hàm tương tự nhau.

</details>

### Giai Đoạn Construction

| Tiêu Chí | Điểm Số |
|-----------|------:|
| Ý Định (Intent) | 0.93 |
| Thiết Kế (Design) | 0.85 |
| Độ Hoàn Thiện (Completeness)| 0.90 |
| **Tổng** | **0.89** |

| Tài Liệu | Ý Định | Thiết Kế | Hoàn Thiện | Tổng |
|----------|-------:|-------:|---------:|--------:|
| `build-and-test-summary.md` | 0.95 | 0.90 | 0.95 | 0.93 |
| `build-instructions.md` | 0.85 | 0.75 | 0.80 | 0.80 |
| `integration-test-instructions.md` | 0.85 | 0.75 | 0.90 | 0.82 |
| `unit-test-instructions.md` | 1.00 | 0.90 | 0.95 | 0.95 |
| `sci-calc-code-generation-plan.md` | 1.00 | 0.95 | 0.90 | 0.96 |

<details><summary><code>build-and-test-summary.md</code> — 0.93</summary>

Hai tài liệu nắm bắt được kết quả của quá trình build và test thành công và sẵn sàng để deploy. Kiến trúc của test bao gồm có 192 số lượng tests pass bao phủ (so với 187 bên REFERENCE do đã có tinh chỉnh). Kèm theo những tài liệu fix bug (NaN validator) chi tiết bên CANDIDATE và SyncTestClient giải pháp asyncio ở nhánh Windows... Chỉ có sự khác biệt nhỏ về cấu trúc báo file dẫn đến khác điểm.

</details>

<details><summary><code>build-instructions.md</code> — 0.80</summary>

Chia sẻ yêu cầu hướng dẫn build dùng Python 3.13+ và biến thể ứng cử thêm nhiều thông tin hướng đến build backend (hatchling), cấu hình version rạch ròi. Chi tiết này bên REFERENCE lược bỏ cho tinh gọn mà tập trung workflow nhanh chóng hơn.

</details>

<details><summary><code>integration-test-instructions.md</code> — 0.82</summary>

Cả hai mô tả tích hợp kiểm nghiệm cho dự án tính toán từ phản hồi/gửi vòng lặp qua HTTP requests và error. Ứng viên (CANDIDATE) thể hiện chi tiết rõ với 63 lượt test qua 7 domain trong khi reference là 5 bối cảnh scenario chung. Điểm mạnh ở việc gia tăng độ chi tiết với endpoint và count test.

</details>

<details><summary><code>unit-test-instructions.md</code> — 0.95</summary>

Cả hai chia sẻ mục tiêu unit testing execution cho toán với pytest và coverage >= 90%. Design gần như là song hành ngoại trừ ứng viên thêm Window asyncio fallback và báo cáo architecture test rõ ràng phân nhãn hơn. Dữ liệu bao phủ có phần chênh số ở khâu thống kê 95.20% bên REFERENCE.

</details>

<details><summary><code>sci-calc-code-generation-plan.md</code> — 0.96</summary>

Tài liệu thể hiện lộ trình mục tiêu tương tự của Scientific calculator API requirement này. Gồm một kiến trúc design y đúc (Engine/models/routes) cùng framework FastAPI, component breakdown. Ứng cử viên đề cập phân mảnh rõ cho mỗi nhánh implementation cho operation error steps trong khi reference gộp thành luồng lớn cơ bản.

</details>

## So Sánh Với Cơ Sở (Baseline)

> So sánh với cơ sở chuẩn (golden baseline): `20260218T125810-b84d042dff254a72b4ffec926fe5ea99`
> Nâng cấp: 2026-02-18T13:45:06+00:00

| | Số Lượng |
|---|------:|
| 🟢 Cải thiện | 0 |
| 🔴 Thụt lùi | 0 |
| ⚪ Không đổi | 20 |

### Unit Test

| Chỉ Số | Chuẩn | Hiện Tại | Chênh Lệch | Thay Đổi |
|--------|-------:|--------:|------:|--------|
| Test Passed | 192 | 192 | ⚪ 0 | không đổi |
| Test Failed | 0 | 0 | ⚪ 0 | không đổi |
| Tổng số Test | 192 | 192 | ⚪ 0 | không đổi |
| % Bao Phủ | 91 | 91 | ⚪ 0 | không đổi |

### Contract Test

| Chỉ Số | Chuẩn | Hiện Tại | Chênh Lệch | Thay Đổi |
|--------|-------:|--------:|------:|--------|
| Contract Passed | 88 | 88 | ⚪ 0 | không đổi |
| Contract Failed | 0 | 0 | ⚪ 0 | không đổi |
| Tổng Contract | 88 | 88 | ⚪ 0 | không đổi |

### Chất Lượng Code

| Chỉ Số | Chuẩn | Hiện Tại | Chênh Lệch | Thay Đổi |
|--------|-------:|--------:|------:|--------|
| Lỗi Lint | 5 | 5 | ⚪ 0 | không đổi |
| Cảnh Báo Lint | 13 | 13 | ⚪ 0 | không đổi |
| Tổng Lint | 18 | 18 | ⚪ 0 | không đổi |

### Định Tính

| Chỉ Số | Chuẩn | Hiện Tại | Chênh Lệch | Thay Đổi |
|--------|-------:|--------:|------:|--------|
| Điểm Định Tính | 0.8910 | 0.8910 | ⚪ 0 | không đổi |
| Điểm Inception | 0.8900 | 0.8900 | ⚪ 0 | không đổi |
| Điểm Construction| 0.8920 | 0.8920 | ⚪ 0 | không đổi |

### Artifacts

| Chỉ Số | Chuẩn | Hiện Tại | Chênh Lệch | Thay Đổi |
|--------|-------:|--------:|------:|--------|
| File Mã Nguồn | 17 | 17 | ⚪ 0 | không đổi |
| File Test | 18 | 18 | ⚪ 0 | không đổi |
| Dòng Code | 3,522 | 3,522 | ⚪ 0 | không đổi |
| File Tài Liệu | 15 | 15 | ⚪ 0 | không đổi |

### Quá Trình Thực Thi

| Chỉ Số | Chuẩn | Hiện Tại | Chênh Lệch | Thay Đổi |
|--------|-------:|--------:|------:|--------|
| Tổng số Token | 9,835,935 | 9,835,935 | ⚪ 0 | không đổi |
| Thời Gian (ms) | 1,445,460 | 1,445,460 | ⚪ 0 | không đổi |
| Bàn Giao | 3 | 3 | ⚪ 0 | không đổi |

---
*Báo cáo được tạo bởi aidlc-reporting v0.1.0*
