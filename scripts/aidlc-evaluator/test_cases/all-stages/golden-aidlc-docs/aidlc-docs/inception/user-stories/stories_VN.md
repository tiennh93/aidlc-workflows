# Câu Chuyện Người Dùng

## Epic 1: Quản Lý Danh Mục

### US-CAT-001: Thêm Sách
**Với tư cách là** Thủ thư, **tôi muốn** thêm một cuốn sách mới vào danh mục với tiêu đề, tác giả, ISBN, thể loại và số lượng bản sao, **để** các thành viên có thể khám phá và mượn sách.

**Tiêu Chí Chấp Nhận:**
- Cho trước tôi đã xác thực với vai trò Thủ thư hoặc Quản trị viên
- Khi tôi gọi API bằng phương thức POST đến `/api/v1/books` kèm theo dữ liệu sách hợp lệ
- Thì hệ thống sẽ tạo mới cuốn sách với một ID duy nhất, gán giá trị `available_copies = total_copies`, và kết quả được trả về thông qua phản hồi
- Và nếu bất kỳ trường bắt buộc nào bị thiếu hoặc không đúng định dạng hợp lệ, tôi sẽ nhận được mã lỗi trả về `422 VALIDATION_ERROR`
- Và nếu tôi đang ở vai trò là một Thành viên, tôi sẽ nhận được mã lỗi trả về `403 FORBIDDEN`

### US-CAT-002: Cập Nhật Sách
**Với tư cách là** Thủ thư, **tôi muốn** cập nhật metadata của sách (tiêu đề, tác giả, ISBN, thể loại, `total_copies`), **để** danh mục sách luôn đạt độ trung thực chính xác.

**Tiêu Chí Chấp Nhận:**
- Cho trước tôi đã xác thực với vai trò Thủ thư hoặc Quản trị viên
- Khi tôi gọi API bằng phương thức PUT đến `/api/v1/books/{book_id}` kèm với các trường thông tin cần cập nhật
- Thì dữ liệu của cuốn sách đó sẽ được cập nhật và bản ghi cập nhật mới nhất sẽ được trả về
- Và nếu cuốn sách đó không tồn tại trong hệ thống, tôi sẽ nhận được thông báo phản hồi lỗi với mã `404 NOT_FOUND`
- Và nếu giá trị thu hẹp của `total_copies` giảm xuống thấp hơn số lượng bản sao hiện tại đang cho mượn, tôi sẽ nhận thông báo lỗi `409 CONFLICT`

### US-CAT-003: Xem Chi Tiết Sách
**Với tư cách là** Thành viên, **tôi muốn** xem tham khảo chi tiết của một cuốn sách cụ thể, **để** tôi có thể đưa ra quyết định xem có nên mượn nó hay không.

**Tiêu Chí Chấp Nhận:**
- Cho trước tôi đã xác thực
- Khi tôi gọi API qua GET `/api/v1/books/{book_id}`
- Thì tôi nhận về toàn bộ thông tin chi tiết của sách cùng với số lượng báo cáo về `available_copies`
- Và nếu cuốn sách không thấy xuất hiện trong hệ thống thư viện, mã lỗi là `404 NOT_FOUND`

### US-CAT-004: Liệt Kê Tất Cả Sách
**Với tư cách là** Thành viên, **tôi muốn** đi dạo duyệt qua toàn bộ số sách trong danh mục, **để** tôi có thể khám phá những cuốn sách hiện có.

**Tiêu Chí Chấp Nhận:**
- Cho trước tôi đã xác thực
- Khi tôi gọi GET `/api/v1/books`
- Thì tôi sẽ nhận được về một danh sách toàn bộ các cuốn sách và thông tin khả dụng của chúng

### US-CAT-005: Xóa Sách
**Với tư cách là** Thủ thư, **tôi muốn** loại bỏ một cuốn sách ra khỏi hệ thống danh mục, **để** cho những cuốn sách cũ hỏng ngừng hoạt động sẽ không còn xuất hiện làm rác danh mục sinh lỗi.

