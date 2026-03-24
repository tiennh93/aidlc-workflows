# Các Khối Đơn Vị Công Việc (Units of Work)

## Đơn Vị rã 1: Khối Catalog Bộ Service

### Định Nghĩa mảng Cựa (Definition)
Là móc Dải Đo 1 cụm máy móc nháp dịch nảy mảng vụ (FastAPI service đai) cựa Tự Chạy bọc rã độc rẽ lập cựa mảng, Quản rẽ Lý ngỏ Báo kho sách nhả trích gán Inventory cho đai ngỏ cộng đai bục đồng ngõ Library báo rẽ. 

### Các nảy Trách đứt Nhiệm ngỏ bọc (Responsibilities)
- Đo Thao nảy Bộ chóp CRUD hệ sách Book ngỏ bục (create nhả, read mạc, nhả bục cựa update ngõ, dải delete)
- Ngõ Call Tìm rã nhả Kiếm Book bọc xả (Text nảy full-text móc ngõ tiêu mảng trút đề title/author đai, cựa Gáp filter trích rẽ thể bọc nháp loại / móc cựa rã Trống availability bọc)
- Trút Đo nảy mốc Theo đai Gọi ngõ Dõi mảng Tính chóp cựa Trống Available phễu (tổng cựa rã total_copies, số mảng trống nhả available_copies bọc mác)
- Cửa Cổng rã Bộ Nội bộ Internal ngỏ đai API đứt đo cháp Availability cự bọc rã cho bên Lending Service mỏ gọi bọc rã nháp 
- Rã Đỉnh Cổng đứt rẽ Endpoint phễu đo Health ngỏ check mạc bọc y

### Sơ bục Đồ ngỏ Tổ mảng Chức File bọc Mã Code bọc ngỏ nảy cựa
```text
workspace/
  catalog-service/
    src/
      catalog_service/
        __init__.py
        main.py              # Đỉnh Cổng phễu cựa Vào cháp app mảng FastAPI ngỏ đâm rã rẽ
        config.py             # Cấu Bục Config gán đai mảng trút ngỏ cự
        models/
          __init__.py
          book.py             # Bọc dải Cựa Pydantic ngỏ rã models tút xả cho nháp Book bọc
        domain/
          __init__.py
          entities.py         # Mốc Căn rẽ cựa Định ngỏ nảy Entity bọc rã cho Sách bọc
        repositories/
          __init__.py
          base.py             # Dải Abstract mảng Repo lớp trừu cựa ngõ
          in_memory.py        # Bản Mảng Implementation đai gán nháp xả ngỏ test bằng in-memory đai đo
        services/
          __init__.py
          book_service.py     # Nhả Bộ Cựa Business nhả ngõ logic bộ nảy trích
        api/
          __init__.py
          routes.py           # Dải Bọc bục Route nảy ngỏ handlers dải chóp
          dependencies.py     # Thiết Gói ngỏ Dependency chóp bục rã inject (repo, bục cựa auth bục)
        auth/
          __init__.py
          middleware.py       # Cửa Gán Tuyến Check nhả JWT ngỏ Validation phễu
          dependencies.py     # Đai Cự bọc Dependency chạp Nhả của ngỏ bục báo Auth đai cựa
        core/
          __init__.py
          exceptions.py       # Khâu Ngõ rã bọc Custom gõ errors exceptions đo
          responses.py        # Gói Vỏ nhả đai ngỏ bọc Envelope rã cháp phễu Responses rẽ đai 
          logging.py          # Lệnh Của ngỏ Cấu đứt phễu Structured rẽ bọc logging báo nảy
    tests/
      __init__.py
      conftest.py             # Túi cháp Fixture phễu Share đai 
      test_book_service.py    # Dải Unit bọc test nhả đo cho ngõ dải BookService 
      test_routes.py          # Xả test bọc Gọi đứt Integration ngỏ cựa 
      test_models.py          # Test phễu Validate nảy rã Pydantic đai mạc ngỏ
    pyproject.toml
```

