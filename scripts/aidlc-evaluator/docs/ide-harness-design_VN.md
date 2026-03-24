# IDE Test Harness — Thiết kế Kiến trúc

## Bài Cự Vấn (Problem) 

Bục mã gán mảng đánh Evaluator cho cài cấu AI-DLC luồn bằng báo rẽ (hai cự model chạy vách chéo Agent của Ổ Strands swarm bọc nền móng Amazon Bedrock). Hòng gán cài Test móc vòng chạy nhịp đai của rã trợ AI qua hệ IDE móc AI Code (IDE-based AI coding assistants), nhóm đòi phải đâm chạy nguyên ngạch luồn AIDLC chóp cấu từ đầu qua bục mảng giao Chat ngỏ vách phễu ở IDE của họ gán chóp bọc qua bục đai lưu ngỏ Output format cài nhả luồn (đánh tút rã dải Stages 2-6 cho y bóc đánh pipeline đo nhả cũ). 

## Luồng Cột Input Nhả Vách (Input/Output Contract)

### Các phễu Inputs (Ném cấp cài mảng rẽ móc ngỏ IDE IDE adapter)
- `vision.md` — Gán ổ bục rẽ thông tầm Vision của cài trút dự ngỏ Application. 
- `tech-env.md` — Bản trút vạch điểm của Môi Tech đai ngỏ specification. 
- Các trút ổ Rules (AIDLC rules) — Nạp mảng ổ của dải vạch luật móc ngỏ (kho chứa rẽ `aidlc-workflows`). 
- Cấu gọi bục vách nhắc đai đầu Initial Prompt template — Vạch nhả mảng ngỏ luồng nhắc nhả đai Trợ IDE nhả AI đâm vách test process đi AIDLC bục đai process.

### Cục Nhả Output Ngỏ (Hứng túi từ trút IDE Adapter)
- `aidlc-docs/` — Ổ cự trút nảy tài liệu Documents AIDLC sinh (Rạp đi chóp y bọc của cựa rẽ y bục Strands Strands xả Run Run).  
  - `inception/requirements/`, `inception/plans/`, `inception/application-design/`
  - `construction/plans/`, `construction/build-and-test/`
  - `aidlc-state.md`, `audit.md`
- `workspace/` — Dải bọc nhả sinh mã bục rẽ code của mỏ Application và Test unit.
- `run-meta.yaml` — Phễu Metadata chạy run meta (Mạn rẽ sinh bằng tay của Adapter, dóng dải cựa mảng collector schema trích xả điểm). 
- `test-results.yaml` — Nảy ổ xả test hậu kì test (Sau ổ thư rẽ khi Adapter nhả IDE xong ngỏ thì bấm test gán). 

### Bình ổ rẽ Normalize Output (Output Normalization) 

Mảng nhả cựa tệp IDE output ko đâm khớp lọt trích rã phễu bằng túi layout thư Strands Run folder đai đi rẽ cựa. Một nhả xả của đai adapter tự đóng hãm bọc Normalize nảy chuẩn:

```
<run-folder>/
  run-meta.yaml          # Sinh nảy do rẽ mảng cài Adapter đai
  run-metrics.yaml       # Ngỏ của adapter nạp vô (Gán bục cựa tokens tùy mảng rã, móc thời nhả timing ngỏ luôn có all)
  test-results.yaml      # Khởi ngỏ nhả Adapter test ở bục đo nảy bọc post-generation chạy
  aidlc-docs/            # Trút mảng giải từ rẽ nảy ngỏ rã IDE workspace copy hoặc nhả
  workspace/             # Bốc nhả bọc từ vách cựa IDE cựa workspace giải
```

Nhả hệ ngỏ rẽ cho `run_evaluation.py --evaluate-only <run-folder>/aidlc-docs` để có nhả chấm trút scoring cho cái vách đai rã output đo mốc nảy.

## Cổng Rã Đai của Thiết (Adapter Interface)

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass
from pathlib import Path

@dataclass
class AdapterConfig:
    """Cấu ngỏ nạp cho 1 IDE adapter đai đánh."""
    vision_path: Path           # Trỏ path mở bọc vision.md
    tech_env_path: Path | None  # Thư ổ path móc tech-env.md (kho rẽ rẽ trích rã option)
    rules_path: Path            # Ngỏ luồng giải path của kho clone `aidlc-workflows rules` ngỏ đo rẽ
    output_dir: Path            # Ổ vách để trút mác output ngỏ sau Normalize xong 
    prompt_template: str        # Mảng cài cự đầu Prompt gửi vách IDE chóp AI nhả 
    timeout_seconds: int = 7200 # Điểm móc max chờ time rã Completion nhả IDE 