**Tiêu Chí Chấp Nhận:**
- Cho trước tôi đã thực hiện xác thực với vai trò Thủ thư hoặc Quản trị viên
- Khi tôi dùng DELETE gọi `/api/v1/books/{book_id}`
- Thì ngay lập tức cuốn sách định danh này sẽ bị dọn gỡ loại bỏ hoàn toàn khỏi hệ danh mục hiện có
- Và nếu thấy sách vẫn còn đang xuất dính dáng vướng vào các biên lai đang mượn hoạt động `active`, tôi nhận mã báo xung đột `409 CONFLICT`
- Và nếu cuốn sách đã không còn tại thư viện, mã trả phản hồi là `404 NOT_FOUND`

### US-CAT-006: Tìm Kiếm Sách
**Với tư cách là** Thành viên, **tôi muốn** được quyền tìm kiếm sách bằng cách tra tiêu đề hoặc bằng tên tác giả, lọc tìm theo danh mục cũng như theo thông số tính khả dụng, **để** tôi có thể tra cứu và tìm thấy các cuốn sách chỉ định một cách tốc độ nahnh chóng.

**Tiêu Chí Chấp Nhận:**
- Cho trước tôi đã qua hệ thống xác thực vào
- Khi tôi gửi truy vấn GET `/api/v1/books/search?q=python&category=programming&available=true`
- Thì tôi sẽ thành công có bảng liệt kê các cuốn sách khớp thông tin với từ khóa ở tiêu đề/tác giả, cùng điều kiện với các luật lọc category hay availability
- Và nếu tra tìm mà ra chuỗi kết rỗng do không cuốn sách nào để thỏa khớp thông tin tìm này, tôi sẽ có trả về hiển một danh sách List trống (không coi đấy là 1 dạng kết hiện mã của một Error lỗi nào)

---

## Epic 2: Quản Lý và Xác Thực Thành Viên

### US-AUTH-001: Đăng Ký Thành Viên
**Với tư cách là** người dùng mới, **tôi muốn** được quyền tham gia với đăng ký thông tin gồm cấp tạo tên, email cá nhân và mật khẩu, **để** tôi có điều kiện quyền hạn cho bắt đầu dùng việc mượn sách.

**Tiêu Chí Chấp Nhận:**
- Cho trước thông tin về việc tôi vốn chưa thông qua lớp xác định người dùng nào tại trạm endpoint ở ngoài Public
- Khi tôi đi lệnh bằng gửi dữ liệu chuẩn POST vào `/api/v1/members/register` cấu hợp chung cho tham số đăng ký name, email, bằng pass bảo mật
- Thì tôi tạo cấp mở của một profile member mới bằng nhãn tài khoản quyền "member", qua trả phản về hồ profile của cơ thể user sinh mới ra (Sẽ ngắt không đi gộp mật khẩu đi qua gói gửi ở đó)
- Và trường hợp email gửi là bị sử rồi có đăng tại 1 bản lưu rồi, báo lỗi trả là mã `409 CONFLICT`
- Và với dạng mà mật khẩu ngắn sơ đẳng bảo mật kém cỏi quá (< 8 phím nhập bằng chuỗi chars), nhận phạt mã là `422 VALIDATION_ERROR`

### US-AUTH-002: Đăng Nhập
**Với tư cách là** người cấp dùng qua tham đăng ký thành từ trước, **tôi muốn** được cho truy gọi vào app đăng nhập đưa cùng email đúng mật chuẩn pass tôi, **để** qua đó lấy thông lệnh gói mã giao JWT gọi là token của API truyền hạn.

**Tiêu Chí Chấp Nhận:**
- Cho trước tôi ở dạng người trơn ngoài chưa qua xác chứng bảo (truy dùng màng ngoài API public gateway)
- Khi gọi lên dữ vào POST ở vùng `/api/v1/members/login` đúng chứng mật 
- Thì tôi hoàn hảo thủ được chìa kết JWT token lưu vào vòng với vòng 24-tiếng thọ gói gồm các thẻ ID của tôi: (member_id, dạng email chuẩn, thẻ mang thông phận "role") 
- Và nếu gõ dở quá hay truy với credential thẻ lỗi là tôi nhận đánh trượt đuổi ngay `401 UNAUTHORIZED`

### US-AUTH-003: Xem Hồ Sơ Của Tôi
**Với tư cách là** Thành viên, **tôi muốn** coi dòng hồ sơ tin cá nhân, **để** lấy số duyệt xác xem tài khoản khai ở đó như thông minh.

