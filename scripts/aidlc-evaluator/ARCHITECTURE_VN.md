# Framework Đánh giá AIDLC — Tài liệu Thiết kế

## 1. Mục đích

Tài liệu này mô tả kiến trúc, các quyết định thiết kế, luồng dữ liệu và cơ chế hoạt động bên trong của **Khung Đánh giá & Báo cáo quy trình làm việc AI-DLC**. Nó dành cho các nhà phát triển cần hiểu cách hệ thống hoạt động, mở rộng hoặc gỡ lỗi (debug).

Framework xác thực các thay đổi đối với kho lưu trữ [quy trình làm việc AI-DLC](https://github.com/awslabs/aidlc-workflows) bằng cách chạy một vòng đời phát triển phần mềm được điều khiển bởi AI (AIDLC) từ đầu đến cuối, sau đó chấm điểm các kết quả đầu ra dựa trên nhiều chỉ số chất lượng: độ chính xác về chức năng, chất lượng mã, sự tuân thủ hợp đồng API và sự tương đồng ý nghĩa so với bản chuẩn (golden baseline).

---

## 2. Kiến trúc Cấp cao

```
                              ┌──────────────────────┐
                              │   Các Điểm Vào (CLI) │
                              └──────────┬───────────┘
                 ┌───────────────────────┼──────────────────────┐
                 │                       │                      │
        run_evaluation.py      run_batch_evaluation.py   run_ide_evaluation.py
         (chạy 1 model)          (vòng lặp multi-model)   (dành cho IDE adapter)
                 │                       │                      │
                 └───────────┬───────────┘                      │
                             │                                  │
              ┌──────────────▼──────────────┐   ┌───────────────▼──────────┐
              │  Pipeline 6-Giai đoạn       │   │     Mảng IDE             │
              │  ┌──────────────────────┐   │   │  ┌───────────────────┐   │
              │  │ 1. Thực thi          │   │   │  │ Adapter (Cursor,  │   │
              │  │    (Strands Swarm)   │   │   │  │ Cline, Kiro, ...) │   │
              │  ├──────────────────────┤   │   │  └────────┬──────────┘   │
              │  │ 2. Test hậu kỳ(Post) │   │   │           │              │
              │  ├──────────────────────┤   │   │  ┌────────▼──────────┐   │
              │  │ 3. Đo lường (Quan)   │   │   │  │ Chuẩn hóa đầu ra  │   │
              │  ├──────────────────────┤   │   │  └────────┬──────────┘   │
              │  │ 4. Test hợp đồng (API)│   │  │           │              │
              │  ├──────────────────────┤   │   └───────────┼──────────────┘
              │  │ 5. Định tính (Qual)  │   │               │
              │  ├──────────────────────┤   │    ┌──────────▼──────────┐
              │  │ 6. Tạo Báo cáo       │   │    │ --evaluate-only     │
              │  └──────────────────────┘   │    │ (stages 2-6)        │
              └─────────────────────────────┘    └─────────────────────┘
                             │
              ┌──────────────▼───────────────┐
              │  runs/<timestamp>/           │
              │   ├── aidlc-docs/            │
              │   ├── workspace/             │
              │   ├── run-meta.yaml          │
              │   ├── run-metrics.yaml       │
              │   ├── test-results.yaml      │
              │   ├── quality-report.yaml    │
              │   ├── contract-test-results… │
              │   ├── qualitative-comparison…│
              │   ├── report.md / .html      │
              │   └── evaluation-config.yaml │
              └─────────────────────────────┘
```

---

## 3. Cấu trúc Gói (Package Structure)

Dự án sử dụng cơ chế **không gian làm việc uv** (được định nghĩa trong `pyproject.toml` ở root) với tám gói nội bộ. Mỗi gói được cấu trúc độc lập với `pyproject.toml`, bố cục `src/` và thư mục `tests/` riêng.

| Gói | Tên PyPI | Mục đích |
|---------|-----------|---------|
| `packages/execution` | `aidlc-runner` | Bầy hai agent (swarm) chạy quy trình AIDLC |
| `packages/qualitative` | `aidlc-qualitative` | Chấm điểm tương đồng ngữ nghĩa của tài liệu vs bản chuẩn |
| `packages/quantitative` | `aidlc-quantitative` | Phân tích tĩnh: linting, bảo mật, code trùng lặp |
| `packages/contracttest` | `aidlc-contracttest` | Kiểm thử hợp đồng API theo đặc tả OpenAPI |
| `packages/nonfunctional` | `aidlc-nonfunctional` | Đánh giá NFR (token, thời gian, tính nhất quán) |
| `packages/reporting` | `aidlc-reporting` | Tạo báo cáo tổng hợp (Markdown + HTML) |
| `packages/ide-harness` | (không xuất bản) | Framework adapter IDE cho các trợ lý AI của bên thứ ba |
| `packages/shared` | `aidlc-shared` | Các tiện ích chung chia sẻ giữa các gói |

**Đồ thị Dependency** (được làm gọn):

```
run_evaluation.py ──► execution (aidlc-runner)
                  ──► quantitative
                  ──► contracttest
                  ──► qualitative
                  ──► reporting ──► reporting.collector
                                ──► reporting.baseline
                                ──► reporting.render_md
                                ──► reporting.render_html
```

Tất cả các gói giao tiếp thông qua **các tệp YAML trên đĩa**. Không có sự phụ thuộc (dependencies) trong tiến trình (in-process) ở cấp độ thư viện giữa các gói đánh giá — bộ điều phối (`run_evaluation.py`) gọi từng gói dưới dạng quy trình con (subprocess) thông qua `python -m <package>`, truyền đường dẫn tệp làm đối số. Thiết kế này giúp các gói có thể hoạt động độc lập và cho phép mỗi gói được chạy một cách cách ly.

---

## 4. Hệ thống Cấu hình

### 4.1 Giải quyết Cấu hình Phân tầng

Cấu hình tuân theo mô hình ưu tiên ba tầng:

```
CLI flags  >  YAML config file  >  Built-in Python defaults
```

1. **Built-in defaults** (Mặc định tích hợp) được định nghĩa là các giá trị mặc định của file field ở class (`RunnerConfig` và các nested class) tại `packages/execution/src/aidlc_runner/config.py`.
2. **YAML config** được tải từ `config/default.yaml` (hoặc cấu hình tùy chọn qua cờ `--config`). Hàm `_merge_dict_into_dataclass()` áp đè dữ liệu đệ quy chèn cấu hình YAML lên cây dataclass.
3. **CLI flags** (vd: `--executor-model`, `--profile`) được áp dụng sau cùng, ghi đè cả YAML và mặc định.

### 4.2 Cấu trúc Phân nhánh của Cấu hình Dataclass 

```python
RunnerConfig
  ├── aws: AwsConfig          # profile, region
  ├── models: ModelsConfig
  │     ├── executor: ModelConfig   # provider, model_id
  │     └── simulator: ModelConfig
  ├── aidlc: AidlcConfig      # rules_source, rules_repo, rules_ref
  ├── swarm: SwarmConfig       # max_handoffs, max_iterations, timeouts
  ├── runs: RunsConfig         # output_dir
  └── execution: ExecutionConfig  # enabled, command_timeout, post_run_tests
```

### 4.3 Cấu hình từng Model (Per-Model Config Files)

Các tệp trong `config/` (vd, `config/sonnet-4-5.yaml`, `config/nova-pro.yaml`) chỉ ghi đè thuộc tính `models.executor.model_id`. Trình chạy hàng loạt (`run_batch_evaluation.py`) phát hiện chúng tự động thông qua quét `config/*.yaml` và loại bỏ `default.yaml`.

---

## 5. Thiết kế dạng ống theo Từng Giai đoạn (Stage-by-Stage)

### 5.1 Giai đoạn 1: Thực thi (`packages/execution`)

Đây là cốt lõi của framework. Nó sử dụng chức năng điều phối nhiều agent của **Strands SDK** để điều hành quy trình làm việc đầy đủ của AIDLC.

#### Kiến trúc Bầy 2 Agent (Two-Agent Swarm)

```
                    ┌──────────────────────┐
                    │   Bầy Strands        │
                    │                      │
 Prompt mồi      ──►│  ┌────────────────┐  │
                    │  │   Agent        │  │
                    │  │   Thực thi     │◄─┤── chuyển giao ──┐
                    │  │                ├──┤── chuyển giao ──│
                    │  └────────────────┘  │                 │
                    │                      │  ┌────────────▼─┐
                    │                      │  │ Agent Giả lập│
                    │                      │  │ (Simulator)  │
                    │                      │  └──────────────┘
                    └──────────────────────┘
```

**Executor Agent (Agent Thực thi)** — Dẫn dắt quy trình AIDLC trải qua mọi giai đoạn (Inception → Construction). Nó:
- Download luật phân phối của nhánh AIDLC quy tập thông qua `load_rule` (tải Lazy giúp hạn giảm lượng dữ liệu bám lưu vào qua cửa sổ ngữ cảnh).
- Rà đọc / Lệnh chép trên thông thư của túi Run qua dạng khoanh ổ của `read_file`, `write_file`, cùng `list_files`.
- Tiến thông mã chạy bằng Shell ảo (vd cài thông bộ cự Dependency, chạy nẩy script test) qua thông cự của mảng `run_command` tool.
- Hất nhả câu mảng (Hands off) đi tới phễu của Simulator nhắm khi cần thiết định có tiếng nói của User con người (ví dụ: bọc duyệt docs, gỡ đáp hỏi câu, duyệt code...).

**Simulator Agent (Agent Giả lập)** — Nhập vai ông chủ rẽ mảng hòm của người điều hướng hay User thật. Việc của nó:
- Định bụng ôm quy rẽ ở bản cấu `vision` (vision document) cùng mảng (tech-env document nếu truyền) móc vô bên trong túi System prompt.
- Gỡ rẽ cho hỏi đáp, móc mảng duyêt đáp phê nhận các lệnh code hay là duyệt docs tài liệu.
- Phễu trả bật lại vách kết luồn cho ông bạn (Executor) dải tiếp mã AIDLC để làm nốt.

**Các quyết định thiết kế then chốt:**
- **Thao tác tệp bị cô lập (Sandboxed)**: Tất cả các file tools sử dụng hàm an toàn `_resolve_safe()` để chặn đứng việc truy cập ngang thư mục ra ngoài hòm chứa Run.
- **Thực thi lệnh bị cô lập (Sandboxed command)**: Công cụ `run_command` sử dụng một môi trường rất hạn chế (chỉ định các rẽ `PATH`, `HOME`, `LANG`) để kẹp chặt cấu hình cô lập lệnh.
- **Nạp quy tắc lười (Lazy rule loading)**: Các quy tắc được nạp từng cái một khi mỗi giai đoạn bắt đầu, thay vì nhét tất cả cùng một lúc từ đầu vào system prompt.
- **Phát trực tuyến tiến độ (Progress streaming)**: Kênh `AgentProgressHandler` nhả log về việc gọi tools vào luồng stderr nhưng tránh xả ồ ạt các phản hồi khổng lồ do LLM trả. `SwarmProgressHook` lưu mốc thời gian chuyển giao.
- **Thu thập Số liệu (Metrics)**: Trình `MetricsCollector` sẽ móc đi ghi chụp thẻ số Token, vòng đồng hồ khi handoff mảng, các vách nháp báo context lớn chừng bao và ghi lỗi (Error events).

#### Các Giai đoạn Quy trình làm việc AIDLC

Bảng Executor chèo lái vọi chuỗi trình sau đây (có những mác tùy cấu hình có thể ngắt tuỳ theo mức scope):

| # | Giai đoạn (Stage) | Pha (Phase) | Điều kiện? (Conditional) |
|---|-------|-------|-------------|
| 1 | Nhận diện Workspace | Inception | Luôn chạy |
| 2 | Kỹ thuật Đảo ngược | Inception | Chỉ với Brownfield |
| 3 | Phân tích Yêu cầu | Inception | Luôn chạy |
| 4 | User Stories (Câu chuyện User) | Inception | Nếu mô hình phức tạp (complex) |
| 5 | Lập KH Kịch bản (Workflow Planning) | Inception | Luôn nhảy |
| 6 | Đồ hình Thiết kế Ứng dụng | Inception | Nếu luồng móc trích đòi |
| 7 | Bóc tách Code (Units Gen) | Inception | Nếu thấy cần thiết |
| 8 | Thiết kế Mảng Hàm (Functional Design) | Construction | Trích đi nếu đòi |
| 9 | Lập yêu cầu Phi Chức năng (NFR) | Construction | Trích nếu có yêu |
| 10 | Đồ án thiết mảng NFR | Construction | Trích rẽ khi phải cần |
| 11 | Lên kiến trúc hạ tầng (Infra Design) | Construction | Có là nếu thấy cần |
| 12 | Sinh Code (Code Generation) | Construction | Luôn chạy (Always) |
| 13 | Build Code và mảng Test | Construction | Luôn chạy (Always) |

Từng nút sẽ trích đai rẽ bóc đúng file quy báo của nó (vd `inception/requirements-analysis.md`) rập vào hòm túi trước khi múa mảng.  Bản (Executor) ghi đai chép bộc thảy Docs tại thư lưu `aidlc-docs/` còn cục nhả code thì bồi thảy cả đai về túi `workspace/`.

#### Cài đặt Bộ Quy tắc (Rules)

Trình runner chọn kiểu:
- **Git clones** toàn mảng kho Github của AIDLC (chỉ tại gốc: `awslabs/aidlc-workflows`, còn cái ref tag thì theo Config) đập nạp ổ dải File túi vào Run vách của cái túi Run folder rẽ nhả bóc cái túi thư cấu mang cái nội bộ tên là `aidlc-rules/`.
- Hoặc sao chép **(Copies)** mốc đai tại phễu local khi rẽ mảng bóc `rules_source: "local"`.

#### Biểu bố Thiết kế Run Folder

```
runs/<YYYYMMDDTHHMMSS>-<rules_slug>/
  ├── vision.md                      # Lệnh mảng chóp input 
  ├── tech-env.md                    # Dải gán mồi Tech-env nhả (Nếu rẽ đi kèm)
  ├── aidlc-rules/                   # Thư khố lưu dải định hướng của Vòng AIDLC
  │   ├── aws-aidlc-rules/           # Lõi phân nhánh quy tắc làm việc của AIDLC workflow
  │   └── aws-aidlc-rule-details/    # Trích lưu file rules ở cấp độ pha thực hành (.md)
  ├── aidlc-docs/                    # Ngách ổ thu nạp các túi sinh mảng báo cáo tài liệu (docs)
  │   ├── inception/                 # Bao hàm thư báo, mảng móc User stories, Bản nháp Design docs
  │   ├── construction/              # Khuyên cài bộ Functional design, code-gen docs
  │   ├── aidlc-state.md             # File lưu thông tin ghi nhớ trạng thái rẽ của AI
  │   └── audit.md                   # Nạp bản in ghi biên mục các hoạt động trong workflow 
  ├── workspace/                     # Mảnh đất túi cho mảng Code nhả cấu thành App thực
  └── run-meta.yaml                  # Chân lý meta lưu hệ bộ bọc cấu hình lúc chạy
```

#### Đánh giá Kiểm thử Hậu kỳ (Post-Run Test)

Sau cữ dải máy Agent ngừng đi nhịp bọc (Swarm Completes), mảng móc nhịp của phễu bọc rẽ `post_run.py` kéo ngầm chốt kiểm tự đánh:
1. **Tìm vết lưu mảng Project**: Mũi sục mỏ dò dạng (BFS scan) lên toàn không bọc `workspace/` móc dò cự điểm mác Marker đi khai sinh của 1 project (vd nhả:  `pyproject.toml`, `package.json`, `Cargo.toml`, hay `go.mod`) sục mức giới cấp 3 
2. **Cập bồi máy móc Dependency**: Kéo file lệnh chạy Install setup thích trích ngôn ngữ (ví nhả ổ `uv pip install -e ".[dev]"`)
3. **Phóng lệnh Test (Test execution)**: Dùng vòi test cho đúng ổ Code đã sinh (ví rẽ ở cấu gọi ngầm mảng `uv run pytest`)
4. **Giải mã rẽ trả (Output parsing)**: Luồng phân định đo bọc móc ngôn phễu parse cho tính thông Parse điểm rẽ xả đai của cấu test pass/fail trút gán dạng Language-specific (ngôn ngữ Python hay cự Test).
5. **Giỏ hứng bọc (Results)**: Cất đai nạp điểm túi test đo rập kết vách tại ổ File:  `test-results.yaml`.

### 5.2 Giai đoạn 2: Bài Test Hậu (Tổng quan / Post-Run Tests)

Giai đai này bọc cài kéo thông mở đai của ổ thư cấu file (test-results.yaml) của vách cài bọc Stage 1, và trích ngỏ đọc cấu chữ ngạch cho User (human-readable summary). Điểm bọc nhả tát vào cài Stage chóp ở Executor — do luồng điều máy đọc bọc thư cho cấu summary nhả phễu ra. 

### 5.3 Mốc đâm đánh lượng đo Số 3 ngầm Static Code (Quantitative)

Xả vòng test trích Static qua luồng file bọc chứa kết sinh mảng rẽ mốc đâm thư : `workspace/`. Khóa điểm cài bằng ổ báo bộ rã quét bám nạp đi thiết gán chóp biết ngôn.

#### Bảng cấu bộ bứt đai chóp (Tool Selection by Project Type)

| Thể Loại cài hệ Project | Màng Đỡ báo lỗi Linter | Vách Test hệ báo an Security Scanner | Lọc nảy đai chạy Copy gán bóp nhái (Duplication) |
|-------------|--------|-----------------|-------------|
| Túi Python | ruff | Bằng nhịp bandit thả cự nhả semgrep | Đai nảy vách của PMD CPD |
| Túi file Node.js | eslint | Gài của rẽ móc bọc mạng npm audit thả semgrep | Cài vách đo chóp điểm báo PMD CPD  |

#### Tuốt cài Ống xả (Analysis Flow)

```
Lệnh trích vòng báo rẽ (scan_workspace(path))
  ├── Dò hệ mảng Project (nhả túi pyproject.toml → rẽ báo Python, rẽ ổ có package.json → trích vòng cho đi móc Node)
  ├── Nạp nhịp run_ruff() bọc nếu chóp đi rẽ bộ vòng run_eslint()         → Bọc màng rẽ thư thông: LintFinding[]
  ├── Túi đâm điểm tính băng rẽ run_bandit() chạy rã trút bọc npm (run_npm_audit())    → Mảng security: SecurityFinding[]
  ├── Vục chạy hệ vòng dải bọc hàm: run_semgrep()                       → Đai điểm lưu cấu rẽ vách: SecurityFinding[]
  ├── Thiết vách cự bọc phễu: run_cpd()                           → Đo bọng dải DuplicationFinding[]
  └── Ghì tổng tút đo qua `compute_summary()`                   → Bọc thư cài rẽ báo QualityReport
```

Mỗi mỏ nháp mảng báo chạy rẽ của tool runner :
1. Check rà cự rẽ cài tool nếu sờ thấy cài cấu chạy rã (`shutil.which` bọc phễu nạp 1 ngỏ của đi tút qua móc test bảng nẹp `uv run --version`)
2. Nảy chạy cự dải test báo nạp đai định JSON output layout 
3. Túi rẽ nẻ cấu trích thành cấu mốc nảy bằng bộ mỏ Model có quy luồng (định mác ngõ báo File Findings).
4. Phễu nhả rã trích bằng túi phễu gọi kết File (ToolResult) đi đo đai gán bọc thông kèm cái Metada mốc phễu.

**Khóa nhẹ bọc trượt mảng (Graceful degradation)**: Có tút file bị gỡ rơi vãi đi chạy không cự dải do chẳng trích Cài Đặt. Nó nhả đập bọc đo file hãm túi lại kèm nhắc nhỏ — Không bao nảy nhả đập ngắt bể hỏng bọc dải ở vách của Pipeline (never fails the evaluation).

Nhả nắn túi File ổ nạm tên: `quality-report.yaml`.

### 5.4 Ống thứ 4: Contract Tests (Khảo thí API)

Trình nãy cài chóp ứng chạy nảy App của API và bục mỏ cài đo chạy thử Test mảng gọi xả điểm nhả đục qua mảng spec ở thư ổ thông chuẩn hóa (OpenAPI 3.x). 

#### Bảng thông cấu nhả mác (Architecture)

```
Mỏ file lưu vách hòm mạng (openapi.yaml) ──► spec.py (Cấu parse mã đọc rẽ) ──► Nảy bộ đai rẽ của ContractSpec
                                          ├── Mảng app trích cấu báo AppConfig (module, port, framework)
                                          └── Trích Test Cấu thư  (TestCase[]) từ file dải báo x-test-cases
 
Ổ thư có bọc cấu workspace/ ──► Bộ Server mồi máy (server.py) nạp trích ServerProcess ──► Vách rẽ máy ngầm nảy ổ uvicorn đi tút nhịp 
                                                  │
                                                  ▼
Ống đẩy báo cấu ContractSpec ──► bục xả đo mảng `runner.py` ──► xả rọi điểm bắn HTTP requests ──► Trích nảy ổ CaseResult[]
                                                  │
                                                  ▼
                                          Bọc báo gộp: ContractTestResults
```

**Bọc kìm cựa đo trích:** 
- **Parser dải cự bục Spec:** Mỏ cài đai OpenAPI báo sử dải Extension ở nội bọc mốc gán rẽ (Custom nảy túi extensions): Mốc đâm ổ thư (`x-app` - cho cấu trạm báo Server) và  `x-test-cases` ( Cho báo ổ test đi từng đầu test chóp endpoint: input cài gì nhả xuất ra sao vạch expected outputs).
- **Trạm chóp móc máy quản mảng bọc (Server management)**: Móc nảy `ServerProcess` dán rẽ tự đâm ngỏ túi ổ hòm ảo Virtual Env (venv) báo chóp ở cái rẽ project trích workspace, nổ súng bộ đai máy ngắt ngầm `uvicorn`, nảy đai lệnh pings nhịp trút chóp xả API cho mảng (poll) mạng cổng rẽ `/health` tới đợi chừng mỏ rẽ nó sẵn Ready để nhận test trút đai, xong chóp test thì thu dao báo tắt gọn máy gán chóp cự shut down. 
- **Chạy vạch (Test execution)**: Dải vách case Test đai sẽ cự chạy Test bắn đạn HTTP tút rẽ đo cháp với kết đục cài đo mạng đính đai Status nhả hòm khớp khít, thân nhả ngạch File (Response Body body) dải cự gán đúng điểm khóa thông có bộ (keys/values cặp) có móc nảy cấu nhả điểm ngầm theo đai so (Deep match test đo tính tính luôn nảy độ vênh đai thả nổi cho cự số thập ở hệ phễu báo floating-point ).
- **Nốt rẽ đi khóa đứt bọc hủy gán** : Nổ hãm ngạch đai nhả Test kết của nhịp sớm đi do chóp vách cự Server ngỏ dạt chết rẽ bục đứt, hay bị gián sập trượt cài của cổng nối Connection gán cự (3 cú mỏ chạy luồn lỗi Error điểm nảy connection cự errors xả đứt).

Bọc đụng nhả cài đai ở bíp ổ : `contract-test-results.yaml`.

### 5.5 Luồng đục Tủy đai đo điểm đo Mô Ngữ (Cột đo 5 Chất rẽ đánh) Qualitative (`packages/qualitative`)

Cắm đai chóp đối cài chập đo các mốc ổ tài thư đai AIDLC đo cự đối báo với bọc cài chuẩn dải mỏ (Golden baseline cựa mảng) để đánh nhịp ở ngả vạch Semantic Semantic đo mốc giải nghĩa tính Semantic.

#### Định mảng đối chóp nạp mốc Test file (Matching Matching)

```
Mảng rẽ gác mộc đính mốc dải golden aidlc-docs/         Mảng file cự ổ trích của ổ candidate aidlc-docs/
  băng bọc nảy inception/                 rẽ nạp cự inception/
    file cự requirements.md    ◄──►   tệp báo requirements.md         (bọc nhả đánh paired đối khớp)
    nhả ổ user-stories.md    ◄──►        (Test) mốc user-stories.md         (Được bục cự paired)
  nhả vách construction/              khóa thư ổ construction/
    xả trích đai code-generation.md ◄──►   code-generation.md      (Dính cút paired)
                               đục nhả ngỏ túi rẽ extra-doc.md             (Dư mả bọc bỏ dính rẽ ổ cự — unmatched candidate)
```

Nạp đai các báo chóp cài được quặp ngầm bởi cấu đường dẫn đâm nháp relative. Mốc cài ổ nội dải File (Như mỏ thư `aidlc-state.md` cùng ổ `audit.md`) đều hãm gán đánh trơn vứt tút đai chóp đi ngoại lai ngỏ gộp trừ gán đo.

#### Đai chóp nhả đi các vạch tính điểm đánh Measuring 

Cỗi mảng đo chéo gán file đánh vạch cấu cài ngỏ thư ổ cho mác đai điểm đi tỷ ở ghim hệ phân: (0.0 tới mốc 1.0 trần điểm đai): 

| Báo chóp chiều đo| Hãm cự rẽ tính Weight | Ý báo ở nó nạp đo đâm vạch gì? |
|-----------|--------|-----------------|
| Ý Bủa Nghĩa Hợp (Intent) | 0.4 | Đính y chóp hướng vách, cái Yêu cầu và Mốc ngỏ ý đồ chung của (purpose) |
| Căn cựa Thiết Kiến Hợp | 0.4 | Bám tút kiến cài đo, ổ đâm mảng bộ máy báo chóp vách patterns  |
| Đai rẽ đánh độ Toàn diện Bọc Completeness| 0.2 | Dải ổ đo nảy đai ứng cự bám nảy phễu nảy các mục lục Reference Topics điểm ngỏ vào trích | 

**Mối bọc xả kết đo thư (Overall) báo tệp 1 văn đai** = bọc ổ 0.4 × điểm mảng intent  + trích nhả ổ đi 0.4 × đo vách cự của nhánh design  + vách ổ cài 0.2 của phần xả ở của đo completeness . 

Điểm nhả nạp mảng rẽ xả kết nạp cấu ngỏ (gộp bọc Phase cự trích (cho mảng inception xả, tới cấu trích vách construction gán)) đi bục vòng tổng nảy ở một ngạch đo (cột vạch Overall). 

#### Bục Hai Lệnh vách Tít Hệ Đánh Điểm Chấm Số cự Scorer

Trình bóc cự tên là offline Offline **HeuristicScorer (Mảng Điểm bục Deterministic không do mốc LLM bắt test nạp)**:
- Hướng Cắm rẽ bọc Intent: Đo móc nhả ở hệ `Cosine Similarity tf-idf` vạch đi mỏ gạt dải ổ Stopword cự rẽ. 
- Ngạch báo đính rẽ móc Design: Nhả điểm chóp Weighted đai cài Jaccard similarity nhả (0.6 vạch cho báo cái kỹ tự gán) đánh kết 0.4 ở cấu nảy nháp bục thư mục đo chữ nhả (Heading structure hòm rẽ).
- Túi Completeness đính cự tính của : Trút cựa tính ở phần tỉ dải bọc đầu gán đai Heading Reference có tồn dải vách của candidate xả tút hay ko cự.

Kiểm gọi LLM mỏ chóp mảng (Default của máy xả là : **LlmScorer** / buột phễu Bedrock) 
- Phái vọt màng cặp file cài cấu vào vách bọc nhả tệp đằng LLM gài nhờ hàm cự API gọi trả của mảng Bedrock `converse API cự API`
- Prompt luồn nhả bắt phễu nó nhả xả ổ tệp nén vạch hòm trả đai kết json JSON xả chóp đo ba thông số dải đai của cột Scoring cho điểm Dimension cùng nạp dòng note chú trút Notes .
- Buộc gài độ phân nảy rẽ (Temperature là chốt móc cứng 0.0) ngỏ ý điểm test đo chốt đi chóp 2 lần phễu gán cự nó rập một xả đánh ngỏ đồng màu Reproducibility Reproducibility nảy đo.
- Mảng nảy cự cữ điểm bị bục vách nháp điểm nảy cắm ngắt thông bị nhả thư bọc Limit Limit là độ 15 Ngàn Chữ cự đo file Document (15K characters characters đai cự).

Kết của nạp ở: file thông đai `qualitative-comparison.yaml` cự vách.

### 5.6 Giai Rập Nhả Lập Báo Test Đo Thông Report (`packages/reporting`) 

Rẽ nải file chóp thu cất đâm nạp tạo đai của túi Test bằng ổ tập hòm nén tổng báo file ở Run thư run Folder rẽ.

(Phần sau sẽ được tự động xử lý tiếp...)
#### Thu thập Dữ liệu (Data Collection)

Hệ thống `reporting.collector.collect(run_folder)` đọc tất cả các tệp YAML và tập hợp chúng vào một dataclass `ReportData` chứa:
- `RunMeta` — Thông tin định danh, thời gian, cài đặt model, và thiết lập của rules
- `RunMetrics` — Thông số tiêu hao token (tổng + từng agent), thời gian chay, cấu trúc thời gian chuyển giao handoff, số lượng số tệp artifact, đếm lỗi bọc và trạng thái ngữ cảnh (context size) 
- `TestResults` — Thông tin pass/fail/tổng cộng của unit test cùng tỷ lệ % Pass
- `QualityReport` — Số liệu đánh dấu linting, quét mã bảo mật và sao chép.
- `ContractResults` — Điểm thử nghiệm API theo định dạng endpoint 
- `QualitativeResults` — Điểm số nghĩa theo ngữ nghĩa ngữ liệu theo pha và từng tài liệu điểm. 

#### So sánh với Mốc Vàng (Baseline Comparison)

Nếu một tệp baseline `golden.yaml` được tìm thấy (tự động móc từ cùng cấp thư mục với tùy chọn bằng cờ `--golden`), báo cáo sẽ tự bọc tính hồi quy:
1. `extract_baseline()` sẽ chia chóp `ReportData` bung cự của thành một mảng cài định `BaselineMetrics` nạp với rẽ trên tầm ~30 tệp trường dữ đai dữ số phân số của phễu.
2. Bộ cài `compare()` sẽ nhẩm toán nhả delta báo và chấm dải ngạch gán cho rẽ mốc trích mỏ: cài nảy tốt vách (improved) / cự mảng tệ đi rã nháp (regressed) / cùng gán móc giữ chóp điểm ngỏ bằng giữ chóp rẽ (unchanged). 
3. Lệnh trích nhả cự nảy đánh đi đúng chiều ngạch của phễu bọc (ví rẽ màng giảm vách lints thì gán thông cấu là improved, hay túi xả nháp chạy báo có Pass của tỷ % Unit test dâng to thì đính vách mảng là cải luồng improved).

#### Định dạng Đầu ra (Output Formats)

- **Markdown**: Phễu cự `render_markdown()` nảy đai bọc của sinh cấu phễu Markdown ở định móc GitHub kèm ngả rẽ các bảng điểm nảy vách báo, mảng hộp tệp ngỏ Table, dải delta chỉ cấu báo trút của cài cùng đai mở phễu chi xả nháp nhả Detail gập cuộn vạch (collapsible detail sections). 
- **HTML**: Vách Ổ `render_html()` gài bục phễu trích bọc nhả cái file của vách Markdown nhả kia cuộn cùng rẽ nháp điểm CSS gán cự styling để mở chạy 1 rẽ cài File xem Browser đo. 

---

## 6. Trình điều phối (Orchestrators)

### 6.1 Đường ống Single-Model (`run_evaluation.py`)

Điểm chạy cài xuất vách vòi chính (main). Rẽ gọi vòng văng trút ngỏ ổ chạy của luồng bằng 6 bước móc tiếp luồn (sequentially):

```
Phân nảy CLI trích đai 
  │
  ├── Chạp chế độ cài rã --test mode ──► móc ngỏ phễu gán pytest chạy cài rẽ trên tút packages ──► Vọng điểm vạch nhả qua nỏ exit
  │
  ├── Rẽ qua `--evaluate-only` (chỉ chấm rẽ qualitative đo) ──► lùi bục nhả vách chóp cự Stage 1 đo 
  │     ├── Đâm nhả mạng vách Stage 3 (vạch tĩnh test - quantitative)
  │     ├── Ngỏ đai phễu vách Stage 4 (báo kết api qua ngỏ - contract)
  │     ├── Đo trích nảy Stage 5 (Đai nạp đo chất báo chữ nhả cự - qualitative)
  │     └── Ngỏ móc Stage 6 (Nhả chốt report báo rẽ)
  │
  └── Cài trút điểm vạch vòng phễu mảng ngạch qua chạy full cài Pipeline 
        ├── Bạt chóp nhánh Stage 1 (Triển khơi - execution) ──► Nhỏ 1 phễu nạp ổ thư Run có ngỏ báo mảng thời nhả cự timestamped.
        ├── Lưu trữ cài rã đai mảng Evaluation config nhả cùng gán cự Repo ngạch info info 
        ├── Kích đai nạp của rẽ mảng Stage 2 (Nháp trút nhả file mảng cự phễu bọc `test-results.yaml` của đai nảy đi ở Stage 1).
        ├── Điểm nảy mốc gọi luồn Stage 3 (Điểm lượng quét Quantitative) 
        ├── Chốt cắm mốc nhả Stage 4 (Thiết bập luồn nếu rẽ ổ túi đai --openapi xả spec cấp)
        ├── Móc kết phễu luồn rã Stage 5 (Định đo xả qualitative)
        ├── Vạch xả đâm rã Stage 6 (Báo giải Report)
        └── Rẽ bọc Summary tóm trích cự, nhả ngỏ cự exit 0 thoát mảng rẽ cài báo lọt đậu trút cự pass.
```

**Khả năng tự bọc gỡ của nhả sụp (Resilience):** Tại mảng trích cài nhả đi Strands swarm đứt nhả nảy của file ổ bằng phễu mã Non-Zero đứt nhả mà các tệp đai cựa Docs AIDLC rẽ vách có nảy nháp được lưu, thì lệnh của phễu vòng nảy Evaluate đánh đo vẫn nhảy bước làm (trường nảy này đo cự ở cấu rẽ khi chóp agents Swarm chết chớp đoạn chóp luồng sát của ngỏ handoff lúc rã Docs của tệp được xuất ghi nảy cấu thành xong). 

### 6.2 Cấu Test Đai Mảng Cự Lô Máy (Batch Evaluation) qua file `run_batch_evaluation.py`

Khởi kích bật rẽ chạy bấu lặp ngỏ trích của `run_evaluation.py` xuyên đo lặp vách for loop mảng từng bấu cự ở túi Config đi bằng file ổ Model:

```
Nhắp điểm rọi (`discover_models()`)     ← Rà vét ổ mỏ lưu của cấu `config/*.yaml`, văng trích luồn loại bỏ ổ `default.yaml` 
  │
  Vòng quặp for each model cự đai đánh:
  │  ├── Cài vòng mã rẽ chạy qua phễu ngỏ cự `--executor-model` trích đai thay thông (override). 
  │  ├── Nẩy gọi bọc trích subprocess xả đai, kìm dòng chóp `stdout/stderr` xả cự vào báo File log. 
  │  ├── Nhặt tìm trích bọc vách của phễu Run ổ rẽ Run cựa lúc timestamped nảy.
  │  ├── Cấu tên ngỏ báo của thư ổ Rename: <timestamp>-<slug>-<model-name>
  │  └── Ghi mỏ báo sổ nhật trích của mảng batch batch-summary.yaml qua tút 
  │
  Nạp file mảng xả batch-summary.yaml trút cùng rẽ của thời đánh cự nảy (timing timing) lẫn trích lọt trượt nảy của các Models gộp pass/fail. 
```

Đứt nảy rẽ cài trút là một vòng cự chạy gộp isolated isolation ổ đai rã bọc — đai chạy vạch subprocess nháp xả báo tệp thư rẽ hòm Run xả cự Folder cài độc cho mảng vạch đo đai riêng rẽ.

### 6.3 Tổ Khớp Báo Cáo Chéo Mảnh Comparison (`run_comparison_report.py`)

Thúc xuất nảy đai bằng bục mảng của rã chéo nhả đánh cài đai báo Matrix sau chạy Batch bục lô lô:

```
Nhắp vòi kéo (`find_model_runs()`)     ← Đai vạch trích ổ nhả gán qua tên đuôi đục rẽ bọc name suffix suffix.
  │
  Vòng lặp rã đo each model đai nảy :
  │  └── Gom nhả `collect()` + xả bọc baseline nảy đo mốc `extract_baseline()` → Phễu gọi BaselineMetrics
  │
  Nạp rẽ chóp ổ túi đo golden mốc (chóp file `golden.yaml`)
  │
  Vạch cài `generate_comparison_markdown()`   → Phễu comparison-report.md
  Gắn mảng rã file `generate_comparison_yaml()`       → Túi comparison-data.yaml
```

Bảng Table rã điểm (comparison table) phễu cự đai cháp khoảng bao gộp 30 mốc điểm chíp số bọc rẽ (metrics rã mảng như vách nhả của Unit Tests, đai đánh rã API bằng Contract, luồng đánh xé đo Code tĩnh gán, với điểm bục đai cấu đo mác Qualitative qua chữ tính nảy gán, nạp phễu túi xả rẽ cài lượng file tệp Artifact gán, cháp ổ chạy tiêu xu cost, bọc gán kích vòng bục ngữ đo chữ Context size nhả context size) — Đi cài móc kẹp bám nhả độ Delta đo cự vách (nhả mũi găm lên rã `^` cài vách better, cự vách `v` cắm điểm worse) xả đo so luồn tham cấu Vàng Baseline (Golden Baseline). 

### 6.4 Mốc Cày rẽ đục Giải IDE Đánh móc Evaluation (`run_ide_evaluation.py`) 

Nọc đi vòng chạy trích cấu quy chuẩn rẽ bọc bục của quy AIDLC (AIDLC qua đai của nhóm 3rd-party vách báo) chóp IDE AI assistants xả đai cựa qua cái Móc Harness IDE (`packages/ide-harness IDE-harness`):

```
Đai kéo `get_adapter(name)`     ← (lazy mảng import trích cài từ cấu gán rẽ dải registry registry nảy đâm máy)
  │
  ├── Kháo cựa kiểm đai bộ mảng tiên nhả `check_prerequisites()`
  ├── Kích vách cựa đai chạy `adapter.run(config)` ──► Đai ngầm nhả chạy qua móc tự thiết IDE-specific automation 
  ├── Định cấu chuẩn mạc `normalize_output()`  ──► Xả gộp nạp thư ổ của form chuẩn vách standard run folder folder layout.
  └── Đánh phễu của cài đai mảng nảy `run_evaluation.py --evaluate-only`  ──► Rẽ vách Stage đo 2-6 (stages stages 2-6). 
```

**Màng vách nẻ rẽ Mẫu theo ngỏ Adapter (Adapter pattern)**: Ổ cự trích của hệ vách IDE đi nảy bọc theo nhóm mạc cài lớp cài lớp Ảo Abstract của cấu (`IDEAdapter ` mảng vách nảy) trút với ba phễu:
- `check_prerequisites()` — Gán mạc thiết dò sự nạp IDE bọc của mảng installed (cài cài installed rã configured configured phễu đai).
- `run(config)` — Tự mở gán đi vòng cấu rẽ màng bằng đai nảy chạy AIDLC vô trút ngỏ ổ của nảy bục IDE IDE.
- `name` — Nhãn phễu cài nảy tên gán vách báo ngỏ (Human-readable báo bục identifier identifier). 

**Chuẩn Mạc rẽ Báo nạp xả trút (Output Normalization):** Phễu nhả cài móc báo vách rẽ cấu mạc của (normalizer.py xả cài `normalizer.py normalizer.py`) nạp vách móc báo cháp trút cấu nảy đai mảng ổ thư thư định dải rã bọc rẽ cháp đục (IDE-specific cụ thể output output layouts) gán chuyển đai cho về kiểu vách chuẩn File theo đai hệ trút cự đai test (standard run folder ổ rã ổ folder expected by evaluation test), gán xé cài phễu nhả luồn móc giả File hệ nhả cho (synthetic run-meta.yaml cự rã run-metrics.yaml rẽ). 

Adapter đi bọc phễu gán ổ hỗ: Cursor, Cline, Copilot, Kiro, Windsurf, Antigravity.

---

## 7. Mảng Cấu Dòng Cài Data Cự Giải Artifact Graph (Dạng File YAML) 

Từng vách rẽ nảy đai cài móc bục (Stage Stage) đi nảy bám kết vòng gán xả yaml YAML cài YAML YAML xả ổ Run ổ bọc Run Folder Folder. Chẳng móc vách in-memory gán cự cài phễu state bọc đai gán phễu state rẽ qua vách ngỏ Stage Stage mảng. 

```
Bục nhả cấu Stage 1 (Triển khơi - execution trích đai) 
  ├── xả nảy Writes bọc: run-meta.yaml rẽ cài run-meta, run-metrics.yaml, test-results.yaml.
  ├── mảng nhả ổ Writes bọc: aidlc-docs/**/*.md nhả đai, workspace/**/* trút cựa rẽ đai.
  │
Vách nạp ổ nhả đánh Quantitative ngỏ Cỡ Stage 3 (Cấu mảng xả rẽ): Nhả file cài cựa ở Reads trút rã từ workspace/ 
  └── Gán nảy xả file: quality-report.yaml rã
  │
Dải cựa ổ bục Stage 4 (Đo Contract trích đai): Cấu rẽ móc lấy bục của ở Thư ổ workspace/ rẽ ổ, cài rẽ openapi.yaml nạp (vào test input cài rẽ đai).
  └── Nhả trả phễu rẽ ổ: contract-test-results.yaml xá 
  │
Đo ở bục nảy Stage 5 (Đai ngỏ Qualitative nhả đai): Phễu cựa móc đai rẽ từ aidlc-docs/ rẽ ổ xả cựa, cùng cài thư vách bọc chuẩn Đối vách Reference golden-aidlc-docs/ đai cài (đội input).
  └── Thả tút cởi rẽ File đo trả ổ xả: qualitative-comparison.yaml nạp.
  │
Gài phễu đo cho móc rẽ report bọc đai Stage 6 (Nhả báo Report bục): Lấy rẽ nhả bục cựa toàn Ổ thư vách bọc của các túi tệp yaml YAML cài (ALL phễu của YAML YAML files rã đai + lấy cài thư golden.yaml golden).
  └── Kết rẽ phễu cài nhả mốc rã tệp: report.md nhả báo rẽ, report.html cài báo.
```

Bảng chóp gán Orchestrator báo này tự trích đai ổ vách ghi mảng cấu (evaluation-config.yaml rã phễu snapshot đai xả gán cài trích) đai nhả cùng cựa ghi nhả trút của tệp (run-meta.yaml cài) cập cài đính mảng ghi bọc cho mác trường đo level test đo mốc đánh ngỏ gán (evaluation-level evaluation-level fields fields).

---

## 8. Các cấu bục mốc của Model bọc nhả Data (Key Data Models)

### 8.1 Kho đệm trích cấu Mạng Metrics thi hành rẽ (run-metrics.yaml)

```yaml
tokens:
  total: {input_tokens, output_tokens, total_tokens, cache_read_tokens, cache_write_tokens}
  per_agent:
    executor: {input_tokens, output_tokens, total_tokens}
    simulator: {input_tokens, output_tokens, total_tokens}
timing:
  total_wall_clock_ms: int
  handoffs: [{handoff: int, node_id: str, duration_ms: int}, ...]
handoff_patterns:
  total_handoffs: int
  sequence: [str, ...]
  per_agent: {agent: {turn_count, total_duration_ms, avg_turn_duration_ms}}
artifacts:
  workspace: {source_files, test_files, config_files, total_files, total_lines_of_code}
  aidlc_docs: {inception_files, construction_files, total_files}
errors:
  throttle_events, timeout_events, failed_tool_calls, model_error_events, ...
context_size:
  total: {min_tokens, max_tokens, avg_tokens, median_tokens, sample_count}
  per_agent: {executor: {...}, simulator: {...}}
```

### 8.2 Biểu đai chóp chấm Qualitative (Định lượng Ngữ nghĩa) (`qualitative-comparison.yaml`) 

```yaml
overall_score: float  # Xả chóp tỉ 0.0 - 1.0 (Số cao là dải đạt) 
phases:
  - phase: inception
    avg_intent: float
    avg_design: float
    avg_completeness: float
    avg_overall: float
    documents:
      - path: inception/requirements.md
        intent_similarity: float
        design_similarity: float
        completeness: float
        overall: float
        notes: str
```

### 8.3 Kích chóp thông bọc mốc Vàng Báo rẽ Golden (`golden.yaml`)

Tệp đánh giá numeric thu nhỏ với ~30 metrics chính xác được thiết kế để dễ đánh giá độ hồi quy regresssion của pipeline khi chạy bằng các bản models mới.

---

## 9. Công Cụ Tích hợp (Tool Integration)

### 9.1 Bẩy móc nhả đai Strands SDK (Agents) 

Gói thực thi sử dụng [Strands Agents SDK](https://github.com/strands-agents/sdk-python) cho:
- `Agent` — gói gọn mô hình Bedrock kèm system prompt và dải tools
- `Swarm` — điều phối các pha handoff giữa các agents, định mức chặn `max handoffs`, `max iterations`, `execution timeout`, `node timeout`
- `@tool decorator` — khai báo hàm Python thành tool cho agent gọi 
- `BedrockModel` — model bọc bởi provider Bedrock.
- Các hàm Hook — như `BeforeNodeCallEvent` và `AfterNodeCallEvent`

### 9.2 Amazon Bedrock

Sử dụng LLM qua API Boto3 Amazon Bedrock.
- Read timeout: 900s tại luồng Execution, 300s với mô hình Scorer
- Connect timeout: 30s
- Retry policy: 10 attempts (chế độ adaptive)
- Models: Cấu hình tuỳ vào Executor/ Simulator/ Scorer.

### 9.3 Điểm bọc đai Tool phân xả Code tĩnh Tĩnh (Static Analysis) 

| Cụ báo Tool đi bọc| Trích chức điểm bọc| Giọt nhả Format| Đệm rẽ nhả lỗi nhẹ |
|------|---------|--------------|---------------------|
| Khóa báo ruff | Python đo nhíp điểm xả đi linting lỗi dải | JSON | Xả qua mảng nếu trượt (skipped) trên đai PATH |
| Bộ bập bandit | Mảng quét bảo mật (security) | JSON | Bỏ qua nếu ko tìm thấy |
| Chốt cài rã semgrep | Rà theo bảo mạng trích mạng (security cự Đa mã — multi-lang)| JSON | Bỏ qua nếu không có module |
| Cấu bọc đai eslint | Đo JS/TS | JSON | Gọi fallback `npx` nếu ko có |
| Túi trích npm audit | NPM audit cự kiểm xả bảo mật thư viện cài | JSON | Yêu cầu `package-lock.json` |
| Máy nạp PMD đi bọc cắm CPD | Bắt phát trùng lặp (duplication) | XML | Gọi theo PATH hoặc cài đặt YAML |

---

## 10. Mô hình An ninh (Security Model)

### 10.1 Hộp cát file (File Sandboxing)

Thủng hết rẽ mọi hoạt cấu nhả cài trút đọc/ghi ổ tệp qua Agent tự đâm qua bằng Sandbox mở xả thiết vào (run folder):
- Luồn hàm `_resolve_safe(run_folder, relative_path)` đi xét an toàn qua ranh giới folder, không cho truy xuất cấu hình File ngoài.
- Trả Exception `ValueError` nếu dính Path Traversal (VD gọi qua `../../pwd`)

### 10.2 Command Sandboxing (Hộp giới Hạn shell) 

Công cụ `run_command` cung cấp môi trường chạy shell ngặt nghèo:
- Khóa toàn mảng `PATH`, định `HOME` trỏ về Run folder, và cung cấp `LANG`, `TERM`. Không nạp root dir. 
- Giới hạn Timeout 120s mặc định.
- Output tự giới hạn cắt ở 50K text.

### 10.3 Khóa Máy API test cục Virtual Env (Server Isolation)

Quá trình test mảng chạy Contract Tests sẽ bịt trong Mảng venv cách ly cho dự án.

---

## 11. Các Test Cases Mẫu (Test Cases) 

Xả cất kho cài tại `test_cases/` đai cự rẽ cấu gập mành rã bóp chuẩn:

```
test_cases/<case-name>/
  ├── vision.md              # Nhõng điểm trút rẽ Vision
  ├── tech-env.md            # Các trích báo dải đi qua Technical Requirements 
  ├── openapi.yaml           # Điểm nhả giao API có extension 'x-test-cases'
  ├── golden-aidlc-docs/     # Ổ ván mảng rẽ cài bục dải Reference
  │   ├── inception/
  │   └── construction/
  └── golden.yaml            # Numeric Baseline trích lưu điểm vàng.
```

Trích mặc định đang là `sci-calc` (scientific calculator API). Tất cả hệ thống luồng ưu tiên chọn sci-calc chạy nếu không báo biến tuỳ chỉnh.

---

## 12. Điểm Mở Dành Mở Rộng Extension (Extension Points)

### Adding a New Model (Gắn 1 module LLM API)

1. Mở file `config/<model-name>.yaml` trỏ `models.executor.model_id` đến Bedrock ID .
2. Runner trọn Batch sẽ gom vô và đọc.

### Adding a New IDE Adapter

1. Tại `packages/ide-harness/src/ide_harness/adapters/<name>.py`. 
2. Triển khai Class Abstract `IDEAdapter` (cấp 3 method báo: `name`, `check_prerequisites`, và `run`). 
3. Tham mưu vào Registry tại File: `_ADAPTER_MAP` trong `packages/ide-harness/src/ide_harness/registry.py ` 

### Adding Static Tool (Đính công cụ đo Static analysis)

1. Gắn hàm xử tại ổ : `packages/quantitative/src/quantitative/analyzers.py ` (bắt trước `run_ruff`).
2. Gài đánh móc mảng rã ở tệp báo Model nếu gặp kiểu Data khác tại ổ `models.py`.
3. Gọi Hàm ở ổ chạy `scanner.py` ngầm dựa trên Mảng Project Type.

### Gắn 1 Bài Golden Test mới

1. Đặt Directory ở nhánh: `test_cases/<case-name>/ `. 
2. Đính file `vision.md`, `tech-env.md`, `openapi.yaml`.
3. Khởi hỏa chạy Pipeline bọc `1 Lần` (run Once) cho thu hoạch Baseline.
4. Chạy tool `reporting.baseline.promote()` cập vào thư để sinh báo `golden.yaml`. 
5. Bưng thư mục Docs lưu thành `golden-aidlc-docs/ `.

---

## 13. Khóa Danh sách Dependency (Dependency Stack) 

| Component| Phương tiện vách (Technology) |
|-----------|-----------|
| Gắn mã Ngôn Ngữ  | Python 3.13+ |
| Chương Trình Quản PKG (Pkg Mgr)| UV (nhánh cấu workspace) |
| Chỉ báo Agent Orchestration  | Strands Agents SDK  |
| Model Provider  | Amazon Bedrock (boto3) |
| API Rest Client | httpx (Contract tests) |
| ASGI Server | uvicorn (Contract Test) |
| Lõi Unit Testing Framework | pytest |
| Vòng lặp báo Serialization Data | PyYAML |
| Kho quét dạt Lint | ruff |
| File nạp báo quét bục an rã | bandit, semgrep |
| Quét Lặp Mã (Duplicate)| PMD CPD (External)|