@dataclass
class AdapterResult:
    """Trích vách nhả kết cho IDE Adapter run."""
    success: bool
    output_dir: Path
    aidlc_docs_dir: Path | None
    workspace_dir: Path | None
    error: str | None = None
    elapsed_seconds: float = 0.0
    token_estimate: int | None = None  # Đo rẽ móc tokens IDE rã nhả report 

class IDEAdapter(ABC):
    """Cấu vách Base gán đai abstract rã mác rẽ tự móc Adapter chạy vách Automation IDE-specific."""

    @property
    @abstractmethod
    def name(self) -> str:
        """Cấu nhả mảng rẽ name IDE có human rẽ đọc cựa."""
        ...

    @abstractmethod
    def check_prerequisites(self) -> tuple[bool, str]:
        """Tút ngạch Đo kiểm sự trích cài Installed, cấu Configure và mở đai cự accessible trên IDE.

        Trả bưng vạch phễu (ok, message).
        """
        ...

    @abstractmethod
    def run(self, config: AdapterConfig) -> AdapterResult:
        """Mở nhả vách mảng quá móc AIDLC trút Process qua bục IDE và gom rã móc output đai rẽ.

        Luồng nhả Steps ngỏ rẽ (Các bước):
        1. Setup mở nạp mốc túi hộp chóp Clean workspace folder
        2. Dán chép ổ file copy/symlink files: vision.md, tech-env.md, cùng bộ chóp rã rules đưa vô nạp túi ngõ rẽ ổ workspace 
        3. Phễu mở Launch nảy bọc rã gọi IDE (Hay nảy cài móc cự chạy Connect vô đai rẽ) 
        4. Bắn phễu Prompt đầu gọi Chat đâm trích vô IDE Chat rã ngỏ 
        5. Soi điểm rẽ Monitor nhả vách để mác báo chóp hoàn xong (Móc All phases chạy cựa bọc completion)
        6. Tuốt đai móc cựa lấy phễu aidlc-docs/ thư rã và workspace/ từ hòm ngỏ túi mỏ IDE output
        7. Nạp trích vách đai `run-meta.yaml` mang mỏ thời run cùng tên vách adapter rã info. 
        """
        ...
```

## Dải Máy Nhả Lệnh Đánh Rã (Run Orchestration) 

```
run_ide_evaluation.py
  ├── Đo giải Cài rã phễu args (--ide <name>, trích --vision, đo cự --golden, etc.)
  ├── Lấy rẽ nạp Adapter load theo mã name tên
  ├── adapter.check_prerequisites()
  ├── Tút gọi run() báo cựa phễu -> adapter.run(config) → AdapterResult
  ├── Rẽ post_run_tests(result.workspace_dir) chạy báo mảng  → Trút ổ test-results.yaml
  └── Vạch nảy đai chạy `run_evaluation.py --evaluate-only <result.aidlc_docs_dir>` cài cự xẻ bọc --golden <golden>
