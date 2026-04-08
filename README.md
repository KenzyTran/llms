# Generative AI Multi-Model Platform

> Project cá nhân — xây dựng các ứng dụng Generative AI bằng cách kết hợp **hơn 20 mô hình frontier và open-source** thông qua HuggingFace, LangChain và Gradio; hiện thực **RAG** và **Agents**; đồng thời **đánh giá / benchmark** mô hình một cách có hệ thống giữa nhiều nhà cung cấp.

Repo này là nơi tôi ghi lại toàn bộ quá trình thực hành khi xây một nền tảng đa mô hình — từ việc gọi API các frontier model (OpenAI, Anthropic, Google, xAI) cho tới việc chạy các mô hình open-source trên HuggingFace và Ollama, rồi ghép chúng lại thành các workflow RAG và hệ đa agent.

![Voyage](assets/voyage.jpg)

## Mục tiêu project

- **Đa mô hình, đa nhà cung cấp:** tích hợp >20 mô hình (GPT, Claude, Gemini, Grok, Llama, Qwen, DeepSeek, Phi, Mistral, …) thông qua cùng một lớp code để dễ so sánh.
- **Đa framework:** HuggingFace Transformers, LangChain, Gradio cho UI, Chroma/FAISS cho vector store.
- **RAG:** truy xuất tri thức trên knowledge base nội bộ và sinh câu trả lời dựa trên ngữ cảnh.
- **Agents:** hệ nhiều agent cộng tác, có công cụ (tool use), có bộ nhớ và có khả năng ra quyết định.
- **Evaluation & benchmarking:** đo chất lượng, chi phí và tốc độ của các mô hình trên cùng một bài toán.

## Cấu trúc repo

Mỗi thư mục `weekN` là một module — các module build tiếp lên nhau, tăng dần độ phức tạp.

| Thư mục | Nội dung chính |
|---|---|
| [01-frontier-and-ollama/](01-frontier-and-ollama/) | Gọi frontier API (OpenAI) và mô hình local qua Ollama. Ứng dụng đầu tiên: web summarizer. |
| [02-multi-provider-gradio/](02-multi-provider-gradio/) | Kết nối đồng thời nhiều provider (OpenAI, Anthropic, Google, Ollama). UI bằng **Gradio**, xây chatbot và công cụ có tool use. |
| [03-huggingface-opensource/](03-huggingface-opensource/) | Chạy các mô hình **open-source HuggingFace** trên Google Colab với GPU — pipelines, tokenizers, generation. |
| [04-code-gen-benchmark/](04-code-gen-benchmark/) | So sánh và **benchmark** nhiều frontier model cho bài toán sinh code hiệu năng cao (Python → C++), đo tốc độ thực thi và độ chính xác. |
| [05-rag-langchain/](05-rag-langchain/) | **RAG** với **LangChain**: embeddings, vector store, retriever, đánh giá chất lượng truy xuất trên `knowledge-base/`. |
| [06-agentic-ai/](06-agentic-ai/) | **Agentic AI**: hệ nhiều agent cộng tác (planner, scanner, pricer, messenger…), kết hợp lại RAG và frontier model để giải quyết bài toán end-to-end. |

## Setup

Hướng dẫn cài đặt đầy đủ cho Windows / macOS / Linux: [setup/SETUP-new.md](setup/SETUP-new.md).

Tóm tắt nhanh:

1. Cài Python 3.11 và [uv](https://docs.astral.sh/uv/) (hoặc Anaconda).
2. `uv sync` để cài dependencies theo `pyproject.toml` / `uv.lock`.
3. Tạo file `.env` chứa các API key cần dùng (`OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, `GOOGLE_API_KEY`, `HF_TOKEN`, …). Bạn có thể bỏ trống key nào không dùng.
4. Cài [Ollama](https://ollama.com) nếu muốn chạy mô hình open-source tại local: `ollama run llama3.2`.
5. Với Week 3 và Week 8 (phần GPU nặng), tôi chạy trên Google Colab — link Colab được nhúng trong từng notebook.

## API cost

Mục tiêu của tôi là giữ chi phí ở mức **vài đô cho toàn bộ project**. Hầu hết notebook có thể chạy bằng model rẻ (`gpt-4.1-nano`, `claude-3-haiku`, `gemini-flash`) hoặc thay thế hoàn toàn bằng Ollama local.

Bảng monitor chi phí:

- OpenAI: https://platform.openai.com/usage
- Anthropic: https://console.anthropic.com/settings/cost
- Google AI Studio: https://aistudio.google.com/

