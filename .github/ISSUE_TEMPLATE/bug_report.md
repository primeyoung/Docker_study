---
name: Bug report
about: Create a report to help us improve
title: ''
labels: ''
assignees: ''

---

**Describe the bug**
A clear and concise description of what the bug is (e.g., CUDA Out of Memory, package dependency conflict, model loading failure).

**Environment Information (Critical for AI/LLM)**
- **OS:** [e.g., Ubuntu 22.04 LTS / CentOS 7.9]
- **GPU(s):** [e.g., NVIDIA RTX 4090 x 2 / A100 80GB]
- **CUDA & cuDNN Version:** [e.g., CUDA 12.1 / cuDNN 8.9]
- **Python Version:** [e.g., Python 3.10.12]
- **Key Libraries:** [e.g., PyTorch 2.4.0, Transformers 4.44.0, Accelerate 0.33.0]
- **Environment Type:** [e.g., Miniconda / Docker / venv]

**Steps to Reproduce**
Steps to reproduce the behavior from cloning to running:
1. Run `git clone https://github.com...`
2. Install dependencies via `pip install -r requirements.txt`
3. Download weights and place them in `...`
4. Run python script: `python main.py --model_path ... --device cuda`
5. See error

**Error Logs / Terminal Output**
```bash
# Paste the full Python Traceback or Linux terminal error logs here
```

**Expected behavior**
A clear and concise description of what you expected to happen (e.g., WebUI starts successfully, inference generates text, training loop begins).

**Model & Dataset Context (If applicable)**
- **Model Name/Repo:** [e.g., Llama-3-8B-Instruct / Qwen2-7B-Instruct]
- **Weights Source:** [e.g., Hugging Face / ModelScope / Custom Checkpoint]
- **Quantization:** [e.g., None / bfloat16 / int4 AWQ / GPTQ]
- **Dataset Format:** [e.g., Alpaca JSON / ShareGPT / Local custom text file]

**Additional context**
Add any other context about the problem here (e.g., used `HF_ENDPOINT` proxy for download, modified `config.json`, running inside a multi-node cluster, or file permission issues on Linux).
