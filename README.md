# Run an 8B Model with a 16K Context

This setup runs **Hermes 3 (8B)** locally using `llama.cpp` and connects it to **AnythingLLM** to give it free web search capabilities (RAG). It's specifically optimized to squeeze a massive 16,000 token context window into an older 4GB VRAM graphics card (like an Nvidia GTX 1650) without crashing.

## Prerequisites
* **OS:** Windows
* **GPU:** Nvidia (Tested on a 4GB GTX 1650)
* **Model:** A GGUF file of `hermes3:8b` (You can easily grab this via Ollama or Huggingface)
* **Software:** A compiled version of `llama.cpp` (specifically `llama-server.exe`) 

---

## Step-by-Step Guide

### Step 1: Fire up the Local AI Server
First, we need to start the brain. We are bypassing standard UI tools and running `llama-server.exe` directly via the command prompt. This lets us force specific memory optimizations so the context cache doesn't instantly fill up our tiny GPU.

Download CMak
Install the Nvidia CUDA Toolkit


Open your Command Prompt and run this exact command. *(Note: You will need to replace the absolute file paths with wherever your specific server and model files are located).*

```cmd
git clone https://github.com/TheTom/llama-cpp-turboquant.git

cd C:\Users\HP\llama-cpp-turboquant
mkdir build
cd build
cmake .. -DGGML_CUDA=ON 
cmake --build . --config Release

C:\path\to\your\llama-server.exe -m "C:\path\to\your\model\hermes3-8b.gguf" -c <window size> --cache-type-k q4_0 --cache-type-v q4_0

e.g C:\Users\HP\llama-cpp-turboquant\build\bin\Release\llama-server.exe -m "D:\2026\LLM\Hermes-3-Llama-3.1-8B-Q4_K_M.gguf" -c 16384 --cache-type-k q4_0 --cache-type-v q4_0
```

-c 16382: This forces a massive ~16K context window, giving the AI plenty of room to read web articles.

--cache-type q4_0: This compresses the short-term memory cache down to 4-bit. This is the magic trick that makes a 16K context window fit on a 4GB card without throwing an Out-of-Memory error!

Once the console says HTTP server listening on 127.0.0.1:8080, leave that black window open in the background.
