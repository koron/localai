# AGENTS.md — localai

This is a bash wrapper around `llama-server` (llama.cpp) that serves GGUF models from HuggingFace.

## Commands

```bash
./bin/localai Qwen3.5-9B          # start serving a model (model name = basename of conf file)
./bin/localai Qwen3.5-9B -- ...   # extra args appended to llama-server
```

## Layout

- `bin/localai` — entrypoint script
- `localai.d/localai.conf` — global defaults (llama-server path, host, port)
- `localai.d/models/<model>.conf` — per-model config: `user`, `model`, `quant`, `args[]`

## Adding a model

Create `localai.d/models/<name>.conf` with:

```bash
user="huggingface_user"
model="repo-name-GGUF"
quant="UD-Q4_K_XL"
args=(--temp 0.6 --top-p 0.95)
```

The script resolves `hf "${user}/${model}:${quant}"` via llama-server.

## Notes

- `HF_TOKEN` auto-loaded from `~/.cache/huggingface/token` if present
- No package manager, no tests, no linter, no CI — it's just a shell script
- `llama-server` must be installed separately (default `/opt/llama.cpp/bin/llama-server`, overridable in `localai.d/localai.conf`)