**Tiêu Chí Chấp Nhận:**
- Cho trước việc tôi đang là người qua cổng xác thực bảo chứng rồi.
- Khi lôi với lấy đi GET cấp `/api/v1/members/me`
- Thì thông tin profile của tự thân bao (gồm cả tên khai, thư mạng đăng, nhóm hạng role, thông tin báo mức account thuộc active ra sau) hiển báo theo phản lệnh.

### US-AUTH-004: Cập Nhật Hồ Sơ Của Tôi
**Với tư cách là** Thành viên, **tôi muốn** gọi cập báo vào hệ ghi để định đúng cho tên cùng thư email tôi, **để** mà gìn bảo tin hồ sơ bản thể kịp thời đại.

**Tiêu Chí Chấp Nhận:**
- Cho trước việc qua vòng kiểm bảo xác chứng nhận minh rồi
- Khi gọi API dạng PUT cho của `/api/v1/members/me` để đôn gói lên lưu (email hay danh tính tên bản thân cập nhật)
- Thì lệnh này cập thành rồi đưa thẳng profile sửa cập báo này qua bản response trả lưu ở đây luôn.
- Và gặp thông hệ thư tín chuẩn hòm thư định lưu update đang xài ở chỗ profile bên user kẻ khác thì lỗi đáp là `409 CONFLICT`

### US-AUTH-005: Xem Bất Kỳ Hồ Sơ Thành Viên Nào (Quản trị viên/Thủ thư)
**Với tư cách là** Admin hay Librarian, **tôi muốn** trỏ vào hồ thông mọi lưu của mọi account con ở, **để** qua quản vận tài khoản của kho member quản được.

**Tiêu Chí Chấp Nhận:**
- Cho trước thân xác chức phận của ta minh thuộc của Admin/Librarian (Thủ thư hay Quản trị quyền có) 
- Khi dùng phép gọi GET cấp vào đường `/api/v1/members/{member_id}`
- Thì có luôn cái response thu profile toàn của người mà cấp của id gửi truy xin
- Và nếu kẻ user đấy không tồn lưu hiện có cho trên thư hệ, báo sẽ trả của mã mã `404 NOT_FOUND`
- Và trong lúc ta với cương là dân mượn member cắc ké vô danh vọc vô truy API này cấm này thì lĩnh của `403 FORBIDDEN`

### US-AUTH-006: Vô Hiệu Hóa Tài Khoản Thành Viên
**Với tư cách là** Admin quản, **tôi muốn** đóng hủy quyền active chạy của 1 account member, **để** chặn việc nó chạy tạo lập khoản thêm mà khi lệnh mượn đang có cho chạy tiếp cho theo.

**Tiêu Chí Chấp Nhận:**
- Cho trước xác cấp phận của Admin
- Khi trỏ PUT API trên `/api/v1/members/{member_id}/deactivate`
- Thì account user trên đánh dập thành inactive đi (làm ngưng của tài thao hoạt ngưng chạy)
- Và các đồ đã ôm qua (checkouts cùng holds chờ giữ chờ của nó) thì coi nguyên dạng chưa chết lệnh mượn cho quyền active còn
- Và cấm triệt kẻ kia khỏi việc tạo hóa mới cho đợt (checkouts mới, xếp lịch đợi lặp mới, báo thêm renewal mới vào)
- Và sai chức với role thì khi gọi vào chửi lại ngay bằng mã `403 FORBIDDEN`

---

## Epic 3: Hoạt Động Mượn Sách

### US-LND-001: Mượn Một Cuốn Sách
**Với tư cách là** Thành viên, **tôi muốn** vay 1 cuộn quyển từ thư sách mang dùng, **để** mà đọc coi của với chu mượn giữ vào vòng 14-ngày.

**Tiêu Chí Chấp Nhận:**
- Cho trước được qua là tài Member của chuẩn thẻ xác chứng được rồi
- Khi gửi POST kết vào truy `/api/v1/checkouts` thông kèm cho ID là `book_id` chuẩn đi kèm 
- Thì thu được ghi thành hóa lệnh mượn ở bản (với cái mốc lưu là `due_date` có mốc là = theo bây thời (now) + mốc 14 ngày & thiết lưu trạng này bằng `status = active`)
- Và lệnh hệ sẽ trừ đánh tụt bằng `-1` `available_copies` có gọi ở bộ phận Catalog Services đồng hành
- Và tra coi vào mã mà book không tồn, mã nhận trả bằng `404 NOT_FOUND`
- Và đánh coi ở vào mà truy vô lượng bản chờ bằng 0 mượn lốt nhẵn nợ trả thông `409 CONFLICT`
- Và tra truy của kẻ có tới mức trên đủ đạt mốc số 5 active checkout từ hiện thời tại điểm thì cấp `409 CONFLICT` báo về "checkout limit exceeded" thông tin hạn mức vượt cấm
- Và ở tra truy khoản thấy ví như chưa trả lỳ nợ trên >= mức chạm $10.00 cho hạn (outstanding threshold fees thì cấm chặn và ném trả `409 CONFLICT` luôn gửi bằng "outstanding fees exceed threshold" thông điệp đi theo cho người bị)

