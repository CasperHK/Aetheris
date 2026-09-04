# Aetheris
A ultra-lightweight, AI-ready, and embeddable browser engine core built on top of Servo and Rust, designed specifically for high-efficiency LLM automation, web scraping, and local edge-AI agent workflows.

---

## 🌟 Vision & Motivation

Mainstream browsers (like Chromium) are engineered for human-centric browsing with massive multi-process overhead, complex GPU compositing, and heavy JavaScript runtimes. However, **AI Agents and automation pipelines** do not need heavy GUI rendering—they need raw speed, minimal memory footprints, and clean semantic DOM extraction.

**RustyAIAgentBrowser** bridges the gap by leveraging **Servo** (the parallel, memory-safe web rendering engine) combined with native Rust AI runtimes (like Hugging Face `candle` and `tract`), providing a low-resource browser built for the AI era.

---

## 🚀 Key Features

* **📦 Ultra-Low Memory Footprint**: Bypasses Chromium's heavy architecture. Consumes a fraction of the RAM, making it ideal for running dozens of concurrent headless instances.
* **🧠 Native AI-Ready Integration**: 
  * Direct integration with local edge AI frameworks (`candle`, `tract`) for on-device embedding generation, summarization, and vision tasks.
  * WebNN and hardware acceleration bindings.
* **🧹 Semantic DOM Distillation**: Strips away human-centric visual bloat, outputting clean, token-optimized semantic trees directly consumable by Large Language Models (LLMs).
* **🔒 Memory Safe & Concurrent**: Built completely in Rust, ensuring thread safety and robust performance under heavy concurrent scraping or agentic loops.
* **🔌 Flexible Embedding API**: Can be easily integrated into Python or Node.js agent frameworks via FFI or gRPC bindings.

---

## 🏗️ Architecture Overview

```text
+-------------------------------------------------------------+
|                     AI Agent Orchestrator                   |
|         (LangChain, AutoGen, CrewAI, or Custom Rust)       |
+------------------------------+------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|                 RustyAIAgentBrowser Core                    |
|  +-----------------------+       +-----------------------+  |
|  |     Servo Engine      | <---> |   Local AI Pipeline   |  |
|  |  (HTML/CSS/DOM & JS)  |       | (Candle / SLM / Embed)|  |
|  +-----------------------+       +-----------------------+  |
+------------------------------+------------------------------+
                               |
                               v
                       Target Webpage / API
```

---

## ⚙️ Getting Started

### Prerequisites

* **Rust Toolchain**: Stable Rust (edition 2021 or later)
* **Servo Dependencies**: Depending on your OS, you may need standard graphics and windowing development libraries (e.g., FreeType, FontConfig, OpenGL/Vulkan headers).

### Installation

Clone the repository and build the core engine:

```bash
git clone https://github.com/your-username/rusty-ai-agent-browser.git
cd rusty-ai-agent-browser

# Build in release mode for maximum performance
cargo build --release
```

### Quick Start Example (Rust)

```rust
use rusty_ai_browser::{BrowserContext, AgentConfig};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Initialize the headless browser context
    let config = AgentConfig::default().with_low_memory_mode(true);
    let mut browser = BrowserContext::new(config).await?;

    // Navigate to a target URL
    let page = browser.navigate_to("https://news.ycombinator.com").await?;

    // Extract AI-ready semantic summary directly
    let summary = page.extract_semantic_summary().await?;
    println!("Page Semantic Summary:\n{}", summary);

    Ok(())
}
```

---


---

## 🧩 Custom Model Configuration (`config.toml`)

To allow AI Agents to process web content using different local models (such as smaller SLMs, custom embedding models, or vision models), **RustyAIAgentBrowser** provides a flexible configuration system via `config.toml`.

### Example Configuration (`config.toml`)

```toml
[browser]
headless = true
low_memory_mode = true
max_concurrent_pages = 10

[ai.pipeline]
default_task = "summarization"

# Define multiple custom models for different pipelines
[ai.models.primary_llm]
provider = "candle" # Options: candle, tract, ollama, custom
model_path = "./models/phi-3-mini-4k-instruct-q4.gguf"
context_window = 4096
temperature = 0.1

[ai.models.embedding_model]
provider = "candle"
model_path = "./models/bge-small-en-v1.5"
pooling = "mean"

[ai.models.vision_model]
provider = "tract"
model_path = "./models/moondream2-q8.onnx"
enabled = false
```

### Loading Custom Config in Rust

```rust
use rusty_ai_browser::{BrowserContext, AgentConfig};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Load custom configuration from a TOML file
    let config = AgentConfig::from_file("config.toml")?;
    let mut browser = BrowserContext::new(config).await?;

    // Switch or specify models dynamically during runtime
    browser.set_active_model("primary_llm").await?;

    let page = browser.navigate_to("https://example.com").await?;
    let structured_data = page.extract_with_custom_model("primary_llm").await?;
    
    println!("Processed Content: {:#?}", structured_data);
    Ok(())
}
```


## 🗺️ Roadmap

- [ ] **Phase 1: Core Integration**
  - [ ] Set up Servo embedding wrapper.
  - [ ] Implement headless viewport and basic DOM traversal in Rust.
- [ ] **Phase 2: AI Optimization**
  - [ ] Integrate Hugging Face `candle` for local text extraction and embeddings.
  - [ ] Build token-optimized semantic tree parser to minimize LLM context usage.
- [ ] **Phase 3: Agentic Tooling**
  - [ ] Provide Python bindings (`PyO3`) for seamless integration with existing AI frameworks.
  - [ ] Implement native action hooks (click, type, extract, scroll) optimized for LLM function calling.

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/your-username/rusty-ai-agent-browser/issues).

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.
