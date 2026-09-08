# LLM Prompt Injection Benchmark / 大语言模型提示注入基准测试

这是一个基于 Ollama 的本地大语言模型安全评估实验。程序让一个模型生成提示注入攻击，再用目标模型回答，最后比较不同攻击策略和防御提示词下的 Flag 泄露情况。

This project is a local security benchmark for large language models powered by Ollama. One model generates prompt-injection attempts, target models respond under different defense prompts, and the pipeline records whether the test secret was disclosed.

## 功能 Features

- 测试基础提示注入和高阶提示注入策略。
- 比较基础防御与带意图分析的防御提示词。
- 支持同时测试多个 Ollama 模型。
- 将每次测试的输入、输出和成功标记保存为 CSV。

- Compare basic and advanced prompt-injection strategies.
- Evaluate multiple defense prompts against local Ollama models.
- Run the same experiment matrix across multiple target models.
- Export prompts, responses, and attack results to CSV.

## 实验结构 Experiment Matrix

当前默认包含三组实验：

1. Basic Attack vs Basic Defense
2. Master Hacker Attack vs Basic Defense
3. Master Hacker Attack vs CoT Defense

Each experiment runs the configured number of attacks for every target model. The default is 20 attacks per experiment.

## 环境要求 Requirements

- Python 3.10 or newer
- Ollama
- Enough local memory for the selected models

安装 Python 依赖：

```powershell
pip install -r requirements.txt
```

下载默认模型：

```powershell
ollama pull qwen3:30b
ollama pull gemma3:27b
ollama pull llama3.1:8b
```

Make sure the Ollama service is running at `http://localhost:11434` before starting the benchmark.

## 运行 Run

PowerShell：

```powershell
$env:SECRET_FLAG = "FLAG{local-test-secret}"
python pipeline_advanced.py
```

Linux/macOS：

```bash
export SECRET_FLAG='FLAG{local-test-secret}'
python pipeline_advanced.py
```

程序完成后，会在当前目录生成类似 `final_30B_ctf_report_<timestamp>.csv` 的报告文件。报告文件默认被 `.gitignore` 排除，不会提交到 GitHub。

## 安全说明 Security Notice

本项目仅用于经过授权的本地模型测试、课程实验和安全研究。不要将真实生产密钥、个人数据或未授权服务输入测试流程。默认 Flag 只是演示值，正式实验请通过 `SECRET_FLAG` 环境变量设置。

This project is intended only for authorized local-model testing, education, and security research. Do not use real production secrets, personal data, or unauthorized services. The default Flag is for demonstration only; use the `SECRET_FLAG` environment variable for experiments.

## 局限 Limitations

- 当前成功标准是响应中是否出现完整 Flag，无法覆盖变形、编码或部分泄露。
- 结果受模型版本、采样参数和本地硬件影响。
- 提示词防御不是生产级的机密保护机制；真正的秘密不应直接放入模型上下文。

The current metric only detects the exact full Flag and may miss partial, encoded, or transformed leakage. Results depend on model versions, sampling settings, and hardware. Production secrets should not be placed in an LLM prompt.

## License

No license has been selected yet. Add an appropriate license before redistributing this project.