### US-LND-002: Trả Sách
**Với tư cách là** Member hay người cấp quản bộ Librarian, **tôi muốn** đem hoàn gửi book giả về cho, **để** mở cho đồ đó ở available nhàn cho lưu khác lưu vòng ở kho có sẵn thư.

**Tiêu Chí Chấp Nhận:**
- Cho trước có qua là qua chức thông
- Khi xài đẩy HTTP đi dạng đẩy bằng POST vào `/api/v1/checkouts/{checkout_id}/return`
- Thì sẽ lưu hoàn tạo cập cho trạng (status của sổ này lưu lệnh mang biến về ở "returned") làm lưu tạo cho cập lệnh cập mốc trả thời của return_date này
- Và bên hệ đi kèm `available_copies` thì gọi Catalog Services tăng lên cất
- Và ví ở cái lỗi trễ quá thời bồi bằng $0.25 vào của tiền vào bồi thêm ở mỗi (capped đỉnh trần tới chốt bằng là $10.00 cho max phạt của lần mượn) của lập lệnh thông tiền bằng lập ghi tạo sổ cất fee 
- Và tra vô cái phần ở chờ nhận của cái phần waiting (Holdings queue list active này cho sách), ở bản của cái hàng đợi đầu của sổ này (hạng cấp chuẩn FIFO cấp vị trí 0) đánh qua báo lên sang thành (ready dạng báo xong có book chờ lấy)
- Và Members dân mượn có chỉ được đánh trả của với lệnh từ người dân vay tạo mình; nhưng dân Admin hay Librarian quản thư có toàn quyền đi thu thanh toán cho hết

### US-LND-003: Gia Hạn Mượn Sách
**Với tư cách là** Mượn viên, **tôi muốn** thông thêm hạn lưu gia tăng vòng cho khoản mượn của tài, **để** để ôm tiếp cầm cuộn thứ sách dùng giữ sang độ cộng đi bằng qua 14 vào mốc cho.

**Tiêu Chí Chấp Nhận:**
- Cho trước qua định đúng người chính danh vay chứng lưu account đúng mượn của mình làm lệnh đi 
- Khi gửi cái báo đi lệnh PUT cho POST gọi báo `/api/v1/checkouts/{checkout_id}/renew`
- Thì độ dài của điểm mốc hạn (due_date) đưa giãn hạn đi của tới bằng (do cộng dồn đưa bằng +14 days đi theo tiếp sau thời hạn nộp) cùng số lượng lịch báo (dùng lưu của renewal_count gia báo +1 biến điểm)
- Và vào số cái phần báo cấp renewal_count đụng đỉnh 2 trần mốc lần cấm là trả mã bằng (mã nhận `409 CONFLICT` với text cho đi gửi "renewal limit exceeded")
- Và của thấy phần có book ở lịch báo có người đợi chực (Holds đang active tranh đồ đợi lấy), thì mã đánh từ báo dội thẳng là cấm ("book has active holds" + thẻ mã `409 CONFLICT`)
- Và ở tại đồ không thuộc diện sổ dở khoản active thì báo cấm luôn bằng thẻ là `409 CONFLICT` 

### US-LND-004: Xem Khoản Sách Đang Mượn 
**Với tư cách là** người xài Member dùng ở tư cách cấp cá, **tôi muốn** tra gọi lại của bảng những mượn ở thông lưu của cái đống cho mượn chưa kết thúc, **để** tự túc xem biết bản tại lưu sổ cầm ở gì và hạn lịch đến lúc là gì.