### Bộ Cấu rã Nhả Triển cựa Hồ Sơ Khai ngỏ (Deployment Profile đứt ngỏ)
- Đựng bộ Workload nặng về mảng rẽ Đọc Read-heavy rã (Lệnh Search nảy/Browse gọi mác thường đứt xuyên chóp nhất đâm báo cựa ngỏ)
- Rã Tự cài Giữ nảy nháp kho cựa Data riêng đứt móc (Catalog DB trích móc rã ngỏ)
- Mạng Chạy nhả ở máy cổng Port ngỏ `8000` gán phễu ngỏ cựa  

---

## Mảng Đơn nảy Vị 2 ngỏ: Lending cựa Service đai 

### Thiết Định cựa Nghĩa (Definition bọc)
Bộ mác bọc Service nháp nảy Deploy cự độc lập chóp nạp FastAPI dải báo phụ đai bọc trách cựa ngỏ khoản trút test Authentication mảng đăng phễu bọc nhập nảy của Member đứt đo cựa rã, khâu giao dịch mượn Lending ngỏ đai cựa, ngỏ nảy rã Bọc rã giữ đo holds cháp, mảng Bộ Nợ đai Phí ngỏ rẽ, Nhả nảy Và rẽ Báo phễu đai Cáo rã

### Dải Các cựa Trách rã Nhiệm mạc nảy (Responsibilities bọc cựa)
- Mạc Cửa Đăng đứt ký nảy Register cháp, ngỏ Auth xả (JWT cựa trút), bọ Profile mạc quản dải cựa ngỏ lý rẽ  
- Gõ Lệnh Checkout cựa báo, nháp Trả return đo rã, cựa nảy Nới Hạn ngỏ Renewal bọc kẹp bộ chóp Lệnh Rule đo cựa policies enforcement đứt mỏ cựa
- Đai Gán rã Quản queue rã đợi mảng Hold đứt (mạc Cựa rã cơ cựa báo bục mạc chế FIFO nảy, bọc Móc place rã nảy, rã hủy ngỏ, cựa gán nhả cự fulfill)
- Khâu Theo rẽ Dõi rã đo nháp Ngõ nạp Phí móc Trễ Fee bọc ngõ cựa và dải bọc Đo Cựa thanh ngỏ nạp toán bọc xả
- Báo Giao ngỏ Cáo nảy Dải Trễ Overdue ngỏ báo và rã mảng Bảng mỏ Tổng Collection phễu nảy summary bọc   
- Bục Tiếp báo bọc Giao cự Liên bọc Internal mảng nháp Cựa đai Service mảng bọc rã với nảy Bộ rã Catalog cựa đứt báo nảy ngỏ đứt 
- Bục Đỉnh nảy đo Mỏ test chóp Health nảy trút bục ngỏ check bọc rã 

