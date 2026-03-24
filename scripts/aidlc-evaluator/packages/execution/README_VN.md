# aidlc-runner

Một bộ điều phối hai agent chạy toàn bộ vòng đời phát triển phần mềm được dẫn dắt bởi AI (AI-Driven Development Life Cycle - AI-DLC). Từ một tài liệu định hướng sản phẩm (vision) và một tài liệu khai báo môi trường kỹ thuật (tech env - tùy chọn), `aidlc-runner` đóng vai trò nhạc trưởng điều phối agent **Executor (Thực thi)** và agent **Human Simulator (Giả lập Con người)** để gánh vác một dự án phần mềm từ khâu lập yêu cầu đến việc sinh mã nguồn, qua đó tự động xuất ra toàn bộ các bản thiết kế tài liệu cũng như mã nguồn ứng dụng có thể hoạt động được.

## Cách Hoạt Động

`aidlc-runner` tạo ra một bầy (swarm) dựa trên [Strands Agents](https://github.com/strands-agents) với hai agent liên tục giao tiếp với nhau qua lại (handoff):

1. **Executor (Thực thi)** — Chèo lái luồng làm việc AIDLC qua từng giai đoạn một. Nó nạp các file quy tắc tương ứng cho từng giai đoạn, tạo ra các chứng thực đầu ra (như yêu cầu, thiết kế, code) và chuyển giao lượt (handoff) cho agent giả lập bất cứ khi nào cần sự can thiệp của con người.
2. **Human Simulator (Giả lập Con người)** — Đóng vai một người trực tiếp nắm bắt dự án (stakeholder). Nó trả lời các câu hỏi làm rõ, phê duyệt các báo cáo tài liệu, và kiểm duyệt mã nguồn được tạo dựa trên các tài liệu định hướng và môi trường kỹ thuật. Sau đó, nó chuyển quyền điều khiển lại cho agent thực thi.

Các agent này lặp lại vòng lặp chuyển giao lượt này xuyên suốt các pha Khởi tạo (Inception) và Xây dựng (Construction) cho tới khi toàn bộ ứng dụng được tạo ra.

### Các Giai Đoạn Vòng Lặp

**Pha Khởi tạo (Inception Phase)** — Cần xây dựng cái gì và tại sao:

| Giai đoạn | Điều kiện kích hoạt |
|---|---|
| Nhận diện Không gian làm việc (Workspace Detection) | Luôn luôn |
| Kỹ thuật Đảo ngược (Reverse Engineering) | Chỉ dành cho dự án brownfield (nâng cấp trên code cũ) |
| Phân tích Yêu cầu (Requirements Analysis) | Luôn luôn |
| Xây dựng User Stories (Câu chuyện Người dùng) | Nếu độ phức tạp dự án đòi hỏi |
| Lên kịch bản kế hoạch (Workflow Planning) | Luôn luôn |
| Thiết kế Ứng dụng (Application Design) | Tùy điều kiện |
| Trích xuất Khối làm việc (Units Generation) | Tùy điều kiện |

**Pha Xây dựng (Construction Phase)** — Cách xây dựng (chạy trên từng khối công việc - units):

| Giai đoạn | Điều kiện kích hoạt |
|---|---|
| Thiết kế Chức năng (Functional Design) | Tùy điều kiện |
| Yêu cầu Phi chức năng (NFR Requirements) | Tùy điều kiện |
| Thiết kế Phi chức năng (NFR Design) | Tùy điều kiện |
| Thiết kế Hạ tầng (Infrastructure Design) | Tùy điều kiện |
| Sinh Code (Code Generation) | Luôn luôn |
| Build và Test thử | Luôn luôn |

## Tiền Điều Kiện

- Python 3.13+
- [uv](https://github.com/astral-sh/uv)
- Git (để tải bộ quy tắc AIDLC; không cần nếu đã dùng tham số `--rules-path`)
- AWS CLI đã định cấu hình profile và có quyền truy cập vào Amazon Bedrock.

## Cài đặt

Từ thư mục gốc (root) của dự án:

```bash
cd aidlc-runner
uv sync
```

## Sử dụng

```bash
uv run aidlc-runner --vision <đường-dẫn-đến-file-vision> [--tech-env <đường-dẫn-đến-file-tech-env>] [các tùy chọn]
```

Tham số duy nhất bắt buộc là `--vision`, nó trỏ tới một file markdown dùng để mô tả chi tiết thứ bạn cần xây dựng. Ngoài ra, tham số tùy chọn `--tech-env` cho phép cung cấp thông tin môi trường kỹ thuật nhằm xác định cách để hệ thống xây dựng nó (ngôn ngữ, framework, chuẩn bảo mật, v.v). Xem tham khảo ở bản [hướng dẫn viết tài liệu đầu vào](GUIDE_TO_WRITING_VISION_DOCS.md) để biết chi tiết.

### Các Ví Dụ

Cơ bản — chỉ dùng toàn thiết lập mặc định:

```bash
uv run aidlc-runner --vision ./my-project-vision.md
```

Kèm với tài liệu môi trường kỹ thuật:

```bash
uv run aidlc-runner --vision ./my-project-vision.md \
  --tech-env ./my-project-tech-env.md
```

Tùy chỉnh AWS profile và region:

```bash
uv run aidlc-runner --vision ./my-project-vision.md \
  --aws-profile my-profile \
  --aws-region us-east-1
```

Sử dụng thư mục quy tắc AIDLC cục bộ thay vì kéo (clone) từ kho GitHub:

```bash
uv run aidlc-runner --vision ./my-project-vision.md \
  --rules-path /opt/aidlc-workflows
```

Thiết lập file cấu hình hoặc thư mục đầu ra (output) tùy biến:

```bash
uv run aidlc-runner --vision ./my-project-vision.md \
  --config ./my-config.yaml \
  --output-dir ./my-runs
```

Áp ghi đè các ID của mô hình LLM:

```bash
uv run aidlc-runner --vision ./my-project-vision.md \
  --executor-model us.anthropic.claude-opus-4-20250514-v1:0 \
  --simulator-model us.anthropic.claude-opus-4-20250514-v1:0
```

### Tham Chiếu Các Cờ Lệnh CLI

| Tham số / Flag | Bắt buộc | Mặc định | Mô tả |
|---|---|---|---|
| `--vision PATH` | Có | — | Nơi trỏ path dẫn vào file vision định hướng / các yêu cầu cấm lệnh markdown file |
| `--tech-env PATH` | Không | — | Cột mốc điểm chót của file cấu hình môi markdown file |
| `--config PATH` | Không | Built-in default | Dẫn đường tệp tùy biến cài qua config YAML |
| `--aws-profile TEXT` | Không | `default` | Tên của mã quyền chạy AWS profile |
| `--aws-region TEXT` | Không | `us-west-2` | Định lại tên trạm khu vực region nối bộ AWS Bedrock |
| `--executor-model TEXT` | Không | Claude Opus 4 | Mã ID chọn điểm máy llm cho executor agent |
| `--simulator-model TEXT` | Không | Claude Sonnet 4.5 | Mã ID đai máy llm rẽ trích simulator agent |
| `--output-dir PATH` | Không | `../runs` | Rã túi Folder đệm sẽ đổ dữ xuất chứa File |
| `--rules-path PATH` | Không | Tải kéo Git | Nhắm mốc path bằng bản cục bộ lưu cho luật rules directory |
| `--no-exec` | Không | Được bật sẵn | Khóa chặn tự thi hành các lệnh shell command trong luồng |
| `--no-post-tests` | Không | Được bật sẵn | Lệnh tắt quá trình build test hậu kỳ (post-run test) |

## Cấu Hình

Thiết lập hệ lấy giá trị dựa lưng vào mô hình ưu tiên từ cao xuống là: **CLI flags (cờ lệnh đánh tay) > YAML config (file gắn) > built-in defaults (Gốc nhúng của Python)**.

### File Cấu Hình Dạng YAML

Dùng cấu hình trong tệp YAML và đẩy nó vô phễu `--config`. Giá trị nào thiếu trích lập sẽ tự xài file chốt của hệ tích hợp built-in.

```yaml
aws:
  profile: "my-profile"
  region: "us-east-1"

models:
  executor:
    provider: "bedrock"
    model_id: "us.anthropic.claude-opus-4-20250514-v1:0"
  simulator:
    provider: "bedrock"
    model_id: "us.anthropic.claude-opus-4-20250514-v1:0"

aidlc:
  rules_source: "git"                  # gõ là "git" hoặc báo "local"
  rules_repo: "https://github.com/awslabs/aidlc-workflows.git"
  rules_local_path: null               # cấu chót path khi cài rules_source là "local"

swarm:
  max_handoffs: 200
  max_iterations: 200
  execution_timeout: 14400             # Giới lệnh báo kết thời hành chờ 4 tiếng (mã count bằng giây)
  node_timeout: 3600                   # Định văng nhả chạy node timeout tới 1 tiếng (bằng giây)

runs:
  output_dir: "../runs"
```

### Bộ Tích Hợp Gốc (Built-in Defaults)

Các giá trị ghim sẵn cấu gốc của thiết chạy giống y dạng mẫu file bên trên. Nó ngự tại file `aidlc-runner/config/default.yaml`.

## Cấu Trúc File Xuất Kết Ra

Kết qua của từng lượt đánh lệnh `aidlc-runner` đóng ra một túi folder in kèm chữ nhật ký ngày tháng trên vỏ thư:

```
runs/
└── 20260212T143022-a1b2c3d4e5f6.../
    ├── run-meta.yaml              # Gắn mạc thẻ chạy đai: timestamps, config snapshot, status trạng báo
    ├── run-metrics.yaml           # Các chỉ mốc NFR: luồng token, canh thời, artifacts chóp, tệp list về errors
    ├── test-results.yaml          # Test báo Pass/Fail ra sao (được nẩy lúc post-run tests)
    ├── vision.md                  # Chóp mảng bản sao của cấu hình ngầm file input vision ban đầu
    ├── tech-env.md                # Lôi nhả bản lưu thiết file cháp của input tech-env (nếu cựa gán)
    ├── aidlc-rules/               # Các túi mảng bộ quy tắc AIDLC đã dùng test (cloned dán ra hoặc copy)
    │   ├── aws-aidlc-rules/
    │   └── aws-aidlc-rule-details/
    ├── aidlc-docs/                # Băng cài các file Documents lưu trút chứng thực (artifacts docs)
    │   ├── inception/             # Tệp Requirements, bộ thư user stories, các mẫu mập designs, v.v.
    │   ├── construction/          # Bộ Functional design, mảng tút code plans, ghi mộc reviews
    │   ├── aidlc-state.md         # Phễu chạy bám tracking điểm rẽ của Trạng Thái Luồng AI State 
    │   └── audit.md               # Lưu bản nhật trích tóm lịch chạy xỏ theo giờ (audit log chạy theo các stage)
    └── workspace/                 # Nơi đập hộp văng bộ Source code đã sinh
        ├── src/
        ├── tests/
        ├── pyproject.toml
        └── ...
```

`run-meta.yaml` trút cất biên lưu báo giờ start/end vòng xoáy, đụng báo mảng status đánh đóng ngỏ vách, hệ handoffs điểm rã cài, lịch history vách và ảnh chép Snapshot config. Và phễu thông báo lượng tài tiêu NFR — `run-metrics.yaml` — gom điểm Tokens Token usage (gom bục + theo tay Agent), mốc timing nhịp Handoff, đếm cựa số bộ cài file tạo và dòng độ dài của source code (lines of code), đồng nhả cài lưu event bục về Error/ Retry events xả trích.

## Cài Phát Triển

### Chạy hệ Test unit 

```bash
cd aidlc-runner
uv run pytest
```

### Chỉnh mã Linting cấu chuẩn

```bash
uv run ruff check . && uv run ruff format .
```

### Trúc Nhánh Dải Project

```
aidlc-runner/
├── config/
│   └── default.yaml                # Bộ mạc đai Configuration
├── src/aidlc_runner/
│   ├── __init__.py                 # Báo lưu chốt version Pack (0.1.0)
│   ├── __main__.py                 # Gọi `python -m aidlc_runner` là xả đai luồng
│   ├── cli.py                      # Đầu bọc Parse args và bộ mảng main()
│   ├── config.py                   # Cài nạp báu chạy cấu dataclasses Config ngỏ load 
│   ├── runner.py                   # Cấp tút thiết cự ổ run folder, mảng bộ luật rules, swarm orchestration báo đánh máy
│   ├── metrics.py                  # Trình rẽ nạp đo ghi móc bọc Metrics, artifact quét luồn, lưu YAML ngỏ xuất
│   ├── progress.py                 # Nhóm vách Callback bục cài móc chóp swarm đo phễu nhả progress
│   ├── post_run.py                 # Rẽ cựa Test đo cài post-run evaluation 
│   ├── agents/
│   │   ├── executor.py             # Nhà ngỏ khởi cấu bục phễu trút Executor agent factory
│   │   └── simulator.py            # Ngỏ bọc cài máy lập báo Simulator agent factory
│   └── tools/
│       ├── file_ops.py             # Nhà ảo đai cự Sandbox đọc/ghi/gọi cho các tools file
│       ├── rule_loader.py          # Lệnh đi bục lấy path của ngỏ Luật AIDLC trích móc
│       └── run_command.py          # Áo giả bọc Sandboxed cài cấu móc shell execution command cự
├── tests/
│   ├── test_config.py              # Đánh Unit cấu của cài Config tests
│   ├── test_metrics.py             # Ngỏ đai test đo nảy mảng Metrics bọc artifact scanner scanner đo tests
│   ├── test_post_run.py            # Chóp trút cài bục test ngầm Post-run trút
│   ├── test_run_command.py         # Kiểm hòng bục móc bộ Test cho ngỏ Command sandboxing test 
│   └── test_two_inputs.py          # Bóc cài đai Two-input-document (mảng test vision nạp cấu tech-env đai móc)
└── pyproject.toml
```

### Mảng Cấu Bộ Core (Key Modules)

- **cli.py** — Cầm trịch Parsing thông CLI arguments đai (báo gồm `--vision` gán `--tech-env`), nạp chóp config, mảng móc `runner.run()`.
- **config.py** — Xả đai móc bục `RunnerConfig` cùng bọc bộ dải cài Dataclasses lồng ổ (`AwsConfig`, `ModelConfig`, `SwarmConfig`, etc.). Mảng gộp cài rã đai chéo (Merges) defaults ngỏ trích với cựa YAML YAML overrides rẽ. 
- **runner.py** — Trút ngỏ lập túi File ổ (Folder), rút chép lấy trích bản `vision` cùng đai nảy `tech-env` docs, trút cài thông rules rẽ đo cài, gọi bung móc đai ổ rẽ của both Agents cự vào trong bục 1 `Swarm`, trích chạy đục ngỏ thi hành Swarm Swarm vách, xả chóp kết lưu test yaml.
- **metrics.py** — Trục gài cài `MetricsCollector` cự thông handoff mảng và đo xả list Error trong ngỏ rã ngỏ run execution, sau xả ráp gom nảy tokens, đai đếm bọc artifact count nảy, xé trút handoff pattern theo vách bục hậu kì ở `run-metrics.yaml`.
- **progress.py** — Phễu vòng lặp `AgentProgressHandler` móc nảy điểm báo tool gán invocations đâm rã cùng xả thông của error event của rã đo cài ngỏ riêng báo Agent. Cựa phễu cài `SwarmProgressHook` đóng nảy giờ chạy bọc đai theo (node-level handoff cự) theo rẽ đai nảy nháp event móc Strands Strands hook.
- **post_run.py** — Máy Test chấm mác của hậu kì: Nhả quét dò bộ thông cự hệ rẽ móc của túi gán Project trong `workspace/`, nạp cựa thông dependency, gõ nhịp đục test luồn bọc trút xả nảy báo File gán `test-results.yaml`.
- **agents/executor.py** — Gọi sinh cự mác bọc Agent cài `Agent` cùng thông của bục móc mảng đo nhả phễu file-ops rẽ tools trích (file-ops móc), luồng móc rule-loader, chạy chóp mở nhả cài 1 cái cài (optional bọc) bộ đo móc `run_command của tool`. Dải System prompt được xả trích cựa gán cài rã bọc chuỗi mảng đánh dải móc quy Sequential (AIDLC cự ngỏ mảng cấu protocol). 
- **agents/simulator.py** — Bịch xả bọc máy giả `Agent` đai dùng cài móc tools thư File ops file-ops. Gán báo mảng Prompt đai đâm tự nảy cài trích bục (dynamically cựa rẽ sinh generated) cho xả gộp cự thông của tệp `vision` cùng luồng kẹp đâm nảy của thư tài bục cài (tech-env tech-env document document nảy nhả đai ngỏ). 
- **tools/file_ops.py** — Phương ngỏ `make_file_tools(run_folder)` trút trả cựa ổ báo bọc hộp đai test do các hàm `read_file`, `write_file`, và gán `list_files` kẽo trút ngỏ gán rã scope bị kìm trong Run folder rẽ nảy (Ngừa phễu trút báo mảng đai gọi chóp file ngoài). 
- **tools/rule_loader.py** — Ngách xả phễu `make_rule_loader(rules_dir)` móc trả nhả mảng `load_rule` là chức điểm luồn để phễu nhả đọc file ngắn báo cự ("shorthand path" ví đo: `"inception/requirements-analysis"`) bung nó ra cho điểm dẫn ngõ path thực full filepath của hệ tệp File ngỏ móc Rule. 
- **tools/run_command.py** — Phương đánh rã `make_run_command(run_folder)` cấp 1 chức đai rẽ (tạo cựa vỏ Sandbox bọc rẽ cho `run_command command`) ứng để mảng chạy bọc phễu luồn cự mảng shell ngỏ cự ngỏ vách trong móc của vòng lặp Test Build.