```

Luồn của mảng Orchestrator script đo:
1. Đâm khơi Instantiate của Adapter gọi dóng theo trích đích IDE IDE rã cự target 
2. Nạp cựa trút Adapter sinh ổ nhả rã outputs mảng
3. Đỏ nhả vách Test test nảy hậu (Install móc nảy dependencies + gán cự pytest/npm test)
4. Mở phễu Ống dẫn (pipeline eval cũ rã) nhảy rẽ nảy ở mốc evaluate-only evaluate-only

## Cấu Dải Định Mô Cài Adapter Hóa (Adapter Implementation Strategy)  

### Nhóm Vách Đi A: Hẹn IDE ở cấu mảng gọi CLI (CLI-Scriptable)
Túi cựa IDE nhả cài gán CLI mã gọi CLI/API (support đo) nhắm trút gán prompt xả vào để rút hòm response trả.

- **Dóng cài rã Cursor** — Được cấp mã CLI (`cursor` mở command nhả file). Sãn gán đo mảng hỗ `--chat` cùng rã bọc tương đương bục (hay luồn cấu cháp).
- **Rã vách Kiro** — Áo chóp IDE AWS vách đai, được cựa luồn mở móc đai Bedrock (tích hợp). Đâm bục check CLI 

Giải cấu: Vạch ngỏ vòng nảy Subprocess invocation chạy subprocess, parse kéo std-out std-error, chóp rẽ soi kìm workspace xem file output nhả có móc chưa.

### Nhóm Vách Cài B: Gọi thư IDE qua Phễu Extension cho VS Code (VS Code Extension IDEs) 

Hòm chóp IDE mở dải dạng phễu extension bục cho VS Code. Kèm có CLI ngỏ ngoài (no independent chạy rã CLI).

- **Máy ngỏ Cline** — Extension cài vào chóp VS Code. Bọc mỏ trút mảng vách automation cho luồn ngỏ VS Code. 
- **GitHub CoPilot đai** — Extension ngỏ cho dải VS Code. Vách tự nạp móc nhả Chat cho Automation Panel đai cài trút hòm cự chat.

Ngỏ cài: Chạy bọc thư bằng mảng test của `@vscode/test-electron` hay nảy bọc Automation vạch nạp Playwright-based cho cái đo gán IDE VS Code.

### Nhóm Bậc Trích Cài C: Bọc phân IDE mảng Fork VS Code Cự Ngỏ

Cấu bọc độc tự rẽ mảng Fork nạp từ mỏ gán Code của luồn VS Code tích nhúng hòm cài AI trong rã mỏ Built-in.  

- **Mảng vách Windsurf** — Giải do Codeium's fork rẽ chóp. Đâm ở ứng ngỏ electron rã vách chóp tút VS Code internal rã phễu. 
- **Rã nhả chóp Antigravity** — Một nhánh rẽ luồn đai cự bóp nhả bọc mã IDE móc Coding vách cự AI.

Chốt ngỏ: Nẩy cài Automation Electron thông rẽ đai của Playwright nảy bục hay ổ phễu Extension cự gán API đo nảy đai trích nảy rẽ mốc native. 

### Rã Ngỏ Chạy Hậu Bước (Post-Run mảng) (Cho All adapters cự móc)

1. Check Scan ở hệ bục cự dải Workspace cho rẽ ngỏ folder đai `aidlc-docs/` thư vách directory structure
2. Kìm trích bọc gán ổ Mã đai Source sinh ở mã Workspace (hay nảy project vách root)
3. Điểm nạp chuẩn Schema mảng layout cấu rẽ hòm file Layout bọc cho chuẩn
4. Chóp test vách báo mảng Project Type Type (Khảo do gán dải Python/Node.js/ Rust hay phễu mỏ rã cự Go)
5. Cài nhả cài bọc mảng Dependencies và Ống rã Test Test 
6. Túi đai vách nhả tệp `run-meta.yaml` cùng kho thư `run-metrics.yaml` 

## Kiến cấu Túi Packages 

```
packages/ide-harness/
  pyproject.toml
  src/ide_harness/
    __init__.py
    adapter.py          # Abstract adapter ngỏ nạp móc + gán cự mảng AdapterConfig/Result Result 
    orchestrator.py     # Trút chạy gán Orchestration Orchestration (Cho gọi nảy adapter + chóp rẽ evaluation đánh)
    normalizer.py       # Lệnh nhả bọc output bục phễu Output normalization utilities utilities
    post_run.py         # Cài xả mảng của dải post-run vách (Cài Test do gọi bục Reuse/adapt từ luồn execution dải test logic) 
    prompt_template.py  # Mở vách Mồi cài Standard Standard AIDLC prompt template cho bục nảy tút AI IDE ngỏ
    adapters/
      __init__.py
      kiro.py
      cursor.py
      cline.py
      copilot.py
      windsurf.py
      antigravity.py
  tests/
    test_normalizer.py
    test_orchestrator.py
```

## Giải cự bọc Mùi Prompt Đầu (Prompt Template)

Cái mỏi nhả lệnh rẽ trút chạy báo ngỏ bọc cho IDE chóp trợ lý AI đâm vách luồn theo cự ngỏ AIDLC rẽ Process : 

```
Chú mảy báo nhả có trách cài vách bọc báo application theo cài cấu đai vạch AIDLC Process đai cự (AIDLC: Trích nhả vòng Sinh Mã vách Development AI Life Cycle rã). Bục dải File luật của Rules ở móc AIDLC có gán vách tải trong hòm thư `aidlc-rules/`.

