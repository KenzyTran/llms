# Hướng dẫn cài đặt — Generative AI Multi-Model Platform

Tài liệu này hướng dẫn cách cài đặt môi trường để chạy project trên Windows, macOS và Linux. Mình ưu tiên dùng **uv** làm trình quản lý package vì nó nhanh và đồng bộ tốt với `pyproject.toml` / `uv.lock` đã có trong repo.

> Nếu bạn đang xem file này trong Cursor, hãy chuột phải vào tên file bên trái và chọn "Open preview" để xem bản đã render.

## Bước 0 — Các "cạm bẫy" nên biết trước

80% các lỗi cài đặt mình từng gặp thực ra rơi vào mấy vấn đề hệ thống dưới đây. Đừng bỏ qua:

1. **Windows — quyền truy cập (permissions):** nếu gặp lỗi không chạy được script hay cài được phần mềm, hãy mở PowerShell bằng "Run as Administrator".
2. **Antivirus / Firewall / VPN:** thường xuyên can thiệp vào quá trình cài và gọi API. Tạm tắt khi cài đặt; thử bật hotspot điện thoại để loại trừ vấn đề mạng.
3. **Windows — giới hạn 260 ký tự cho đường dẫn:** nếu `git clone` báo lỗi đường dẫn quá dài, bật long path bằng: `git config --system core.longpaths true` (chạy PowerShell admin).
4. **Windows — Microsoft Build Tools:** một số package Data Science cần Visual C++ Build Tools. Cài từ https://visualstudio.microsoft.com/visual-cpp-build-tools/.
5. **macOS — XCode Command Line Tools:** chạy `xcode-select --install` nếu chưa có.
6. **SSL / chứng chỉ:** nếu ở mạng công ty gặp lỗi SSL khi gọi API hay tải model từ Ollama / HuggingFace, thường cần cấu hình CA certificate của công ty hoặc đổi mạng.

## Bước 1 — Git, thư mục dự án và Cursor

### Windows

1. **Cài Git** (nếu chưa có): tải từ https://git-scm.com/download/win và cài với mọi option mặc định.
2. **Tạo thư mục projects:**
   ```powershell
   cd $HOME
   mkdir projects
   cd projects
   ```
   Tránh đặt repo trong thư mục OneDrive vì dễ bị sync nhầm.
3. **Clone repo:**
   ```powershell
   git clone https://github.com/KenzyTran/llm_engineering.git
   cd llm_engineering
   ```
4. **Cài Cursor (hoặc VS Code):** tải từ https://cursor.com, cài mặc định, sau đó `File > Open Folder` và chọn thư mục `llm_engineering`.

### macOS / Linux

1. **Cài Git:** chạy `git --version` — nếu chưa có, macOS sẽ gợi ý cài Command Line Tools; Linux dùng package manager của distro (`sudo apt install git`, `sudo dnf install git`, …).
2. **Tạo thư mục projects:**
   ```bash
   cd ~
   mkdir -p projects
   cd projects
   ```
3. **Clone repo:**
   ```bash
   git clone https://github.com/KenzyTran/llm_engineering.git
   cd llm_engineering
   ```
4. **Cài Cursor / VS Code** tương tự phần Windows.

## Bước 2 — Cài `uv` và đồng bộ môi trường

Mình dùng [uv](https://docs.astral.sh/uv/) — package manager siêu nhanh, thay thế cho pip + venv + conda trong phần lớn tình huống.

1. Mở Terminal trong Cursor (`View > Terminal`), kiểm tra:
   ```bash
   pwd           # phải đang ở trong thư mục llm_engineering
   uv --version  # nếu báo lỗi thì cài uv theo link bên dưới
   ```
2. Nếu chưa có uv, cài theo Standalone Installer ở: https://docs.astral.sh/uv/getting-started/installation/
3. Sau khi cài, **mở terminal mới** rồi chạy:
   ```bash
   uv self update
   uv sync
   ```
   `uv sync` sẽ đọc `pyproject.toml` + `uv.lock` và tạo ra môi trường ảo `.venv/` với đúng phiên bản các thư viện đã được lock.

Ghi nhớ cách dùng uv:

- `uv add <package>` thay cho `pip install <package>`
- `uv run python script.py` thay cho `python script.py`
- Không cần tự activate venv — uv tự xử lý.

## Bước 3 — (Tùy chọn) Đăng ký các nhà cung cấp model

Project hỗ trợ nhiều provider. Bạn có thể dùng **tất cả**, hoặc chỉ một phần, hoặc chỉ dùng Ollama local mà không tốn một xu.

| Provider | Link đăng ký | Key trong `.env` |
|---|---|---|
| OpenAI | https://platform.openai.com | `OPENAI_API_KEY` |
| Anthropic | https://console.anthropic.com | `ANTHROPIC_API_KEY` |
| Google AI Studio | https://aistudio.google.com | `GOOGLE_API_KEY` |
| HuggingFace | https://huggingface.co/settings/tokens | `HF_TOKEN` |
| xAI (Grok) | https://console.x.ai | `XAI_API_KEY` |

Với OpenAI/Anthropic, nhớ **tắt Auto-Recharge** và chỉ nạp $5 để giới hạn chi phí. Toàn bộ project mình chạy hết khoảng vài đô.

## Bước 4 — Tạo file `.env`

Ở thư mục gốc của project, tạo file tên chính xác là `.env` (4 ký tự, không phải `.env.txt`). Nội dung ví dụ:

```dotenv
OPENAI_API_KEY=sk-proj-xxxxxxxx
ANTHROPIC_API_KEY=sk-ant-xxxxxxxx
GOOGLE_API_KEY=xxxxxxxx
HF_TOKEN=hf_xxxxxxxx
```

Key nào không có thì cứ để trống hoặc bỏ dòng đó đi. Nhớ **Save** file (`Ctrl+S` / `Cmd+S`).

File `.env` đã được `.gitignore` loại trừ nên sẽ không bị commit lên git.

## Bước 5 — Cài extension và mở notebook đầu tiên

Trong Cursor / VS Code, cài các extension:

- **Python** (ms-python hoặc anysphere)
- **Jupyter** (ms-toolsai)

Sau đó mở `01-frontier-and-ollama/day1.ipynb`, nhấn "Select Kernel" ở góc trên phải, chọn `.venv (Python 3.12.x)`. Nếu `.venv` không hiện ra, chạy lại `uv sync` rồi mở lại notebook.

## (Tùy chọn) Cài Ollama để chạy model local

Nếu muốn chạy các mô hình open-source mà không tốn phí API:

1. Tải Ollama từ https://ollama.com.
2. Chạy thử: `ollama run llama4` (mặc định là Scout, 17B active params). **Tránh** `llama4:behemoth` — quá nặng cho máy cá nhân.
3. Nếu không chạy được, mở thêm một Terminal khác và chạy `ollama serve`, rồi thử lại lệnh trên.

## Xong rồi!

Sau khi setup xong, mở `01-frontier-and-ollama/day1.ipynb` và chạy từng cell để xác nhận môi trường ổn. Nếu gặp vấn đề, xem thêm `setup/troubleshooting.ipynb` và `setup/diagnostics.ipynb` trong repo.
