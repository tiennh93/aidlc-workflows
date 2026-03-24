# AI-DLC Evaluation Framework - Cấu trúc Tệp Định

```
aidlc-regression/
├── README.md                          # Trút nhìn qua của rã Project 
├── VISION.md                          # Nhổ bục định Vision với Goals cài mốc
├── FAQ.md                             # Giải nảy phễu cự hỏi đáp trích 
├── OPERATING_PRINCIPLES.md            # Các cài quyết ngỏ định chéo của luồng
├── CONTRIBUTING.md                    # Dải cấu nảy định đóng góp trích
├── pyproject.toml                     # Cài đai cấu cho môiWorkspace 
├── uv.lock                           # Thiết khóa rẽ package lock dependency
│
├── aidlc-runner/                      # Mã túi lõi Execution (trút chạy mảng rẽ cài bọc 2 agents  AIDLC)
│   ├── pyproject.toml
│   ├── config/
│   │   └── default.yaml
│   ├── src/
│   │   └── aidlc_runner/
│   │       ├── cli.py                # Cấu bọc nối đầu ngõ lệnh nhập xuất CLI 
│   │       ├── config.py             # Truy gán đai tải cho cài config. (Loading configuration)
│   │       ├── runner.py             # Mã cốt chóp đai Orchestration 
│   │       ├── metrics.py            # Móc thu lượng đai NFR metrics rẽ
│   │       ├── post_run.py           # Ổ đánh rẽ nảy test hậu (Post-run test evaluation) 
│   │       ├── progress.py           # Bể nhả bọc mảng tiến cự Progress 
│   │       ├── agents/               # Ngỏ các factory cho bộ phễu Executor & Simulator agents 
│   │       └── tools/                # Phễu túi cài cự hòm ảo Sandboxed, mỏ rule lười load, run_command
│   ├── tests/
│   └── planning/                     # Gán rẽ luồng chạy của backlog cùng phễu plans.
│
├── packages/                          # Túi ôm hệ vách các mảng check Evaluation (monorepo)
│   ├── qualitative/                   # Đánh phễu Qualitative (Ngữ Nghĩa Lượng)
│   │   ├── pyproject.toml
│   │   ├── src/
│   │   │   └── qualitative/
│   │   │       ├── __init__.py
│   │   │       ├── comparator.py     # Cài móc gán đối so đai Orchestration vòng
│   │   │       ├── document.py       # Túi nhả móp load Document với map mapping theo bọc Phase
│   │   │       ├── scorer.py         # Mã nhả bọc điểm cự Scoring theo vạch nhả + implementations
│   │   │       └── models.py         # Kho trích rã data cho đai Models móc result 
│   │   └── tests/
│   │
│   ├── quantitative/                  # Nảy điểm xả Static đánh cho Cự lượng (Code Evaluation quantitative)
│   │   ├── pyproject.toml
│   │   └── src/
│   │       └── quantitative/
│   │           ├── __init__.py
│   │           ├── linting.py        # Mã báo đo Ruff/eslint 
│   │           ├── security.py       # Phân tích Security báo bảo Semgrep/bandit 
│   │           └── organization.py   # Rã móc Code trùng duplication, vạch chóp ổ structure cấu Code
│   │
│   ├── nonfunctional/                 # Test vách nạp ngỏ của NFR 
│   │   ├── pyproject.toml
│   │   └── src/
│   │       └── nonfunctional/
│   │           ├── __init__.py
│   │           ├── tokens.py         # Mảng cài theo dải đo mức móc tiêu nhả Tokens (Token consumption Tracking)
│   │           ├── timing.py         # Hẹn đo thời phễu rã time nhả (Execution time measurement)
│   │           └── consistency.py    # Gắn móc đo tính rã chéo nhả Models (Cross-model consistency check)
│   │
│   ├── reporting/                     # Cấu phễu móc Report rã Generation báo xuất 
│   │   ├── pyproject.toml
│   │   └── src/
│   │       └── reporting/
│   │           ├── __init__.py
│   │           └── generate.py       # Ngách đai mở chóp File mảng phát Report Generator main chính
│   │
│   └── shared/                        # Túi chạy ngỏ bọc móc dùng chung rã (Common utilities utils)
│       ├── pyproject.toml
│       └── src/
│           └── shared/
│               └── __init__.py
│
├── test_cases/                        # Túi chứa cục vàng Test cases Nhả Input của (AIDLC)
│   ├── instructions.md
│   └── sci-calc/
│       ├── vision.md
│       └── tech-env.md
│
├── runs/                              # Trút ổ rã túi cài vách xuất báo Output của Test Evaluation 
│   └── {timestamp}-{uuid}/
│       ├── run-meta.yaml
│       ├── run-metrics.yaml
│       ├── test-results.yaml
│       ├── vision.md
│       ├── tech-env.md
│       ├── aidlc-docs/               # Nhả đai xả gán Documents Aidlc xả (Generated AIDLC Docs)
│       └── workspace/                # Đầu nhả mảng cục rã Application Source mã
│
├── writing-inputs/                    # Dải phễu nhả các file đai guides móc mảng tầm nhìn vision docs
│
├── overall_project/                   # Túi bọc mảng ngỏ rẽ strategy chiến luật mảng
│
└── docs/                              # Các rẽ phễu phụ file cài Docs
```