**Tiêu Chí Chấp Nhận:**
- Cho trước là định đã khai lưu thành phận đăng đúng.
- Khi lôi đống báo thông ở gọi truy vô báo API qua lấy ở GET với thông query `GET /api/v1/checkouts?status=active` của
- Thì lấy đống bảng danh cho lưu có theo là đống thông active sổ ghi khoản bao (gồm cả đồ thông của sách: book_id, và của book_title tựa, ở phần mượn theo thời ngày mượn (checkout_date), lịch ấn chuẩn thời ngày nộp ấn hạn (due_date), số lượt xin mở hạn cấp renewal_count của cho lấy).  
- Và Admins / Librarian của thư viện quản được phái gửi lồng phần tham quy vào ở query phần cho định member_id bằng xem của ở một ở thành này nào của cho người kiểm ở member nào.

---

## Epic 4: Quản Lý Đặt Trước (Hold Management)

### US-HLD-001: Đặt Trước Sách
**Với tư cách là** Member, **tôi muốn** chen phiếu đặt hàng trên cuốn sách đương ở không được mượn lúc tạm ở thì tại (unavailable), **để** xếp được truy cho lấy tài quyển theo đúng quy khi nó ở lúc sau hoàn sách do giả lại về.

**Tiêu Chí Chấp Nhận:**
- Cho trước với vai trò đã chứng thực Member lấy vô 
- Khi đặt chỗ gửi đính dùng thamPOST lên gọi lưu cất theo URL là `/api/v1/holds` đính gửi ID ở của book gọi là: book_id đi
- Thì sinh lấy cái hóa tạo ra của phần hold ghi bằng "waiting" có lưu xếp thứ chờ lưu trật bằng queue đợi vào luồng có đợi theo quy "FIFO" chuẩn vị đặt.
- Và cuốn này đang dư, mã báo bằng cấm (nhận về là `409 CONFLICT` + "book is available for checkout")
- Và do đã trót lỡ đăng chờ xếp book này ở 1 lưu Hold Active thì (đánh bằng `409 CONFLICT` cùng từ text báo lại: "duplicate hold" do vi lỗi chen bản 2 lệnh cho trùng).
- Và lỗi thứ mà vi trần 3 mốc lần tối hạn ở cữ Holds Wait (Hạn đặt trần active hold > 3 là bị cấm thì bị gửi mã là đuổi: `409 CONFLICT` và nội của nó "hold limit exceeded").

### US-HLD-002: Hủy Đặt Trước
**Với tư cách là** Member xài, **tôi muốn** thông gạch đơn xếp lịch hold xin ở chờ, **để** mà còn thả chỗ trống đôn lên hàng danh lên trên đôn chờ có những người khác nữa đợi còn lên phiên nhận.

**Tiêu Chí Chấp Nhận:**
- Cho trước xác chuẩn mình 
- Khi tôi DELETE gửi vào lệnh `/api/v1/holds/{hold_id}` 
- Thì bảng sẽ cất gạch tên lệnh ra đánh vào của vị vào status dọn ra "cancelled" (làm của dồn đánh chỉ vị ở phần có thứ cho vị đợi hàng FIFO queue positions dời trật dời lại còn đôn vị xếp đánh trên dời lên). 
- Và xài lệnh của cho xài cởi chờ ở của tài cá thì được hủy cá đồ của riêng không có xóa hộ người của, Librarian với phần Admin thì đi có cắt cho phần toàn của bất hold kỳ ai lệnh đồ ở của cái này đi.

### US-HLD-003: Xem Hàng Đợi Đặt Trước
**Với tư cách là** dùng Member người thư cá, **tôi muốn** dòm được danh mục coi đang hàng tại đỗ chỗ nơi danh vào xếp thứ của lấy hold cho mượn book, **để** tự tính để mình chờ có xem biết là bao độ dài lúc chừng lâu cho của.

**Tiêu Chí Chấp Nhận:**
- Cho trước chuẩn rồi chứng đăng
- Khi ngó lấy đống danh bằng lấy GET phần vào đường gọi của `/api/v1/holds?book_id={book_id}` cho mượn 
- Thì thu cả cái bảng đồ của Hold với danh hạng chức queue xếp cho statuses ở trạng đi.  
- Và dòm có tìm bằng ra thấy chỉ thân coi chốn của trong hàng dòm danh đống là được. 

