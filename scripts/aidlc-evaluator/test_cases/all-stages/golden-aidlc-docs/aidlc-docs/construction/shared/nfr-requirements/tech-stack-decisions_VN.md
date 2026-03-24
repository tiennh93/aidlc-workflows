# Quyết định Chọn Ngăn Xếp Công nghệ (Tech Stack Decisions) — Hai Nhánh Services

## Công Nghệ Cốt Lõi (Core Stack)

| Mảng Công nghệ (Technology) | Bản Version | Điểm Mục đích (Purpose) |
|---|---|---|
| Python | 3.13+ | Trạm Máy Runtime |
| FastAPI | 0.115+ | Mạng Hệ REST API framework |
| Pydantic | mỏ 2.x | Nhả Request/response Bộ nảy Mảng validation kiểm |
| uvicorn | 0.34+ | Ổ cựa ASGI nạp Server chạy |
| PyJWT | rã 2.x | Mỏ Mảng Sinh JWT gen dải và đai Kiểm Auth Nhả Validation |
| passlib[bcrypt] | cựa 1.7+ | Trích Bộ Nhả Nảy Băm Mác Mật Ngỏ Khẩu Password Hashing |
| httpx | phễu 0.28+ | HTTP Trống Client (Móc Từ Lending → Sang cựa Catalog, rẽ nảy test client ngỏ) |

## Dải Móc Ngăn Tooling Kiểm Thử (Testing Stack)

| Bộ Phễu Trích Nhả Công Nghệ (Technology) | Version Nhả | Để đai Làm cựa Ngỏ Gì (Purpose) |
|---|---|---|
| pytest | 8.x | Ổ rẽ Móc Test rã cựa runner ngỏ nảy |
| pytest-asyncio | mỏ 0.24+ | Đai Support nhả Gói rẽ Async test bọc móc test |
| pytest-cov | nạp 6.x | Trút Báo cựa đai Report cho rã nảy đo Test rã Nhả Coverage mảng |
| httpx | nảy 0.28+ | Hệ bọc ngỏ AsyncClient dải ngỏ Dùng Cho rã test phễu đai test Integration ngỏ rã |

## Bộ Đai Dụng Cụ Của Hệ Dev Phát Mảng Triển (Development Tools)

| Lệnh Toolkit cựa Mảng Công (Technology)| Bản rã Ver | Mục cựa Ý Trích (Purpose)|
|---|---|---|
| uv | bản xả mới nhất | Quản móc cài Trút mảng đo Nhả Package đai Management cài |
| ruff | mảng 0.9+ | Gán mạc Cựa Tool Linter rẽ cựa Linting đứt nảy + rẽ format túi gán Formatting |

## Cấu Dải Chiến Bọc Lược cựa Thiết Data Database (Database Strategy mảng)

| Lớp Mảng Cửa Layer Tầng | Ổ Công rẽ Bộ Nghệ Technology | Của Mục Purpose cựa |
|---|---|---|
| Dải Lớp Cổng Repository Interface | Túi Lệnh nhả Abstract rã mảng base báo class | Mốc Định nạp Chuẩn đo Hợp rã Data access nảy trút Contract |
| Điểm Gắn Trong MVP Implementation | Nhả In-memory cựa (Dài qua mảng Python dict) | Chạy Dev/Test ngỏ Zero-dependency cựa bọc đai gán phễu đai (Không báo đứt nháp 1 Trích Hệ Nào cấu đai External bọc) |
| Trút Tương Cựa Giờ Prod đai phễu Future | Đo Mỏ DynamoDB hoặc bọc ngỏ nạp PostgreSQL | Dành Lúc bọc nảy Deploy Cloud (Trong cựa đai Phase ngỏ 2 cựa) |

## Thiết Cấm Ngõ Bọc Kìm Code Technologies Prohibited (Cấu lệnh nảy từ bọc mác tech-env.md đo cựa)

| Ngỏ Các rã Nạp Cấm Lệnh Prohibited | Lệnh Dùng Giải Rã Kế By Thay rẽ ngỏ Mác (Use Instead) |
|---|---|
| Máy Flask, báo Django | Xài trích FastAPI đai nhả |
| Mỏ requests cựa bọc | Nảy trút xài httpx |
| Các pandas, bọc nạp numpy | Cái túi nảy Standard đứt mỏ Python cựa xả |
| Ổ cựa pip, túi poetry, ngỏ pipenv | Bộ uv |
| Mảng black, nháp flake8, dọn isort | Rã Gán qua bọc ruff |
| EC2 (hệ trực bọc direct cựa máy) | Hệ nảy Lambda mảng hoặc dùng trích Fargate (móc trích phễu tương chóp sau Future đai) |
| Elastic Beanstalk | Chạy ngỏ cắn AWS CDK (phễu rã bọc cho nảy tương cưa mỏ cài Lai) |

## Cấu Thư Mảng Mốc Khúc Trúc Lệnh Project Root (Project Structure)

Mảng Mỗi service nảy cựa đai báo Nhả phễu 1 Túi đo mảng Phễu Python bọc Độc Lệnh Independent ngỏ Python trích package mảng:
- `catalog-service/` — nảy túi trong cựa đai `pyproject.toml`, tới cựa `src/catalog_service/`, nạp cựa `tests/`
- `lending-service/` — có bọc ổ mạc đai `pyproject.toml`, trích `src/lending_service/`, vách cùng cựa `tests/`

Cả 2 chóp ngõ bọc máy Đều mạc dùng tool chóp `uv` làm ổ phễu đai bọc package nạp manager với nạp đo ngỏ `pyproject.toml` để bục định file nghĩa báo các cấu dependency phễu.
