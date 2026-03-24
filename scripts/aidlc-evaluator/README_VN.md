# Khung Đánh giá & Báo cáo Quy trình làm việc AI-DLC

Khung kiểm thử và báo cáo tự động để xác thực các thay đổi đối với kho lưu trữ [quy trình làm việc AI-DLC](https://github.com/awslabs/aidlc-workflows).

## Tổng quan

Framework này được tổ chức xung quanh sáu luồng công việc chính ("big rocks"):

1. **Golden Test Case (Test case Vàng)** — Các test case cơ sở được giám sát cẩn thận (tài liệu AIDLC + mã nguồn đầu ra) được sử dụng làm đầu vào tham chiếu cho tất cả các đánh giá
2. **Execution Framework (Framework Thực thi)** — Lõi điều phối chạy các test case qua quy trình đánh giá
3. **Semantic Evaluation (Đánh giá Ngữ nghĩa)** — Đánh giá dựa trên AI về tính chính xác, tính đầy đủ và tính phù hợp của đầu ra (được báo cáo @k để tính đến sự không xác định)
4. **Code Evaluation (Đánh giá Mã nguồn)** — Phân tích tĩnh mã nguồn được tạo (kiểm tra lỗi, quét bảo mật, phát hiện sao chép)
5. **NFR Evaluation (Đánh giá NFR)** — Kiểm tra các yêu cầu phi chức năng (tiêu thụ token, thời gian thực thi, tính nhất quán qua các mô hình)
6. **GitHub CI/CD Integration & Management (Quản lý và Tích hợp CI/CD GitHub)** — Các đường ống dẫn (pipelines) tự động kích hoạt quá trình đánh giá trên PR và gắn kèm các báo cáo

## Bắt đầu nhanh

```bash
# Cài đặt các gói phụ thuộc
uv sync

# Chạy tất cả các unit test
uv run python run.py test
# Lưu ý: Trên Windows, mong đợi sẽ có 7 test trong test_run_command.py thất bại
# bởi vì chúng dùng các lệnh UNIX shell (echo, exit, sleep, v.v.) không có sẵn trên Windows.

# Build bộ máy docker cho hộp cát sandbox
./docker/sandbox/build.sh

# Chạy full toàn bộ pipeline: thực thi AI-DLC workflow + evaluate + report (bắt buộc kết nối với Bedrock) cấu hình default
uv run python run.py full

# Chạy full toàn bộ pipeline: thực thi AI-DLC workflow + evaluate + report (bắt buộc kết nối với Bedrock)
uv run python run.py full \
    --vision test_cases/sci-calc/vision.md \
    --tech-env test_cases/sci-calc/tech-env.md \
    --golden test_cases/sci-calc/golden-aidlc-docs \
    --openapi test_cases/sci-calc/openapi.yaml

# Đánh giá 1 phiên bản chạy tồn tại (Bỏ mảng chạy thực thi tự khởi, chỉ chấm điểm dựa Bedrock)
uv run python run.py full \
    --evaluate-only runs/<run-folder>/aidlc-docs \
    --golden test_cases/sci-calc/golden-aidlc-docs \
    --openapi test_cases/sci-calc/openapi.yaml
```

## Luồng luân chuyển Pipeline đánh giá (Evaluation Pipeline)

Hệ thống đánh giá (`run.py full` hay lệnh từ chuỗi `scripts/run_evaluation.py`) đóng trục cho 6 cấu kiện nhịp theo giai đoạn:

| Giai đoạn (Stage)| Bộ Gói (Package)| Miêu tả chung |
| ------- | --------- | ------------- |
| 1. Thực thi | `packages/execution` | Làm hành khởi chạy AI-DLC ở kiểu đi bằng hai-agent (two-agent workflow) chạy sinh sinh tài liệu + source code|
| 2. Nhịp sau lệnh | (nằm tại môi trường execution) | Kéo đặt gói thư viện phụ thuộc và chạy các môi test của luồng dữ liệu (project's tests)|
| 3. Mã đo Phân lượng| `packages/quantitative` | Phân tích quét thông số đo cấu như lint lỗi, security-scan, và duplication test file (phát hiện mảng trùng code)|
| 4. Báo qua giao thức mạng| `packages/contracttest` | Định danh kéo đẩy trên Application chạy thông lệnh ở các cổng API |
| 5. Chất định tính| `packages/qualitative` | Ánh chép lại so các tài liệu trích sinh cùng các dòng tệp điểm vàng qua cấu máy Bedrock LLM|
| 6. Nhịp trả biên| `packages/reporting` | Gọi phân đoạn trích tống xuất ra các tờ gộp chung tệp hệ Markdown và bộ web HTML|

Dữ liệu chạy xả trên thư mục nhánh lưu định chuẩn dạng thời gian trong ô `runs/`:

```txt
runs/<timestamp>/
  ├── aidlc-docs/                    # AIDLC workflow documents
  ├── workspace/                     # Các lệnh dạng xuất sinh bằng ứng dụng qua mã Code
  ├── run-meta.yaml                  # Bộ nhân dạng của Run + cấu bộ mã
  ├── run-metrics.yaml               # Hệ thẻ số liệu giờ Tokens, thẻ thời lệnh, bộ lỗi...
  ├── test-results.yaml              # Kết xuất xả lệnh cuối đợt
  ├── quality-report.yaml            # Khóa dữ kiện từ hệ lỗi phần Linting + security + phát trùng lặp
  ├── contract-test-results.yaml     # Hệ cổng xác tín qua giao thông số của API
  ├── qualitative-comparison.yaml    # Kho lưu bảng định thành điểm mã
  ├── evaluation-config.yaml         # Trích sao toàn dải thiết lệnh hệ sinh
  ├── report.md                      # Báo phân đoạn tài liệu nén chung thành Markdown (để hiển thị Git)
  └── report.html                    # Lên hệ thống tệp đính kèm nén lại ở dạng File HTML cho mạng
```

## Cấu hình (Configuration)

### Cấu file qua file YAML (`config/default.yaml`)

Phiên chép tổng kiểm trên file thiết dòng đả thông cùng hệ AWS và gọi qua dòng phân mã với thông lượng swarms, biên đóng lệnh tắt (timeout), cấu dải của cấu đường Tool phụ. Tùy ý đâm đè dòng này mà làm lệch ngầm lệnh (bằng mác `--config`):

```yaml
aws:
  profile: "default"
  region: "us-east-1"

models:
  executor:
    provider: "bedrock"
    model_id: "global.anthropic.claude-opus-4-6-v1"
  simulator:
    provider: "bedrock"
    model_id: "global.anthropic.claude-opus-4-6-v1"
  scorer:
    provider: "bedrock"
    model_id: "global.anthropic.claude-opus-4-6-v1"

aidlc:
  rules_source: "git"   # "git" hay nhánh dạng cục bộ ("local")
  rules_repo: "https://github.com/awslabs/aidlc-workflows.git"
  rules_ref: "main"
  rules_local_path: null

swarm:
  max_handoffs: 200
  max_iterations: 200
  execution_timeout: 14400
  node_timeout: 3600

runs:
  output_dir: "./runs"

execution:
  enabled: true
  command_timeout: 120
  post_run_tests: true
  post_run_timeout: 300

execution:
  sandbox:
    enabled: true
    image: aidlc-sandbox:latest
    memory: 2g
    cpus: 2

tools:
  pmd_path: null   # Ghi thông qua path tìm chạy tới file PMD. Ở giá trị rỗng (null) nó móc tự từ máy thông báo môi trường (PATH) tìm ứng dụng "pmd".
```

Trật tự ghi đè cấu định: `CLI cờ ngầm flags > Cài thiết lập YAML config > bộ mở default (từ dạng Python built-in)`

### Phân dòng cấu hình cấu trúc model cá biệt

Khi lưu ở dạng rẽ ra các files YAML tại dòng ổ nhánh `config/` tự bám ngắt để khóa lệnh dọn chỗ (override) cài cấu vào định model ở riêng nhưng phần còn lấy tắp lự theo đúng rập cấu cho lệnh (`default.yaml`):

| File YAML gán biến | Model dòng trỏ tới |
| ------ | ------- |
| `config/opus-4-6.yaml` | Cấu Claude Opus 4.6 |
| `config/opus-4-5.yaml` | Cấu Claude Opus 4.5 |
| `config/sonnet-4-6.yaml` | Claude Sonnet 4.6 |
| `config/sonnet-4-5.yaml` | Claude Sonnet 4.5 |
| `config/nova-premier.yaml` | Cho qua Amazon Nova Premier |
| `config/nova-pro.yaml` | Mở đi Amazon Nova Pro |
| `config/nova-lite.yaml` | Lập Amazon Nova Lite |
| `config/mistral-large-3.yaml` | Mistral Large 3 (Bản hệ 675 tỷ B| 675B) |
| `config/devstral-2.yaml` | Bám cho Mistral Devstral 2 (Bản rèn cho coding| 123B, code-specialized) |

### Sandbox môi trường cho hệ nhúng bằng công nghệ (Docker Sandbox)

Nhằm giới tuyến khoanh riêng không gây sự vắt chéo tác hệ chạy lây sang mạng Máy chủ (Host). Bản tạo từ khung test cô rạch lưu qua 1 hệ hộp màng dạng môi trường phân luồng riêng ở bên của (dạng thùng ảo Container của nhà thông công nghệ docker). Hình chiếu thùng được đính đóng dạng: `Python 3.13`, trình móc `uv`, `Node.js 22` móc song cấu `npm`, bộ thông rã chung (không mang dải ranh hệ cấu bằng gốc qua người dùng Quyền Root - non-root user).

#### Các kiện phụ

Chắn chốt bộ sinh hệ ảo dạng chạy container Docker phải đang nằm được giải bọc ở bên ngay mặt sàn. Trình đi phải đảm thiết lôi cài từ ban khởi nguyên cho phía bên Máy gốc lưu.

#### Đóng định sinh bản hình thùng ảo

```bash
# Gây màng chạy khởi bộ ảnh container Docker Image (dữ định 1 lần từ khi khởi xướng / Cập hình từ cấu build Dockerfile mới)
./docker/sandbox/build.sh

# Build tay chạy tệp tùy thông lệnh tay
docker build -t aidlc-sandbox:latest docker/sandbox/
```

Việc này sẽ nhả ra image `aidlc-sandbox:latest` được tham chiếu mặc định trong các file cấu hình.

#### Tùy Biến Cài cấu hình

Thông qua rà cài thông ở tập ngầm cấu YAML đóng `config/default.yaml` mà chĩa tại nhánh có tag: `execution.sandbox`:

```yaml
execution:
  sandbox:
    enabled: true                # Đặt false nhằm bẻ thông qua màng chắn đi chốt trúng hệ máy chính Máy host(host system) 
    image: aidlc-sandbox:latest  # Docker định dạng image (Tên buộc phải Build trước mới gắn)
    memory: 2g                   # Hạn đóng sức mức vùng của RAM Container
    cpus: 2                      # Cho đóng giới hạn lõi của Container
```

Mỗi chừng bật mạng thùng áo Sandbox, mạng tiến bộ cấu thành của Stage phần dải giải mã chạy sau (thể ngạch của: Nhịp tiến kiểm thứ `số 2`/ Bộ Test kết API `số 4`) chạy trốn nắp vung dãn trong vũng ảo bên hệ Container. Hệ đính cho ổ tệp gán chép (mounted into) `workspace/` qua ngõ trạm kết của đường truyền hệ thư (`/workspace`). Trong lỡ như chẳng sinh bật nổi cấu định mở dòng dải của máy `Docker` đi cài vào (hoặc là ấn dạng mở là false). Thì những câu lênh Command luồn ở chế chạy thông rát vào cái trần máy chính.

### Hệ cài thiết chế của trạm (Tool configuration)

**Kho PMD (Bẫy đi cài cấu đả thông bắt đạo nhái mã lưu chèn mã):** Đi bắt những loại cài móc ở (PMD CPD / copy-paste detection) cài hệ dò trượt vào tại bước đánh mã Stage 3. Nếu xài tới ấn chỉnh dòng:

```yaml
tools:
  pmd_path: /path/to/pmd    # Cho địa chỉ đích thị chạy hệ ngầm của đuôi PMD xài trực lệnh vào
  # pmd_path: null           # Báo cho thiết dòng máy là ấn null hòng bỏ cấu này tự cắm bẫy tự móc nối máy để đi định hướng do PATH dẫn tự động.
```

Nhỡ cái cấu vấp bị thất bại là PMD không thấy được. Luồng lệnh cứ tiếp qua (skip analysis with a note)—Không quất cho đứt báo hỏng sập gãy cấu của dải.

### Hệ bảng nhúng vào hệ thống dạng giao thiết CLI gán mã 

```bash
uv run python run.py full \
    --vision test_cases/sci-calc/vision.md \
    --tech-env test_cases/sci-calc/tech-env.md \
    --golden test_cases/sci-calc/golden-aidlc-docs \
    --openapi test_cases/sci-calc/openapi.yaml \
    --config config/default.yaml \
    --profile my-aws-profile \
    --region us-west-2 \
    --executor-model global.anthropic.claude-opus-4-6-v1 \
    --scorer-model us.anthropic.claude-sonnet-4-5-20250929-v1:0 \
    --report-format both
```

Một số phân mã được hộ cản bảo trợ:

- `--config` — định trỏ path đến tệp quy cài dạng YAML (Cài chế chạy theo đường default ngầm định tại: `config/default.yaml`)
- `--test` — Chỉ rà luồng cho lệnh kiểm cục bộ unit test.
- `--vision`, `--tech-env` — Mồi tài đầu định châm (input vào execution).
- `--evaluate-only` — Trích qua hệ đánh thông ghi nhận không thôi tại dạng mốc folder tại dạng: (folder rẽ ổ thư ghi chú aidlc-docs) cấm không cho sinh cấu thực thi đản bảo test mới.
- `--golden` — Tệp rẽ đi chỉ trích tham khảo baseline docs directory. 
- `--openapi` — Định gán đặc chốt theo spec của luồng contract.
- `--report-format` — Lựa chọn kết ra bảng dạng xuất đuôi: `markdown`, `html`, hay cấu tạo gộp ra đủ đôi `both`.
- `--baseline` — Áp quyền chèn ghi đè trên folder cài tự dán lên quy theo điểm gốc `golden.yaml` (Do tính năng chìm nếu bật tự bứt đính vô đi liền đai cùng tag dạng chốt `--golden`).
- `--output-dir` — Gán đổi ổ phát rẽ tại luồng tạo Output tự động
- `--results` — Định ổ cho điểm chấm ra ở rẽ Qualitative lưu dạng kết ra qua format YAML tại phân ngạch đính thư thiết bằng tay path
- `--profile`, `--region` — Các cấu mã luồng đóng aws thiết quyền chèn vùng cho hệ mạng kết tới bộ máy bên Bedrock 
- `--executor-model` — Gán biến chèn mở execution 
- `--scorer-model` — Gán thay ghi dòng chấm giải qua dạng Model qualitative Scoring 
- `--rules-ref` — Khoá trỏ git ref (thẻ lưu dạng nhánh rẽ / branch / tag / commit qua mảng) tạo ở dạng cẩm nang rải dải AIDLC

## Rà soát đồng chạy cấu trúc chùm máy đa quy lô hàng Test lô (Batch Evaluation)

Chạy pipeline dọc cùng đợt bám quét bửa cấu rải qua đủ mảng chóp máy (Bedrock model), trích đạn tạo rải luôn báo về qua bộ ma trận báo chênh đánh tổng.

### Lên sổ ghi thông máy hỗ qua mô cấu (List available models)

```bash
uv run python run.py batch --list
```

### Triển hệ dòng đạn quét chùm Batch

```bash
# Phun đạn lên sạch các mô cấu models đã điểm ghi vô sổ YAML Config
uv run python run.py batch --models all

# Chỉ đạo dòng chạy súng đặc nhiệm (trích đích thị vào cấu mô YAML cài hệ tại folder ghim của thư mục config/ )
uv run python run.py batch --models nova-pro,sonnet-4-5

# Bơm quyền AWS thay quyền rà 
uv run python run.py batch --models all \
    --profile my-aws-profile \
    --region us-east-1
```

Phân lưu chạy mảng sẽ đi qua định cất ở nơi khu tại ngọn `runs/<model-name>/` vọn nén bám cùng đụn đủ tệp dán hệ. Sinh cài rải luôn chốt cất thành biến sổ dính cấu ở tên (batch-summary.yaml) về dán ở dòng rễ mẹ hầm Runs directory (gom dải tổng hiệu chạy Pass/Fail lẫn time).

### Tạo So sánh Chéo luồng Model Comparison

Mỗi hồi dòng trút xả báo cáo lô batch xong mảng:

```bash
# Đối chất đối đâm toàn dòng (Chạy tất bộ model runs chui nằm ở runs/)
uv run python run.py compare

# Ngắt chỉ lấy so riêng 2 bảng để ném đối mốc với bảng vàng baseline
uv run python run.py compare \
    --models nova-pro,sonnet-4-5 \
    --baseline test_cases/sci-calc/golden.yaml
```

Mục này nhả ra đôi tệp ở chéo ngọn chùm `runs/comparison/comparison-report.md` cùng file dính data `runs/comparison/comparison-data.yaml` đánh chéo đo chỉ bằng đòn ngang hông giữa hằng bao mẫu đo cho hệ thông.

## Đánh giá bằng lệnh điều (CLI Evaluation)

Chạy bệ trích thông qua lệnh giải đáp ứng trả AIDLC dùng chung khung gán qua các đai trợ giúp thông lệnh của AI sinh tại bộ dán (Claude Code, Kiro CLI) chui dọc dải từ mảng khung trích (`packages/cli-harness`).

### Bảng gọi bộ cài Adapter List available adapters 

```bash
uv run python run.py cli --list
```

Hỗ trợ dải Adapter: `claude-code`, `kiro-cli`.

### Lấy đà kích dải dòng chạy cấu CLI Test 

```bash
# Dải mảng băng Claude bằng lệnh gán --cli
uv run python run.py cli --cli claude-code \
    --vision test_cases/sci-calc/vision.md \
    --golden test_cases/sci-calc/golden-aidlc-docs

# Hất dòng bằng đụn máy tại Kiro thông qua Model cài --model
uv run python run.py cli --cli kiro-cli \
    --vision test_cases/sci-calc/vision.md \
    --golden test_cases/sci-calc/golden-aidlc-docs \
    --model claude-sonnet-4

# Kháo duyệt các khóa điều (prerequisite) dính trong adapter k test ngầm --check-only
uv run python run.py cli --cli claude-code --check-only
```

Cho chèn xả ổ đầu xuống rạch `runs/<cli-name>-<timestamp>-<uuid>/`. Bệ CLI trỏ qua khích kích chạy lệnh sinh giải hệ cho đai bộ Adapter, sau khi chạy đục vào dòng trích luồng `--evaluate-only` nhả xả chỉ qua lệnh chèn dải bộ scoring cho mảng 2-6 (stages 2–6).

## IDE Evaluation

Rọi dải AIDLC thông đai đèo IDE AI chắp ngoài qua cái phễu (IDE harness) luồn tại `packages/ide-harness`.

### Định bảng List Adapter cài IDE 

```bash
uv run python run.py ide --list
```

Giúp dải định của: Cursor, Cline, Copilot, Kiro, Windsurf, Antigravity.

### Gọi chạy của mảng cài giải luồng IDE 

```bash
# Thử bẻ cấu Cursor
uv run python run.py ide --ide cursor \
    --vision test_cases/sci-calc/vision.md \
    --golden test_cases/sci-calc/golden-aidlc-docs

# Rà dẻ điều kiện chuẩn của ide-adapter Adapter 
uv run python run.py ide --ide kiro --check-only
```

Hứng trả cục file vô ngọn của `runs/ide-<adapter-name>/`.

## Thử điểm cắm Móc nối Mở (Extension Hook Testing)

Nẻ mạch chạy rẽ hướng kiểm của AIDLC theo từng lớp chắp thiết rã ở nhóm cài đặt bộ quy tắc extensions hooks. Giúp dò la khả năng bóc gắn cấu phần mở qua chuỗi móc găm theo trích chọn từ đắn câu giải cài tuỳ biến qua trắc opt-in. 

```bash
# Liệt dòng nhóm nối cấu của tệp đính cấu bảng config 
uv run python run.py ext-test --list-configs

# Cứ dải thông chuẩn (lắp tuốt các hệ hook so với mốc gỡ trơn trui hệt bộ no hook extensions) 
uv run python run.py ext-test --scenario sci-calc

# Giải gắn bám chỉ đạo một nhánh rule nào chắp có support extensions cài hệ
uv run python run.py ext-test --scenario sci-calc \
    --rules-ref feat/extension_hook_question_split
```

Lệnh gài bộ kiểm qua cho đôi lần:
1. Thông chắp 1 vòng cho các dạng chĩa chốt nối là dòng Yes. (Mức guidance max đụng nóc).
2. Xổ 1 nháy bằng móc câu đóng cửa qua No (Lấy đè nguyên cấu trơ baseline đi ra).

Nhả hệ số đính file ở mỏ tại `runs/<scenario>/extension-test/` kèm file so sánh dòng ảnh cấu khi thay bộ extensions khác đi (impact của luồng).

Mục kĩ rẽ qua bọc lưu tài ở [Tài Liệu Móc Điểm cắm mở - Extension Hook Testing Guide](./docs/extension-hook-testing.md).

## Dải Phân Xu Phát hướng lưu đánh Trends (Trend Reporting)

Trình ghi lại theo chiều bảng lịch các chỉ số chốt định đo (metrics trend) đi chạy dải qua suốt của đánh. Nhảy vô luồng đi vớt bảng cất tại nhánh nhả (actions output artifacts + GH releases), tóm đổ về kết mạc cho ba luồng Markdown, Web (HTML), File rẽ của YAML.

```bash
# Trích nhả mã rẽ cho bản gốc trend (Phải khóa auth CLI GitHub của 'gh' mới xin file do mảng kéo)
uv run python run.py trend --baseline test_cases/sci-calc/golden.yaml

# Xả tóm dòng ở file html dính đai chi tiết (-v ) dải xuất xả bằng cữ ngặt `html -v`
uv run python run.py trend --baseline test_cases/sci-calc/golden.yaml --format html -v

# Đệm chập ổ bundle giải địa đệm bằng máy dải `--local` (máy trạm ở nhà)
uv run python run.py trend --baseline test_cases/sci-calc/golden.yaml \
    --local-bundle runs/my-run/report.zip

# Bẫy sập sườn trượt do trích xuất điểm xuống hạng (Thả Gate - Exit chìm mã Zero nếu trúng mộc tụt) 
uv run python run.py trend --baseline test_cases/sci-calc/golden.yaml --gate
```

Xuất nhả tổng sơ lược Web HTML sẽ mở điểm chiếu ghim theo 6 trục đo đánh:
- Khóa Điểm dòng chữ nghĩa (**Qualitative Score**) — định giải chập lại bảng với cái trục vàng chuẩn Golden Baseline. (Tỉ số đi cho to là đẹp).
- Thử cắm tại Mạch nối Contract (**Contract Tests**) — Thả dòng đo cửa cổng giải API tại phần ngạch tẩn Pass rate trên hệ chia phần Trượt/Pass. (Càng to đạt Pass là Win).
- Hệ Đo cấu Tĩnh cục Module (**Unit Tests**) — Rate Pass cho ra bảng %, cho thấy tỉ thắng qua unit. (Để Cao % càng to)
- Rà điểm bắt phốt lỗi cấu chuẩn Lint (**Lint Findings**) — Khắc đánh dòng Static bám vào điểm phát hiện (Để giảm/bé cầng tốt).
- Dòng chạy đi giải đo Thời (**Execution Time**) — Định quy tính tính sinh thời lưu hệ Generation. (Ít càng tốt).
- Hệ số đóng giải tiêu Lượng tokens (**Total Tokens**) — Mảng đi nhả qua LLM theo giá cước lượng (Token xài) bị đục cướp đứt hao theo lệnh. (Càng ít điểm này càng lợi).

Lưu ổ tại trục có khoá `runs/`.
Bản coi chơi trích chép ngỏ mã nhúng làm HTML lưu trỏ `packages/trend-reports/examples/trend-report.html`.

## Rẽ bám luồn thẳng cấu hành Bộ Lọc thực trực điều Execution Component Directly

Muốn làm chóp điều đĩa chạy không trù trừ chêm qua tay ngầm rã lệnh thì nhả tay bạo `aidlc-runner` mở chạy nguyên dòng độc cô (trực tiếp - directly): 

```bash
uv run aidlc-runner \
    --vision test_cases/sci-calc/vision.md \
    --tech-env test_cases/sci-calc/tech-env.md \
    --config config/default.yaml \
    --aws-profile my-aws-profile \
    --aws-region us-west-2 \
    --executor-model global.anthropic.claude-opus-4-6-v1 \
    --simulator-model us.anthropic.claude-sonnet-4-5-20250929-v1:0 \
    --output-dir ./runs
```

Cấu mảng luồng đả chuyển execution-specific qua cọc ngắt:
- `--rules-path <local-rules-dir>` — Khóa cản buộc chỉ theo lấy cưa rules từ túi Local 
- `--no-exec` — Tước bỏ dải luồn cài chạy Execution Command tự động trong bộ dán.
- `--no-post-tests` — Tước luôn cài lệnh kiểm Test Post-run

## Cấu tổ phân luồng Thư Móc kho chạy mẹ

```txt
.
├── run.py                     # Mút trỏ đi Master trỏ chóp vào bảng Evaluation phễu chính
├── scripts/                   # Xả mảng mã chạy đặc hiệu cho chạy nhịp Script
│   ├── run_evaluation.py      # Dải bộ kéo Pipeline Single 1 mảng máy model đo
│   ├── run_batch_evaluation.py    # Dải lô batch cho hệ đâm gôm báo các Model
│   ├── run_comparison_report.py   # Rẽ lập Report chéo so ngầm cho dải các báo cáo Model qua
│   ├── run_cli_evaluation.py      # Dải kích ngòi đánh điểm ở bên CLI hệ Adapter báo đánh
│   ├── run_ide_evaluation.py      # Tương tự như trên cho bản rẽ nẻ cấu trích đai IDE adapter
│   ├── run_extension_test.py      # Cài thử gán test bẫy dải mở tùy gắn (extension hook) opt-in
│   ├── run_trend_report.py        # Luồn xả băm báo cấu trend nhảy qua phát hành Cross-release
│   └── README.md              # Trợ chú báo cho dải Scripts folder
├── config/
│   ├── default.yaml           # Hệ lõi Default ban sơ nguyên cấu hình máy Models/AWS.
│   ├── nova-premier.yaml      # Luồn riêng override gán bộ biến Amazon Nova Premier cài lên executor
│   ├── nova-pro.yaml          # Amazon Nova Pro trích cài thế đè cấu
│   ├── sonnet-4-5.yaml        # Gán của chú em Claude Sonnet 4.5 thế cấu executor.
│   └── sonnet-4-6.yaml        # Nhỏ con này hệ của Claude Sonnet 4.6 thế vô.
├── packages/
│   ├── execution/             # Cuộn trình quản dải hai lõi hai chóp orchestrator Strands của AIDLC flow
│   ├── qualitative/           # Áp bộ qua chạy chữ ngữ nghĩa thiết dạt từ mốc điểm intent băm qua Bedrock
│   ├── quantitative/          # Xả điểm code từ đi dò bắt Linting/ Security quét, ngầm bắt cấu đi duplicated sao nhái rọi chiếu qua PMD CPD.
│   ├── contracttest/          # API tóm lệnh qua cấu chuẩn API OpenAPI (contract test) 
│   ├── nonfunctional/         # Kho đệm lưu số tính thời quy Token/ Điểm timing đồng trục 
│   ├── reporting/             # Chéo trộn chắp mã gộp rọi ở dạng trích report (HTML + MD)
│   ├── trend-reports/         # Đón mạch luồn report nhảy dải trend cho Release
│   ├── cli-harness/           # Dải chĩa kết phễu từ máy chạy Claude/ Kiro bằng CLI (adapters)
│   ├── ide-harness/           # Hệ nhúng kết nẻ Adapter luồng trên IDE (cho: Antigravity/ Cusion/ Cursor) v.v 
│   └── shared/                # Khối tảng Common mảng chung (utilities module)
├── test_cases/                # Túi ôm hệ điểm chốt Golden bản lưu dính Vision+Tech env đính docs Aidlc (chuẩn sao vàng)
├── runs/                      # Đệm chóp rẽ tạo nơi chứa trút xuất qua Evaluation
├── docker/
│   └── sandbox/               # Bằng chạy sinh mảng hòm thư chạy mác Sandbox chặn ngoài
├── docs/                      # Cập xấp lưu tập giải thông báo Tài trợ Document thêm
│   ├── extension-hook-testing.md  # Note hướng test dạng bẫy nhánh Extension chồi ra ở cấu Opt 
│   ├── ide-harness-design.md      # Thiết hệ dải nối phễu adapter qua hệ dòng mốc IDE 
│   └── file-structure.md          # Reference lưu chiếu cho đường mốc tổ Folder Repo thư sinh Code
├── pyproject.toml             # Config mở trạm qua uv cấu trúc của toàn project Workspace
└── uv.lock                    # Típ chóp ngắt Dependency qua điểm cọc bám (chống nhảy mã update).
```

## Tài liệu liên đính

- [Cẩm nang hỏi nhanh FAQ](./FAQ_VN.md) — Gỡ bí chóp cấu hỏi xoáy
- [Mô quy Contributing](./CONTRIBUTING_VN.md) — Dẫn nguồn thiết kết chóp mã Push vào dự án
- [Kiến giải Architecture](./ARCHITECTURE_VN.md) — Đóng chằng xé nẻ kỹ thuật System luồn ở trong
- [Extension Hook Testing](./docs/extension-hook-testing.md) — Kiểm bộ xẻ nhánh giải Hook qua nhiều dải cắm Configuration mở
- [Thiết quy IDE Adapter Architecture](./docs/ide-harness-design.md) — Bản phân IDE giải chốt kỹ.
- [Thiết Tổ thư - Cấu File Reference](./docs/file-structure.md) — Cách bày biện của thư mục kho.

## Đóng Góp

Lót chiếu đến tệp nhúng tại trang cờ đỏ [CONTRIBUTING_VN.md](./CONTRIBUTING_VN.md) hiểu quy ngầm quy chót làm trỏ cành vào repo.

## Giấy cấp Mác (License)

[Nơi gán đính dính thông của bằng thả bản quyền. Update later...]