### US-HLD-004: Xem Các Yêu Cầu Đặt Trước Của Tôi
**Với tư cách là** người dùng Member cá nhân, **tôi muốn** thông theo báo dòm ra tổng tất cái phần số đợi active lệnh holds của bộ cho mượn phần của ta đang tại, **để** tiện việc cai tự mình lưu quản ở mình số dọn holds chờ.

**Tiêu Chí Chấp Nhận:**
- Cho trước là xác chứng ở rồi 
- Khi gọi dò để API gửi bảng của ở GET của `/api/v1/holds/me` 
- Thì dòm trọn lại toàn bộ danh Holds chờ đợi với đống tham của như ID: (có lấy book_id), có vị cữ thông xếp chờ trạng cất hiện hành tình lưu giữ (có dùng waiting / ready / hoặc là do tự xưng bị là dạng cancelled), vào đi hạng cái hàng lấy lưu đợi vị lưu xếp cho thứ hàng theo queue_position, vào lưu lưu thời điểm Hold Date đặt.

---

## Epic 5: Quản Lý Phí 

### US-FEE-001: Xem Phí Của Tôi
**Với tư cách là** Member, **tôi muốn** check tổng của số những phần phí đang treo mắc tồn dư của mượn chưa đóng trả thanh toán ở khoản (tính theo của cá, **để** cá phần hiểu tường tỏ ở nợ nần bao khoản bao ngân tiền).

**Tiêu Chí Chấp Nhận:**
- Cho trước xác cấp ở chứng bảo ở mình cho API
- Khi dùng dò lấy lệnh qua GET bảng của gọi của URL API: `/api/v1/fees/me`  
- Thì báo lại đầy thu phần cho các fees cho phần (có ID ở: fee_id, có lưu số lượng ở amount báo nợ, ghi lấy của cấp hạng hiện chưa ở chưa thu hay rồi status ra thu của tại định ngày lấy vào của qua tạo created_date lưu của, gọi ứng biên mượn theo số gọi với của cho ID mượn gọi ở checkout_id liên quan của mượn gây bồi).  
- Và ở dưới kết toàn gọi số tính lấy bảng tổng của số còn dư ra nợ chi balance (chưa có đưa của số ngân toàn này cho tính này).

### US-FEE-002: Xem Phí Của Bất Kỳ Thành Viên Nào
**Với tư cách là** Quản lý hoặc thư viện Admin/Librarian, **tôi muốn** tra dò cho số tiền cho dư chi của toàn bất thành của viên user người nợ bất kỳ (kể cả có, **để** cho hộ của đi thu trợ của khi truy có hỗ phần giải có chi thắc đáp ở bồi các phiền mức thư báo cho về nợ).

**Tiêu Chí Chấp Nhận:**
- Cho trước xác minh cho ở cấp Admin vào hay cho Thủ thư ở Librarian thiết chứng vào
- Khi dùng gọi truy ở API gửi dò gửi GET của URL: `/api/v1/fees?member_id={member_id}` vào bằng mã người
- Thì danh lục số ở của member ứng báo phí bồi trả ra hồ cất sơ phí nạp trả lưu đầy toàn lấy ở lại cho của.

### US-FEE-003: Xử Lý Thanh Toán Phí
**Với tư cách là** Librarian ở thư hoặc Admin quản đồ, **tôi muốn** thu đút lập biên cập ở của lấy chi thu đóng bù vi bồi thanh này vào ghi của ngân phần thanh đóng có trả thanh báo ứng vào, **để** cân bằng trừ lấy tính mới giảm trừ của dư nợ ra cân đúng.

**Tiêu Chí Chấp Nhận:**
- Cho trước quản phận có đúng chứng làm vào việc của ở Admin hay Librarian  
- Khi POST dùng đút đẩy khoản API thông mã URL đưa vào lấy của là `/api/v1/fees/payments` có tham cùng ghi số của `member_id` tiền chi ở đưa là `amount` 
- Thì bảng ghi hệ cập ứng ở đánh đóng xong giảm của trả đút hoàn trừ đóng tiền chưa ở nộp (phí thu của dư Outstanding fees xuống số amount) 
- Và báo trả hệ có gọi là giảm ứng đút của ứng với thanh trích có đưa lẻ của phần (các mức này giảm có trừ đóng lấy mức đóng lấy xuống) 
- Và cái lệnh của hồi báo phản có thì ở phải đưa tính có tính cả khoản hiện chưa ở mức trả còn ở báo tính có vào ở dư (tính gọi mới ở dư phần chưa số balance ở trả còn nợ này ra trả vào kết response báo có kết)