## Giải Chóp mảng Nạp Ngỏ Dải (Rocks Package Mapping) 

```
1. Túi cựa Test Vàng (Golden Test Case)        → test_cases/
2. Lưới Framework vạch chạy (Execution Framework)     → aidlc-runner/
3. Rã đo điểm chíp mảng rã Qualitative (Ngữ Căn đo)   → packages/qualitative/
4. So dải móc chóp cự nhả Code (Code Evaluation)      → packages/quantitative/
5. Trích ổ vạch mảng cài NFR Evaluation          → packages/nonfunctional/
6. Test cài vòng bục vách rã GitHub CI/CD            → .github/workflows/  (Lên plan dự định)
```

## Chuỗi rã móc Dependencies Bọc Packages 

```
aidlc-runner (Chạy trút độc cô tự nảy bọc luồn — ngỏ móc luồn vòng của bọc chạy workflow chóp cự ra túi file thư ổ trích Run folders run) 

qualitative
├── shared
quantitative
├── shared
nonfunctional
├── shared
reporting
├── shared
├── qualitative  (Dành chạy vạch bục để trích nhả đai đọc mảng gán của rã Qualitative đai semantic)
├── quantitative (Đọc tút mảng điểm cho của cựa Code static kết rã kết Code eval)
└── nonfunctional (Luồn đọc nhả bọc test NFR của trích nảy rã kết NFR) 
```

## Các phễu Định thiết Cốt y Cọc (Key Design Decisions)

1. **Rã nhóm mốc chứa bằng đai kho Monorepo móc vách dải UV:** Giải phễu giúp ngỏ đơn gán đo vòng cấu chóp móc dependency và đục làm Code dễ vách nạp rẽ chéo Package.
2. **Khóa Ổ của vòng móc Python đai mác 3.13:** Dải mã mốc Stable bản tốt đai chạy tính ngỏ cấu tân tân Features chóp.
3. **Mảng Gộp đo nảy đánh vạch rẽ Separate packages bằng dải nhả Types:** Túi khoanh chia cự bục cắm rẽ điểm Evaluation types rã độc nhả dải bọc Evolution. 
4. **Vòng mảng test Execution cài mảng aidlc-runner cự:** Khóa ổ Run đóng cho vách phễu thư Run folders mở phục vách Evaluation Test dải đi consume (Trích lấy tải đai).
5. **Gán phễu nháp dải nạp test chóp cự Golden test:** Chuyển vạch mở gán cấu thành tham rã inputs bọc Reproducible có kiểm tự gán báo Baseline reference.
6. **Móc trích chung phễu bục Shared:** Dải ngỏ túi gán mã bọc common xài chéo tái xả đâm rẽ gán All rã các Test packages
7. **Đai Reporting gom Aggregate All All:** Bộ gán túi entry point trích kết chung độc cự xả rã mác Báo rẽ Report mảng đi.