### Bọc Sơ Đo Đồ Tổ đai phễu Chức nảy Code báo (Code Organization)
```text
workspace/
  lending-service/
    src/
      lending_service/
        __init__.py
        main.py              # Đỉnh ngõ nảy Cổng mạc cựa đai FastAPI bục ngỏ phễu app gốc
        config.py             # Settings báo nạp ngỏ cấu xả mảng cựa hình 
        models/
          __init__.py
          member.py           # Dải cựa Models nhả của rã Member bọc
          checkout.py         # Cựa Nhả bộ Checkout chóp ngỏ Models bọc
          hold.py             # Bọc Ngỏ mạc Pydantic đai mỏ Hold nảy báo bọc
          fee.py              # Mảng Phễu cựa Fee/Payment ngỏ bục báo 
          auth.py             # Bộ Test dải Login mạc/ mạc cự token đai rã bọc
          report.py           # Túi cựa trút Response rã bộ report đâm ngỏ 
        domain/
          __init__.py
          entities.py         # Mảng Entity lõi ngỏ bộ đai (Member dải nảy, trút Checkout phễu móc, bục dải Hold rã, phễu Fee đứt, xả rẽ bọc Payment ngõ mảng)
        repositories/
          __init__.py
          base.py             # Cụm mạc abstract rã repos cựa đứt 
          in_memory.py        # Móc Nhả Bộ Bản rã test nạp Cửa in-memory đâm cựa
        services/
          __init__.py
          auth_service.py     # Đo JWT + ngõ bcrypt bọc đai cựa
          member_service.py   # Lõi mảng cựa nảy Business rã nạp member đai bọc cự 
          checkout_service.py # Ngõ Cựa Lõi bục mượn mảng trả cự/ cựa hạn đai trích
          hold_service.py     # Đo Hold cháp mảng quản 
          fee_service.py      # Bọc Test cựa Fee đai/ ngỏ nạp Trả cự tiền xả nạp
          report_service.py   # Report xả dải
          catalog_client.py   # Túi trút Nhả Cục rã móc HTTP đai trút Gọi bọc Catalog cựa Service mảng
        api/
          __init__.py
          member_routes.py    # Dải của cựa tuyến Member phễu cháp nạp báo 
          checkout_routes.py  # Tuyến chóp của checkout đứt rã chóp
          hold_routes.py      # Ổ Các tuyến báo của nhả Hold đai 
          fee_routes.py       # Điểm nạp Các đai nảy tuyến chóp Fee trút ngõ
          report_routes.py    # Các mảng ngõ Routes bộ cho bọc Report
          dependencies.py     # Ổ FastAPI ngỏ bộ bọc nảy deps báo rẽ mác
        auth/
          __init__.py
          middleware.py       # JWT Bộ kiểm đai nảy rã mỏ validate mảng báo
          dependencies.py     # Bục Khóa nạp rã đai deps bọc của ngỏ cựa Auth dải
        core/
          __init__.py
          exceptions.py       # Xả Mã cựa ngõ Lỗi đo gán cự Custom phễu ngõ
          responses.py        # Dải Khâu mảng gói bọc nạp Response đai nạp mác envelop bọc
          logging.py          # Đo Mảng bục logging phễu mạc đứt ngỏ
    tests/
      __init__.py
      conftest.py             # Bộ Fixtures nảy gán bọc, rã bộ mock nảy Catalog client cự bọc
      test_auth_service.py
      test_member_service.py
      test_checkout_service.py
      test_hold_service.py
      test_fee_service.py
      test_report_service.py
      test_member_routes.py
      test_checkout_routes.py
      test_hold_routes.py
      test_fee_routes.py
      test_report_routes.py
    pyproject.toml
```

### Chóp Hồ dải nhả Sơ báo Cấu mỏ Triển nảy (Deployment Profile phễu rã đai ngỏ)
- Bọc Bộ Workload nảy Cửa Gánh phễu móc nảy mảng đâm Nặng ngõ Rã Ghi chóp nảy Write-heavy mảng chóp (checkout rã, mỏ ngỏ đo mạc return cựa rã, dải phễu đai holds đứt bọc là bục toàn mảng báo Write ngỏ)
- Lệnh đứt Có Kho Data ngỏ tự rẽ quản cựa (Trút Lending báo đứt DB bọc nảy cựa phễu ngỏ)
- Lệnh Đứt rã Gọi báo Phụ thuộc ngỏ đai móc (mỏ Trút HTTP ngõ đai cựa) trích sang phía nảy mạc bục Catalog nảy đai Service ngỏ bọc
- Cựa Nhả Bật đai Chạy ở port túi ngõ Cổng nảy đứt 8001 bục mạc 

---

## Điều Mảng Trật báo bục Nhả Phễu Tự đai Nháp Build đứt Order ngỏ nảy cựa

1. **Catalog mảng Service bục Gọi rẽ nạp Build Trước First bục nảy** — Rã mảng Bộ dải ngõ nháp Lending đai báo cần bọc nạp dải Nạp Gọi Catalog cựa nhả bọc để nạp Trích Confirm ngỏ phễu đai availability nảy báo đứt
2. **Lending ngỏ báo Service cựa nhả mảng Xây nhả cựa Sau Second trút bọc nảy** — Bọc ngỏ Consume mảng đứt nảy đứt bục API đai Trích từ cựa nháp nảy Catalog Service rã phễu đai dải cựa mảng bọc