---

## Epic 6: Báo Cáo

### US-RPT-001: Xem Báo Cáo Trả Sách Trễ Quá Hạn
**Với tư cách là** Librarian (Thư viên), **tôi muốn** coi dòng số lôi truy bảng cho thư checkouts bị bẩn trễ hẹn (đang vào quá, **để** tôi có cách dùng tin nhắn liên gọi báo giục thúc gửi các ứng người member mượn. 

**Tiêu Chí Chấp Nhận:**
- Cho trước được qua là tài Thủ quản Admin / hay vào Librarian  
- Khi API gọi truy gọi đến báo GET vô theo ở `/api/v1/reports/overdue` 
- Thì thu ra ở kết trả bằng đống lấy phần của bảng các checkouts bị tồn nạp sai giờ (quá của hạn) với đầy ở của lấy các phần đủ như đính lấy: Tên (member_name) lấy người, member_email để thư mail, tựa cuốn theo báo (book_title), mượn từ đợt (checkout_date), trả theo ở quy hạn (due_date), và sai số sai mốc mượn để vi mốc ra của ngày ở sai là bao days_overdue.  
- Và do cấp phận này cho xem, ném lấy thư gọi lỗi ở `403 FORBIDDEN` khi người cho bằng gọi xem lại là gọi ở bằng quyền phần ở cá là người Member mượn có thường.

### US-RPT-002: Xem Tổng Hợp Thư Viện (Collection Summary)
**Với tư cách là** Quyền của chức Admin, **tôi muốn** dòm thống xem tóm thông kết theo thống ở toàn quản chạy đồ thư báo làm gì, **để** để ôm phần của ở phần dashboard ở hiển xem quan bảng của diện thông nhìn.

**Tiêu Chí Chấp Nhận:**
- Cho trước chứng đúng ở Admin quyền rồi
- Khi truy tìm gọi tới gọi vào của `/api/v1/reports/summary` qua ở truy bằng GET 
- Thì nhận số đồ tổng thông liệt lưu như (total_books đầu), có phần người tổng đăng tính total_members, tính đầu xuất giao số báo đã giao tính là books_checked_out, tài lượng tổng cuốn xuất còn trong rỗi thư báo có sẵn tính là books_available, và toàn nợ chấn sổ chi phải đóng cho dư ra ở toàn chi tính toàn phạt total_outstanding_fees còn của nợ đóng.
- Và cấm ném mã trả `403 FORBIDDEN` khi nếu cho không phần phận quyền là ở người Admin. 

---

## Epic 7: Vận Hành Hệ Thống (System Operations)

### US-SYS-001: Theo Dõi Sức Khỏe Catalog (Health Check)
**Với tư cách là** Hệ thiết của làm bộ vận giám theo dõi báo chạy báo hệ System, **tôi muốn** theo check truy báo gọi Catalog Health Services đo sức kết hệ chạy trạm, **để** tự thông phần nhận dò gọi tìm hỏa trạm phát của sập sự để sụp lỗi out hỏng chạy báo do (outages) đo sự.

**Tiêu Chí Chấp Nhận:**
- Khi vào test của check chạy gọi URL: `GET /api/v1/catalog/health` thì (không có bắt quyền chặn - là cổng public endpoint)  
- Thì báo lại từ trạm trả là theo hai của tình cấp trạng chạy máy báo System Status lưu với số bản gọi version

### US-SYS-002: Theo Dõi Sức Khỏe Lending (Health Check)
**Với tư cách là** Máy đồ thông báo truy giám system kết, **tôi muốn** đâm báo lên điểm check coi bộ Lending System báo đang đo chạy hoạt sức ra, **để** coi qua hỏng báo chạy trễ sập sự sụp hỏng đi lỗi sập. 

**Tiêu Chí Chấp Nhận:**
- Khi chạy gọi dò trên test của đường `/api/v1/lending/health` dùng gọi của phương gọi là GET ở hệ (lấy public API do endpoint màng ngoài public của hệ endpoint tự chạy gọi truy)
- Thì điểm trạm của nơi hồi phản về qua của 2 trạng báo cập ghi có status của máy lên hệ (ra tình có) cập cũng với theo phần gọi máy số ở mốc của cấp có phiên mang là version có theo. 

---

## Bảng Ánh Xạ Câu Chuyện Người Dùng Lên Vai Trò (Story-Persona Mapping)

| Mã Chỉ Câu Chuyện Story | Admin Người Quản | Thủ Thư Librarian | Thành Viên Dùng Member | Public Có Ngoài Dùng |
|-------|-------|-----------|--------|--------|
| US-CAT-001 Add Book Sách Mới | ✅ | ✅ | ❌ | ❌ |
| US-CAT-002 Update Book Update Sửa | ✅ | ✅ | ❌ | ❌ |
| US-CAT-003 Get Book Tra Báo  | ✅ | ✅ | ✅ | ❌ |
| US-CAT-004 List Books Liệt Mực Báo | ✅ | ✅ | ✅ | ❌ |
| US-CAT-005 Delete Book Xóa | ✅ | ✅ | ❌ | ❌ |
| US-CAT-006 Search Books Tìm Sách Hỏi Truy  | ✅ | ✅ | ✅ | ❌ |
| US-AUTH-001 Register Tạo Hồ Đăng Sơ Đăng | ❌ | ❌ | ❌ | ✅ |
| US-AUTH-002 Login Vô Xác Kiểm JWT Lấy Đăng Truy  | ❌ | ❌ | ❌ | ✅ |
| US-AUTH-003 View My Profile Báo Coi Tra Lại Thông Tin  | ✅ | ✅ | ✅ | ❌ |
| US-AUTH-004 Update Profile Sửa Thông | ✅ | ✅ | ✅ | ❌ |
| US-AUTH-005 View Any Profile Coi Mọi Profile  | ✅ | ✅ | ❌ | ❌ |
| US-AUTH-006 Deactivate Khóa Treo Ngừng Quyền | ✅ | ❌ | ❌ | ❌ |
| US-LND-001 Checkout Checkout Đi Vay | ✅ | ✅ | ✅ | ❌ |
| US-LND-002 Return Gửi Fake Trả | ✅ | ✅ | ✅(own của tài riêng) | ❌ |
| US-LND-003 Renew Đôn Gửi Gia Thuê Mượn  | ✅ | ✅ | ✅(own riêng quyền) | ❌ |
| US-LND-004 Active Checkouts Lưu Xem Vẫn Lưu Active Ở Nạp | ✅(any coi cho bất ai quyền thư vào) | ✅(any của đi bất được) | ✅(own ở một riêng tài) | ❌ |
| US-HLD-001 Place Hold Yêu Đặt Cầu Sách Hết Tạm Sắp  | ✅ | ✅ | ✅ | ❌ |
| US-HLD-002 Cancel Hold Hủy Quền Không Gọi Nữa  | ✅(any trảm bỏ hết) | ✅(any bỏ trảm nào được) | ✅(own cho bản mình quyền) | ❌ |
| US-HLD-003 Hold Queue Đợi Lệnh Tra Cũ Đi Liệt Hàng Check Sổ  | ✅ | ✅ | ✅ | ❌ |
| US-HLD-004 My Holds Xem Kho Lưu Wait Đặt Trách Wait Báo | ✅ | ✅ | ✅ | ❌ |
| US-FEE-001 My Fees Của Có Ở Số Vi Truy Xem Phạt Vi Của Fee Nợ Tôi  | ✅ | ✅ | ✅ | ❌ |
| US-FEE-002 Any Fees Các Tra Cho Phí Vi Nợ Đố Số Liệt Bất Của Kì Ai  | ✅ | ✅ | ❌ | ❌ |
| US-FEE-003 Payment Biên Giao Vi Nhận Trả Fee Thu Báo Quyền Trả Thanh Lập  | ✅ | ✅ | ❌ | ❌ |
| US-RPT-001 Overdue Ra Vùng Số Check Quá Trễ | ✅ | ✅ | ❌ | ❌ |
| US-RPT-002 Summary Bảng Coi Dân Khảo Thông Thư Số Dashboard Tổng   | ✅ | ❌ | ❌ | ❌ |
| US-SYS-001 Catalog Health Trạm Kiểm Ở App Hệ Cấp Services Kiểm System  | ✅ | ✅ | ✅ | ✅ |
| US-SYS-002 Lending Health Trạm Check Xem App Báo Dò Của System  | ✅ | ✅ | ✅ | ✅ |
