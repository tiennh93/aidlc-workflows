# Quy tắc Bảo mật Cơ sở (Baseline Security Rules)

## Tổng quan

Các quy tắc bảo mật này là BẮT BUỘC (MANDATORY), mang tính chất ràng buộc xuyên suốt áp dụng trên tất cả các giai đoạn của AI-DLC. Chúng không phải là hướng dẫn tùy chọn — chúng là những ràng buộc cứng mà các giai đoạn PHẢI thực thi khi tạo câu hỏi, tạo các artifact thiết kế, tạo mã và trình bày các thông điệp hoàn thành.

**Thực thi**: Tại mỗi giai đoạn có thể áp dụng, mô hình PHẢI xác minh sự tuân thủ các quy tắc này trước khi hiển thị thông điệp hoàn thành giai đoạn cho người dùng.

### Hành vi Phát hiện Ngăn chặn Bảo mật (Blocking Security Finding Behavior)

Một **phát hiện ngăn chặn bảo mật** có nghĩa là:

1. Phát hiện đó PHẢI được liệt kê trong thông điệp hoàn thành giai đoạn ở mục "Các phát hiện bảo mật" (Security Findings) với ID quy tắc SECURITY và mô tả.
2. Giai đoạn này KHÔNG ĐƯỢC trình bày tùy chọn "Tiếp tục sang Giai đoạn Tiếp theo" cho đến khi tất cả các phát hiện ngăn chặn được giải quyết.
3. Mô hình PHẢI chỉ hiển thị tùy chọn "Yêu cầu Thay đổi" cùng với giải thích rõ ràng về những gì cần thay đổi.
4. Phát hiện đó PHẢI được ghi lại trong `aidlc-docs/audit.md` kèm theo ID quy tắc SECURITY, mô tả và ngữ cảnh giai đoạn.

Nếu một quy tắc SECURITY không áp dụng cho dự án hiện tại (ví dụ: SECURITY-01 khi không có kho lưu trữ dữ liệu), hãy đánh dấu nó là **N/A** trong bản tóm tắt tuân thủ — điều này không tính là một phát hiện ngăn chặn.

### Thực thi Mặc định

Theo mặc định, tất cả các quy tắc trong tài liệu này đều là **ngăn chặn (blocking)**. Nếu tiêu chí xác minh của bất kỳ quy tắc nào không được đáp ứng, thì đó sẽ là một phát hiện ngăn chặn bảo mật — hãy tuân theo hành vi phát hiện ngăn chặn được định nghĩa ở trên.

### Định dạng Tiêu chí Xác minh (Verification Criteria Format)

Các mục xác minh trong tài liệu này là các gạch đầu dòng dạng văn bản thuần túy mô tả các bước kiểm tra tuân thủ. Chúng khác với các hộp kiểm theo dõi tiến độ `- [ ]` / `- [x]` được dùng trong các tệp kế hoạch của các giai đoạn. Mỗi mục phải được đánh giá là tuân thủ (compliant) hoặc không tuân thủ (non-compliant) trong quá trình đánh giá.

---

## Câu hỏi về Khả năng Áp dụng (Applicability Question)

Câu hỏi sau đây sẽ tự động được đưa vào danh sách các Câu hỏi Làm rõ của phần Phân tích Yêu cầu khi extension này được nạp:

```markdown
## Câu hỏi: Các Extension Bảo mật

Có nên thực thi các quy tắc extension bảo mật cho dự án này không?

A) Có — thực thi tất cả các quy tắc SECURITY dưới dạng ràng buộc ngăn chặn (khuyến nghị cho các ứng dụng cấp production)
B) Không — bỏ qua tất cả các quy tắc SECURITY (phù hợp cho các PoC, nguyên mẫu, và các dự án thử nghiệm)
X) Khác (vui lòng mô tả sau thẻ [Answer]: bên dưới)

[Answer]:
```

---

## Quy tắc SECURITY-01: Mã hóa Trạng thái Nghỉ (Encryption at Rest) và Mã hóa Đang truyền (Encryption in Transit)