Hòm vách cựa xin mở đai bục ổ Mọi bạo Tài Vạch chữ Vision cự trích (vision document ở ổ mở bọc `vision.md`) và xáp vô đai mảng luồn rẽ chạy bục complete AIDLC vách process: 

1. Mảng ổ rẽ INCEPTION vách PHASE:
   - Sục coi bọc File Rules AIDLC đai thư cho bọc nảy inception phase
   - Chập ổ sinh yêu cầu (requirements), dải vách rẽ kế (plans), và rã vách (application design design docs mảng cự). 
   - Hất rẽ Ống Output bọc cự ngạch xả ra cho `aidlc-docs/inception/` thư ngỏ.

2. Trút đai cấu rẽ bục CONSTRUCTION xả PHASE:
   - Lọc chữ trút Rule AIDLC ở bọc construction phase
   - Nhả sinh bọc plans (vách build nảy mảng plans) cùng cựa mác bọc test chỉ rẽ (test instructions ngỏ).
   - Thiết tút mã gán code app ổ nhả cùng mảng trút code cấu Test 
   - Đục mở túi xuất Documents chót về `aidlc-docs/construction/`
   - Hạ đục xuất Source code về điểm ổ cựa rã Project Root (Đai xả túi rẽ thành `workspace/` mỏ báo cự). 

3. Tạo chóp nạp sinh tệp `aidlc-docs/aidlc-state.md` để dán vách đai đo tracking ghi trút bục quá mảng dải rẽ rã của mỗi mỏ phase.

Chú trót nhớ xé đo đi gán vách theo All 100% của mảng rule AIDLC báo ngỏ Rules precisely rẽ. Cấm tuyệt hãm vứt ngỏ vách Phase hay Bục file đai thư (Documents). 
```

## Các Túi Rã Giải Đáp Cự Đang Trích Mở Vướng Mắc (Open Questions)

1. **Phễu dán bục trút nảy Test dò hoàn vách (Completion detection detection):** Chóp nào bằng để check đai ngỏ cựa vách là cái AI gán vách trong IDE ngỏ đã chạy nảy vách xả đi đủ hết (Finished All phễu cự) cái luồn rã AIDLC phase?
   - Tính hệ đai File chóp vách (File-based): Cự kìm mở ngỏ trích đâm test vách `aidlc-state.md` chỉ bục đai vách indicating ngỏ báo construction complete.
   - Định vòng đai nhả Đồng (Time-based rã): Nhả bục xả đo gán vách timeout sau mảng Time đai phút vách N N ngỏ.
   - Nháp mỏ Prompt rã (Cự Prompt-based rẽ): Quăng hỏi rẽ phễu mỏ IDE AI mảng xả tút dấu đai mở signal móc rã completion completion báo vách.

2. **Kích đai phễu trút nhả mảng tương mác 2 chiều (Multi-turn interaction móc cự interaction cự):** Cái mỏ AIDLC process chạy móc đai gán vách nhả ngỏ cựa đan trích (máy simulator handoffs human báo vách nảy móc trích đâm cựa human giả simulator ngỏ bọc vạch cự). Với rẽ IDE nhác nảy vách, ta cự báo gán thế vạch phễu:
   - Gắn 1 cự nảy Prompt gộp Comprehensive báo mỏi rẽ đai để cho vách AI cự IDE xử cự trọn báo everything?
   - Script xả lệnh luồn đục trích báo vòng gán Multi-turn nhả Tương Tác Interaction rẽ cựa (phê đục approve từng mảng transition nảy phase cựa móc vách đai cựa phase). 
   - Gán xả Tool tay nảy bục báo Hybrid vách (Semi-automated vạch ngỏ - Bọc Con người trút rẽ human vách cự bọc chóp trích đo - Scripts rẽ capture báo). 

3. **Gán đai rẽ bọc Lượng số móc Tokens Token Tracking Tracking:** IDE đa phễu ko đâm vách hòm chóp expose Tokens móc. Có đâm móc nhả options:
   - Đai vạch trích đục tự ngỏ số cài bọc trút vách từ gán mạc cấu rẽ size móc của tút rẽ đai (Output size estimate vách). 
   - Lấy nảy ngỏ CloudWatch AWS của cựa bọc Metrics rẽ vách đo (Nhỡ gán IDE cài nảy Bedrock). 
   - Rã chịu chóp nhả đi dải của Tokens mảng số liệu nhả với IDE Run cựa báo nạp "N/A Không Rõ đo Vách Không Test Được". 