**Quy tắc**: Mọi kho lưu trữ dữ liệu bền bỉ (cơ sở dữ liệu, object storage, file systems, caches, hoặc bất kỳ hệ thống tương đương nào) ĐỀU PHẢI có:

- Mã hóa ở trạng thái nghỉ được bật bằng cách sử dụng dịch vụ khóa được quản lý (managed key service) hoặc các khóa do khách hàng quản lý (customer-managed keys)
- Mã hóa đang truyền được thực thi (TLS 1.2+ cho toàn bộ việc di chuyển dữ liệu vào và ra khỏi kho lưu trữ)

**Xác minh**:

- Không có tài nguyên lưu trữ nào được định nghĩa mà không có khối cấu hình mã hóa.
- Không có chuỗi kết nối cơ sở dữ liệu nào sử dụng giao thức không được mã hóa.
- Object storage phải thực thi mã hóa ở trạng thái nghỉ và từ chối các yêu cầu không phải TLS thông qua các bộ policy.
- Các instance của database có bật mã hóa lưu trữ và thực thi các kết nối TLS.

---

## Quy tắc SECURITY-02: Ghi nhật ký Truy cập trên các Trung gian Mạng (Access Logging on Network Intermediaries)

**Quy tắc**: Mọi mạng trung gian hướng ra ngoài xử lý lưu lượng truy cập bên ngoài PHẢI được bật ghi nhật ký truy cập (access logging). Bao gồm:

- Load balancers → access logs đến một kho lưu trữ bền bỉ
- API gateways → ghi nhật ký thực thi và ghi nhật ký truy cập đến một dịch vụ log tập trung
- Các phân phối CDN → logging tiêu chuẩn hoặc nhật ký thời gian thực (real-time logs)

**Xác minh**:

- Không định nghĩa tài nguyên load balancer nào mà chưa bật cấu hình logging truy cập.
- Không thiết lập API gateway stage nào mà chưa cấu hình logging truy cập.
- Không định nghĩa CDN distribution nào mà không có cấu hình logging.

---

## Quy tắc SECURITY-03: Ghi nhật ký ở cấp Độ Ứng dụng (Application-Level Logging)

**Quy tắc**: Mọi thành phần ứng dụng được triển khai PHẢI bao gồm cơ sở hạ tầng ghi nhật ký có cấu trúc:

- Một logging framework PHẢI được cấu hình
- Đầu ra của log PHẢI được hướng đến một dịch vụ log tập trung
- Logs PHẢI bao gồm: dấu thời gian (timestamp), correlation/request ID, cấp độ log (log level), và thông điệp
- Dữ liệu rạy cảm (mật khẩu, token, PII) KHÔNG ĐƯỢC XUẤT HIỆN trong đầu ra log

**Xác minh**:

- Mọi service/function entry point đều bao gồm một logger đã được cấu hình.
- Không có bất kỳ các câu lệnh log thủ công tự phát nào được sử dụng như là cơ chế log chính trong code production.
- Cấu hình log phải đưa output tới một dịch vụ log tập trung.
- Không có bí mật (secrets), token, hoặc PII nào được log ra.

---

## Quy tắc SECURITY-04: Header Bảo mật HTTP cho Ứng dụng Web

**Quy tắc**: Các HTTP response header sau đây PHẢI được thiết lập trên tất cả các endpoint trả về nội dung HTML:

| Header                      | Giá trị Yêu cầu                                                      |
| --------------------------- | -------------------------------------------------------------------- |
| `Content-Security-Policy`   | Định nghĩa một chính sách hạn chế (ít nhất là: `default-src 'self'`) |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains`                                |
| `X-Content-Type-Options`    | `nosniff`                                                            |
| `X-Frame-Options`           | `DENY` (hoặc `SAMEORIGIN` nếu cần dùng iframe)                       |
| `Referrer-Policy`           | `strict-origin-when-cross-origin`                                    |

**Lưu ý**: `X-XSS-Protection` không còn được hỗ trợ bởi các trình duyệt hiện đại. Dùng `Content-Security-Policy` để thay thế.

**Xác minh**:

- Middleware hoặc phản hồi interceptor đặt đầy đủ các header được yêu cầu.
- CSP policy không dùng `unsafe-inline` hoặc `unsafe-eval` mà không có văn bản giải trình lý do rõ ràng.
- HSTS max-age ít nhất là 31536000 (1 năm).

---

## Quy tắc SECURITY-05: Xác thực Dữ liệu Đầu vào (Input Validation) trên Mọi Tham số API

**Quy tắc**: Mỗi điểm cuối API (REST, GraphQL, gRPC, WebSocket) PHẢI xác thực tất cả phần tử đầu vào trước khi xử lý. Quá trình xác thực PHẢI bao gồm:

- **Kiểm tra kiểu dữ liệu**: Từ chối các dạng dữ liệu không mong muốn
- **Giới hạn Độ dài/Kích thước**: Bắt buộc tuân thủ chiều dài tối đa trên strings, giới hạn kích thước tối đa trên các mảng hoặc payloads
- **Xác nhận định dạng (Format validation)**: Sử dụng các danh sách quy định cho phép (allowlists - qua regex hoặc schema) cho các truy vấn có cấu trúc (email, thời gian, IDs)
- **Làm sạch dữ liệu (Sanitization)**: Loại bỏ các mã lệnh hay cấu trúc HTML trong thông tin đầu vào do người dùng nhập vào để phòng tránh XSS
- **Phòng chống Tiêm mã (Injection prevention)**: Bắt buộc dùng parameterized queries với moi hành vi cơ sở dữ liệu (tuyệt đối không nối string)

**Xác minh**:

- Mỗi trình xử lý API đều sử dụng thư viện validation hoặc schema.
- Không có thông tin đầu vào thô của người dùng được gộp dính rập khuôn vào truy vấn SQL, NoSQL hoặc Hệ điều hành
- Input dạng Strings có kiểm tra độ dài tối đa rõ ràng
- Size payloads của HTTP requests được chặn từ sớm ở góc độ framework hoặc cấp độ gateway lớn

---

## Quy tắc SECURITY-06: Nguyên tắc Chính sách Rút gọn Đặc Quyền tối Thiểu (Least-Privilege Access Policies)

**Quy tắc**: Bất kỳ hệ thống IAM Policy, phần vai trò (role) nào và các ranh giới cho phép PHẢI vận hành theo dạng ít đặc quyền nhất (least privilege):

- Dùng identifier nhận dạng resource chính xác — KHÔNG BAO GIỜ được thiết lập biểu tượng `*` wildcard để cho phép tất cả các tài nguyên trừ trường hợp các hệ thống thiết bị API chưa kịp hỗ trợ giới hạn resource level permission (hãy chú thích chi tiết vào báo cáo nếu có ngoại lệ này).
- Áp dụng quyền chi tiết — KHÔNG áp dụng cho mọi lệnh quyền lực.
- Giới hạn conditions bằng ngữ cảnh khi có thể
- Chia tách đọc/ghi thành nhiều lệnh khác nhau trong statement

**Xác minh**:

- Không một chính sách (policy) nào chứa wildcard về actions hoặc wildcard resource nếu không có tài liệu chú ý cho các trường hợp ngoại lệ.
- Không system roles hay Service Role nào nắm lượng quyền ưu tiên lớn vượt quyền hạn những gì Service đó cần phải request tới.
- Mọi vai trò system đều có trust policies bám sát vào mức dịch vụ đích giới hạn đó

---

## Quy tắc SECURITY-07: Cấu Hình Mạng Hạn chế Định Tuyến (Restrictive Network Configuration)

**Quy tắc**: Mọi cấu hình nền tảng Networking (tường lửa security groups, Network ACLs, Route table) PHẢI quy định quyền chặn mặc định (deny-by-default):

- Tường lửa: Chỉ cho phép lưu thông các ports rất nhỏ quan trọng theo ứng dụng
- Không rules luồng Inbound nhận vào bắt nguồn từ `0.0.0.0/0` ngoại trừ duy nhất tài nguyên cho Load Balancers hỗ trợ bên ngoài với port là 80/443
- Không rule Outbound hướng đi đến tận `0.0.0.0/0` trải đều trọn mọi cổng bất kể khi nào khi chưa cung cấp tài liệu giải trình cụ thể
- Cấu hình Private subnets tuyết đối KHÔNG cung cấp lộ trình điều hướng truy cập route cho các Gateway hạ tầng kết nối mạnh internet
- Lợi dụng mạng hỗ trợ Private endpoints đối với các ứng dụng nội bộ môi trường cloud khi khả dụ.

**Xác minh**:

- Không lệnh cấu hình tường lửa nào ủng hộ dải network IP đi vào là `0.0.0.0/0` cho mỗi ports được xem ngoài port số 443 hoặc 80 của khu tường firewall hướng nội
- Mọi database / server nội bộ block hạn định cứng chỉ theo CIDR duy nhất.
- Tài nguyên trong Private subnets phân luồng gói tin kết nối điều vận (routing) dọc qua một máy chủ NAT

---

## Quy tắc SECURITY-08: Quản Lý Phân quyền Và Phân Truy Nguồn Kiểm Soát tại Cấp Ứng dụng (Application-Level Access Control)

**Quy tắc**: Mỗi một endpoints gọi của phần backend xử lý lấy thông tin tác động lên đối tượng CẦN XÓA/THAY ĐỔI/CẬP NHẬT/READ bắt buộc có cơ chế quản trị danh tính an ninh tại nền tảng lập trình (application layer):

- **Chặn theo mặc định**: Mọi routing xử lí tác động vào hệ thống phải cấu trúc cho định danh login người sở hữu đăng nhập / được authenticate xác định mới thao tác, loại các API khai báo đặc quyền công khai ra.
- **Tính ủy quyền theo Đối tượng (Object-level authorization)**: Yêu cầu trích gọi tài nguyên do API call dùng qua chuỗi object ID PHẢI đảm bảo hệ thống xem xét đúng các ID đó khớp vào quyền thuộc và sỡ hữu xác đáng từ chính user-actor request chủ sở hữu đó thì mới gọi kết quả cho qua (mục tiêu nhằm chống đỡ lỡ lỗi kịch bản Insecure Direct Object References (IDOR))
- **Logic kiểm thử quản trị ranh giới hàm (Function-level authorization)**: Điều khiển cho nhóm admin backend KHÔNG NÊN làm chỉ một vế che lấp FrontEnd mà buộc phải được đánh giá Authorization server-side với roles từ tài khoản hiện tại có phù hợp xử lý cao.
- **Mã nguồn lệnh CORS policy**: Dịch vụ thông suốt Cross-origin resource chia sẻ bị kềm chế lại để ưu đãi gốc địa chỉ IP hoặc định kiến cụm hostname, đừng bao lâu áp dụng các dòng lệnh thả trôi trên RESTful auth. (thí dụ: `Access-Control-Allow-Origin: *`)
- **Giải tính trích Token Validation**: JSON Web Token JWT / mã xác thực chứng nhân session PHẢI xem kĩ càng từ cấu trúc của token cho từng calls ở máy Server. Kiểm lại mọi mặt của thành phần định dạng bên trong (tạo ra signature từ issuer/audience phù hợp).

**Xác minh**:

- Từng các phần routing controllers/handlers cần trang bị lớp xác duyệt an toàn phân phối giữa Middleware hoặc hệ Guards ứng dụng
- Không mã nguồn phần điều khiển lệnh API trích xuất tài nguyên kết quả về phía user khách mà chưa vượt qua công tác Authentication xác định danh tính với Object quyền điều quản tương thân
- Thông qua kiểm chứng không khai thác lệnh CORS hỗ trợ tất cả luồng Wildcard '\*' khi đó là đường line Endpoints có áp dụng tài khoản mật khẩu
- Công việc Token check / kiểm tính chuỗi hash thực hiện mọi mặt các bước của app, không buông phó bỏ dở chỉ lúc làm cho chức phận đăng nhập.

---

## Quy tắc SECURITY-09: Tăng cường Bảo mật (Hardening) và Phòng chống Cấu hình sai (Misconfiguration Prevention)

**Quy tắc**: Tất cả các thành phần được triển khai PHẢI tuân theo yêu cầu tăng cường bảo mật tĩnh:

- **Không sử dụng thông tin xác thực mặc định**: Usernames/Bí mật cấp mặc định của cài đặt gốc PHẢI bị vô hiệu tính năng trước sự triển khai
- **Lược bớt tính năng thừa thải dư cài đặt**: Loại bỏ hoặc chặn phần tiện ích / mã chức năng sample test bị sót, cùng với tài liệu mẫu bị dính trong gói.
- **Điều quan hệ cho luân chuyển Errors Error handling**: Thống kê dữ liệu báo lỗi lúc hỏng ra khỏi mạng hệ thống Production tới đầu ứng dụng dùng cuối của end user CẤM hiển thị thông báo chi chít Stack Traces hay tên frameworks, hoặc phiên bản server chạy.
- **Directory listing**: File server quản lí dịch vụ tài sản tĩnh không cho xem danh mục lộ diện tài nguyên (vô hiệu thư mục cây)
- **Cloud storage**: Chức năng thiết lập lưu trữ mạng cloud khóa đứng hoàn toàn khu vực kết nối đại trà cho public unless phi khi nó cần được phục vụ để dùng ngoại và kèm theo bản văn chép minh chứng.
- **Tiến trình cập nhật bản vá Patch management**: Quần cảnh môi trường xử lí thực thi, mã khung thư việc phải đạt các dòng versions mới và còn hậu thuẫn cập nhật bảo đảm an toàn.

**Xác minh**:

- Nhận thức sẽ không dò bắt thấy dữ liệu account gốc mặc định lưu trú thẳng tại mã kho repo hay trong biến môi trường khai báo lúc đưa bản IaC ra.
- Kiểm độ sai lệch hệ thống xử lý (error messages), bảo đảm cấu trúc lỗi đẩy gửi ra khách gọn nhẹ an toàn thân thiện / không bị đọng lộ tin.
- Object file cấp quyền đám mây được block khóa tính lan truyền mở toang, duy chỉ chừa khe hỗ trợ mở nếu có chứng thư ghi lý do mở rõ từ nội bộ.
- Framework mã cũng như runtime base versions hỗ trợ đảm bảo còn tiếp tục không lỗi nhịp.

---

## Quy tắc SECURITY-10: Cảnh quan Bảo vệ Dây chuyền Cung ứng Phần Mềm (Software Supply Chain Security)

**Quy tắc**: Mỗi project luôn phải chú ý rà soát phần hỗ trợ Software supply chain:

- **Gắn mã bảo mộc mã thư viện ngoài Dependencies**: Mọi thành phẩm thư viện mang vào dự án bị chốt vào dùng gói mã hóa chính xác qua chuỗi verions cứng / hoặc sử dụng loại Lock file đồng vị chốt hạ
- **Vulnerability scanning**: Máy quyét quét lỗ hỏng kịch bản bên trong thư mục cài ráp nền (như NPM hay Nuget, vv) PHẢI cài đặt thêm
- **Không thư viện ứ động thừa**: Các Packages rọc đi mà project không cần ngó đến sử dụng hoạt động trong thời gian hoạt động.
- **Cam kết qua nguồn đáng tin dùng (Trusted sources only)**: Downloads từ registry trang chủ chính, bỏ đi mã thư viện lạ ở cấp độ các nhóm unverified cung cấp bên các bên thứ 3 vô danh.
- **SBOM**: Tiến trình cần sinh báo biểu danh sách thiết bị cung ứng bản dựng gọi tắt Software Bill of Materials ngay khâu tung gói bản Deploy
- **Sức trụ vững ở CI/CD**: Các bản image nền xây dựng chạy chuỗi tiến hành đóng gói có tag tên gói cẩn thận - miễn dùng cờ `latest` đối với bản Build CI Dockerfile thành công Production

**Xác minh**:

- Nhận dạng file .lock (lock file) được đưa vào source nhánh.
- Tìm vết bộ lọc dò lỗi dependency được thiết lập ở CI pipeline cho phần cấu trúc khai báo
- Không gói Dockerfiles hoặc CI deploy nhận lệnh image mã `latest` để sản xuất bản thành phẩm
- Cân chỉnh dùng registry chuẩn

---

## Quy tắc SECURITY-11: Cấu tạo Kiến trúc Nguyên Lý Thiết Kế Cơ sở Dữ liệu Bảo mật (Secure Design Principles)

**Quy tắc**: Vận hành bản thiết kế nên lồng dệt các góc độ quan trọng ngay từ thời khắc khởi nguồn:

- **Tính chia cắt Separation of concerns**: Mã luồng kinh quan liên tiếp Auth Access / Xác nhận chức phận Auth cho người nắm thẩm quyền cùng lúc có bộ phân lọc tiền Payments Authorization - các thứ trên bị giam lọt tách các khối độc lập Modules.
- **Bảo hiểm nhiều lớp Defense in depth**: Mất lớp chống đở nọ phải bổ thay thêm các cơ chế bổ sung kết bè lại
- **Kiểm định tỷ lệ chia Rate limiting**: Hệ hỗ trợ cung cấp API hướng ra nhận bên công cộng bắt buộc đưa tham chiếu bảo mật ngăn luồng lũ lụt thông tin Throttle chống khai phá tràn bờ bộ nhớ.
- **Điều lưu vi phạm kinh doanh nghiệp (Business logic abuse)**: Xây lên ranh giới ngăn vi phạm trục lợi

**Xác minh**:

- Ngăn tách khu quản trị logic ứng thiết của mô-đun an toàn đứng chung cùng mô-đun.
- Setup quy định mức tốc hành Rate limiting của các RESTful public ra mặt đại diện.

---

## Quy tắc SECURITY-12: Sự Thẩm định, và Quy Hoạch Quản lý Chứng Minh Độ Tín Nhiệm(Authentication and Credential Management)

**Quy tắc**: Tất thẩy các giải pháp lập trình hệ nào có xác danh người chơi/nhân viên, đều tích hợp:

- **Loại điều luật mật lệnh cấu Password policy**: Hướng dẫn nhỏ nhất là 8 chử và chạy hệ quét qua danh tự bị ngã trên mạng từ list Password của thế giới.
- **Lưu kho bảo mật tài khoản (Credential storage)**: Băm mã chuỗi từ mã thuật tiên tiến. Tuyệt kỵ chép không định dạng hoặc yếu dạng
- **Lớp mã khóa Multi-factor (MFA)**: Chu cấp bảo quản cho ban tổ chức admin account.
- **An linh bảo quan khu vực trạng thái Sessions**: Session quản định tuổi sống nhất định được đặt thời thạn cho phía Server. Các khu đóng chặn log off xoá toàn hệ, cấu định dạng qua khóa HTTP SECURE / HttpOnly/ SameSite đối với lệnh kết cookie trao chéo
- **Rào cản tấn mã Brute-force**: Sắp sếp luống giới cấm thử đượt chê lệnh tài khoản quá quy mức bằng giải thuật hãm đà (loại ngắt / khóa / ngưng dóng tài khoản trong ít phút hoặc đánh captcha).
- **Tuyệt mệnh chèn thông tin thô cấu mặc định mật thư (No hardcoded credentials)**: Dọn đi trơn trọi những ký dạng nhúng ngầm cho việc test qua tài liệu file config hệ thông.

**Xác minh**:

- Password hash được đưa thuật băm dạng tiến bộ
- Setup của gói phần mềm cookie truyền tài nguyên Sessions xác nhận các thuộc tĩnh Secure, HttpOnly bảo trợ chống chèn
- Endpoints điều lệ cho Login phải có màn ngách đánh Brute-force.
- Rà không nhét bí khóa bên Source.
- Kiểm thực tài khoản nhóm Admin bắt dùng định chức bảo vệ yếu tố 2FA (MFA). Mọi quy chuẩn thời hạn sessions đóng hoàn tất hết ở giai đoạn rà.

---

## Quy tắc SECURITY-13: Quy Trình Xác minh tính Đồng Bước và Lễ Toàn Mã Lõi Trong Dữ liệu (Software and Data Integrity Verification)

**Quy tắc**: Hệ vận hành kiểm thử tính vẹn thể đối chiếu từ Code app đến kết chuyển lưu DATA:

- **Chuỗi hóa nguy cơ (Deserialization safety)**: Các cấu liệu nạp ngoài luồng bị cấm cản giải gói nếu chưa kinh qua bước đối xác (thường sử dụng danh sách mẫu khai loại format)
- **Tình trạng chuẩn bảo mật đóng gói cài theo (Artifact integrity)**: Bật kiểm nhận cho Plugins, gói download bằng hệ ký sinh thông qua dấu tính Signature hoặc chữ kí điện tử checksums.
- **Vệ an đường đi tự động hóa (CI/CD pipeline security)**: Quy định phân nhóm trách nhiệm cấu tạo pipelines.
- **Tài khoản dùng ké CDN hỗ trợ (CDN and external resources)**: Các files gọi tắt external từ thư viện mở rộng Javascript tải mượn bằng lệnh script của CDN trang ngoài PHẢI gắn tem xác xuất toàn cấu Subresource Integrity (SRI) hashes vào thuộc tính tải ảnh tài nguyên.
- **Data integrity**: Thay đổi các cấu tố dữ liệu cốt cán bắt buộc đưọc chèn nhúng khả năng tra cứu nguồn gốc truy vết lịch sử log

**Xác minh**:

- Phủ nhận tải unpack Deserialization mà không chặn luồng thô tin
- Scripts tải qua CDN có lồng kèm đặc nhiệm Subresource Integrity tại khai báo file script HTML DOM.
- Thay đổi liên tục lịch sử Log lại đầy đủ diễn biến nội hàm của dữ liệu bị biến thiên.

---

## Quy tắc SECURITY-14: Báo nguy rủi cảnh (Alerting) Và Thu Hoạch Thống kê (Monitoring)

**Quy tắc**: Song hành cùng hệ log tiêu chuẩn sinh quy ở (SECURITY-02, SECURITY-03), giải hình này kèm cặp thêm cho:

- **Khối thông quan báo hiệu sự cố**: Alert báo phát nguy ra tiếng cho chuỗi cố ý hack login hàng loạt thất bại hay truy tài khoản có đặc quyền siêu cấp lạ...
- **Giới bảo toàn tin vết Logging trạng thái Integrity**: Log không ai được nhúng lại nhằm ngụy tạo để tẩy xóa dấu tích — cấu hình app nội cấm hẳn luồng delete/mutate nội dịch trong bảng ghi cấu nhật ký
- **Tuổi sống bảo lưu (Log retention)**: Bảo tồn dữ liệu tối thiểu tầm độ tháng 3 quý cho chuẩn định mức dự án. (Default bám: 90 Ngày).
- **Lập quan sát Dashboards (Monitoring dashboards)**:

**Xác minh**:

- Phủ lớp Alarm báo động cho trạng tình vi phạm xâm nhập lệnh truy
- Nhóm Roles của phần mềm điều khiển KHÔNG giữ trong tay cái uy của dòng code chép xoát log Stream bảng chính.

---

## Quy tắc SECURITY-15: Cơ Cấu Hành Xử Trong Khống Chế Điểm Ngoại Lệ (Exception Handling and Fail-Safe Defaults)

**Quy tắc**: Hệ phần mềm app phải bảo trợ ứng phó điểm Lổi hụt cho an toàn bằng phương án:

- **Rào lưới chặn (Catch and handle)**: Ngoại gọi mạng API ngoài hay DB, hệ files PHẢI trích cất bộ hàm Exception không nhả tràng lỗi lộ dạng trên sản phẩm.
- **Chặn cửa khi rớt (Fail closed)**: Nếu không xử lí được quá trình, dừng truy nhập ngưng việc — tuyệt đường bung thoát (fail open).
- **Gọn ghẽ vùng tài khoản (Resource cleanup)**: Thu thả tài nguyên khóa chặn ngắt mạng như lệnh truy lock nối DB qua Try/Finally tránh ùn ứ leak nhồi nhớ...
- **Errors cho người ngoại quốc dùng Users-end**: Ghi câu chung chung trót nhận lổi, tuyệt nín mọi chi tiết ruột báo về nội bộ
- **Bắt mạch lỗi toàn ứng (Global error handler)**: Dự trù gói toàn diện phần hệ lỗi lọt sàn cho việc chặn ngoại nhập báo lổi cao nhất của Framework. Trả gói thông báo ổn, chuyển về LOG nội trích lại SECURITY-03.

**Xác minh**:

- Không bỏ lại Promise hụt cảnh rejections hoặc chuỗi uncaught exception khơi văng trong hệ thống ứng phó.
- Hàm tóm mạch Errors toàn khu vực cấu đặt trên chóp hệ ứng thiết
- Điểm tắt khi xảy gẫy của chương trình không bao che rớt khỏi vòng lọc uỷ thác kiểm auth

---

## Tích hợp Thực Thi Bảo mật (Enforcement Integration)

Đây là những ràng buộc xuyên suốt áp dụng trên toàn bộ các chu trình phát triển của AI-DLC. Tại mỗi giai đoạn:

- Tiêu duyệt bộ phân đánh giá trên toàn cục khung theo Artifacts đưa lại đối chuẩn cùng các mức yêu cầu của quy tắc SECURITY.
- Tại văn thư diễn lược tóm gọn "Thông báo hoàn cảnh Giai Đoạn", đề ra hạng thẻ "Sự Vận Tuân Security" đi đôi từng bước điểm chuẩn cho (Tuân theo - Compliant, Từ chối chuẩn non-compliant, Hoặc bỏ phần chuẩn không phù nếp N/A)
- Phá vỡ dù chỉ một quy luật thì toàn khung ngưng không đẩy Giai Đoạn (nắm giữ là Lỗi Phát Hiện An Ninh Blocking).
- Liệt phần chỉ dẫn bộ thư tham khảo cho quá trình Test hoặc cho bảng Document xây đắp phần cấu kiện ứng thiết (Design Rules).

---

## Phụ Lục: Mốc Tham Chiếu Rà Soát Điểm Giao Thoa Từ OWASP

Mục đich gửi hỗ trợ người kiểm kê, thiết bị bên dưới chỉ đối lưu giữa danh bản phân tầng các RULE Security cho Cột chuẩn Top 10 của OWASP (2025):

| Quy tắc SECURITY | Hạng mốc mã số OWASP                             |
| ---------------- | ------------------------------------------------ |
| SECURITY-08      | A01:2025 – Broken Access Control                 |
| SECURITY-09      | A02:2025 – Security Misconfiguration             |
| SECURITY-10      | A03:2025 – Software Supply Chain Failures        |
| SECURITY-11      | A06:2025 – Insecure Design                       |
| SECURITY-12      | A07:2025 – Authentication Failures               |
| SECURITY-13      | A08:2025 – Software or Data Integrity Failures   |
| SECURITY-14      | A09:2025 – Logging & Alerting Failures           |
| SECURITY-15      | A10:2025 – Mishandling of Exceptional Conditions |
